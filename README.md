# WIMS — Where Is My Stat

**WIMS** on the home screen, **Where Is My Stat** everywhere there's room to say it: a GameChanger-style scoring app for **recreational slowpitch softball leagues** (iOS 17+ / iPadOS, SwiftUI + SwiftData).

See `SoftballScorer-Spec.md` for the full data-model and architecture design. Module names (`SlowpitchEngine`, `SlowpitchStore`, `SlowpitchUI`) describe what the code does and are deliberately not branded — they never appear in front of a user.

## The engine: `SlowpitchEngine`

A dependency-free Swift package implementing the core scoring logic as an **event-sourced reducer**:

> Scoring is an append-only log of events, and game state is a pure function of that log.

`Engine.reduce(state, event)` folds one event into the state; `Engine.state(of: log)` folds a whole game. Because it's pure and deterministic, the same log always yields the same state — which is what makes undo, resume-after-crash, corrections and eventual cloud sync tractable.

### What the engine handles automatically
- Ball/strike count (including a configurable starting count), walks with forced base running, and strikeouts.
- The **one-foul rule** (a foul with two strikes is an out).
- Outs, inning/half transitions, and batting-order wrap — **extra hitters (EH) just work**, because the order wraps around whatever length the lineup is.
- The **run-per-inning limit** ("5-run rule") with an **open last inning**, capping runs mid-play.
- **Mercy rules**, regulation end, walk-offs, and skipping the bottom half when the home team already leads.
- **Double plays** (two outs), the **force-out timing play** (no run scores when the third out is a force), and RBI suppression on GIDPs and reached-on-error.
- Running R/H/E, a linescore, per-batter box-score lines, and a **plate-appearance projection** with scorebook notation (`6-3`, `F8`, `E6`, `2B`) for the scorebook grid.
- **Hit locations** normalized to a canonical field space, with the spray-chart **zone derived automatically**.

### Validate before you append
`reduce` is deliberately total — it never throws and never rejects input, so the UI can always append. The cost is that a mis-tap could otherwise be scored quietly wrong. So call `validate` first:

```swift
let issues = engine.validate(result, against: state)
// .error   → block: something would be silently dropped
// .warning → confirm: scorable, but maybe not what was meant
if engine.isScorable(result, against: state) {
    log.append(.inPlay(result))
}
```

It catches advances from empty bases, base collisions that would lose a runner, more outs than remain, double plays with no second out, sac flies with nobody scoring, and runs the per-inning cap will swallow.

### Corrections
Nothing is ever deleted. `GameLog` is append-only; a mis-scored play is **voided** and optionally replaced, and the game rescores on replay:

```swift
log.undoLast()                          // undo
log.void(id: someEvent.id)              // fix a play from three batters ago
log.replace(id: single.id, with: .hit(.double))
let state = engine.state(of: log)       // always derived, never mutated
```

### Rules: enforced vs. recorded
`RuleSet` is split so it can't promise enforcement it doesn't deliver:

- **`ScoringRules`** — read by the engine. Innings, outs, count, walk/strikeout thresholds, one-foul rule, run limit, open inning, mercy thresholds, home-run cap.
- **`ConductRules`** — recorded only, never consulted by the reducer. Time limits, flip-flop, roster shape (defensive players, EH caps, coed alternation), courtesy runners, pitch arc, no-steal/no-bunt/no-leadoff, safety base. Validate these in the UI; changing one does **not** change scoring.

`RuleSet.genericRec` is a sensible default preset.

## Build & test

### Quick check — works with only the Command Line Tools (no Xcode needed)

```bash
swift run EngineChecks
```

Compiles the engine and runs the full scenario suite, printing PASS/FAIL (nonzero exit on failure, so it works in CI).

### Full XCTest suite — requires Xcode

`swift test` needs `XCTest`, which ships **inside Xcode.app**, not the Command Line Tools. If it reports `no such module 'XCTest'`:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
swift test
```

Or open `Package.swift` in Xcode and run the tests (⌘U). You'll need Xcode for Phase 1 regardless.

## Layout

```
Sources/SlowpitchEngine/
  Model.swift               # Value types: Side, BySide, Base, Position, FieldLocation/Zone
  RuleSet.swift             # ScoringRules (enforced) + ConductRules (recorded)
  Events.swift              # Event vocabulary, PlayEvent, and the append-only GameLog
  GameState.swift           # Reduced state: bases, linescore, box score, PA projection
  Engine.swift              # The pure reducer — the heart of Phase 0
  Validation.swift          # ScoringIssue + validate(), so mis-taps fail loudly
  Scoring+Convenience.swift # Readable constructors for events and advances
Sources/SlowpitchEngineTestKit/
  Fixtures.swift            # Shared fixtures (tests and EngineChecks use the same ones)
Sources/EngineChecks/
  main.swift                # XCTest-free self-check runner
Tests/SlowpitchEngineTests/
  EngineBasicsTests.swift   # Count, outs, order wrap, walks, strikeouts
  RuleTests.swift           # Slowpitch rules: one-foul, run limit, mercy, HR cap, DP, force-out
  ValidationTests.swift     # Silent-mis-scoring guards
  LogAndProjectionTests.swift # Corrections, PA projection, field zones
  FullGameTests.swift       # Full games, walk-off, JSON round-trips
```

## Status

- **Phase 1 — done.** SwiftData persistence, live scoring, tap-the-field diagram.
- **Phase 2 — done.** Teams, rosters and lineup building.
- **Phase 3 — done.** Seasons and cross-game stats. Exactly one season is current, new games are filed under it, and stats are scoped to a season — so a new season starts from zero without deleting last season.
- **Phase 4 — done.** Box-score PDF, share card, season report, CSV, and a portable JSON archive for backup and sharing.
- **Spray charts — done.** Every located batted ball, replayed from the log, filterable by batter and result.

- **iCloud sync — on by default, falls back cleanly.** A stored preference decides whether `SlowpitchContainer` asks for a CloudKit-backed store, and it falls back to a local store either way, so the same binary works with or without the iCloud capability. See `CLOUDKIT.md` for the Xcode steps and the one real cost (a paid developer membership; the storage itself is free).

## Not done yet

- **Pitch-by-pitch detail** beyond balls and strikes, and defensive stats.
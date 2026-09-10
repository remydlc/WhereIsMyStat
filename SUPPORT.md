# WIMS — Support

**Where Is My Stat** — scorekeeping for recreational slowpitch softball.

Questions, bugs or feature requests: **nine3one2[@]gmail[.]com**
(Written with brackets to keep address harvesters off it — remove them to send.)

Please include your iPhone model, your iOS version, and what you were doing
when it went wrong. If it happened during a game, the inning and the play help
a lot.

---

## Common questions

### Where is my data kept?

On your phone. If iCloud sync is on (Settings → Storage), it's also mirrored to
your own iCloud account. Nothing is sent to a server — WIMS doesn't have one.

### How do I back up, or move to a new phone?

**Settings → Backup & sharing → Export everything.** That produces a single JSON
file holding your teams, seasons and every game with its full play-by-play. Save
it to iCloud Drive or Files, and import it on the new phone from the same
screen.

Do this before changing phones even if iCloud sync is on. It takes ten seconds
and it's the only copy entirely under your control.

### Sync doesn't seem to be working

Check **Settings → Storage**. "Currently" should read *iCloud*. If it says
*This device*:

- Make sure the sync toggle is on, then close and reopen WIMS — the connection
  is made once, at launch.
- Check the phone is signed into iCloud (the Settings app → your name).

Sync is not instant, and it needs the app to have been opened at least once on
the other device.

### Someone else is scoring for me this week

1. **Settings → Backup & sharing → Export everything**, and send them the file.
2. They import it, so both phones share the same rosters.
3. They score the game, then swipe left on it in Games and send it back.
4. You import that — just the one game arrives.

Step 1 matters. Players are matched by an internal id, not by name, so a
stand-in who retypes the same roster creates a *different* team and none of it
counts towards your season.

### I scored a play wrong two innings ago

Press and hold the play in the play-by-play list, then **Change result** or
**Remove play**. Everything after it is worked out again, so runners, outs and
the score may shift — glance at the score afterwards.

To undo the most recent play instead, use the undo arrow at the top.

### Somebody turned up late

**⋯ → Add batter.** Pick them from the roster (which keeps their season stats
together) or type a name for a guest. They bat last in the order from that
point on; earlier innings are untouched.

### A regular didn't show and I need a sub

Keep recurring subs on the team roster marked inactive. They appear under
**Subs and reserves** when you build a lineup, and picking one for tonight
doesn't change their status.

### The rules aren't behaving as I expect

Rules are copied onto a game when it starts, so editing a preset doesn't change
a game already in progress. To see what the game in your hand is actually
using: **⋯ → Rules for this game**.

### Why did stats reset?

They're counted per season. Starting a new season puts everyone back to zero
without deleting last year — switch seasons from the stats screen to see it
again.

---

## Known limits

- iPhone only, iOS 17 and later.
- iCloud sync covers your own devices. There's no live sharing with players.
- The other team's at-bats can be entered as a lump sum per half-inning rather
  than batter by batter — that's deliberate, since nobody scores both sides.
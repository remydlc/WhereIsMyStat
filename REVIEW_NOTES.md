# App Review — standing answers

Apple's information request on the first submission asked six questions. These
are the answers, kept so the next submission doesn't start from scratch. Paste
the relevant ones into the Resolution Center, or into App Review Information →
Notes to head the questions off next time.

---

## 2. Purpose and target audience

WIMS (Where Is My Stat) is a scorekeeping app for recreational **slowpitch
softball** — beer leagues, coed leagues, workplace and church leagues.

The problem it solves: slowpitch has rules that general baseball and softball
scoring apps treat as edge cases — batting orders longer than the defense
(extra hitters), four outfielders including a rover, a run limit per inning with
an open final inning, mercy rules, the one-foul rule, home run caps. Existing
apps either can't express those rules or misapply them, so scorekeepers end up
with a paper scorebook and a spreadsheet.

WIMS lets a team manager or volunteer scorekeeper configure their league's rules
once, score a game by tapping a field diagram, and get a box score, season
statistics and spray charts out of it.

Target audience: adult recreational league players and team managers, typically
one designated scorekeeper per team.

## 3. Setting up and accessing the main features

**No account, login or credentials are required.** The app opens straight to its
main screen and every feature is available immediately. No demo account is
needed — there is nothing to sign into.

To exercise the app from a clean install:

1. **Teams tab → +** to create a team. Add players individually, or use
   **Auto-fill roster** to generate placeholder batters instantly.
2. **Games tab → +** (New game). Both sides default to "Quick team", which needs
   no setup at all — just tap **Start game**.
3. On the scoring screen, tap **Ball**, **Strike**, or a result such as **1B**
   or **GO**. For a batted ball, a sheet opens where you tap the field diagram
   to record where it went and how runners advanced, then **Record**.
4. **⋯ → Box score** shows the line score and batting lines.
5. **Teams → a team → Stats** shows season statistics and the spray chart.
6. **Settings → Backup & sharing** exports everything as a JSON file.

No sample files are required. The app creates its own data.

## 4. External services, tools and platforms

**None.** WIMS has no backend server and makes no network requests to any
service operated by the developer or any third party.

- No data providers, no authentication service, no payment processor, no
  analytics, no advertising, no AI services, and no third-party SDKs of any kind.
- The only network functionality is **Apple's CloudKit**, used solely to mirror
  the user's own data into **their own iCloud private database** so it syncs
  between their devices. The developer cannot read, retrieve or delete that
  data. This is optional and can be switched off in Settings → Storage.
- Everything else is on-device: scoring, statistics, PDF and CSV generation.

## 5. Regional differences

**None.** The app functions identically in all regions. There is no
region-locked content, no geolocation, no regional pricing (the app is free with
no in-app purchases), and no region-dependent features. It is currently
localised in English (U.S.) only, and behaves the same everywhere.

## 6. Regulated industry / protected third-party material

**Not applicable.** WIMS is not in a regulated industry and contains no
protected third-party material.

- The app icon, interface and all artwork are original and owned by the
  developer.
- No league, team, or sporting body names, logos or marks are included.
- The slowpitch rules the app implements (innings, run limits, mercy rules) are
  rules of play that the user configures themselves; the app ships no
  copyrighted rulebook text.
- All other content is entered by the user — their own team and player names.

---

## Points worth pre-empting

### User-generated content (Guideline 1.2)

WIMS does let a user type in names — their own teammates and opponents — but
this is **not user-generated content in the sense the guideline addresses**.
There is no feed, no service that distributes content between users, no way for
one user to see another user's content, and no server that receives it. Data
stays on the user's device and, optionally, in their own private iCloud.

The export feature writes a file that the user shares themselves through the
system share sheet, exactly as a document-based app does. Because no content is
ever published or shared through a service the developer operates, the content
reporting and blocking requirements do not apply.

### Accounts (Guideline 2.1)

There are no accounts, so there are no demo credentials to supply.

### In-App Purchase (Guideline 3.1.1)

The app is free and contains no in-app purchases or subscriptions.

### Business model (Guideline 3.2)

WIMS is a general consumer app for the public, not limited to any specific
business, organisation or employee group. Public App Store distribution is the
correct channel.

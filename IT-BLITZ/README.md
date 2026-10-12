# IT BLITZ — Information Technology Challenge

A retro arcade "Pick-a-Box" IT quiz game show. Everything is in one
self-contained file — no install, no server, works offline.

## How to open in VS Code
1. Open this `IT-BLITZ` folder in VS Code (File → Open Folder…).
2. Open `index.html`.
   - To play: right-click `index.html` → **Open with Live Server**
     (install the "Live Server" extension), **or** just double-click
     `index.html` in your file explorer to open it in any browser
     (Chrome, Edge, Firefox).

## Files
- `index.html` — the game.
- `docs/IT-BLITZ-Mechanics.pdf` — printable mechanics sheet (English).
- `docs/sample-question-pack.json` — the default content, settings and branding as an
  importable file (version 3) (copy it as a starting point for your own pack).
- `screenshots/` — start screen, team setup, board, and a question.

## Editing content, settings & branding (no code)
On the start screen press **✏️ Edit / Settings**.

| Tab | What you can change |
|---|---|
| 🏷 Game Name | Title (wordmark), subtitle, tagline, call-to-action line — with live preview |
| ❓ Categories & Questions | Pick a category → rename it → pick a point value → edit the question, the 4 choices (A–D), and click a letter to mark the one correct answer. ◀ ▶ step through all 30. |
| 🎤 Bonus Questions | Add / edit / delete tie-breaker questions (at least one) |
| ⚙ Settings | Default timers, wrong-answer penalty, steal, score to win, tie-breaker value/time, team count, team names & colors |
| 🎨 Branding | School/organization name + logo, accent colors (4 presets or custom), interface language (English / Tagalog) |
| ℹ Help & JSON | The pack file format |

- **✔ Save** (or Ctrl+S) checks everything first: no empty question or choice,
  no duplicate choices, exactly one correct answer, unique category names,
  timers/points are positive whole numbers, team names filled in and unique.
  Problems are listed — click one to jump to it.
- Saved packs are kept in the browser's localStorage (survives refresh).
  If storage is blocked, edits still work for the current session.
- **💾 Export** downloads content, settings **and** branding as one `.json`; **📂 Import**
  loads one back into the editor (review, then Save). A file without
  `"settings"` keeps your current settings; a file without `"branding"`
  (version 1–2) gets the default branding.
- **↺ Reset to Default** restores the original IT BLITZ content, settings and branding (asks first).

### Game settings
| Setting | Default | Allowed | Change per game on Team Setup? |
|---|---|---|---|
| Time per question | 15 s | 5–300 | — |
| Steal time | 10 s | 3–120 | — |
| Tie-breaker question time | 20 s | 5–300 | — |
| Round timer | On, 10 min | Off or 1–180 min | ✔ |
| Wrong answer / timeout costs | Full points | Full / Half / Nothing | ✔ |
| Steal on wrong answer | On | On / Off | ✔ |
| Score to win | Off | Off or any positive number | ✔ |
| Tie-breaker bonus value | 10,000 | 1–1,000,000 | — |
| Teams | 3 (Cyan, Magenta, Lime, Gold, Violet) | 3–5, any name/color | ✔ count & names |

The penalty rule also applies to a missed tie-breaker bonus; a missed steal
never costs points. Saving settings resets the Team Setup choices to the new defaults.

### Branding
- **School / organization:** a name and an optional logo, shown as a small badge
  above the title on the start screen and above the results. Uploaded logos are
  shrunk to fit 400×400 px and stored as an embedded image (data URL), so they
  work offline and travel inside Export files. You'll see a warning when a big
  image was shrunk. **Remove Logo** clears it.
- **Accent colors:** Primary (cyan), Secondary (magenta) and Gold (points), using
  4 ready-made themes (Neon Arcade, Ocean Breeze, Sunset Fiesta, Maroon & Gold)
  or color pickers. A color that's too dark to read on the dark background is
  lightened automatically, and the editor tells you when that happens. The 5
  category columns and team colors keep their own colors.
- **Language:** English or Tagalog for all built-in interface text: buttons, labels,
  verdicts, steal and tie-breaker prompts, How to Play, results and review. Your
  title, categories, questions and team names are never translated. Players can
  flip it with the **EN | TL** button on the start screen and in the game header;
  that choice is remembered. The editor itself stays in English.
- Branding changes preview live; **Close** without saving puts the old look back.

Note: localStorage is per browser *and* per location — opening the file from
a different folder or computer starts from the defaults. Use Export/Import to
move a pack.

## Question pack JSON format
```json
{
  "format": "it-blitz-question-pack",
  "version": 3,
  "title": "IT BLITZ",
  "subtitle": "Information Technology Challenge",
  "tagline": "Pick a category. Choose your points. Test your IT knowledge.",
  "callToAction": "Are you ready to test your IT knowledge?",
  "categories": [
    {
      "name": "Programming",
      "questions": [
        { "points": 2500,
          "question": "What keyword is used to define a function in Python?",
          "choices": ["function", "def", "func", "define"],
          "correct": "B" }
      ]
    }
  ],
  "bonus": [
    { "question": "How many bits are in one byte?",
      "choices": ["4", "8", "16", "32"], "correct": "B" }
  ],
  "settings": { "questionTime": 15, "penalty": "full" },
  "branding": {
    "schoolName": "Rizal National High School",
    "logo": "data:image/png;base64,iVBORw0KGgo…",
    "accentColors": { "primary": "#25e0ff", "secondary": "#e23af0", "gold": "#ffd23f" },
    "language": "en"
  }
}
```
- Exactly **5** categories, each with **6** questions (2500, 5000, 7500, 10000, 15000, 20000).
- Exactly **4** choices per question; `"correct"` is `"A"`–`"D"` (a number 0–3 also works, 0 = A).
- `"points"` puts a question on the right tile; without it, list order is used.
- `"bonus"` is optional — if missing, the built-in bonus questions are used.
- `"settings"` is optional — missing keys keep your current values:
```json
"settings": {
  "questionTime": 15, "stealTime": 10, "bonusTime": 20,
  "roundMinutes": 10,      // 0 = round timer off
  "penalty": "full",       // "full" | "half" | "none"
  "stealOn": true,
  "bonusPoints": 10000,
  "targetScore": 0,        // 0 = off
  "teamCount": 3,          // 3, 4 or 5
  "teams": [ {"name": "Team Cyan", "color": "#25e0ff"}, … 5 in total ]
}
```
  (the `//` comments are for explanation only — real JSON can't contain them.)
- `"branding"` is optional (version 3). Missing or invalid values use the defaults:
  `schoolName` (text, "" = hidden), `logo` (a `data:image/…` URL, "" = none),
  `accentColors` (`#rrggbb` hex for `primary`, `secondary`, `gold`), and
  `language` (`"en"` or `"tl"`). Version 1 and 2 files still import.

## Editing in code (optional)
In `index.html`, these are the **defaults** (what "Reset to Default" restores).
A pack saved in the editor overrides them in that browser.
- `DEFAULT_BRAND` — title, subtitle, tagline, call-to-action.
- `DEFAULT_CATEGORIES` — the 5 category names.
- `DEFAULT_QUESTIONS` — keyed by category name (must match `DEFAULT_CATEGORIES`
  exactly); 6 entries each, easy → expert:
  `{q:"question", a:["A","B","C","D"], c:1}` where `c` is the correct index (0 = A … 3 = D).
- `DEFAULT_BONUS` — tie-breaker questions, same shape.
- `DEFAULT_SETTINGS` — every game setting above (timers, `penalty`, `stealOn`,
  `roundOn`/`roundMinutes`, `targetOn`/`targetScore`, `bonusPoints`, `teamCount`,
  `teams` names & colors). Each line is commented.
- `DEFAULT_BRANDING` — school name, logo, `accentColors` and `language`. Each line is commented.
- `THEME_PRESETS` — the one-click color themes in the 🎨 Branding tab (add your own).
- `UI_TEXT` — **all interface text**: `UI_TEXT.en` (English) and `UI_TEXT.tl` (Tagalog),
  each key side by side. Edit a wording there; `{name}`, `{pts}`, `{n}`, `{s}`, `{total}` are
  filled in automatically, so keep them. If a `tl` key is missing, the English one is used.
  The difficulty labels on the tiles are the `diff` entry in each language.
- `POINTS` / `DIFF` — tile values and the editor's difficulty labels.
- Other spots, if you go deeper: `penaltyFor()` (how Full/Half/None is worked out),
  `settingsProblems()` (the allowed ranges Save checks), `fillChips(...)` calls in
  `renderSetup()` (the preset chips on Team Setup, e.g. 5/10/15 min), and the
  `.col:nth-child(n)` CSS rules (category column colors), `readableColor()` (the
  readability lift, 4.5:1 contrast), `LOGO_MAX_SIDE` / `LOGO_MAX_CHARS` (logo size cap),
  and `fitGame()` (the game screen tightens itself only when it would otherwise scroll).
- The accent colors are CSS variables in `:root` (`--cyan`, `--magenta`, `--gold`, plus
  `-rgb` and `-2` variants). They're overwritten at runtime from the branding, so change
  the defaults in `DEFAULT_BRANDING`, not in the CSS.
- Don't edit `QUESTION_TIME`, `STEAL_TIME`, `BONUS_TIME`, `BONUS_POINTS`,
  `TEAM_THEMES` directly — they're filled from the settings when the page loads.

If you change the defaults in code and don't see them, an editor-saved setup
is overriding them: open the editor and press **Reset to Default**.

## Quick rules
- 3–5 teams take turns picking a category + point value (2,500–20,000).
- 15s per question (configurable). Correct = +points (auto back to board).
  Wrong/timeout = −points by default (full / half / none — configurable; can go negative).
- Steal: on a wrong answer, the game manager picks which team buzzes in
  (10s window); correct = they take the points, miss = no penalty.
- Tie for 1st → sudden-death "grab the mic" bonus question (10,000 pts).

Live version: https://claude.ai/artifact/MqXmrz78NAWSgC7wCDST4E

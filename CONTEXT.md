# CONTEXT.md — First & Learn
> Drop this file in your project root. At the start of every Claude Code session, say: "Read CONTEXT.md and get oriented before we start."

---

## Project Overview

**First & Learn** is a single-file HTML/CSS/JS e-learning game built for the Articulate E-Learning Heroes Challenge #554 ("Design Like an Art Director"). It's a football-themed cybersecurity training game.

- **Live URL:** https://mcroney531-ctrl.github.io/firstdown/
- **Repo:** GitHub → GitHub Pages auto-deploy
- **Stack:** Vanilla HTML + CSS + JS, single file (`index.html`)

---

## File Structure

```
/
├── index.html          ← entire app (HTML + CSS + JS, single file)
├── helmet.png          ← AI-generated grey-shell helmet, transparent bg (canvas pixel-tinted per team)
├── ball.png            ← real football PNG, transparent bg (normal mode only)
├── pixelhelmet.png     ← pixel helmet template for T-Mode (extracted from inline base64; canvas pixel-tinted per team)
├── Cowboys_2_0_D.otf   ← used for EZ text, midfield logo abbreviation, yard numbers
├── Tecmo_Bowl.ttf      ← used in T-Mode for all heading-level text
└── CONTEXT.md          ← this file
```

**Coming assets:** additional fonts, icons, images — all drop into repo root.

---

## Game Mechanic

- 10 cybersecurity modules mapped to 10-yard increments on a football field
- Ball starts at the 10-yard line; each module = read content → answer 2 MCQs
- **Both correct** → first down, ball advances 10 yards, small flag with topic label drops at that yard line (clickable to review)
- **One wrong** → "2nd Down, Try Again" (retry from content screen)
- **Two/three fails** → "4th Down — Incomplete Pass. Time to punt." → PUNT resets ball to 10, module restarts
- **All 10 complete** → Touchdown screen (team-colored)
- Content overlay: field fades dark, single centered card overlay ("Madden briefing room energy")
- Immediate per-question feedback
- Back button on questions → returns to content screen (not team select)
- Close button → exits overlay, shows field; yellow RESUME button anchored to ball re-enters where you left off

---

## 3-Screen Flow

### Screen 1 — Welcome
- Dark green field aesthetic, yard lines/hash marks
- Two-column layout: title block left, rules right
- SELECT YOUR TEAM button → fade transition to team select

### Screen 2 — Team Selector
- Madden-style carousel, all 32 NFL teams, city names only (no logos/trademarks)
- Real official NFL hex colors
- Banner: helmet (canvas-tinted), city name, team colors bleeding into background
- Arrows embedded in top corners of banner (◄ left, ► right)
- Shuffle icon in bottom-right corner of banner
- Left panel: SELECT dropdown (quick-pick) + Mode dropdown below it
  - **Practice Mode** (default) — SKIP button on questions, relaxed briefing language
  - **Game Mode** — no skip, "real deal" briefing language
- **T-MODE toggle** floats to the right of the card with "✦ TRY IT" label
- START GAME button anchored near bottom of card, yellow gradient

### Screen 3 — Game
- Full SVG overhead football field
  - Dark green, alternating stripe pattern, yard lines, hash marks, hash number marks on both ends
  - End zones: team primary color, full city name in Cowboys 2.0 font (large, bevel/stroke)
  - Midfield: team-colored circle with abbreviation in Cowboys 2.0, double ring in secondary color
- Ball (`ball.png`) starts at 10-yard line, advances with smooth animation
- Yellow gradient FIRST DOWN button anchored to bottom-right of ball, small, with "Click to begin the activity" hint text
- 9 topic labels pre-loaded on field at designated yard lines (faint white)
- HUD pills: MODULE 01 · 1ST DOWN · TEAM ABBR — with orange/black down marker board showing live down number
- Game briefing popup on START GAME: team-colored gradient header, BEGIN button with team gradient → dismiss to begin

---

## T-Mode (Tecmo Bowl Mode)

Toggled from team select screen (floating right of card).

- Swaps body fonts → `Press Start 2P` (Google Fonts)
- Swaps heading fonts → `Tecmo Bowl.ttf`
- Swaps realistic `helmet.png` → pixel-tinted version of `pixelhelmet.png` (separate file, same canvas tinting logic; referenced via `pixelHelmetImg.src = 'pixelhelmet.png'`)
- EZ text, midfield logo abbreviation, yard numbers, topic labels → Tecmo Bowl font
- `ball.png` → **canvas-drawn pixel football** (programmatically drawn, no image file, guaranteed transparent bg)
- Abbreviation on helmet: upper-left dome area (~42% across, 30% down), smaller size

### ⚠️ Last Open Item — Pixel Football
The canvas-drawn pixel football for T-Mode was **in progress when the last session ended**. Plan:
- Draw entirely in canvas (no image file) to avoid remove.bg/alpha channel problems from earlier attempts
- Normal mode keeps `ball.png` as-is
- This is the first thing to pick up on resuming

---

## Color System & Fonts

| Role | Value |
|---|---|
| Base background | `#ffffff` white / `#f7f9f8` off-white |
| Primary accent | `#0c4230` deep forest green |
| Down badge | `#FFD700` yellow, black border, black text |
| All body/content text | `#000` / `#111` black |
| Callout box bg | team primary @ 7% opacity |
| Callout box border | team primary, left side |
| Advance buttons | team primary → secondary → primary diagonal gradient |

**Fonts:**
- Heading: Bebas Neue (4-directional dark stroke on overlay banner titles, 40px for module banners)
- Body: Barlow Condensed / Barlow
- Special (EZ, midfield, field): Cowboys 2.0 (`Cowboys_2_0_D.otf`)
- T-Mode heading: Tecmo Bowl (`Tecmo_Bowl.ttf`)
- T-Mode body: Press Start 2P (Google Fonts)

**Rejected:** Asphalt Sports font (tried, rejected — Bebas Neue confirmed for banner module titles)

---

## Helmet Colorization

Canvas pixel-tinting logic:
- Source: `helmet.png` — grey-shell, transparent background
- Brightness-based colorization + white lerp for facemask
- Per-team using NFL primary/secondary hex
- T-Mode: same logic applied to `pixelhelmet.png` (NOTE: this was previously embedded as inline base64 in index.html — extracted to a file to cut token size by ~60%. Do NOT re-embed assets as base64; keep them as referenced files.)

---

## 10 Modules — Content Architecture

Each module contains: icon + threat label visual, 3-stat bar with real statistics, bold/italic key terms, callout box with core concept, 2 Best Practices with numbered circle icons (no fill, green stroke).

| # | Topic | Anchor |
|---|---|---|
| 1 | Phishing | Sony/Lazarus Heist (spear vs. spray; AMC vs. Sony contrast) |
| 2 | Social Engineering / Pretexting | Pig butchering scam documentary |
| 3 | Password Strength & Reuse | Scenario-based |
| 4 | Software Updates & Patching | WannaCry / EternalBlue / Lazarus Group |
| 5 | Multi-Factor Authentication | Scenario-based |
| 6 | Public Wi-Fi & Network Security | "Free_Airport_WiFi" scenario |
| 7 | Data Handling & Classification | Scenario-based |
| 8 | Ransomware Basics | WannaCry/Lazarus thread |
| 9 | Social Media & OSINT Exposure | Reconnects to pig butchering; LinkedIn/Tinder/Instagram; work badge/office photo risks |
| 10 | Cybersecurity in the AI Age | Deepfakes + social engineering at scale; AI lowering attack barrier |

**Answer key (correct answer index, 0-based):**
- Module 4 Q2 ("Both B and C") → index `3`
- Module 9 Q2 (office photo) → index `0`
- All others → index `1`

---

## Design Principles

- Single file (`index.html`) — do not split into multiple files unless explicitly asked
- No external frameworks — vanilla HTML/CSS/JS only
- No NFL logos or trademarks — city names and hex colors only
- CSS custom properties for team colors (`--team-primary`, `--team-secondary`, `--team-btn-gradient`, `--team-callout-bg`)
- Overlay transitions: field fades dark, card slides in — "Madden briefing room energy"
- All interactions feel like a broadcast/stadium UI — tight typography, bold hierarchy

---

## Session Protocol

At the start of each session:
1. Read this file
2. Check `index.html` for current state (especially JS game state and T-Mode canvas code)
3. Confirm the pixel football canvas item is either done or still open
4. Ask what we're working on today before touching any code

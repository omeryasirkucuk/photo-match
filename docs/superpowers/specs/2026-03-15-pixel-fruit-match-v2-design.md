# Pixel Fruit Match v2 - Competitive Core Design

## Overview
Upgrade the existing pixel-art memory card game with competitive features: difficulty levels, timer, combo system, sound effects, animations, and a localStorage-based leaderboard. Single HTML file, no external dependencies beyond Google Fonts.

## Target Audience
Competitive/casual players who want to beat their own scores.

## Features

### 1. Difficulty Levels & Dynamic Grid
Three levels selectable from a start screen:

| Level | Grid | Cards | Pairs | Emoji Pool |
|-------|------|-------|-------|------------|
| KOLAY (Easy) | 4x4 | 16 | 8 | 8 fruit emojis |
| ZOR (Hard) | 6x6 | 36 | 18 | 18 food emojis |
| HARDCORE | 8x8 | 64 | 32 | 32 food emojis |

- Start screen shows 3 pixel-art buttons replacing the board area
- `grid-template-columns` set dynamically via JS: `repeat(n, 1fr)`
- Board max-width scales per level: 450px / 550px / 650px
- Card font-size scales down for larger grids
- FRUITS array expanded to exactly 32 visually distinct emojis:
  Fruits: 🍎🍇🍊🍓🍉🍒🍋🍑🍌🍍🥝🥭🫐🍈🥥🍐
  Vegetables/Food: 🌽🥕🥑🍆🌶🥦🧅🍄🥜🫑🥒🧄🍅🥬🫒🥔
- Each level slices the needed count from the pool

### 2. Timer
- Starts on first card click (not on level select)
- Counts up every second: `SURE: 00:00` format (mm:ss)
- Uses `setInterval`, stored as elapsed seconds
- Stops when last pair is matched
- Displayed in stats bar and win screen

### 3. Combo System
- Consecutive correct matches increment combo counter: `COMBO x2`, `COMBO x3`...
- Wrong match resets combo to 0
- Visual feedback: floating text notification with CSS scale + fade animation
- Purely motivational — does not affect score calculation

### 4. Score System
- Score = total moves + elapsed seconds (lower is better)
- Displayed on win screen: `HAMLE: 14 | SURE: 0:32 | SKOR: 46`
- Per-level best scores stored in localStorage
- Leaderboard sorted ascending by score; tie-break: fewer moves first, then earlier date

### 5. Sound Effects (Web Audio API)
All sounds generated programmatically using OscillatorNode — no external audio files.
AudioContext must be created/resumed on first user gesture (click) to comply with browser autoplay policies.
- **Card flip:** Short click/tick sound (~50ms, high freq)
- **Match found:** Ascending two-tone beep (~200ms)
- **Mismatch:** Low buzz (~150ms)
- **Combo:** Higher pitch variation of match sound
- **Win:** Short ascending melody (~500ms, 3-4 notes)

### 6. Enhanced Animations (CSS)
- **Card flip:** Existing 3D rotateY (keep as-is)
- **Match found:** Bounce keyframe + green glow (enhance existing)
- **Mismatch:** Horizontal shake keyframe (~300ms)
- **Combo notification:** Scale-up + fade-out floating text
- **Win:** CSS confetti-like particle animation using pseudo-elements or small divs

### 7. Leaderboard
- localStorage key: `pixelFruitMatch_leaderboard`
- Structure: `{ easy: [...], hard: [...], hardcore: [...] }` — keys use English lowercase; each array holds top 5 entries
- Entry: `{ score, moves, time, date }`
- Viewable from start screen via "SKORLAR" button
- Displayed as a simple pixel-art styled table/list
- New high score highlighted on win screen

### 8. UI Flow
```
START SCREEN
  -> Title + 3 difficulty buttons + SKORLAR button
  -> Select difficulty
GAME SCREEN
  -> Stats bar: HAMLE | ESLESME | SURE | COMBO
  -> Board (dynamic grid)
  -> MENU button (returns to start screen, timer cleared, game abandoned)
  -> First click starts timer
  -> Play until all pairs matched
WIN SCREEN (overlay)
  -> Score summary (moves, time, total score)
  -> "YENI REKOR!" if applicable
  -> Top 5 leaderboard for current level
  -> TEKRAR OYNA + MENU buttons
```

### 9. Responsive Design
- All grid sizes work on mobile via percentage-based card sizing
- 8x8 on very small screens (~320px): reduce gap to 3px, border to 2px, smaller emoji font
- Media queries: 480px and 360px breakpoints

### 10. Data Persistence (localStorage)
- `pixelFruitMatch_leaderboard`: top 5 scores per level
- Read on app load, write after each game completion
- Graceful fallback if localStorage unavailable (scores just not saved)

## Technical Constraints
- Single `index.html` file (HTML + CSS + JS inline)
- No external dependencies except Google Fonts (Press Start 2P)
- No build tools, no frameworks
- Web Audio API for sounds (no audio files)
- CSS-only animations (no JS animation libraries)

## Out of Scope
- Theme selection / dark-light mode
- Multiplayer
- User accounts / online leaderboard
- Sound on/off toggle (could be added later but not in v2)

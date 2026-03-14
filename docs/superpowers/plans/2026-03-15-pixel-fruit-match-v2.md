# Pixel Fruit Match v2 Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Upgrade the pixel-art memory card game with difficulty levels (4x4/6x6/8x8), timer, combo system, Web Audio API sounds, CSS animations, and localStorage leaderboard.

**Architecture:** Single `index.html` file rewrite. The game has three screens managed by showing/hiding DOM sections: start screen (level select + leaderboard), game screen (board + stats), win overlay. All state lives in JS variables; persistent data in localStorage.

**Tech Stack:** Vanilla HTML/CSS/JS, Google Fonts (Press Start 2P), Web Audio API.

**Spec:** `docs/superpowers/specs/2026-03-15-pixel-fruit-match-v2-design.md`

---

## File Structure

Single file modification:
- **Modify:** `index.html` — complete rewrite of HTML structure, CSS styles, and JS logic

---

## Chunk 1: Core Restructure — Start Screen, Difficulty Levels, Dynamic Grid

### Task 1: Expand emoji pool and add level config

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Replace FRUITS array with full 32-emoji pool and add LEVELS config**

Replace the existing `FRUITS` array (lines 219-228) with:

```javascript
const ALL_EMOJIS = [
    '🍎','🍇','🍊','🍓','🍉','🍒','🍋','🍑',
    '🍌','🍍','🥝','🥭','🫐','🍈','🥥','🍐',
    '🌽','🥕','🥑','🍆','🌶','🥦','🧅','🍄',
    '🥜','🫑','🥒','🧄','🍅','🥬','🫒','🥔'
];

const LEVELS = {
    easy:     { cols: 4, pairs: 8,  maxWidth: 450, label: 'KOLAY (4x4)' },
    hard:     { cols: 6, pairs: 18, maxWidth: 550, label: 'ZOR (6x6)' },
    hardcore: { cols: 8, pairs: 32, maxWidth: 650, label: 'HARDCORE (8x8)' }
};
```

- [ ] **Step 2: Verify emoji count**

Count the emojis in `ALL_EMOJIS` — must be exactly 32. Each level slices `pairs` count from this array.

---

### Task 2: Add start screen HTML and update game screen structure

**Files:**
- Modify: `index.html` (HTML section)

- [ ] **Step 1: Replace the body HTML with three-screen structure**

Replace everything inside `<body>` with:

```html
<div class="game-container">
    <h1>PIXEL FRUIT<br>MATCH</h1>

    <!-- START SCREEN -->
    <div id="startScreen" class="screen">
        <div class="menu-buttons">
            <button class="btn level-btn" data-level="easy">KOLAY (4x4)</button>
            <button class="btn level-btn" data-level="hard">ZOR (6x6)</button>
            <button class="btn level-btn" data-level="hardcore">HARDCORE (8x8)</button>
        </div>
        <button class="btn" id="scoresBtn" style="margin-top:16px;">SKORLAR</button>
    </div>

    <!-- GAME SCREEN -->
    <div id="gameScreen" class="screen" style="display:none;">
        <div class="stats">
            <div>HAMLE: <span id="moves">0</span></div>
            <div>ESLESME: <span id="pairs">0</span>/8</div>
            <div>SURE: <span id="timer">00:00</span></div>
        </div>
        <div id="comboDisplay" class="combo-display"></div>
        <div class="board" id="board"></div>
        <button class="btn" id="menuBtn">MENU</button>
    </div>
</div>

<!-- WIN OVERLAY -->
<div class="overlay" id="overlay">
    <div class="win-modal">
        <h2>TEBRIKLER!</h2>
        <div id="newRecord" class="new-record" style="display:none;">YENI REKOR!</div>
        <p id="winText"></p>
        <div id="winLeaderboard" class="leaderboard"></div>
        <div class="win-buttons">
            <button class="btn" id="playAgainBtn">TEKRAR OYNA</button>
            <button class="btn" id="winMenuBtn">MENU</button>
        </div>
    </div>
</div>

<!-- SCORES OVERLAY -->
<div class="overlay" id="scoresOverlay">
    <div class="win-modal">
        <h2>EN IYI SKORLAR</h2>
        <div class="scores-tabs">
            <button class="btn tab-btn active" data-tab="easy">KOLAY</button>
            <button class="btn tab-btn" data-tab="hard">ZOR</button>
            <button class="btn tab-btn" data-tab="hardcore">HARDCORE</button>
        </div>
        <div id="scoresLeaderboard" class="leaderboard"></div>
        <button class="btn" id="scoresBackBtn" style="margin-top:16px;">GERI</button>
    </div>
</div>
```

---

### Task 3: Add CSS for start screen, menu buttons, dynamic grid sizes, and new responsive breakpoints

**Files:**
- Modify: `index.html` (CSS section)

- [ ] **Step 1: Add start screen and level button styles**

Add after existing `.btn:active` rule:

```css
.screen { }

.menu-buttons {
    display: flex;
    flex-direction: column;
    gap: 12px;
    align-items: center;
    margin-bottom: 8px;
}

.level-btn {
    width: 260px;
}

.scores-tabs {
    display: flex;
    gap: 6px;
    justify-content: center;
    margin-bottom: 16px;
    flex-wrap: wrap;
}

.tab-btn {
    font-size: 0.5rem;
    padding: 8px 12px;
    background-color: #0f3460;
}

.tab-btn.active {
    background-color: #e94560;
}

.leaderboard {
    text-align: left;
    font-size: 0.5rem;
    margin: 12px 0;
    line-height: 2.2;
    color: #ccc;
}

.leaderboard .entry {
    display: flex;
    justify-content: space-between;
    border-bottom: 2px solid #0f3460;
    padding: 4px 0;
}

.leaderboard .highlight {
    color: #16c79a;
}

.new-record {
    color: #ffcc00;
    font-size: 0.8rem;
    margin-bottom: 12px;
    animation: pulse 0.6s ease-in-out infinite alternate;
}

.win-buttons {
    display: flex;
    gap: 12px;
    justify-content: center;
    flex-wrap: wrap;
}
```

- [ ] **Step 2: Add card size variants for 6x6 and 8x8 grids**

Add after `.board` rule:

```css
.board.grid-6 {
    grid-template-columns: repeat(6, 1fr);
    max-width: 550px;
}

.board.grid-6 .card-front { font-size: 1.1rem; }
.board.grid-6 .card-back { font-size: 1.6rem; }

.board.grid-8 {
    grid-template-columns: repeat(8, 1fr);
    max-width: 650px;
}

.board.grid-8 .card-front { font-size: 0.7rem; }
.board.grid-8 .card-back { font-size: 1.1rem; }
.board.grid-8 .card-front,
.board.grid-8 .card-back { border-width: 3px; }
```

- [ ] **Step 3: Add 360px media query and update 480px query for larger grids**

Add after existing `@media (max-width: 480px)`:

```css
@media (max-width: 360px) {
    .board { gap: 3px; }
    .board.grid-8 .card-front,
    .board.grid-8 .card-back { border-width: 2px; font-size: 0.8rem; }
    .board.grid-6 .card-back { font-size: 1.2rem; }
    .level-btn { width: 220px; font-size: 0.55rem; }
    .stats { font-size: 0.45rem; gap: 8px; }
}
```

---

### Task 4: Rewrite JS for screen management, level selection, and dynamic grid

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Replace all JS with new state management and screen flow**

Replace the entire `<script>` content. New JS must include:

```javascript
// --- STATE ---
let currentLevel = null;
let totalPairs = 0;
let cards = [];
let flippedCards = [];
let moves = 0;
let matchedPairs = 0;
let isLocked = false;
let timerInterval = null;
let elapsedSeconds = 0;
let timerStarted = false;
let comboCount = 0;
let audioCtx = null;

// --- DOM REFS ---
const startScreen = document.getElementById('startScreen');
const gameScreen = document.getElementById('gameScreen');
const board = document.getElementById('board');
const movesEl = document.getElementById('moves');
const pairsEl = document.getElementById('pairs');
const timerEl = document.getElementById('timer');
const comboDisplay = document.getElementById('comboDisplay');
const overlay = document.getElementById('overlay');
const winText = document.getElementById('winText');
const newRecordEl = document.getElementById('newRecord');
const winLeaderboard = document.getElementById('winLeaderboard');
const scoresOverlay = document.getElementById('scoresOverlay');
const scoresLeaderboard = document.getElementById('scoresLeaderboard');
```

- [ ] **Step 2: Add screen navigation functions**

```javascript
function showScreen(screen) {
    startScreen.style.display = 'none';
    gameScreen.style.display = 'none';
    if (screen === 'start') startScreen.style.display = '';
    if (screen === 'game') gameScreen.style.display = '';
}

function goToMenu() {
    clearInterval(timerInterval);
    timerInterval = null;
    overlay.classList.remove('active');
    scoresOverlay.classList.remove('active');
    showScreen('start');
}
```

- [ ] **Step 3: Add level selection and initGame**

```javascript
function startLevel(levelKey) {
    currentLevel = levelKey;
    const level = LEVELS[levelKey];
    totalPairs = level.pairs;

    const emojis = ALL_EMOJIS.slice(0, level.pairs);
    const pairList = emojis.flatMap(e => [e, e]);
    shuffle(pairList);

    cards = pairList.map((emoji, i) => ({
        id: i, emoji, isFlipped: false, isMatched: false
    }));

    flippedCards = [];
    moves = 0;
    matchedPairs = 0;
    isLocked = false;
    comboCount = 0;
    elapsedSeconds = 0;
    timerStarted = false;
    clearInterval(timerInterval);
    timerInterval = null;

    updateStats();
    renderBoard(level.cols);
    showScreen('game');
}
```

- [ ] **Step 4: Update renderBoard for dynamic columns**

```javascript
function renderBoard(cols) {
    board.className = 'board';
    if (cols === 6) board.classList.add('grid-6');
    if (cols === 8) board.classList.add('grid-8');

    board.innerHTML = '';
    cards.forEach(card => {
        const el = document.createElement('div');
        el.className = 'card';
        el.dataset.id = card.id;
        el.innerHTML = `
            <div class="card-inner">
                <div class="card-front">?</div>
                <div class="card-back">${card.emoji}</div>
            </div>
        `;
        el.addEventListener('click', handleCardClick);
        board.appendChild(el);
    });
}
```

- [ ] **Step 5: Update handleCardClick to start timer on first click**

```javascript
function handleCardClick(e) {
    const el = e.currentTarget;
    const id = parseInt(el.dataset.id);
    const card = cards[id];

    if (isLocked || card.isFlipped || card.isMatched || flippedCards.length >= 2) return;

    if (!timerStarted) {
        timerStarted = true;
        startTimer();
    }

    initAudio();
    playSound('flip');

    card.isFlipped = true;
    el.classList.add('flipped');
    flippedCards.push({ card, el });

    if (flippedCards.length === 2) {
        moves++;
        updateStats();
        checkForMatch();
    }
}
```

- [ ] **Step 6: Update checkForMatch with combo logic and dynamic win condition**

```javascript
function checkForMatch() {
    const [first, second] = flippedCards;

    if (first.card.emoji === second.card.emoji) {
        first.card.isMatched = true;
        second.card.isMatched = true;
        first.el.classList.add('matched');
        second.el.classList.add('matched');
        matchedPairs++;
        comboCount++;
        playSound('match');

        if (comboCount >= 2) showCombo(comboCount);

        first.el.classList.add('bounce');
        second.el.classList.add('bounce');

        updateStats();
        flippedCards = [];

        if (matchedPairs === totalPairs) {
            clearInterval(timerInterval);
            playSound('win');
            setTimeout(showWinScreen, 600);
        }
    } else {
        isLocked = true;
        comboCount = 0;
        playSound('mismatch');
        first.el.classList.add('shake');
        second.el.classList.add('shake');

        setTimeout(() => {
            first.card.isFlipped = false;
            second.card.isFlipped = false;
            first.el.classList.remove('flipped', 'shake');
            second.el.classList.remove('flipped', 'shake');
            flippedCards = [];
            isLocked = false;
        }, 900);
    }
}
```

- [ ] **Step 7: Add event listeners for all buttons**

```javascript
// Level select buttons
document.querySelectorAll('.level-btn').forEach(btn => {
    btn.addEventListener('click', () => startLevel(btn.dataset.level));
});

// Menu buttons
document.getElementById('menuBtn').addEventListener('click', goToMenu);
document.getElementById('winMenuBtn').addEventListener('click', goToMenu);
document.getElementById('playAgainBtn').addEventListener('click', () => {
    overlay.classList.remove('active');
    startLevel(currentLevel);
});

// Scores
document.getElementById('scoresBtn').addEventListener('click', () => {
    showScoresOverlay('easy');
    scoresOverlay.classList.add('active');
});
document.getElementById('scoresBackBtn').addEventListener('click', () => {
    scoresOverlay.classList.remove('active');
});
document.querySelectorAll('.tab-btn').forEach(btn => {
    btn.addEventListener('click', () => {
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        showScoresOverlay(btn.dataset.tab);
    });
});

// Init
showScreen('start');
```

- [ ] **Step 8: Open in browser, verify start screen shows 3 level buttons, selecting a level renders correct grid size**

---

## Chunk 2: Timer, Combo Display, Score, Sound, Animations, Leaderboard

### Task 5: Timer system

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Add timer functions**

```javascript
function startTimer() {
    timerInterval = setInterval(() => {
        elapsedSeconds++;
        timerEl.textContent = formatTime(elapsedSeconds);
    }, 1000);
}

function formatTime(secs) {
    const m = String(Math.floor(secs / 60)).padStart(2, '0');
    const s = String(secs % 60).padStart(2, '0');
    return m + ':' + s;
}
```

- [ ] **Step 2: Add updateStats to show dynamic pair count**

```javascript
function updateStats() {
    movesEl.textContent = moves;
    pairsEl.textContent = matchedPairs + '/' + totalPairs;
    timerEl.textContent = formatTime(elapsedSeconds);
}
```

---

### Task 6: Combo display

**Files:**
- Modify: `index.html` (CSS + JS sections)

- [ ] **Step 1: Add combo CSS**

```css
.combo-display {
    height: 30px;
    font-size: 0.7rem;
    color: #ffcc00;
    text-shadow: 2px 2px 0 #000;
    margin-bottom: 4px;
}

.combo-display.show {
    animation: comboAnim 0.8s ease-out forwards;
}

@keyframes comboAnim {
    0% { transform: scale(0.5); opacity: 0; }
    30% { transform: scale(1.3); opacity: 1; }
    100% { transform: scale(1); opacity: 0; }
}
```

- [ ] **Step 2: Add showCombo JS function**

```javascript
function showCombo(count) {
    comboDisplay.textContent = 'COMBO x' + count + '!';
    comboDisplay.classList.remove('show');
    void comboDisplay.offsetWidth; // force reflow
    comboDisplay.classList.add('show');
}
```

---

### Task 7: Sound effects (Web Audio API)

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Add audio system**

```javascript
function initAudio() {
    if (audioCtx) return;
    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    if (audioCtx.state === 'suspended') audioCtx.resume();
}

function playSound(type) {
    if (!audioCtx) return;
    const now = audioCtx.currentTime;

    if (type === 'flip') {
        playTone(800, 'square', 0.05, 0.08, now);
    } else if (type === 'match') {
        playTone(523, 'sine', 0.1, 0.12, now);
        playTone(659, 'sine', 0.1, 0.12, now + 0.1);
    } else if (type === 'mismatch') {
        playTone(200, 'sawtooth', 0.08, 0.15, now);
    } else if (type === 'win') {
        playTone(523, 'sine', 0.1, 0.15, now);
        playTone(659, 'sine', 0.1, 0.15, now + 0.15);
        playTone(784, 'sine', 0.1, 0.15, now + 0.3);
        playTone(1047, 'sine', 0.12, 0.25, now + 0.45);
    }
}

function playTone(freq, waveType, volume, duration, startTime) {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = waveType;
    osc.frequency.value = freq;
    gain.gain.value = volume;
    gain.gain.exponentialRampToValueAtTime(0.001, startTime + duration);
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start(startTime);
    osc.stop(startTime + duration);
}
```

---

### Task 8: CSS animations (bounce, shake, confetti, pulse)

**Files:**
- Modify: `index.html` (CSS section)

- [ ] **Step 1: Add keyframe animations**

```css
@keyframes bounce {
    0%, 100% { transform: rotateY(180deg) scale(1); }
    50% { transform: rotateY(180deg) scale(1.15); }
}

.card.bounce .card-inner {
    animation: bounce 0.4s ease;
}

@keyframes shake {
    0%, 100% { transform: translateX(0); }
    20% { transform: translateX(-6px); }
    40% { transform: translateX(6px); }
    60% { transform: translateX(-4px); }
    80% { transform: translateX(4px); }
}

.card.shake .card-inner {
    animation: shake 0.3s ease;
}

@keyframes pulse {
    from { transform: scale(1); }
    to { transform: scale(1.1); }
}

@keyframes confetti {
    0% { transform: translateY(0) rotate(0deg); opacity: 1; }
    100% { transform: translateY(-60px) rotate(720deg); opacity: 0; }
}

.confetti-piece {
    position: absolute;
    width: 8px;
    height: 8px;
    animation: confetti 1s ease-out forwards;
}
```

---

### Task 9: Leaderboard (localStorage)

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Add leaderboard read/write functions**

```javascript
function getLeaderboard() {
    try {
        return JSON.parse(localStorage.getItem('pixelFruitMatch_leaderboard')) || { easy: [], hard: [], hardcore: [] };
    } catch {
        return { easy: [], hard: [], hardcore: [] };
    }
}

function saveScore(levelKey, score, movesCount, time) {
    const lb = getLeaderboard();
    const entry = { score, moves: movesCount, time, date: new Date().toLocaleDateString('tr-TR') };
    lb[levelKey].push(entry);
    lb[levelKey].sort((a, b) => a.score - b.score || a.moves - b.moves);
    lb[levelKey] = lb[levelKey].slice(0, 5);
    try { localStorage.setItem('pixelFruitMatch_leaderboard', JSON.stringify(lb)); } catch {}
    return lb[levelKey].findIndex(e => e.score === entry.score && e.moves === entry.moves && e.date === entry.date);
}
```

- [ ] **Step 2: Add leaderboard rendering functions**

```javascript
function renderLeaderboard(container, entries, highlightIndex) {
    if (!entries || entries.length === 0) {
        container.innerHTML = '<div style="text-align:center;color:#666;">Henuz skor yok</div>';
        return;
    }
    container.innerHTML = entries.map((e, i) => {
        const cls = i === highlightIndex ? 'entry highlight' : 'entry';
        return `<div class="${cls}">
            <span>#${i + 1}</span>
            <span>SKOR: ${e.score}</span>
            <span>${e.moves} hamle</span>
            <span>${formatTime(e.time)}</span>
        </div>`;
    }).join('');
}

function showScoresOverlay(levelKey) {
    const lb = getLeaderboard();
    renderLeaderboard(scoresLeaderboard, lb[levelKey], -1);
}
```

- [ ] **Step 3: Update showWinScreen to calculate score and save to leaderboard**

```javascript
function showWinScreen() {
    const score = moves + elapsedSeconds;
    const highlightIdx = saveScore(currentLevel, score, moves, elapsedSeconds);
    const lb = getLeaderboard();
    const isNewRecord = highlightIdx === 0 && lb[currentLevel].length > 0;

    newRecordEl.style.display = isNewRecord ? '' : 'none';
    winText.innerHTML = `HAMLE: ${moves} | SURE: ${formatTime(elapsedSeconds)} | SKOR: ${score}`;
    renderLeaderboard(winLeaderboard, lb[currentLevel], highlightIdx);

    spawnConfetti();
    overlay.classList.add('active');
}
```

---

### Task 10: Confetti effect

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Add confetti spawner**

```javascript
function spawnConfetti() {
    const modal = document.querySelector('.win-modal');
    const colors = ['#e94560', '#16c79a', '#ffcc00', '#0f3460', '#00ff00'];
    for (let i = 0; i < 30; i++) {
        const piece = document.createElement('div');
        piece.className = 'confetti-piece';
        piece.style.left = Math.random() * 100 + '%';
        piece.style.top = Math.random() * 100 + '%';
        piece.style.backgroundColor = colors[i % colors.length];
        piece.style.animationDelay = Math.random() * 0.5 + 's';
        piece.style.animationDuration = (0.8 + Math.random() * 0.6) + 's';
        modal.appendChild(piece);
        setTimeout(() => piece.remove(), 2000);
    }
}
```

---

### Task 11: Final integration and manual verification

- [ ] **Step 1: Open `index.html` in browser**
- [ ] **Step 2: Verify start screen with 3 level buttons + SKORLAR button**
- [ ] **Step 3: Play KOLAY (4x4) — verify timer starts on first click, combo shows on consecutive matches, sounds play, shake on mismatch, bounce on match**
- [ ] **Step 4: Complete game — verify win screen shows score, leaderboard, confetti**
- [ ] **Step 5: Play ZOR (6x6) — verify 36 cards render correctly, grid scales**
- [ ] **Step 6: Play HARDCORE (8x8) — verify 64 cards render, responsive on narrow viewport**
- [ ] **Step 7: Check SKORLAR — verify scores persist across reloads, tab switching works**
- [ ] **Step 8: Test MENU button during game — verify timer stops, returns to start**
- [ ] **Step 9: Commit final version**

```bash
git add index.html
git commit -m "feat: upgrade to v2 — difficulty levels, timer, combo, sounds, leaderboard"
```

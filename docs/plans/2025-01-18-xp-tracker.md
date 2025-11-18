# D&D XP Tracker Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a web-based XP tracker with token-based progression for two D&D characters using localStorage persistence.

**Architecture:** Single-page HTML application with vanilla JavaScript for state management, localStorage for persistence, and Pokemon-inspired card UI matching existing D&D tools styling.

**Tech Stack:** HTML5, CSS3, Vanilla JavaScript, localStorage API, File API

---

## Task 1: Create Base HTML Structure

**Files:**
- Create: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html`

**Step 1: Create HTML file with base structure**

Create the file with standard header, navigation, and container structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>D&D XP Tracker</title>
    <link rel="icon" type="image/x-icon" href="assets/favicon.ico">
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
    <link rel="stylesheet" href="dimension/assets/css/main.css" />
    <link rel="stylesheet" href="radial.css" />
    <link rel="stylesheet" href="assets/custom.css" />
    <noscript>
        <link rel="stylesheet" href="dimension/assets/css/noscript.css" />
    </noscript>
    <style>
        /* Custom styles will go here */
    </style>
</head>

<body class="is-preload">
    <div id="wrapper">
        <header id="header">
            <div class="logo">
                <span class="dndicon" onclick="window.location.href='dnd.html'"></span>
            </div>
            <div class="content">
                <div class="inner" style="max-height: none; max-width: 90%; margin-left: auto; margin-right: auto;">
                    <h1>XP Tracker</h1>
                    <p>Token-based character progression tracker</p>

                    <!-- Character cards will go here -->
                    <div id="character-cards"></div>

                    <!-- XP Management interface will go here -->
                    <div id="xp-management"></div>

                    <!-- Quick reference will go here -->
                    <div id="quick-reference"></div>
                </div>
            </div>
        </header>

        <footer id="footer">
            <p class="copyright">&copy; Max Saller 2023. Design: <a href="https://html5up.net">HTML5 UP</a>.</p>
        </footer>
    </div>

    <div id="bg"></div>

    <script src="dimension/assets/js/jquery.min.js"></script>
    <script src="dimension/assets/js/browser.min.js"></script>
    <script src="dimension/assets/js/breakpoints.min.js"></script>
    <script src="dimension/assets/js/util.js"></script>
    <script src="dimension/assets/js/main.js"></script>
    <script>
        // Application code will go here
    </script>
</body>
</html>
```

**Step 2: Verify file loads in browser**

Expected: Page loads with header, title, and footer visible

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: add XP tracker base HTML structure"
```

---

## Task 2: Add Custom CSS Styling

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (style section)

**Step 1: Add character card styles**

Add inside the `<style>` tag:

```css
/* Character Cards */
.character-cards-container {
    display: flex;
    gap: 2rem;
    margin-bottom: 3rem;
    flex-wrap: wrap;
    justify-content: center;
}

.character-card {
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
    min-width: 350px;
    max-width: 450px;
    flex: 1;
}

.character-card.seraphina {
    background: linear-gradient(135deg, #ffc6e0 0%, #ffffff 100%);
    color: #2a2a2a;
}

.character-card.konstantin {
    background: linear-gradient(135deg, #6b9bd1 0%, #ffd700 100%);
    color: #1a1a1a;
}

.character-header {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
}

.character-portrait {
    width: 80px;
    height: 80px;
    border-radius: 8px;
    border: 3px solid rgba(0, 0, 0, 0.2);
    object-fit: cover;
    margin-right: 1rem;
}

.character-info {
    flex: 1;
}

.character-name {
    font-size: 1.5rem;
    font-weight: bold;
    margin: 0;
    line-height: 1.2;
}

.character-class-level {
    font-size: 0.9rem;
    font-style: italic;
    opacity: 0.8;
    margin: 0.25rem 0 0 0;
}

.progress-container {
    margin: 1rem 0;
}

.progress-bar-wrapper {
    position: relative;
    background: rgba(0, 0, 0, 0.2);
    height: 30px;
    border-radius: 15px;
    overflow: hidden;
    margin-bottom: 0.5rem;
}

.progress-bar {
    height: 100%;
    background: linear-gradient(90deg, #4CAF50 0%, #8BC34A 50%, #FFC107 100%);
    transition: width 0.5s ease;
    border-radius: 15px;
}

.progress-text {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    font-weight: bold;
    font-size: 0.9rem;
    color: white;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
}

.token-count {
    font-size: 1rem;
    margin-top: 0.5rem;
}

/* XP Management */
.xp-management {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 8px;
    padding: 1.5rem;
    margin-bottom: 2rem;
}

.xp-management h3 {
    margin-top: 0;
}

.xp-controls {
    display: flex;
    gap: 1rem;
    align-items: center;
    flex-wrap: wrap;
}

.xp-controls select,
.xp-controls input {
    flex: 1;
    min-width: 150px;
}

.xp-controls button {
    padding: 0.75rem 1.5rem;
}

/* Quick Reference */
.quick-reference {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 8px;
    padding: 1.5rem;
    margin-top: 2rem;
}

.quick-reference h3 {
    margin-top: 0;
}

.quick-reference table {
    width: 100%;
    border-collapse: collapse;
    margin: 1rem 0;
}

.quick-reference th,
.quick-reference td {
    padding: 0.5rem;
    text-align: left;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.quick-reference th {
    font-weight: bold;
}

/* Admin Mode */
.admin-toggle {
    position: absolute;
    top: 1rem;
    right: 1rem;
    z-index: 1000;
}

.admin-mode-active .editable {
    cursor: pointer;
    border-bottom: 2px dashed yellow;
    padding: 2px 4px;
    transition: background 0.2s;
}

.admin-mode-active .editable:hover {
    background: rgba(255, 255, 0, 0.2);
}

/* Utility Classes */
.hidden {
    display: none;
}

.highlight {
    animation: highlight 0.5s ease;
}

@keyframes highlight {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; transform: scale(1.05); }
}

/* Export/Import */
.data-management {
    margin: 1rem 0;
    display: flex;
    gap: 1rem;
}

.last-saved {
    font-size: 0.8rem;
    opacity: 0.6;
    margin-top: 0.5rem;
}
```

**Step 2: Verify styles are applied**

Expected: Styles load correctly (no console errors)

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "style: add character card and XP tracker CSS"
```

---

## Task 3: Implement Data Model and localStorage

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add data model and storage functions**

Add inside the `<script>` tag:

```javascript
// Data Model
const DEFAULT_DATA = {
    characters: [
        {
            name: "Seraphina",
            class: "Rogue",
            level: 2,
            currentXP: 0,
            tokensEarned: 0,
            portraitPath: "assets/seraphina.jpg"
        },
        {
            name: "Konstantin",
            class: "Bard",
            level: 2,
            currentXP: 0,
            tokensEarned: 0,
            portraitPath: "assets/konstantin.jpg"
        }
    ],
    lastUpdated: new Date().toISOString()
};

// Storage key
const STORAGE_KEY = 'dnd_xp_tracker_data';

// Load data from localStorage
function loadData() {
    try {
        const stored = localStorage.getItem(STORAGE_KEY);
        if (stored) {
            return JSON.parse(stored);
        }
    } catch (error) {
        console.error('Error loading data:', error);
    }
    return JSON.parse(JSON.stringify(DEFAULT_DATA));
}

// Save data to localStorage
function saveData(data) {
    try {
        data.lastUpdated = new Date().toISOString();
        localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
        updateLastSavedDisplay();
        return true;
    } catch (error) {
        console.error('Error saving data:', error);
        alert('Error saving data. Please export a backup!');
        return false;
    }
}

// Calculate XP needed for next token
function calculateXPForNextToken(level, tokensEarned) {
    return 1000 * level * (2 + tokensEarned);
}

// Update last saved timestamp display
function updateLastSavedDisplay() {
    const data = loadData();
    const timestamp = new Date(data.lastUpdated);
    const display = document.getElementById('last-saved');
    if (display) {
        display.textContent = `Last saved: ${timestamp.toLocaleString()}`;
    }
}

// Initialize app
let appData = loadData();
let adminMode = false;
```

**Step 2: Test data functions in console**

Open browser console and verify:
- `loadData()` returns default data
- `saveData(appData)` saves successfully
- `calculateXPForNextToken(2, 0)` returns 4000

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: implement data model and localStorage functions"
```

---

## Task 4: Render Character Cards

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add character card rendering function**

Add to script section:

```javascript
// Render character cards
function renderCharacterCards() {
    const container = document.getElementById('character-cards');
    const cardsHTML = appData.characters.map((char, index) => {
        const xpNeeded = calculateXPForNextToken(char.level, char.tokensEarned);
        const progress = Math.min((char.currentXP / xpNeeded) * 100, 100);
        const cardClass = char.name.toLowerCase();

        return `
            <div class="character-card ${cardClass}">
                <div class="character-header">
                    <img src="${char.portraitPath}" alt="${char.name}" class="character-portrait" onerror="this.src='assets/favicon.ico'">
                    <div class="character-info">
                        <h2 class="character-name">
                            <span class="editable" data-char="${index}" data-field="name">${char.name}</span>
                        </h2>
                        <p class="character-class-level">
                            <span class="editable" data-char="${index}" data-field="class">${char.class}</span> •
                            Level <span class="editable" data-char="${index}" data-field="level">${char.level}</span>
                        </p>
                    </div>
                </div>
                <div class="progress-container">
                    <div class="progress-bar-wrapper">
                        <div class="progress-bar" style="width: ${progress}%"></div>
                        <div class="progress-text">
                            <span class="editable" data-char="${index}" data-field="currentXP">${char.currentXP}</span>/${xpNeeded} XP
                        </div>
                    </div>
                </div>
                <div class="token-count">
                    ${char.name} currently has <span class="editable" data-char="${index}" data-field="tokensEarned"><strong>${char.tokensEarned}</strong></span> token${char.tokensEarned !== 1 ? 's' : ''}.
                </div>
            </div>
        `;
    }).join('');

    container.innerHTML = `<div class="character-cards-container">${cardsHTML}</div>`;
    attachEditableListeners();
}

// Attach click listeners for admin mode editing
function attachEditableListeners() {
    document.querySelectorAll('.editable').forEach(elem => {
        elem.addEventListener('click', function() {
            if (!adminMode) return;

            const charIndex = parseInt(this.dataset.char);
            const field = this.dataset.field;
            const currentValue = appData.characters[charIndex][field];

            const input = document.createElement('input');
            input.type = field === 'level' || field === 'currentXP' || field === 'tokensEarned' ? 'number' : 'text';
            input.value = currentValue;
            input.style.width = '100px';

            const parent = this.parentElement;
            parent.replaceChild(input, this);
            input.focus();
            input.select();

            const saveEdit = () => {
                let newValue = input.value;
                if (input.type === 'number') {
                    newValue = parseInt(newValue) || 0;
                    if (newValue < 0) newValue = 0;
                }

                appData.characters[charIndex][field] = newValue;
                processTokenChanges(charIndex);
                saveData(appData);
                renderCharacterCards();
            };

            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', (e) => {
                if (e.key === 'Enter') saveEdit();
                if (e.key === 'Escape') renderCharacterCards();
            });
        });
    });
}
```

**Step 2: Add render call to initialization**

Add at end of script:

```javascript
// Initial render
document.addEventListener('DOMContentLoaded', function() {
    renderCharacterCards();
});
```

**Step 3: Verify cards render**

Expected: Both character cards display with portraits, names, progress bars at 0%, and 0 tokens

**Step 4: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: render character cards with progress bars"
```

---

## Task 5: Implement Token Gain/Loss Logic

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add token processing function**

Add to script section:

```javascript
// Process token gains/losses based on XP changes
function processTokenChanges(charIndex) {
    const char = appData.characters[charIndex];

    // Handle token gains (XP overflow)
    while (char.currentXP >= calculateXPForNextToken(char.level, char.tokensEarned)) {
        const xpNeeded = calculateXPForNextToken(char.level, char.tokensEarned);
        char.currentXP -= xpNeeded;
        char.tokensEarned += 1;

        // Visual feedback
        setTimeout(() => {
            const tokenElement = document.querySelector(`[data-char="${charIndex}"][data-field="tokensEarned"]`);
            if (tokenElement) {
                tokenElement.parentElement.classList.add('highlight');
                setTimeout(() => tokenElement.parentElement.classList.remove('highlight'), 500);
            }
        }, 100);
    }

    // Handle token losses (XP deficit)
    while (char.currentXP < 0 && char.tokensEarned > 0) {
        if (!confirm(`This will remove a token from ${char.name}. Continue?`)) {
            char.currentXP = 0;
            break;
        }

        char.tokensEarned -= 1;
        const xpNeeded = calculateXPForNextToken(char.level, char.tokensEarned);
        char.currentXP += xpNeeded;
    }

    // Can't go negative at 0 tokens
    if (char.currentXP < 0 && char.tokensEarned === 0) {
        char.currentXP = 0;
    }
}
```

**Step 2: Test token logic manually**

In console, test:
```javascript
appData.characters[0].currentXP = 5000; // Should gain 1 token, have 1000 XP left
processTokenChanges(0);
console.log(appData.characters[0]); // Verify: tokensEarned=1, currentXP=1000
```

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: implement token gain/loss processing logic"
```

---

## Task 6: Create XP Management Interface

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add XP management UI rendering**

Add to script section:

```javascript
// Render XP management interface
function renderXPManagement() {
    const container = document.getElementById('xp-management');

    const html = `
        <div class="xp-management">
            <h3>Manage XP</h3>
            <div class="xp-controls">
                <select id="char-select">
                    ${appData.characters.map((char, i) =>
                        `<option value="${i}">${char.name}</option>`
                    ).join('')}
                </select>
                <input type="number" id="xp-amount" placeholder="XP Amount" value="0" />
                <button onclick="modifyXP('add')" class="button primary">Add XP</button>
                <button onclick="modifyXP('subtract')" class="button">Subtract XP</button>
            </div>
            <div class="data-management">
                <button onclick="exportData()" class="button">Download Backup</button>
                <button onclick="importData()" class="button">Upload Backup</button>
            </div>
            <div id="last-saved" class="last-saved"></div>
        </div>
    `;

    container.innerHTML = html;
    updateLastSavedDisplay();
}

// Modify XP (add or subtract)
function modifyXP(action) {
    const charIndex = parseInt(document.getElementById('char-select').value);
    const amount = parseInt(document.getElementById('xp-amount').value) || 0;

    if (amount === 0) {
        alert('Please enter an XP amount');
        return;
    }

    const char = appData.characters[charIndex];

    if (action === 'add') {
        char.currentXP += amount;
    } else if (action === 'subtract') {
        char.currentXP -= amount;
    }

    processTokenChanges(charIndex);
    saveData(appData);
    renderCharacterCards();

    // Reset input
    document.getElementById('xp-amount').value = 0;
}
```

**Step 2: Add render call to initialization**

Update the DOMContentLoaded listener:

```javascript
document.addEventListener('DOMContentLoaded', function() {
    renderCharacterCards();
    renderXPManagement();
    renderQuickReference();
});
```

**Step 3: Test XP management**

Expected: Can select character, enter XP amount, add/subtract XP, see progress bar update

**Step 4: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: add XP management interface with add/subtract controls"
```

---

## Task 7: Implement Admin Mode

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html`

**Step 1: Add admin mode toggle to header**

Modify the header section in HTML to add admin toggle:

```html
<header id="header">
    <div class="logo">
        <span class="dndicon" onclick="window.location.href='dnd.html'"></span>
    </div>
    <button class="admin-toggle button small" onclick="toggleAdminMode()">
        <span id="admin-icon">🔒</span> <span id="admin-text">Locked</span>
    </button>
    <div class="content">
        <!-- rest of content -->
    </div>
</header>
```

**Step 2: Add admin mode toggle function**

Add to script section:

```javascript
// Toggle admin mode
function toggleAdminMode() {
    adminMode = !adminMode;
    const wrapper = document.getElementById('wrapper');
    const icon = document.getElementById('admin-icon');
    const text = document.getElementById('admin-text');

    if (adminMode) {
        wrapper.classList.add('admin-mode-active');
        icon.textContent = '🔓';
        text.textContent = 'Admin Mode';
    } else {
        wrapper.classList.remove('admin-mode-active');
        icon.textContent = '🔒';
        text.textContent = 'Locked';
    }
}
```

**Step 3: Test admin mode**

Expected:
- Click toggle → editable fields get yellow dashed border
- Click editable value → becomes input field
- Enter/blur saves, Escape cancels

**Step 4: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: add admin mode for manual value editing"
```

---

## Task 8: Implement Export/Import

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add export function**

Add to script section:

```javascript
// Export data as JSON file
function exportData() {
    const data = loadData();
    const jsonString = JSON.stringify(data, null, 2);
    const blob = new Blob([jsonString], { type: 'application/json' });
    const url = URL.createObjectURL(blob);

    const today = new Date().toISOString().split('T')[0];
    const filename = `xp-tracker-backup-${today}.json`;

    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
}

// Import data from JSON file
function importData() {
    const input = document.createElement('input');
    input.type = 'file';
    input.accept = 'application/json';

    input.onchange = (e) => {
        const file = e.target.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = (event) => {
            try {
                const importedData = JSON.parse(event.target.result);

                // Validate data structure
                if (!importedData.characters || !Array.isArray(importedData.characters)) {
                    throw new Error('Invalid data format');
                }

                // Confirm before overwriting
                if (!confirm('This will replace all current data. Continue?')) {
                    return;
                }

                appData = importedData;
                saveData(appData);
                renderCharacterCards();
                renderXPManagement();
                alert('Data imported successfully!');
            } catch (error) {
                console.error('Import error:', error);
                alert('Error importing file. Please check the file format.');
            }
        };
        reader.readAsText(file);
    };

    input.click();
}
```

**Step 2: Test export/import**

Expected:
- Export downloads JSON file with current data
- Import prompts for file, validates, confirms, and loads data

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: add export/import functionality for data backup"
```

---

## Task 9: Create Quick Reference Section

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html` (script section)

**Step 1: Add quick reference rendering**

Add to script section:

```javascript
// Render quick reference guide
function renderQuickReference() {
    const container = document.getElementById('quick-reference');

    const html = `
        <div class="quick-reference">
            <h3>XP Award Reference</h3>

            <h4>Token Formula</h4>
            <p><strong>XP per Token = 1000 × Level × (2 + Tokens Earned)</strong></p>
            <p>Example at Level 2: Token #1 costs 4,000 XP, Token #10 costs 24,000 XP</p>

            <h4>Skill Challenges</h4>
            <table>
                <thead>
                    <tr>
                        <th>Type</th>
                        <th>Formula</th>
                        <th>Level 2</th>
                        <th>Level 5</th>
                        <th>Level 10</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Minor</td>
                        <td>100 × Level</td>
                        <td>200 XP</td>
                        <td>500 XP</td>
                        <td>1,000 XP</td>
                    </tr>
                    <tr>
                        <td>Major</td>
                        <td>250 × Level</td>
                        <td>500 XP</td>
                        <td>1,250 XP</td>
                        <td>2,500 XP</td>
                    </tr>
                    <tr>
                        <td>Pivotal</td>
                        <td>500 × Level</td>
                        <td>1,000 XP</td>
                        <td>2,500 XP</td>
                        <td>5,000 XP</td>
                    </tr>
                </tbody>
            </table>

            <h4>Quest Completion</h4>
            <table>
                <thead>
                    <tr>
                        <th>Type</th>
                        <th>Formula</th>
                        <th>Level 2</th>
                        <th>Level 5</th>
                        <th>Level 10</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Side Quest</td>
                        <td>200 × Level</td>
                        <td>400 XP</td>
                        <td>1,000 XP</td>
                        <td>2,000 XP</td>
                    </tr>
                    <tr>
                        <td>Main Quest</td>
                        <td>500 × Level</td>
                        <td>1,000 XP</td>
                        <td>2,500 XP</td>
                        <td>5,000 XP</td>
                    </tr>
                    <tr>
                        <td>Major Arc</td>
                        <td>1000 × Level</td>
                        <td>2,000 XP</td>
                        <td>5,000 XP</td>
                        <td>10,000 XP</td>
                    </tr>
                </tbody>
            </table>

            <h4>Creature Kills (Last Hit)</h4>
            <p>Use CR-based XP from the Monster Manual. Common examples:</p>
            <ul>
                <li>CR 1/4: 50 XP</li>
                <li>CR 1/2: 100 XP</li>
                <li>CR 1: 200 XP</li>
                <li>CR 2: 450 XP</li>
                <li>CR 3: 700 XP</li>
                <li>CR 4: 1,100 XP</li>
                <li>CR 5: 1,800 XP</li>
            </ul>
            <p><em>Note: Adjust values based on actual challenge difficulty and story impact.</em></p>
        </div>
    `;

    container.innerHTML = html;
}
```

**Step 2: Verify reference displays**

Expected: Quick reference section shows all tables and formulas clearly

**Step 3: Commit**

```bash
git add dnd_xp_tracker.html
git commit -m "feat: add XP award quick reference section"
```

---

## Task 10: Add Navigation Link

**Files:**
- Modify: `/home/max/repos/maxsaller.github.io/dnd.html`

**Step 1: Add XP Tracker to navigation**

Locate the `<nav>` section in dnd.html and add new link:

```html
<nav>
    <ul>
        <li><a href="#calendar"><b>Calendar</b></a></li>
        <li><a href="#sending"><b>Sending</b></a></li>
        <li><a href="dnd_xp_tracker.html"><b>XP Tracker</b></a></li>
    </ul>
</nav>
```

**Step 2: Test navigation**

Expected:
- Click XP Tracker link from dnd.html → opens tracker page
- Click D&D icon on tracker → returns to dnd.html

**Step 3: Commit**

```bash
git add dnd.html
git commit -m "feat: add XP tracker link to D&D tools navigation"
```

---

## Task 11: Copy Character Portraits

**Files:**
- Copy images from `/mnt/c/Users/maxim/Downloads/` to `assets/`

**Step 1: Identify portrait files**

Run:
```bash
ls /mnt/c/Users/maxim/Downloads/*.jpg | grep -iE "(seraphina|konstantin)"
```

**Step 2: Copy and rename portraits**

Assuming files are found as `seraphina.jpg` and `konstantin.jpg`:

```bash
cp /mnt/c/Users/maxim/Downloads/seraphina.jpg assets/seraphina.jpg
cp /mnt/c/Users/maxim/Downloads/konstantin.jpg assets/konstantin.jpg
```

If files have different names, adjust accordingly.

**Step 3: Verify images load**

Open `dnd_xp_tracker.html` and verify portraits display correctly

**Step 4: Commit**

```bash
git add assets/seraphina.jpg assets/konstantin.jpg
git commit -m "assets: add character portraits for XP tracker"
```

---

## Task 12: Final Testing and Polish

**Files:**
- Test: All functionality in `/home/max/repos/maxsaller.github.io/dnd_xp_tracker.html`

**Step 1: Test complete workflow**

1. Add 5000 XP to Seraphina → should gain 1 token, have 1000 XP remaining
2. Export data → verify JSON downloads
3. Clear localStorage: `localStorage.clear()`, reload page
4. Import data → verify data restores
5. Enable admin mode → edit level to 3
6. Verify token XP requirement updates (should now be 5000 for next token)
7. Add 10000 XP to Konstantin → should gain 2 tokens
8. Subtract 6000 XP → should lose 1 token with confirmation

**Step 2: Test edge cases**

1. Try to subtract XP below 0 with 0 tokens → should clamp to 0
2. Add huge XP amount (50000) → should gain multiple tokens correctly
3. Try importing invalid JSON → should show error
4. Verify responsive design on mobile viewport

**Step 3: Browser testing**

Test in Chrome, Firefox, Safari (if available)

**Step 4: Fix any issues found**

Make necessary adjustments based on testing

**Step 5: Final commit**

```bash
git add dnd_xp_tracker.html
git commit -m "test: verify all XP tracker functionality and edge cases"
```

---

## Verification Checklist

Before marking complete, verify:

- [ ] Both character cards display with correct styling
- [ ] Progress bars update smoothly
- [ ] XP add/subtract works correctly
- [ ] Token gains/losses process correctly with confirmations
- [ ] Admin mode allows editing all values
- [ ] Export downloads valid JSON
- [ ] Import validates and loads data
- [ ] Quick reference displays all formulas
- [ ] Navigation works between dnd.html and tracker
- [ ] Portrait images display correctly
- [ ] localStorage persists between page loads
- [ ] Responsive on mobile and desktop
- [ ] No console errors

---

## Post-Implementation

After completing all tasks:

1. Test thoroughly with real gameplay scenarios
2. Consider adding features from "Future Considerations" in design doc
3. Update design doc with any implementation changes
4. Add screenshots to documentation if desired

## Notes

- **YAGNI**: Don't add features not in the plan
- **DRY**: Reuse functions where possible
- **Frequent commits**: Commit after each task completion
- **Error handling**: All user inputs are validated
- **Accessibility**: Consider adding ARIA labels in future iteration

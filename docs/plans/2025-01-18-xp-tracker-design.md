# D&D XP Tracker Design Document

**Date:** 2025-01-18
**Status:** Approved
**Campaign:** Milestone-based 5e campaign (50+ sessions per level)

## Overview

A web-based XP tracker for two D&D 5e characters that uses a token-based progression system. Tokens are earned through XP accumulation and can be spent on permanent character improvements (ASIs, feats) to provide meaningful progression during long stretches at the same level.

## Characters

- **Seraphina** - Level 2 Rogue
- **Konstantin** - Level 2 Bard

Both start with 0 XP and 0 tokens earned.

## Core Mechanics

### Token Formula
```
XP_per_token = 1000 × character_level × (2 + tokens_earned)
```

**Examples:**
- Level 2, Token 1: 4,000 XP
- Level 2, Token 10: 24,000 XP
- Level 10, Token 1: 20,000 XP
- Level 10, Token 10: 120,000 XP

### Token Usage
- 10 tokens = Ability Score Improvement (ASI)
- 4-8 tokens = Various feats (DM discretion)

### XP Award Guidelines

**Creature Kills (Last Hit):**
- Use CR-based XP from Monster Manual

**Critical Skill Challenges:**
- Minor: 100 × level
- Major: 250 × level
- Pivotal: 500 × level

**Quest Completion:**
- Side quest: 200 × level
- Main quest: 500 × level
- Major arc: 1000 × level

*Note: DM can adjust these values per instance based on actual performance.*

### Pacing Target
- Moderate pace: ~1 token every 6-10 active sessions
- At level 2: ~1,000-1,500 XP per typical session
- Natural flow based on player activity

## Architecture

### Page Structure
- **File:** `dnd_xp_tracker.html`
- **Navigation:** Add link from `dnd.html` nav menu
- **Styling:** Matches existing D&D tools (dimension template)
- **Logo:** Links back to `dnd.html`

### Data Model

Stored in `localStorage` as JSON:

```json
{
  "characters": [
    {
      "name": "Seraphina",
      "class": "Rogue",
      "level": 2,
      "currentXP": 0,
      "tokensEarned": 0,
      "portraitPath": "assets/seraphina.jpg"
    },
    {
      "name": "Konstantin",
      "class": "Bard",
      "level": 2,
      "currentXP": 0,
      "tokensEarned": 0,
      "portraitPath": "assets/konstantin.jpg"
    }
  ],
  "lastUpdated": "ISO timestamp"
}
```

### Key Calculations
- `XP_for_next_token = 1000 × level × (2 + tokens_earned)`
- `progress_percentage = (currentXP / XP_for_next_token) × 100`
- Auto-increment tokens when `currentXP >= XP_for_next_token`
- Carry over excess XP to next token
- Auto-decrement tokens when removing XP drops below threshold

## UI Design

### Character Cards (Pokemon-inspired)

**Layout:**
```
┌─────────────────────────────────────┐
│ [Portrait] CHARACTER NAME           │
│            Class • Level N          │
│                                     │
│ [████████░░░░] 2500/4000 XP        │
│                                     │
│ Character currently has N tokens.  │
└─────────────────────────────────────┘
```

**Styling:**
- **Seraphina Card:**
  - Background: Light pastel pink and whites gradient
  - Portrait: Rounded square border

- **Konstantin Card:**
  - Background: Light royal blue and golds gradient
  - Portrait: Rounded square border

- **Typography:**
  - Name: Bold, 18-20px
  - Class/Level: Italic, 12-14px, slightly muted
  - Token count: Regular, readable

- **Progress Bar:**
  - Rounded corners
  - Gradient fill (creative liberty for visual appeal)
  - Empty portion: Light gray
  - Smooth animation on changes
  - XP fraction overlay on right

- **Card Effects:**
  - Rounded corners
  - Subtle box shadow for depth
  - Responsive: Stack vertically on mobile, side-by-side on desktop

### XP Management Interface

Located below character cards:

**Components:**
- **Dropdown:** Select character (Seraphina / Konstantin)
- **Number Input:** XP amount (positive or negative)
- **Buttons:** "Add XP" | "Subtract XP"

**Validation:**
- Input must be numeric
- Visual feedback on value changes
- Smooth progress bar updates

### Quick Reference Section

Always visible or collapsible section showing:

**Token Formula:**
- Formula with current examples at player's level

**Skill Challenges:**
| Type | Formula | Level 2 | Level 10 |
|------|---------|---------|----------|
| Minor | 100 × level | 200 XP | 1,000 XP |
| Major | 250 × level | 500 XP | 2,500 XP |
| Pivotal | 500 × level | 1,000 XP | 5,000 XP |

**Quests:**
| Type | Formula | Level 2 | Level 10 |
|------|---------|---------|----------|
| Side | 200 × level | 400 XP | 2,000 XP |
| Main | 500 × level | 1,000 XP | 5,000 XP |
| Arc | 1000 × level | 2,000 XP | 10,000 XP |

**Note:** Creature kills use CR-based XP from Monster Manual

### Admin Mode

**Toggle:** Top-right corner button
- Locked (default): 🔒 "Locked"
- Active: 🔓 "Admin Mode"

**When Active:**
- Yellow border glow on editable fields
- Click level, currentXP, or tokensEarned to edit inline
- Press Enter to save, Escape to cancel
- Toast notification: "Admin mode active"
- Changes immediately recalculate and save

**When Locked:**
- Values are read-only
- Only XP management interface works

## Features

### Auto-Save
- Every change saves immediately to localStorage
- Display "Last saved: [timestamp]" subtly

### Export/Import

**Export:**
- Button: "Download Backup"
- Filename: `xp-tracker-backup-YYYY-MM-DD.json`
- Contains full character data with timestamp

**Import:**
- Button: "Upload Backup"
- Opens file picker
- Validates JSON before loading
- Confirmation: "This will replace current data. Continue?"
- Error handling for invalid files

### Token Gain/Loss Logic

**Gaining Tokens:**
1. When `currentXP >= XP_for_next_token`
2. Increment `tokensEarned` by 1
3. Set `currentXP = currentXP - XP_for_next_token`
4. Repeat check (handles multiple tokens in single award)
5. Visual feedback: Brief highlight on token count

**Losing Tokens:**
1. When subtracting XP would make `currentXP < 0`
2. If `tokensEarned > 0`:
   - Show confirmation: "This will remove a token. Continue?"
   - Decrement token, recalculate threshold, adjust XP
3. If `tokensEarned = 0`:
   - Set `currentXP = 0` (can't go negative)

### Initial Load
- Check localStorage for existing data
- If found: Load and display
- If not found: Initialize with default values (level 2, 0 XP, 0 tokens)

## Error Handling

- **Invalid import file:** Show error, don't corrupt data
- **Invalid input:** Prevent submission, show validation message
- **localStorage unavailable:** Warn user to export backup
- **NaN values:** Reject input, show friendly error

## Visual Feedback

- **XP changes:** Brief animation/highlight on affected values
- **Token gained:** Subtle celebratory effect
- **Admin mode:** Visual indicator when active
- **Progress bar:** Smooth transition animations

## Assets Required

- Portrait images (JPEGs from `/mnt/c/Users/maxim/Downloads/`)
  - `seraphina.jpg` → copy to `assets/`
  - `konstantin.jpg` → copy to `assets/`

## Technical Stack

- HTML5
- CSS3 (matching dimension template)
- Vanilla JavaScript
- localStorage API
- File API (for export/import)

## Future Considerations

- Add more characters (extensible data model)
- XP history log
- Token spending tracker
- Character notes field
- Mobile app version

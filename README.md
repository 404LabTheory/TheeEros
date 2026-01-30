# Eros Index Chatbot

A polished, elegant conversational assessment chatbot featuring a newspaper + sacred-gilded aesthetic. This is a complete, self-contained single-page application using vanilla HTML/CSS/JavaScript with no external dependencies.

## Overview

**Eros Index** is an interactive chatbot that guides users through a structured psychological assessment. It features:

- **Elegant Newspaper + Sacred Design**: Aged paper texture, gold accents, royal purple headings, deep red warnings
- **Responsive Chat Interface**: Message bubbles styled like editorial blocks, user responses in contrasting panels
- **Live Scoreboard**: Archetype preview, confidence meter, progress tracking, and subfactor snapshot
- **Full Scoring Logic**: Raw subfactor averages, within-person normalization, prototype distance calculation
- **Dynamic Follow-ups**: Conditional logic to trigger follow-up questions based on assessment thresholds
- **Data Persistence**: LocalStorage support to resume assessments
- **JSON Export**: Download assessment results for analysis

## Files

- **index.html** - Complete SPA with embedded CSS and JavaScript
- **data/config.json** - Application configuration (maxBaseItems, scoring settings, UI toggles, ethics disclaimers)
- **data/instrument.v1.json** - Assessment items with types (likert7, text), domains, subfactors, and follow-up definitions
- **data/prototypes.v1.json** - Archetype prototypes with profiles (subfactor scores for each archetype)

## GitHub Pages Deployment

### Option 1: Deploy from Repository Root

1. Push this repository to GitHub
2. Go to **Settings > Pages**
3. Select **Deploy from a branch**
4. Choose **main** branch and **/ (root)** folder
5. Save and wait for deployment

Your site will be live at: `https://USERNAME.github.io/TheeEros/`

### Option 2: Deploy from `/docs` Folder

1. Copy all files to a `/docs` folder in your repository
2. Go to **Settings > Pages**
3. Select **Deploy from a branch**
4. Choose **main** branch and **/docs** folder
5. Save and wait for deployment

## Usage

### Starting the Assessment

1. Open the website
2. Read the disclaimer (if enabled in config)
3. Type **"start"** in the chat input to begin
4. Answer questions using either:
   - **Likert 7-point scale**: Click pill buttons (1–7)
   - **Text input**: Type your response and press Send

### Assessment Flow

- **Base Items**: Questions defined in `instrument.v1.json`
- **Follow-ups**: Automatically triggered if enabled and thresholds are met:
  - `norm.ABD > 70` → enqueue `FU_ABD_1`
  - `norm.ENG > 70` → enqueue `FU_ENG_1`
  - `norm.CONTROL > 65` → enqueue `FU_CTRL_1`
  - `norm.ERASE > 65` → enqueue `FU_ERASE_1`
  - `norm.FLIGHT > 75` → enqueue `FU_FLIGHT_1`

### Scoreboard Updates (Live)

The right-side scoreboard updates in real-time as you answer:

- **Archetype Preview**: Best-matching archetype name and description
- **Confidence Meter**: How confident the assessment is (0–100%)
- **Progress**: Answered base items / total base items
- **Follow-up Count**: Number of queued follow-up questions
- **Subfactor Snapshot**: Current normalized scores (0–100) for each subfactor

### Controls

- **Reset**: Clear all responses and start over (confirmation required)
- **Export JSON**: Download results including responses, raw/normalized scores, best archetype, and confidence

## Configuration (config.json)

```json
{
  "app": {
    "name": "Eros Index Chatbot",
    "version": "v1.0",
    "environment": "production",
    "debug": false
  },
  "data": {
    "instrument": "instrument.v1.json",
    "prototypes": "prototypes.v1.json"
  },
  "assessment": {
    "maxBaseItems": 69,
    "enableFollowups": true,
    "confidenceSeparationDenominator": 18,
    "resumeFromLocalStorage": true
  },
  "ui": {
    "allowJsonExport": true,
    "showConfidenceMeter": true,
    "showLiveSubfactorSnapshot": true,
    "startKeyword": "start"
  },
  "storage": {
    "useLocalStorage": true,
    "storageKey": "eros_index_chat_v1"
  },
  "ethics": {
    "showDisclaimer": true,
    "disclaimerText": "This assessment is for research and self-reflection purposes only. It is not a clinical or diagnostic tool."
  }
}
```

### Key Configuration Parameters

| Parameter | Purpose |
|-----------|---------|
| `maxBaseItems` | Expected total base items in instrument (for progress tracking) |
| `enableFollowups` | Enable conditional follow-up question logic |
| `confidenceSeparationDenominator` | Divisor for confidence calculation (higher = stricter) |
| `resumeFromLocalStorage` | Resume assessment from previous session |
| `allowJsonExport` | Enable/disable JSON export button |
| `startKeyword` | Command to begin assessment (default: "start") |
| `storageKey` | LocalStorage key for saving responses |

## Instrument Format (instrument.v1.json)

```json
{
  "version": "v1",
  "scaleLabel": "1=Strongly Disagree … 7=Strongly Agree",
  "items": [
    {
      "id": "ITEM_001",
      "type": "likert7",
      "domain": "Domain Name",
      "sub": "ABD",
      "prompt": "Question or statement to rate?",
      "isFollowup": false
    },
    {
      "id": "FU_ABD_1",
      "type": "text",
      "domain": "Follow-up",
      "sub": "ABD",
      "question": "Tell us more about your experience.",
      "isFollowup": true
    }
  ]
}
```

### Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Unique identifier for item |
| `type` | "likert7" \| "text" | Yes | Response type |
| `domain` | string | No | Category/domain name |
| `sub` | string | Yes | Subfactor key (e.g., "ABD", "ENG", "CONTROL") |
| `prompt` / `question` | string | Yes | Question text to display |
| `isFollowup` | boolean | No | Mark as follow-up item (default: false) |

## Prototypes Format (prototypes.v1.json)

```json
{
  "version": "v1",
  "archetypes": [
    {
      "id": "CONQUISTADOR",
      "name": "The Conquistador",
      "description": "Bold, independent, driven by novelty."
    },
    ...
  ],
  "profiles": {
    "CONQUISTADOR": {
      "ABD": 40,
      "ENG": 75,
      "CONTROL": 60,
      "ERASE": 30,
      "FLIGHT": 70
    },
    ...
  }
}
```

### Prototype Fields

- **Archetype**: Unique ID, display name, brief description
- **Profile**: Subfactor scores (0–100) representing the "ideal" prototype for each archetype

## Scoring Algorithm

### Raw Scores
Convert Likert 7 responses (1–7) to 0–100 scale:
```
raw[subfactor] = (mean(responses) - 1) / 6 * 100
```

### Normalization (Within-Person)
Apply z-score transformation with centering:
```
z = (raw[subfactor] - mean(all_raw)) / stddev(all_raw)
normalized[subfactor] = clamp(50 + z * 15, 0, 100)
```

### Archetype Matching
Calculate Mean Absolute Error (MAE) distance to each prototype:
```
distance[archetype] = mean(|normalized[subfactor] - profile[archetype][subfactor]|)
bestArchetype = argmin(distance)
```

### Confidence
Separation metric between best and runner-up:
```
confidence = clamp((distance[runner-up] - distance[best]) / denominator, 0, 1)
```

## Design System

### Color Palette

| Name | Hex | Usage |
|------|-----|-------|
| Gold | `#D4AF37` | Primary accent, borders, highlights |
| Purple | `#5B2C83` | Headings, labels, user input |
| Red | `#8B1E2D` | Warnings, errors, disclaimers |
| Dark BG | `#2A1F2F` | Main background |
| Paper | `#EBE7DE` | Card backgrounds, text areas |
| Text Dark | `#1A1410` | Primary text |
| Text Light | `#5A5050` | Secondary text, meta |
| Border Light | `#D0CCBF` | Subtle borders |

### Typography

- **Headings**: Georgia, Times New Roman (serif)
- **Body**: System font stack (Segoe UI, Roboto, etc.)
- **Hierarchy**: Large headlines → subheadings → body → small-caps meta

### Texturing

- **Paper Grain**: CSS grid noise using `linear-gradient` patterns
- **Aged Effect**: Subtle radial gradient overlay with paper tone
- **Borders**: Thin gold rules and subtle purple accents
- **Shadows**: Soft, offset drop shadows for depth

## Accessibility

- **Contrast**: All text meets WCAG AA standards (4.5:1+)
- **Keyboard Navigation**: Tab/Enter support for pills and buttons
- **Focus States**: Visible outlines on interactive elements
- **ARIA Labels**: Semantic HTML structure for screen readers
- **Responsive**: Mobile, tablet, and desktop layouts

## Local Development

No build tools required. Simply:

1. Clone the repository
2. Serve files locally (e.g., `python -m http.server 8000`)
3. Open http://localhost:8000 in your browser
4. Edit `data/*.json` or `index.html` and refresh

## Browser Compatibility

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Any modern browser with ES6 and fetch API support

## Notes

- **No External Dependencies**: All CSS, HTML, and JavaScript are self-contained
- **No Network Calls**: Assessment data is loaded locally from `/data/` folder
- **No Frameworks**: Pure vanilla JavaScript with semantic HTML
- **No Build Steps**: Deploy directly to GitHub Pages
- **Comment Safety**: Code avoids inline comments after code on same line to prevent parsing issues

## License

This project is provided as-is for research and educational purposes.

## Support

For issues or feature requests, open a GitHub issue on the repository.

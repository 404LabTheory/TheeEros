# Eros Index Assessment

**A Research-Grade Mixed-Method Psychometric Tool for Relational Orientation and Sexual Expression**

---

## Overview

The Eros Index is a comprehensive 69-item mixed-method assessment designed to measure five distinct domains of relational and sexual psychology:

1. **Attachment Orientation** — How individuals navigate bonding, fear of loss, and relational security
2. **Emotional Regulation Style** — Preferred mechanisms for managing conflict and emotional intensity
3. **Relational Investment Strategy** — How individuals allocate resources, sacrifice, and maintain commitment
4. **Desire Activation Pattern** — What psychological and emotional conditions enhance or diminish sexual/romantic desire
5. **Identity Stability vs Adaptation** — The degree to which self-concept remains fixed or fluid in relationships

---

## Assessment Structure

### Quantitative Items (63)

- **47 Likert-7 items** (1=Strongly Disagree → 7=Strongly Agree)
  - Distributed across 5 domains, measuring 5 subfactors: ABD, ENG, CONTROL, ERASE, FLIGHT

- **8 Multiple-Choice items** (single select)
  - Exploratory response categories, stored without quantification (optional future scoring)

- **5 True/False items**
  - Binary relational belief statements

- **4 Fill-in-the-blank items** (5–10 words expected)
  - Qualitative proxies for relational patterns

### Qualitative Items (6)

- **4 Short-answer items** (2–4 sentences expected)
  - Encourage narrative description of internal processes

- **1 Long-answer closing narrative** (150–300 words target)
  - Retrospective relationship story reflecting all five domains

### Follow-up Items (5)

Triggered conditionally based on normalized subfactor scores:

- **FU_ABD_1** (Attachment Orientation | ABD > 70)
- **FU_ENG_1** (Attachment Orientation | ENG > 70)
- **FU_CTRL_1** (Relational Investment Strategy | CONTROL > 65)
- **FU_ERASE_1** (Relational Investment Strategy | ERASE > 65)
- **FU_FLIGHT_1** (Emotional Regulation Style | FLIGHT > 75)

---

## Subfactors and Psychological Constructs

### ABD (Anxious/Avoidant Bonding Dynamics)
Items probe fear of abandonment, sensitivity to relational shifts, and seeking reassurance.

**Domain Spread:** Attachment Orientation, Desire Activation Pattern, Identity Stability vs Adaptation

### ENG (Engagement & Closeness Discomfort)
Items measure resistance to emotional intimacy, freedom loss, and need for autonomy.

**Domain Spread:** Attachment Orientation, Desire Activation Pattern

### CONTROL (Relational Command & Predictability)
Items assess need for influence, structure-building, and trust via predictability.

**Domain Spread:** Emotional Regulation Style, Relational Investment Strategy, Desire Activation Pattern

### ERASE (Self-Erasure & Sacrifice)
Items capture identity suppression, unmet need acceptance, and peaceful submission.

**Domain Spread:** Relational Investment Strategy, Identity Stability vs Adaptation

### FLIGHT (Conflict Avoidance & Withdrawal)
Items measure emotional shutdown, escape impulse, and space-seeking during tension.

**Domain Spread:** Emotional Regulation Style

---

## Scoring Pipeline

### Step 1: Raw Likert Conversion (Likert-7 items only)

For each Likert-7 response (1–7):

$$v_{01} = \frac{\text{response} - 1}{6}$$

$$v_{100} = v_{01} \times 100$$

Result: 0–100 scale where 1=0, 7=100.

### Step 2: Subfactor Raw Means

For each subfactor, calculate the mean of all valid (non-null) Likert-7 responses:

$$\text{raw}_{sub} = \frac{\sum v_{100}}{n}$$

where $n \geq 1$.

### Step 3: Within-Person Normalization (Z-Score Mapping)

**If ≥3 valid subfactors exist:**

1. Calculate mean across all subfactors: $\mu = \frac{1}{k} \sum \text{raw}_i$
2. Calculate standard deviation: $\sigma = \sqrt{\frac{1}{k} \sum (\text{raw}_i - \mu)^2}$
3. Calculate z-score: $z = \frac{\text{raw}_{sub} - \mu}{\sigma}$ (if $\sigma > 0$; else $z = 0$)
4. Map to 50±15 scale: $\text{norm}_{sub} = 50 + z \times 15$
5. Clamp to [0, 100]: $\text{norm}_{sub} = \max(0, \min(100, \text{mapped}))$

**If <3 valid subfactors:**

$$\text{norm}_{sub} = \text{raw}_{sub}$$

### Step 4: Archetype Matching (Mean Absolute Error)

For each of 12 archetypes, calculate Mean Absolute Error (MAE) using **available keys only**:

$$\text{MAE}_{\text{arch}} = \frac{\sum_{i \in \text{available}} |\text{norm}_i - \text{profile}_i|}{|\text{available}|}$$

Select archetype with **minimum MAE**.

### Step 5: Confidence Calculation

$$\text{confidence} = \text{clamp}\left(\frac{\text{distance}_{\text{runner-up}} - \text{distance}_{\text{best}}}{\text{denominator}}, 0, 1\right)$$

where `denominator = config.assessment.confidenceSeparationDenominator` (default: 18).

---

## 12 Archetypes

### Conquistador
**Strategic, ambitious, driven by achievement and dominance.**
- Primary profile: High CONTROL (85), high ENG (55), low ERASE (25)
- Seeks power, predictability, strategic advantage in partnerships

### Siren
**Alluring, emotionally complex, seeks intensity and admiration.**
- Primary profile: High ENG (80), high CONTROL (70), moderate ABD (45)
- Energized by mystery, danger, and the thrill of being wanted

### Magistrate
**Fair, principled, values equity and clear agreements.**
- Primary profile: High CONTROL (75), low ENG (35), moderate ABD (40)
- Trusts logic, contracts, and predictable relational rules

### Devotee
**Loyal, self-sacrificing, deeply invested in relational continuity.**
- Primary profile: High ABD (75), high ERASE (80), moderate CONTROL (40)
- Fears loss above all; finds meaning in service and endurance

### Paladin
**Protective, principled, seeks to defend and elevate partners.**
- Primary profile: High CONTROL (65), moderate ABD (55), moderate ERASE (50)
- Bridges warrior and caregiver; stable yet attuned

### Oracle
**Intuitive, introspective, values depth and psychological understanding.**
- Primary profile: Balanced across all (45–55), slightly high FLIGHT (55)
- Seeks meaning-making and integration of shadow

### Jester
**Playful, adaptive, uses humor and lightness to navigate relationships.**
- Primary profile: High ENG (75), high FLIGHT (60), low CONTROL (35)
- Agile, witty, avoids heaviness through levity and distance

### Muse
**Creative, inspiring, thrives on emotional and intellectual stimulation.**
- Primary profile: High ENG (70), moderate across others (45–50)
- Energized by novelty, ideas, and imaginative partners

### Shaman
**Healer, transformative, seeks to integrate shadow and wholeness.**
- Primary profile: High ERASE (60), balanced ENG/ABD (55 each), moderate FLIGHT (50)
- Comfortable with complexity, cycles, and relational alchemy

### Oasis
**Nurturing, grounding, offers emotional refuge and stability.**
- Primary profile: High ABD (65), high ERASE (70), moderate CONTROL (55)
- Safe harbor; willing sacrifice with relational depth

### Tyrant
**Commanding, controlling, uses power to maintain dominance and security.**
- Primary profile: Very high CONTROL (90), moderate ABD (60)
- Anxiety masked by command; trust = compliance

### Martyr
**Self-effacing, sacrificial, finds identity through relational suffering.**
- Primary profile: Very high ABD (80), very high ERASE (85), high FLIGHT (65)
- Enmeshed fear; suffering as loyalty

---

## File Structure

```
/TheeEros/
├── index.html                    # Single-page application with embedded CSS/JS
├── data/
│   ├── config.json              # Application configuration
│   ├── instrument.v1.json       # 69 items + 5 follow-ups
│   └── prototypes.v1.json       # 12 archetype profiles
└── README.md                    # This file
```

---

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
    "resumeFromLocalStorage": true,
    "allowPartialCompletion": true
  },
  "scoring": {
    "method": "prototype_distance",
    "distanceMetric": "mean_absolute_error",
    "weights": "uniform",
    "normalizeWithinPerson": true
  },
  "ui": {
    "tone": "neutral_academic",
    "showArchetypePreview": true,
    "showConfidenceMeter": true,
    "showLiveSubfactorSnapshot": true,
    "allowJsonExport": true,
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

### Key Parameters

- **confidenceSeparationDenominator** (default: 18) — Controls confidence range. Higher = more generous confidence.
- **enableFollowups** (boolean) — If `true`, follow-up items trigger based on subfactor thresholds.
- **resumeFromLocalStorage** (boolean) — If `true`, assessment state persists across sessions.
- **allowPartialCompletion** (boolean) — If `true`, users can export results mid-assessment.

---

## Deployment to GitHub Pages

### Prerequisites

- Repository initialized with Git
- GitHub account with repository access

### Steps

1. **Clone or create the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/TheeEros.git
   cd TheeEros
   ```

2. **Ensure file structure:**
   ```
   index.html
   data/config.json
   data/instrument.v1.json
   data/prototypes.v1.json
   ```

3. **Verify file paths are relative** (e.g., `./data/config.json`, not `/data/config.json`)

4. **Commit and push:**
   ```bash
   git add .
   git commit -m "Initial commit: Eros Index assessment"
   git push origin main
   ```

5. **Enable GitHub Pages:**
   - Go to repository **Settings** → **Pages**
   - Select **Branch: main** and **Folder: / (root)**
   - Save

6. **Access the assessment:**
   ```
   https://YOUR_USERNAME.github.io/TheeEros/
   ```

### Verification Checklist

- ✅ Health Check panel shows "OK" for all 3 files
- ✅ Base URL correctly displays repo root
- ✅ Typing "start" begins assessment
- ✅ Likert-7 pills render and select
- ✅ Multiple-choice options appear
- ✅ True/False toggle appears
- ✅ Fill-in and short-answer textareas accept input
- ✅ Progress updates after each response
- ✅ Export button downloads JSON
- ✅ Email button opens mailto with results
- ✅ Results show archetype, confidence, and subfactor scores

---

## Troubleshooting

### "404 Error loading instrument"

**Cause:** Relative paths not resolving correctly on GitHub Pages.

**Solution:**
1. Open browser DevTools (F12) → Console
2. Check Health Check panel for HTTP status
3. Verify file paths use `./data/filename.json` format
4. Confirm files exist in `/data/` directory
5. Clear browser cache and reload

### Assessment loads but questions don't appear

**Cause:** instrument.v1.json has invalid JSON or missing `items` array.

**Solution:**
1. Validate JSON: `npm install -g jsonlint && jsonlint data/instrument.v1.json`
2. Ensure `instrument.json` has `items: [{id, type, prompt, ...}]` structure
3. Check browser console for parsing errors

### Textbox hidden or not responding

**Cause:** CSS display rules or JavaScript interference.

**Solution:**
1. The codebase includes permanent visibility rules (`display: flex !important`)
2. Clear localStorage: `localStorage.clear()` in console
3. Refresh page (Ctrl+Shift+R for hard refresh)
4. Check DevTools → Elements for inline `display: none` styles

### Archetype not matching correctly

**Cause:** Insufficient Likert-7 responses or missing subfactor values.

**Solution:**
1. Ensure ≥3 Likert-7 items answered per subfactor
2. Check normalized scores in browser console: `window.app.calculateScores()`
3. Verify prototypes.v1.json has all 12 archetypes with valid profile values
4. Confirm scoring formula uses only "available keys" (answered subfactors)

### Follow-ups not triggering

**Cause:** Threshold not met or `enableFollowups` disabled.

**Solution:**
1. Check config.json: `"enableFollowups": true`
2. Verify subfactor normalized score exceeds threshold:
   - ABD > 70, ENG > 70, CONTROL > 65, ERASE > 65, FLIGHT > 75
3. Check browser console: `window.app.followupQueue` should show queued items

---

## Example JSON Export

```json
{
  "timestamp": "2026-01-30T14:22:45.123Z",
  "duration_seconds": 342,
  "responses": {
    "L01": 6,
    "L02": 5,
    "MC01": 0,
    "TF01": true,
    "FB01": "that something is fundamentally wrong with me",
    "SA01": "I go quiet and mentally retreat.",
    "LA01": "I once loved someone who was emotionally unavailable..."
  },
  "scores_raw": {
    "ABD": 72.5,
    "ENG": 45.0,
    "CONTROL": 58.3,
    "ERASE": 65.0,
    "FLIGHT": 78.2
  },
  "scores_normalized": {
    "ABD": 68.4,
    "ENG": 32.1,
    "CONTROL": 51.7,
    "ERASE": 62.3,
    "FLIGHT": 81.5
  },
  "archetype_match": "Devotee",
  "confidence": 0.732,
  "visited_items": 72
}
```

---

## Design Considerations

### Visual Theme

- **Color Palette:** Gold (#D4AF37), Purple (#5B2C83), Red (#8B1E2D), Dark charcoal (#2a2a2a)
- **Typography:** Serif (Garamond/Georgia) for academic, aged aesthetic
- **Texture:** CSS-based newspaper grain (no external images)
- **Layout:** 2-column desktop (chat + scoreboard), single-column mobile

### Responsive Behavior

- **Desktop (>768px):** Chat section on left, scoreboard on right
- **Mobile (<768px):** Scoreboard above chat, full-width inputs, stacked buttons

### Accessibility

- Semantic HTML (labels, explicit form controls)
- ARIA attributes for screen readers (future enhancement)
- Keyboard navigation (Enter to submit, Tab between elements)
- High contrast text on dark backgrounds

---

## Research Ethics & Disclaimers

This assessment is **not a clinical diagnostic tool**. It is designed for:

- Self-reflection and relational awareness
- Research into romantic and sexual psychology
- Archetype exploration within attachment and desire frameworks
- Consensual relationship dialogue

**Users should not:**

- Use results for clinical judgment or mental health diagnosis
- Share results as proof of psychological classification
- Apply results to minors or non-consenting individuals
- Substitute assessment for therapy or professional counseling

---

## Technical Stack

- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+)
- **Data Format:** JSON (config, instrument, prototypes)
- **Storage:** Browser LocalStorage (resumable state)
- **Deployment:** GitHub Pages (static hosting)
- **External Dependencies:** None (zero CDN usage, zero frameworks)

---

## License & Attribution

This assessment is provided for research and educational purposes. If you use this tool in published research, please cite:

> 404LabTheory. (2026). Eros Index Assessment: A mixed-method psychometric tool for relational orientation and sexual expression. *https://github.com/404LabTheory/TheeEros*

---

## Support & Issues

For technical issues, feature requests, or data validation problems:

1. Check this README's Troubleshooting section
2. Review browser console for error messages
3. Validate JSON files using an online JSON validator
4. Open an issue on GitHub with:
   - Browser and OS version
   - Steps to reproduce
   - Health Check panel status screenshot
   - Browser console errors (if any)

---

**Version:** 1.0  
**Last Updated:** January 30, 2026  
**Maintained by:** 404LabTheory

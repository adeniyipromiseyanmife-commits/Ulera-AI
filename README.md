# Ùlẹ̀rà AI — Burnout Detector Update
## Files in this folder

| File | Where it goes in your repo | What it does |
|------|---------------------------|--------------|
| `burnout-assessment.html` | Root (next to `index.html`) | The weekly check-in page |
| `burnout-engine.js` | `/js/` folder | Scoring logic (PHQ-9 + PSS-10 + GAD-7 → BRI) |
| `burnout-storage.js` | `/js/` folder | Saves results to localStorage, weekly cadence |
| `burnout-gauge.js` | `/js/` folder | Animates the gauge widget on your dashboard |
| `burnout.css` | `/css/` folder | All styles for assessment + gauge |

---

## Step-by-step integration

### 1. Copy the files
```
your-repo/
├── burnout-assessment.html   ← new
├── index.html                (your existing dashboard)
├── css/
│   ├── burnout.css           ← new
│   └── (your existing CSS)
└── js/
    ├── burnout-engine.js     ← new
    ├── burnout-storage.js    ← new
    ├── burnout-gauge.js      ← new
    └── (your existing JS)
```

### 2. Update the redirect in burnout-assessment.html
Open `burnout-assessment.html` and find:
```js
const DASHBOARD_URL = 'index.html';
```
Change `index.html` to whatever your dashboard filename is.

### 3. Add the gauge widget to your dashboard
Paste this HTML wherever you want the burnout gauge to appear in `index.html`:

```html
<div class="burnout-gauge-wrapper" id="burnoutGaugeWidget">
  <div class="burnout-gauge-title">Burnout Risk</div>
  <svg class="burnout-gauge-svg" viewBox="0 0 160 160">
    <circle class="gauge-track" cx="80" cy="80" r="62"/>
    <circle class="gauge-fill"  cx="80" cy="80" r="62" id="dashGaugeFill"/>
    <text class="gauge-bri-label"    x="80" y="76" id="dashBRILabel">—</text>
    <text class="gauge-bri-sublabel" x="80" y="96" id="dashBRISublabel">check in</text>
  </svg>
  <div class="burnout-gauge-level"   id="dashGaugeLevel">No data yet</div>
  <div class="burnout-gauge-message" id="dashGaugeMessage">
    Complete your first weekly check-in to activate the burnout indicator.
  </div>
  <div class="burnout-gauge-trend hidden" id="dashGaugeTrend"></div>
  <a class="burnout-gauge-cta" id="dashGaugeCTA"
     href="burnout-assessment.html">Take Weekly Check-in</a>
</div>
```

### 4. Add scripts and styles to your dashboard
In the `<head>` of `index.html`, add:
```html
<link rel="stylesheet" href="css/burnout.css">
```

At the bottom of `index.html`, before `</body>`, add:
```html
<script src="js/burnout-engine.js"></script>
<script src="js/burnout-storage.js"></script>
<script src="js/burnout-gauge.js"></script>
```

---

## How it works (flow)

1. User opens dashboard → gauge widget shows last result (or prompts first check-in)
2. User taps CTA → goes to `burnout-assessment.html`
3. PHQ-9 (9 Qs) → PSS-10 (10 Qs) → GAD-7 (7 Qs) — one screen per question
4. If PHQ-9 Item 9 > 0 → safety screen shown (SURPIN hotline)
5. Results screen → animated gauge + score breakdown + 4-week trend chart
6. Result saved to localStorage → user redirected back to dashboard
7. 7 days later → CTA prompts next check-in

---

## Privacy notes
- All data stays in the user's browser localStorage
- Nothing is sent to any server
- Users can clear data via `BurnoutStorage.clearAllData()` (call from your settings page)
- PHQ-9 Item 9 safety message always shows when score > 0, regardless of overall BRI
https://claude.ai/public/artifacts/f5cfac5c-0375-44dd-b18f-1ae34ce0efc0

# 📚 EV Calculator Maintenance Guide

## Overview
This guide will help you safely edit and maintain the EV calculator without breaking functionality.

---

## 🏗️ Code Architecture

### File Structure
```
ev_apple.html          → Production version (Apple design)
ev_apple_documented.html → Same code with extensive comments
ev_ive.html           → Dark theme version
```

### Main Sections

1. **CSS Styles** (Lines 7-406)
   - Reset & Base Styles
   - Layout Containers
   - Hero Section
   - Main Card
   - Input Fields
   - Multiplier Chips
   - Confidence Slider
   - Results Card
   - Responsive Breakpoints

2. **HTML Structure** (Lines 407-498)
   - Container wrapper
   - Hero (title/subtitle)
   - Main card (inputs + slider)
   - Results card (sticky bottom)

3. **JavaScript** (Lines 500-668)
   - Utility functions
   - DOM references
   - Calculation engine
   - Event listeners
   - Initialization

---

## 🎨 Safe Edits Guide

### ✅ SAFE TO EDIT

#### 1. **Colors & Themes**
```css
/* Change these variables for different color schemes */
background: #f5f5f7;      /* Background color */
color: #1d1d1f;          /* Text color */
#007AFF                  /* Blue accent (buttons, links) */
#34C759                  /* Green (positive values) */
#FF3B30                  /* Red (negative values) */
#FF9500                  /* Orange (warning/neutral) */
#86868b                  /* Gray text */
```

#### 2. **Text Content**
```html
<!-- Safe to change any text labels -->
<h1>Is this trade worth it?</h1>  <!-- Main title -->
<p>Type a few numbers...</p>       <!-- Subtitle -->
<div class="input-hint">...</div>  <!-- Helper text -->
```

#### 3. **Default Values**
```javascript
const maxLossPct = parseFloat(maxLoss.value) || 25;  // Change default from 25
confidence.value = "20"  // In HTML, change default slider position
```

#### 4. **Verdict Thresholds**
```javascript
// In calculate() function, adjust these business rules:
if (ev > 0 && R >= 2) {           // "Good" threshold
} else if (R >= 1.5 && R < 2) {   // "Borderline" threshold
```

#### 5. **Slider Range**
```html
<!-- Change min/max for different confidence ranges -->
<input type="range" min="5" max="50" value="20" step="1">
```

---

### ⚠️ EDIT WITH CAUTION

#### 1. **Input Parsing**
```javascript
// parseShorthand() function - handles 2M, 500k, etc
// Be careful with the suffix detection logic
if (upperValue.endsWith('K'))  // Don't change these
```

#### 2. **Calculation Logic**
```javascript
// Core math - test thoroughly if you change:
const ev = (conf * upsideAmount) - ((1 - conf) * downsideAmount);
const R = downsideAmount > 0 ? (upsideAmount / downsideAmount) : 0;
```

#### 3. **Event Listeners**
```javascript
// Make sure calculate() is called after any input change
input.addEventListener('input', calculate);
```

---

### 🚫 DO NOT EDIT

#### 1. **Quote Characters**
```javascript
// ALWAYS use straight quotes in JavaScript:
'string'  ✅ Correct
'string'  ❌ Will cause syntax errors
```

#### 2. **DOM Query Pattern**
```javascript
// This pattern is used throughout - don't change it:
const $ = id => document.getElementById(id);
```

#### 3. **Class Toggle Logic**
```javascript
// Results card visibility - critical for UX:
resultsCard.classList.add('visible');
resultsCard.classList.remove('visible');
```

---

## 🧪 Testing Checklist

After making any changes, test these scenarios:

### Basic Functionality
- [ ] Enter `2M` for Entry MC → Should parse correctly
- [ ] Enter `10M` for Target MC → Should parse correctly  
- [ ] Enter `2.5k` for Position → Should parse correctly
- [ ] Drag confidence slider → Display updates immediately
- [ ] Results card appears when all inputs filled

### Edge Cases
- [ ] Entry MC = 0 → Results card should hide
- [ ] Target < Entry → Should show warning/negative EV
- [ ] Max Loss = 100% → Should calculate correctly
- [ ] Confidence = 5% → Should show mostly negative EV
- [ ] Confidence = 50% → EV depends on R multiple

### Visual Checks
- [ ] Mobile view (< 480px) → Everything readable
- [ ] Tablet view (< 768px) → Proper layout
- [ ] Desktop view → Centered, proper spacing
- [ ] Results card → Sticks to bottom, blur effect works

### Interactions
- [ ] Multiplier chips (×2, ×5, ×10, ×50) → Update target correctly
- [ ] "Set to breakeven" → Updates confidence slider
- [ ] Focus on input → Selects all text
- [ ] Type invalid input → Handles gracefully

---

## 🔧 Common Modifications

### Add a New Input Field
```html
<!-- 1. Add HTML input -->
<div class="input-group">
  <label class="input-label" for="newField">New Field</label>
  <input type="text" id="newField" class="input-field" />
</div>

<!-- 2. Add JavaScript reference -->
const newField = $('newField');

<!-- 3. Add to event listeners -->
[entry, target, position, maxLoss, newField].forEach(input => {
  input.addEventListener('input', calculate);
});

<!-- 4. Use in calculate() function -->
const newValue = parseShorthand(newField.value);
```

### Change Color Scheme to Dark Mode
```css
/* Swap these values in CSS */
body {
  background: #000000;  /* Was #f5f5f7 */
  color: #f5f5f7;      /* Was #1d1d1f */
}

.main-card {
  background: #1c1c1e;  /* Was #ffffff */
}

.input-field {
  background: #2c2c2e;  /* Was #f6f6f6 */
  color: #ffffff;
}
```

### Add New Verdict State
```javascript
// In calculate() function:
if (ev > 100) {
  verdictClass = 'amazing';
  verdictText = 'Exceptional opportunity!';
} else if (ev > 0 && R >= 2) {
  // ... existing logic
}

// Add CSS for new state:
.verdict-amount.amazing {
  background: linear-gradient(45deg, #34C759, #007AFF);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

---

## 🐛 Troubleshooting

### Problem: Slider doesn't update
**Solution:** Check that `calculate()` is called in the slider event listener:
```javascript
confidence.addEventListener('input', (e) => {
  confidenceDisplay.textContent = e.target.value + '%';
  calculate();  // THIS MUST BE HERE
});
```

### Problem: JavaScript syntax error
**Solution:** Check for smart quotes - they must be straight quotes:
```javascript
// Use your text editor's find/replace:
Find: ['']  Replace with: '
Find: [""]  Replace with: "
```

### Problem: Results card won't appear
**Solution:** Check the visibility logic:
```javascript
// Must have all three inputs:
const hasMinimumInputs = entryMC > 0 && targetMC > 0 && posSize > 0;
```

### Problem: Shorthand (2M, 500k) not working
**Solution:** Check parseShorthand() is being called:
```javascript
const entryMC = parseShorthand(entry.value);  // Not parseFloat!
```

---

## 📦 Deployment

### For Production Use
1. Use `ev_apple.html` (minified, production-ready)
2. Test all inputs before deploying
3. Check mobile responsiveness
4. Verify no console errors

### For Development
1. Use `ev_apple_documented.html` (has comments)
2. Make changes incrementally
3. Test after each change
4. Keep backups of working versions

---

## 💡 Pro Tips

1. **Always test on mobile** - This is primarily a mobile tool
2. **Use the browser console** - Check for JavaScript errors
3. **Keep the math simple** - The core EV formula is sacred
4. **Preserve the slider** - It's the most fragile component
5. **Don't over-engineer** - Simplicity is the design philosophy

---

## 🆘 Getting Help

If you break something:
1. Check this guide first
2. Compare with `ev_apple_documented.html` (has all the comments)
3. Use git to revert to a working version
4. The core formula is: `EV = (probability × upside) - ((1-probability) × downside)`

Remember: The tool is intentionally simple. Resist the urge to add complexity.

---

*"Simplicity is the ultimate sophistication."* — Leonardo da Vinci (and Jony Ive)

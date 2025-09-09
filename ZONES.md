# EV Check Tool - Zone Structure
*Last Updated: December 2024*

## Current State
The tool is a single-page expected value calculator for cryptocurrency trades, designed with mobile-first principles and sub-5-second decision time.

## Zone Breakdown for Iterative Refinement

### ZONE 1: Header & Identity
**Current Implementation:**
- Title: "EV Check"
- Subtitle: "Expected value calculator"
- Pure black background (#000000)
- SF Pro typography

**Files:** Lines 444-449 in ev_ive.html

---

### ZONE 2: Market Cap Inputs
**Current Implementation:**
- Entry Market Cap field (accepts: 2M, 500k, 1.2B, raw numbers)
- Target Market Cap field (same parsing)
- Multiplier chips: ×2, ×5, ×10, ×50
- Live preview when typing shorthand
- Smart formatting on blur/focus

**Features:**
- parseShorthand() function for K/M/B suffixes
- formatToShorthand() for display
- Select-all on focus
- Comma formatting for raw numbers

**Files:** Lines 451-498 in ev_ive.html

---

### ZONE 3: Position & Risk
**Current Implementation:**
- Position Size in SOL (numeric input)
- Max Loss % of position (default 25%)
- Exit Slippage % (optional, default 0%)

**Logic:**
- Downside = Position × (MaxLoss% / 100)
- Upside applies slippage reduction

**Files:** Lines 500-541 in ev_ive.html

---

### ZONE 4: Confidence Slider
**Current Implementation:**
- Range: 5% to 50%
- Default: 20%
- Live display of percentage
- "Set to breakeven" button
- Haptic feedback at thresholds

**Interactions:**
- Vibrates at 10%, 20%, 30% marks
- Vibrates when crossing EV=0
- Scales on drag for visual feedback

**Files:** Lines 543-565 in ev_ive.html

---

### ZONE 5: Results Display
**Current Implementation:**
- Upside in SOL (after slippage)
- Downside in SOL
- Break-even probability %
- Dark containers with borders

**Calculations:**
- Upside = Position × (Multiple - 1) × (1 - Slippage%)
- Downside = Position × MaxLoss%
- Breakeven = Downside / (Upside + Downside)

**Files:** Lines 567-585 in ev_ive.html

---

### ZONE 6: Verdict
**Current Implementation:**
- Large EV amount (42px font)
- Pass/Fail logic: Requires BOTH EV > 0 AND R ≥ 2
- Color states: Green (allowed) / Red (fold)
- Shows R-multiple in message
- Breathing animation when positive

**Messages:**
- "✓ Allowed (3.2R)" when both pass
- "✗ Fold (R=1.5 < 2)" when R fails
- "✗ Fold (Negative EV)" when EV fails
- "✗ Fold (Bad EV & Low R)" when both fail

**Files:** Lines 587-593 in ev_ive.html

---

### ZONE 7: Actions & Meta
**Current Implementation:**
- Skip button (clears form)
- Take Trade button (logs and clears)
- LocalStorage persistence
- History tracking (last 20 trades)
- Confidence calibration after 10 trades

**Features:**
- Auto-focus first field after action
- Ripple effects on buttons
- Fade transitions between states

**Files:** Lines 595-603 in ev_ive.html

---

## Technical Architecture

### Core Functions
1. `parseShorthand(value)` - Parses 2M, 500k, etc to numbers
2. `formatToShorthand(num)` - Formats numbers to readable form
3. `formatSOL(value)` - Smart decimal places based on magnitude
4. `calculate()` - Main calculation engine
5. `trackTrade(taken)` - Records decisions for calibration

### State Management
- LocalStorage key: `evLastValues`
- Persists: entryMC, targetMC, position, maxLoss, slippage, confidence
- History key: `evHistory`

### Design System
- Colors: Black, grays, green (#00c851), red (#ff3b30)
- Typography: SF Pro Display/Text
- Animations: cubic-bezier(0.4, 0, 0.2, 1)
- Mobile breakpoint: 720px

---

## Review Process

### How to Give Feedback
1. Reference specific zone: "Zone 3 Notes:"
2. Be specific about the issue
3. Suggest desired behavior
4. One zone at a time

### Example Feedback Format
```
Zone 2 Notes:
- Issue: Multiplier chips too small on iPhone SE
- Desired: Increase touch target to 44px minimum
- Issue: "K" should be lowercase 
- Desired: Accept both, display as lowercase
```

### Change Protocol
- Changes isolated to specified zone only
- No cascading modifications
- Test after each zone completion
- Document significant changes here

---

## Version History

### v1.0 - Initial Jony Ive Build
- Clean, single-screen design
- 5-second decision flow
- Mobile-first principles

### v1.1 - Math Nerd Integration
- Added Max Loss % (replaced Stop Distance)
- Added multiplier chips
- Dual requirement: EV > 0 AND R ≥ 2
- "Set to breakeven" functionality
- Optional slippage field

---

## Next Zones to Review
*To be populated based on user feedback*

---

## Testing Checklist
- [ ] Loads in under 1 second
- [ ] All inputs accept shorthand (2M, 500k)
- [ ] Multiplier chips work
- [ ] Set to breakeven works
- [ ] R ≥ 2 requirement enforced
- [ ] LocalStorage persistence works
- [ ] Mobile responsive (iPhone SE to iPad)
- [ ] No console errors

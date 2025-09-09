# Iteration 2: Ruthless Simplification

## Vision
A single-screen, mobile-first decision calculator that turns a vague trade idea into a clear "go / borderline / fold" with the fewest possible inputs, zero confusion, and gentle, satisfying feedback.

## Execution Phases

### Phase 1: Core Structure Refactor
**Goal:** Establish the right input order and mental model

- [ ] **INPUT ORDER** - Reorder to match trader thinking:
  1. Entry market cap (where you'd buy)
  2. Position size in USD (how much you'd put in)
  3. Max loss with $/% toggle (what you're okay losing)
  4. Target market cap (where you'd be happy to sell)
  5. Confidence % (gut feel)

- [ ] **REMOVE CRUFT**
  - Remove Demo button
  - Remove Reset button
  - Remove Skip/Take Trade buttons
  - Remove current results grid layout

### Phase 2: Advanced Drawer
**Goal:** Hide complexity behind progressive disclosure

- [ ] Create collapsible "Advanced" section containing:
  - Exit slippage % (default 0)
  - Fees in basis points (default 0)
  - Bankroll in USD (triggers Kelly display)
  - Second target for 50/50 scale-out (future)

- [ ] Drawer mechanics:
  - Collapsed by default
  - Smooth 120ms expand/collapse
  - Subtle chevron indicator

### Phase 3: Sticky Results Card
**Goal:** Single focal point for decisions

- [ ] **CARD STRUCTURE**
  ```
  ┌─────────────────────────┐
  │ ALLOWED                 │ <- Large verdict
  │ EV: +5.5% ($137)       │ <- Justification
  │ R: 2.3                 │
  │                        │
  │ [▼ Why?]              │ <- Expandable details
  └─────────────────────────┘
  ```

- [ ] **VISIBILITY RULES**
  - Only appears when: entry, position, max loss, target all have values
  - Sticks to bottom on mobile scroll
  - Updates live with 120ms transitions

### Phase 4: Three-State Verdict System
**Goal:** Honest guidance without false confidence

- [ ] **LOGIC**
  - **Allowed:** EV > 0 AND R ≥ 2
    - Copy: "Numbers align. Positive EV and R ≥ 2."
    - Color: Subtle green tint
  
  - **Borderline:** |EV%| ≤ 1% OR 1.5 ≤ R < 2
    - Copy: "Close call. Consider improving R or reducing risk."
    - Color: Amber/yellow tint
  
  - **Fold:** Otherwise
    - Copy: "Math doesn't support it. Reduce loss or raise target."
    - Color: Subtle red tint

### Phase 5: Microcopy & Hints
**Goal:** Guide without patronizing

- [ ] **FIELD HINTS** (small text under each input)
  - Entry MC: "Where you'd buy"
  - Position: "How much you'd put in"
  - Max loss: "What you're okay losing"
  - Target: "Where you'd be happy to sell"
  - Confidence: "Gut feel. You can set it to breakeven"

- [ ] **PLACEHOLDERS** (example-driven)
  - Entry: "e.g., 2.3M"
  - Position: "e.g., 2.5k"
  - Max loss: "e.g., 625" or "e.g., 25"
  - Target: "e.g., 10M"
  - Confidence: (slider, no placeholder)

### Phase 6: Just-in-Time Education
**Goal:** Explain concepts only when needed

- [ ] **TOOLTIP SYSTEM**
  - Small (?) icons next to: EV, R, Breakeven%, Kelly
  - Tap to reveal one-sentence explanation
  - Auto-dismiss after 3 seconds or tap elsewhere

- [ ] **TOOLTIP COPY**
  - EV: "Your average result if you repeated this setup many times"
  - R: "Reward vs. loss. 2R means reward is twice the loss"
  - Breakeven: "Win rate needed so EV ≈ 0 for this setup"
  - Kelly: "Growth-optimal sizing. Most use half-Kelly"

### Phase 7: Visual Polish
**Goal:** Calm, professional, obvious

- [ ] **TYPOGRAPHY**
  - Use tabular figures for all numbers
  - Consistent sizing: 17px base, 24px verdict, 13px hints
  - SF Pro throughout

- [ ] **ANIMATION** (all 120ms ease, no bounce)
  - Value changes: fade transition
  - Chip taps: scale(0.98) then settle
  - Card appearance: slide up from bottom
  - Verdict changes: color fade, no flash

- [ ] **COLOR DISCIPLINE**
  - Remove harsh red/green
  - Use tints, not solid colors
  - Greyscale mode option for emotion-neutral

### Phase 8: Mobile Optimization
**Goal:** Thumb-friendly, one-handed use

- [ ] Touch targets minimum 44px
- [ ] Sticky card reachable with thumb
- [ ] Input ordering for natural flow
- [ ] Number keypads for appropriate fields
- [ ] Select-all on focus for quick edits

### Phase 9: Smart Defaults
**Goal:** Never show empty or confusing states

- [ ] Progressive reveal: each input unlocks next insight
- [ ] Smart number formatting: accept "2.3M", display with commas
- [ ] Clamp percentages: max loss ≤ 95%, confidence 5-50%
- [ ] Warning for impossible trades: target ≤ entry

### Phase 10: Final Polish
**Goal:** The details that make it feel inevitable

- [ ] Footer microcopy: "This is first-principles math..."
- [ ] Remove all decorative elements
- [ ] Test on iPhone SE to iPhone 14 Pro Max
- [ ] Ensure instant load (<100ms)
- [ ] Zero external dependencies

## Success Criteria

1. **Time to decision:** < 10 seconds from open to verdict
2. **Inputs required:** Maximum 5 (rest optional)
3. **Taps to verdict:** < 10 including typing
4. **Cognitive load:** No concept requires explanation to use
5. **Mobile first:** Fully usable one-handed on iPhone SE

## What We're NOT Building

- Trading history
- Multiple positions
- Complex strategies
- Educational content
- Social features
- Charts or graphs
- Login or accounts
- Saving trades

## Development Order

1. Core structure refactor (Phase 1)
2. Three-state verdict (Phase 4)
3. Sticky results card (Phase 3)
4. Microcopy & hints (Phase 5)
5. Advanced drawer (Phase 2)
6. Tooltips (Phase 6)
7. Visual polish (Phase 7)
8. Mobile optimization (Phase 8)
9. Smart defaults (Phase 9)
10. Final polish (Phase 10)

## Testing Checkpoints

After each phase:
- [ ] Works on iPhone SE
- [ ] No console errors
- [ ] Sub-100ms response
- [ ] Verdict updates live
- [ ] All inputs accept shorthand

## Design Principles

**Jony Ive's Laws Applied:**
1. "No design is better than good design"
2. "Simplicity is the ultimate sophistication"
3. "Every element must be necessary"
4. "The user should feel the tool was inevitable"

## Next Action

Begin with Phase 1: Core Structure Refactor. This breaks everything else, so it must come first.

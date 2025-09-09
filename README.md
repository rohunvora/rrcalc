# EV Check - Expected Value Calculator

A minimal, mobile-first tool for evaluating cryptocurrency trades through expected value and risk/reward calculations.

## Features

- **5-second decisions** - Single screen, instant calculations
- **Smart inputs** - Accept market caps as "2M", "500k", etc
- **Dual requirements** - Must pass both EV > 0 and R ≥ 2 tests
- **Mobile optimized** - Designed for trading on the go
- **Confidence calibration** - Tracks your prediction accuracy over time

## Files

- `ev_ive.html` - Main calculator (production version)
- `ev.html` - Alternative simpler version
- `starter.html` - Original complex version with ladder strategy
- `ZONES.md` - Development documentation and zone structure

## Usage

Simply open `ev_ive.html` in any modern browser. No installation required.

### Quick Start

1. Enter entry market cap (e.g., "2M")
2. Enter target market cap (e.g., "10M") or use multiplier chips
3. Enter position size in SOL
4. Set max loss % you're willing to accept
5. Adjust confidence slider
6. Get instant verdict: Allowed or Fold

## Design Philosophy

Built following Jony Ive's design principles:
- Remove everything that isn't essential
- Make the primary action obvious
- Respond instantly to every interaction
- Feel inevitable, not impressive

## Development

See `ZONES.md` for the component structure and contribution guidelines.

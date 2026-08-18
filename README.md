# Tethered UAS power architecture — interactive concept diagram

A single-file, interactive block diagram for a tethered UAV power delivery system: ground-station step-up, high-voltage tether transmission, and airborne step-down. Built as a proposal/concept aid — not a validated design tool.

<img width="890" height="608" alt="Peek 2026-08-18 17-18" src="https://github.com/user-attachments/assets/43d4ce18-17bc-4819-84d4-3d69c62b8314" />

## What it shows

Three stages, left to right / top to bottom:

1. **Ground station** — AC input → PFC + step-up → isolated driver, producing the high-voltage DC tether bus (~400 V nominal).
2. **Tether** — the transmission link. Live current and I²R loss/meter are computed from your power, voltage, and conductor gauge inputs.
3. **Airborne module** — step-down converter feeding the vehicle rail (50 V or 25 V).

## How to use it

- Open `tethered_uav_power_architecture.html` in any browser. No install, no server, no dependencies — it's a single self-contained file.
- **Top control bar**: adjust continuous power, tether voltage, and output rail. The four readout tiles (tether current, loss/m, system efficiency, airborne output current) update live.
- **Click any block** in the diagram to open its detail panel. Each stage has its own editable parameters:
  - *AC input* — input voltage class
  - *PFC + step-up* — topology choice, target efficiency
  - *Isolated driver* — isolation class
  - *Tether* — length, conductor gauge (AWG)
  - *Step-down converter* — topology choice, target efficiency
  - *Vehicle rail* — output voltage (mirrors the top control)
- Changes in a detail panel immediately update the diagram labels and the readout tiles. Press `Esc` or click outside the panel to close it.

## What the numbers mean (and their limits)

- **Tether current** = power ÷ tether voltage. This is the core reason the architecture runs at ~400 V: for a given power level, higher transmission voltage means lower current, which means lighter conductors and lower I²R loss.
- **I²R loss/meter** uses standard AWG resistance-per-meter values, doubled to account for both the outbound and return conductor. It's a first-pass estimate for comparing gauge/voltage trade-offs, not a substitute for a real cable spec calculation once thermal, tensile, and flex-life requirements are known.
- **System efficiency** is just the product of the two converter stage efficiencies you set (ground-side × airborne-side) — it doesn't model connector, isolation, or thermal derating losses.
- **Airborne output current** = power delivered at the vehicle rail, back-calculated from overall efficiency.

Treat every number here as a planning estimate to support a conversation about requirements — not as a finalized spec.

## Why it's built this way

Made for early-stage client conversations where the actual power level, tether length, and voltage targets aren't nailed down yet. Rather than présenting one fixed diagram, this lets you (or a client) drag a couple of sliders and immediately see how the current, loss, and efficiency numbers move — which tends to be a faster way to converge on real requirements than a static spec sheet.

## Tech notes

- Single HTML file, no external dependencies, no build step.
- Vanilla JS, inline SVG diagram, CSS custom properties for theming.
- Works offline. Safe to email or attach directly — nothing is loaded from the network.
- Tested for basic responsiveness down to mobile widths; the SVG diagram is the tightest constraint on small screens.

## Known limitations

- Efficiency and loss figures are simplified planning estimates, not simulation-grade results.
- No persistence — refreshing the page resets all values to defaults.
- Single AC input phase assumed; three-phase input isn't modeled.
- Isolation class and topology choices are presented as options, not validated against a specific safety standard — final selection should be confirmed against applicable regulations for the deployment.

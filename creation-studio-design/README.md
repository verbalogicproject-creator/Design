# Creation Studio · Synth tab design handoff (for milestone M4)

Design for the **Synth tab**: a modular synth plus DAW for electronic music (psytrance and cinematic textures), with AI generation (Lyria) beside local synthesis. Mobile first (Android phone in Chrome), then desktop, then a plugin window.

Visual identity: **the console and the scribble strip**. Steel panels, masking-tape labels in marker pen, amber for signal, tally red for recording and money, channel colour on fader caps. Dark only.

## What is in this folder
| Path | What it is |
|---|---|
| `tokens/tokens.json` | All tokens: colour, type scale, spacing, radii, depth (shadows), sizes, motion. |
| `tokens/tokens.css` | The same as CSS variables, prefixed `--cs-`. Rename to match your own. |
| `specs/components.md` | Component specs: sizes, paddings, geometry, states, touch and keyboard alternatives. |
| `specs/structure.md` | App shell, responsive rules, regions per view, data shapes the UI needs, interaction model, suggested React component map, accessibility. |
| `specs/token-notes.md` | What was kept, changed and added in the token set, and why. |
| `specs/open-questions.md` | Assumptions, placeholder values, and what is not designed yet. **Read before M4.** |
| `screens/*.png` | Every screen and library board as an image at 1x. `screens/INDEX.md` lists them. |
| `html/static/*.html` | Rendered HTML of every screen (structure and class names as designed), styled by `html/synth.css` and the bundled fonts. Open them directly in a browser. They are static snapshots: not interactive. |
| `html/synth.css` | The shared stylesheet the designs use. A reference for measurements, not code to ship. |
| `source/` | The design canvas source files (`*.dc.html`, `canvas.json`). These only render inside the design tool. |

## How to read it
1. Start with `screens/System.png` and `screens/Foundations.png`, then the four library boards (`Controls`, `Editors`, `Objects`, `Parts`).
2. Read `specs/components.md` alongside them. Every component lists its states and its **tap alternative** for every drag.
3. Use `screens/INDEX.md` to find a screen. Phone screens are 412 x 880, landscape 892 x 412, desktop 1440 x 900, plugin 900 x 600.
4. `specs/structure.md` is the guide for rebuilding in React with your own CSS variables.

## The signature feature: Morph
`screens/Morph.png` and spec 24 in `specs/components.md`. A scene-morph pad: four stored sounds on the corners, drag between them and every knob blends live, tap a corner to glide over a musical length, Ride to record the movement as automation. On the live canvas it is a working prototype.

## Hard rules the design follows
Touch targets at least 44 x 44. No information by colour alone. Body text at least 4.5:1. Reduced motion respected. The page never scrolls sideways (only the timeline, roll and patch canvases pan). Every drag has a tap alternative.

## Live canvas
The interactive design canvas is at https://claude.ai/artifact/TenhEpHfsgjfcZCFYDTySJ (private until shared). The screens here were rendered from the same source.

## Note on the drawings
The designs are static. Keyboard behaviour, focus order and gestures are **specified in the text** (components.md), not drawn.

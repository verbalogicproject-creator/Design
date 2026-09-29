# Notes on the token set: what is kept, changed and added

## Kept exactly as given
slate, slate-deep, slate-lift, seam, alu, alu-dim, tape, tape-shadow, ink, amber, tally, text, and the five cap colours. Fonts are Archivo Variable and Permanent Marker (tape only). Dark only.

## Changed (one value)
| Token | Was | Now | Why |
|---|---|---|---|
| `text-dim` | `#a9b4bc` | `#b8c2c9` | It was 5.0:1 on the page but **4.25:1 on `slate-lift` plates**, where most secondary text sits. Now 5.9:1 on the page and 5.0:1 on plates. |

## Added
| Token | Value | Why |
|---|---|---|
| `tally-text` | `#ffa594` | `tally` (`#e0452b`) is only 2.6:1 on `slate` and 3.0:1 on `slate-deep`, so it cannot be text. Prices and error text use this. `tally` stays for borders, lamps and fills. |
| `blue-text` | `#a0c3ef` | `cap-blue` is 2.6:1 on `slate`. Used for note names and confidence text. |
| `green-text` | `#8fd1a9` | For modulation and "saved / good" text. `cap-green` is fine for cables and caps but weak as text. |
| `well` | `#1c2429` | A step darker than `seam` for meter wells, LCD glass and text fields, so recesses read as deeper than plates. |
| `alu-shade` | `#6c757b` | The underside of an aluminium key (the 3px "thickness"). Keeps keys physical without gradients. |
| `amber-off` | `#4d4020` | An unlit lamp or meter segment, so a lit lamp reads by contrast and shape, not only by colour. |
| Type scale | see tokens.json | The brief gave families but no sizes. Nine steps, minimum 11px. |
| Spacing 4/8/12/16/24/32 | | As requested. |
| Radii | 2, 3, 4, 6, sheet, round | Small and hard-edged, like machined parts. |
| Depth (-2 to +12) | see tokens.json | Elevation expressed as **physical depth in px** instead of arbitrary shadow levels. Wells are negative. |
| Cable line style and port shape | | Audio solid + circle, notes dashed + square, mod dotted + diamond, so the three types differ without colour. |

## Rules that came out of the design
- `alu-dim` is never used for text (3.6:1 on the page).
- `ink` on `cap-red` is only 3.4:1 and on `cap-blue` 3.6:1, so text on those caps is avoided. Channel identity is the name on tape and a number beside the cap.
- Amber means signal and where you are: level, playhead, focus, lit lamps. The focus ring is the same amber on purpose.
- Tally red always appears with an icon or a price: recording (record key, `RECORDING` label), money (coin + price), destructive (trash icon or the word).
- The grain texture uses a repeating gradient. It is texture, not decoration; everything else is flat colour.
- New tokens are proposals. Rename freely; the values and roles matter.

## Lite profile: new tokens
**None.** Every Lite screen (Drum Kit, drum grid, sample picker, Lite Synth, Track FX sheet, frozen states, On PC chip) uses the existing `--cs-*` tokens.
- The **On PC** chip is a dashed `alu-dim` outline with `text-dim` text; the dashed outline (not a colour) is what separates it from the solid FROZEN tag.
- Frozen instrument names use `text-dim` in italics rather than a fainter colour, to stay at 4.5:1.
- The licence chips (CC0 / CC-BY) reuse the badge styles.

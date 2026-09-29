# Open questions, assumptions and what is not designed

## Placeholder values (invented for the drawings, replace with real ones)
- Prices: Generate **$0.08** (from the brief), Polish **$0.04** (placeholder). Balance **$4.20**, monthly cap **$5.00**, month total $1.36.
- Recording delay **42 ms** (measured). Buffer sizes 128 / 256 / 512 / 1024.
- Preset names (Forest 146, Goa Rubber, Night Sub, Acid Root, Dust Roll), project name "Rolling Bass 03", revision numbers.
- Export is **free and on the device**. If export costs or uploads anything, it needs the price chip and possibly the consent sheet.
- Provider terms: the consent sheet says how long the provider keeps audio is "set by the provider" and links to their terms. No retention figure is invented.

## Behaviour assumed (please confirm)
1. **Locked section** = the marker and blocks inside stay put when bars are inserted or deleted. Stricter locks (no edits inside) were not chosen.
2. AI takes are charged when they **finish**, not when they fail. A failed take says "You weren't charged".
3. Voice or family audio to the cloud shows the consent sheet **every time**; "Ask every time" is on and cannot be turned off for voice.
4. **Sing** works locally (pitch to notes). Only "Polish with AI" sends audio out.
5. A new automation clip starts at the parameter's current value and covers the current pattern (4 bars).
6. Mixer strips have **8 insert slots**, two return buses (Verb, Delay) and group buses. Master has no pan, mute or sends.
7. **Fold to scale** is how notes get 44 px rows on touch. Desktop rows are 22 px.
8. Audio needs one tap to start (Chrome autoplay rule) on first run.

## Known deviations from the brief
- Landscape piano roll rows are **44 px**, so only five scale rows are visible and the roll scrolls vertically.
- On desktop, pointer-only density goes below 44 (26 to 36 px). Coarse pointers always get 44.

## Not designed yet
- Tablet and foldable widths (600 to 1024).
- Landscape Rack, Patch and Mod.
- A portrait automation editor (only landscape).
- Pattern rename, recolour, length; step velocity hold-and-slide as a drawing (it is in the spec text).
- Loop region editing, and a tempo map.
- Reduced-motion variants (specified, not drawn).
- The Creation Studio frame around the tab (header, navigation) and a project list.
- Sing on desktop (assumed: a side sheet reusing the phone frames).
- Sample library and audio recording of hardware inputs; MIDI mapping screens.
- Collaboration, sharing, cloud sync states beyond the revision badge.

## Known drawing gaps
- The Psy Bass and Kick panel headers show a back button but no undo/redo pair (every other phone header has one).
- The automation editor's Y-axis label `125 Hz` wraps onto two lines.

## Things the drawings do not show
- Keyboard focus order and shortcuts (see components.md and structure.md).
- Motion timing (only "two things move" is specified).
- Screen-reader announcements beyond the list in structure.md.
- Real audio: waveforms, meters and pitch traces are illustrative.

## Recommended before M4 starts
1. Confirm the assumptions above, especially 1, 2, 3 and the export cost.
2. Decide the `--cs-` prefix and how these tokens map onto existing Creation Studio variables. `text-dim` changes value (see token-notes.md).
3. Build the **Key**, **Tape**, **Knob** and **Fader** primitives first; most of the screens are made from them.

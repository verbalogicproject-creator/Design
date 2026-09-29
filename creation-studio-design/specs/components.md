# Synth tab: component specs

All sizes are CSS px at 1x. **Touch** means a coarse pointer (phone, tablet, touchscreen). Every interactive part is at least 44 x 44 on touch. On a fine pointer (mouse) dense parts may shrink to 26 to 36 px, never below 26. Focus is always `2px solid amber` with a 2px offset. Text is never smaller than 11px. Colour is never the only signal.

Token names refer to `tokens/tokens.json`. Screens named in **Seen in** are in `screens/`.

Common state rules (apply to every component below unless it says otherwise)

| State | Rule |
|---|---|
| Default | as drawn |
| Hover | lighten the part about 8 %, or a 2px pale halo on round parts. Pointer only. |
| Focus | amber focus ring. Keyboard and programmatic focus only (`:focus-visible`). |
| Pressed | key travels 2px down (`translateY(2px)`), shadow shortens from `key` to `key-pressed`. Lamps stay as they were. |
| Disabled | 42 % opacity, hatched fill on keys, `aria-disabled`. Value text becomes an em dash. |
| Modulated | green: knob gets a modulation ring, faders a green bar in the slot, toggles a green `~`, editors a dashed green ghost line and a `~ mod` badge. |
| Automated | white dot (9px, 2px seam ring) on the corner of the control, and `A` before the value on the tape. |

---

## 1. Knob
- **Sizes:** sm 44, md 60, lg 76 (diameter of the drawn dial). Hit area is at least 44 x 44 and includes the name above and the tape below.
- **Anatomy:** name (label style, above) then dial then value on tape (`tape-s`, below). Gap 5px.
- **Dial geometry (viewBox 64):** value track r=26, 4px stroke, seam colour, 270 degree sweep starting at 135 degrees. Value arc same path in `text`. Modulation ring r=30, 3px stroke, `cap-green`, plus a 3.4 radius dot for the live position. Cap r=18 `alu` with a 2px knurled ring (`alu-dim`, dash 1 1.4). Pointer 3.2px `ink` line.
- **Value mapping:** pointer angle = -135 + 2.7 x value(0 to 100) degrees.
- **Pointer input:** vertical drag, full range over 200px of travel. A second finger held anywhere on the panel switches to **fine mode**: one tenth the speed, tape shows one more decimal and the word `fine`. On desktop hold Shift.
- **Tap alternative:** tap opens **number entry**: a sheet or popover with the tape name, a field (focused, right-aligned, 20px bold, unit after it), `-` and `+` steppers (44 x 44), preset keys (Reset, Snap to note), Cancel and Set. Hold opens the knob menu (see 19).
- **Double tap:** reset to default.
- **Keyboard:** the dial is `role="slider"` with `aria-valuetext` equal to the tape text. Arrows step, Shift+arrows fine, PageUp/PageDown large steps, Home/End range ends, Enter opens number entry.
- **States:** default; hover halo; focus ring around the dial (offset 3); pressed = dial scales to 95 % and the tape grows to 115 % with an amber outline; disabled; modulated (ring); automated (dot + `A`).
- **Seen in:** Controls, PsyBass, KickPanel, Mod, Plugin.

## 2. Fader with stereo meter
- **Anatomy (left to right):** optional dB scale (24 wide) then track (44 wide) then meter (two 7px bars, 2px apart). Above: clip lamp row (14 high). Below: value on tape.
- **Track:** slot 6px wide, `well`, inset shadow. Cap 40 x 26, radius 3, channel colour, `key`-style shadow, a 2px centre groove (ink on light caps, white on dark). Cap travel = track height - 26.
- **Touch:** the cap is drawn 40 x 26 but the hit area is the full 44 wide track column, and dragging works from anywhere on it.
- **dB mapping (piecewise linear):** 0 = minus infinity, 20 = -24, 40 = -12, 60 = -6, 80 = 0 (unity), 100 = +6. Scale ticks at +6, 0, -6, -12, -24.
- **Meter:** bars fill from the bottom in `amber` with unlit segment lines (3px lit, 2px `well`). Peak hold is a 2px `text` line that falls after 2 s. Clip: the lamp turns `tally`, the word `CLIP` shows (idle text is `OK`), the top 5px of both bars go `tally`. Tap the lamp to clear.
- **Tap alternative:** tap the dB value on the tape opens number entry (dB, 0.5 dB steps). Double tap the cap = 0 dB.
- **Keyboard:** slider role, arrows 0.5 dB, Shift 0.1 dB, PageUp 3 dB, Home = -inf, End = +6.
- **States:** hover brightens cap; focus ring around cap; pressed = cap sinks and a tape tip shows the value; disabled; modulated = green bar in slot; automated = dot on cap; clipping.
- **Mini fader (Rack rows):** horizontal, 112 x 44, same geometry rotated, cap 22 x 28. The dB text beside it is a 44 px button (see number entry).
- **Seen in:** Controls, Mixer, LandMixer, MixerSingle, Rack.

## 3. Toggle and segmented buttons (aluminium keys)
- **Key:** min 44 x 44, padding 0 12, radius 4, `alu`, label 13px/700 upper, width 80 %. Shadow `key`.
- **Toggle:** a lamp (14 x 4, radius 2) sits 5px from the top edge. Off = `amber-off`. On = `amber` plus a 5px glow, key pressed in 2px. Record uses a `tally` lamp.
- **Segmented:** a `seam` tray (padding 4, radius 7) holding keys with 4px gaps. Exactly one key on: lamp lit and key pressed in.
- **Keyboard:** toggle is `button aria-pressed`. Segmented is `role="group"`; arrows move focus, Space or Enter selects.
- **States:** off, on, hover, focus, pressed, disabled (hatched), modulated (green `~` bottom right), automated (dot top left).
- **Seen in:** Controls, Transport, every tool row.

## 4. Envelope curve editor (AHDSR)
- **Canvas:** any size, default 320 x 150 (phone 354 x 84 to 372 x 96). 16px inner padding. Grid: 8 vertical lines, 3 horizontal, `seam`.
- **Geometry:** attack, hold, decay lengths are percentages of inner width. Sustain plateau is a fixed 20 %. Release is a percentage. Sustain level is the height of the decay end point. Fill under the curve `amber` at 12 %.
- **Handles:** 4 round (r=7 drawn, r=22 hit area = 44): attack end, hold end, decay end (also sets sustain level), release end. 3 diamonds (9px, rotated 45) on stages A, D and R for curve shape. Curve shape is -1 to +1: pull toward the corner for a fast start, away for a slow start. Hollow diamond = straight.
- **Under the canvas:** five tapes `A 2 ms`, `H 0 ms`, `D 180 ms`, `S 60%`, `R 260 ms`.
- **Tap alternative:** tap any handle: number field for its value with -/+ steppers and a Slow / Lin / Fast shape control.
- **Keyboard:** handles are focusable in order A, H, D, R. Arrows move. Tab moves to the next.
- **States:** selected handle has an amber ring (r=12); pressed shows guide lines and a value tape tip; disabled = dashed grey line and `bypass` badge; modulated = dashed green ghost path and `~ mod`; automated = dot and `auto`.
- **Seen in:** Editors, PsyBass, DeskInstrument, Plugin, Mod.

## 5. EQ curve
- **Canvas:** default 420 x 200. Log frequency axis 20 Hz to 20 kHz, gain axis +/-15 dB (labelled +12, 0, -12). Spectrum behind: `amber` at 22 % fill, 50 % outline. Curve: `text`, 2.6px.
- **Bands (5):** numbered dots (r=9, fill `slate-deep`, 3px ring in the band's cap colour, number 11px/800 in `text`). 1 = high-pass, 2 to 4 = bells, 5 = high shelf. Band colour is a second cue; the number is the first.
- **Hit area:** 44 x 44 per dot.
- **Tap alternative:** tap a dot for Freq, Gain and Q knobs (sm) with number entry. Dot selected: amber ring and crosshair.
- **Keyboard:** dots focusable 1 to 5; arrows move freq and gain, Shift+arrows fine, `[` and `]` change Q.
- **States:** as editors: disabled = flat dashed line and `bypass`; modulated = green ghost curve; automated = dot.
- **Seen in:** Editors, DeskMixer, Plugin.

## 6. XY pad
- **Size:** square, min 150. Grid at quarters. Puck r=10 (`alu`, 2.5px ink ring) with crosshair lines (`alu-dim` 1px). Hit r=22.
- **Under the pad:** two tapes `X Cutoff` and `Y Reso`.
- **Tap alternative:** tap anywhere jumps the puck. Two number fields (X, Y) via the tapes.
- **States:** focus/pressed ring r=17; pressed shows a `76 x 22` tape tip with both values; modulated = green dashed range rectangle plus `~ mod`; automated = dot and `auto`.
- **Seen in:** Controls.

## 7. Step-grid cell
- **Sizes:** touch 44 x 44 (two rows of 8 on a phone). Pointer 28 x 32 or 16 x 26. Preview (read-only) 9 x 16, 2px gap, 3px extra gap every 4th.
- **Anatomy:** off = `well` recess. On = `alu` key with lamp; an ink velocity bar 6px in from each side, height up to 28 % of the cell; accent adds a `>` mark and a 2px ink outline. Playing step: 3px `amber` bar under the cell.
- **Gestures:** tap toggle. Hold and slide up or down sets velocity (bar follows). Double tap sets accent.
- **Tap alternative:** **cell inspector**: Velocity `-` `+`, Accent toggle, Probability toggle.
- **Keyboard:** grid role; arrows move, Space toggles, `V` opens the inspector.
- **States:** off, on soft, on loud, accent, playing, hover/focus, pressed, disabled (dimmed when the channel is muted), modulated (green `%` = probability), automated (dot).
- **Seen in:** Controls, Rack, RackOpen (retired), Parts.

## 8. Pattern block
- **Size:** width = bars x bar width, height 44 (track row 52 to 56, block top offset 4 to 6).
- **Anatomy:** body fills with the channel colour, radius 4, `key`-style bottom shadow. Left and right grips 9px wide with ridges. **Name on tape** (`tape-s`) at left 12, top 3. **Kind tag** (MIDI / AUDIO / AUTO, 11px/800, `ink` chip with `tape` text) at the right. Preview area below: MIDI = ink note bars; audio = ink waveform; automation = ink curve with points.
- **Resize:** drag either grip. Hit area extends 22px beyond the block edge.
- **Tap alternative:** select a block, then a bar with Duplicate, Split, Make unique, Delete (Delete has a `tally` outline).
- **Linked copies:** a name ending `x4` means four blocks share one pattern. **Make unique** gives this block its own copy.
- **States:** default; selected (amber outline, grips darkened); pressed = lifted (`lifted` shadow, translateY -4) with a tape tip showing the bar it snaps to; muted = 50 % opacity, hatch, struck-through name; automated = the automation block itself. Modulated does not apply.
- **Seen in:** Playlist, LandPlaylist, DeskWorkspace, Editors.

## 9. Automation lane and clip
- **Layout:** the clip opens as a sheet over the playlist with a notch pointing at the block. Header: target parameter on tape, bar range chip, tool segmented Point / Pencil / Line, Snap, Done.
- **Canvas:** grid at 16ths (bars stronger). Y axis in the parameter's units. Points r=7. Curve `cap-yellow` (the channel colour) 3px, fill 12 %.
- **Point tools:** tap adds. Drag moves. Diamonds bend segments. Hollow diamond = straight.
- **Pencil:** freehand raw path (thin, `text` 55 %) snaps to a stepped line on the grid; lifting finishes. Options: Stepped / Smooth, Thin to points, Undo stroke.
- **Tap alternative:** select a point: Time and Value steppers, Curve Step / Lin / Bend, Delete.
- **Seen in:** AutoClip, AutoClipPencil, Editors, DeskInstrument.

## 10. Patch node, ports, cables
- **Node:** width 116 to 150, radius 6, `slate-lift` plate. Header 30px: name on tape, subtitle small. Rows 44px each: left port, label, right label, right port. Selected: amber outline.
- **Port:** 44 x 44 hit area with an 18px glyph: audio = circle (`amber`), notes = rounded square (`blue`), mod = diamond (`green`). Connected = filled glyph. 3px ring colour = type.
- **Cables:** audio 4.5px solid `amber`. Notes 3.5px dashed `9 5` `blue`. Mod 3.5px dotted `1 6` round caps `green`. Bezier with horizontal handles of max(50, half the distance).
- **Connect by tap:** tap an output (it rings), only compatible inputs stay lit (dashed ring) and others dim to 35 %, tap one to connect. Drag also works. A banner names the armed source with Cancel.
- **Cable states:** default; selected (`text` halo 5px wider); dragging (10 6 dash); bypassed (40 % and a red x).
- **Seen in:** Patch, DeskPatch, Objects.

## 11. Macro knob
- One knob (lg) plus **target chips** (min 44 high): parameter name, a range bar (8px, `well`, green fill from min to max), and `20-70`. Tap a chip to edit: two range handles (28 x 32) plus Min and Max steppers, Invert, Remove. `+ Add target` at the end.
- States: no targets = disabled; modulated (an LFO moves the macro); automated.
- **Seen in:** Mod, DeskWorkspace, Objects.

## 12. Take card
- **Layout:** tape title and duration; kind badge; mini waveform (40 high, notes in `blue-text`); segmented **Voice / Instrument** (or Original / In project key); confidence badge (`High` green, `Medium` amber) with its note; a warning line with an icon; **Use this take** (or `In project, tap to undo`) and Discard (tally outline).
- Chosen: 2px amber outline.
- **Seen in:** Objects, Sing, Generate.

## 13. Cost and consent chip
- **Cost chip:** min 44 high, padding 0 14, 2px `tally` border, radius 6, `slate-deep` fill, `text` label, a 20px coin (2px `tally-text` ring, `$`), and the price in `tally-text`. Label is the verb: `Generate . $0.08`.
- **States:** hover (warmer fill), focus, pressed (sinks, charges on release), disabled (dashed border, reason shown). **Free actions never use the chip.**
- **Consent sheet:** bottom sheet with what is sent (words), to whom, what is kept (link to provider terms), a required checkbox that everyone heard has agreed, an Ask-every-time toggle, `Keep on device` and the cost chip. Shown every time for voice or family audio.
- **Seen in:** Objects, Consent, Generate, SingReview.

## 14. Toasts and states
- **Toast:** plate, radius 6, icon tile 28 x 28, bold title, one plain sentence telling what to do, one button. Kinds: error (`tally` border), warning (`amber` border), success, info.
- **Rules:** say what happened, then what to do. Never an error code as the message. If money is involved say whether the user was charged.
- **Empty state:** dashed 2px `alu-dim` box, tape headline, one sentence, one key.
- **Loading:** progress bar (10px, `amber` segments), text with time left, Cancel.
- **Revision badge:** `saved r12` (check), `unsaved . r12` (dashed amber border), `saving...`, `not saved . retry` (tally border).
- **Audio lost:** `amber` key, warning icon, `LOST x2`, only present after a dropout. Tap for details, hold to clear.
- **Seen in:** Objects, Transport.

---

## Additional components

### 15. Transport bar
Phone: 106 high, two rows, grid `104px 84px 1fr` then buttons and Panic. Wide: 56 high, one row (buttons, position, tempo, CPU, then Panic at the right). Buttons 44 x 44 (Play, Stop, Record, Loop). Panic is a `slate-deep` key with a 2px `tally` outline, octagon-x icon and the word. LCD: `well`, amber 20px numerals, 11px label. CPU: ten 6px segments, tally from 90 %. **Tempo** LCD is a button: tap opens the tempo sheet.

### 16. View navigation
Same seven items everywhere (Rack, Playlist, Mixer, Roll, Patch, Mod, Sing). Bottom bar 60 high on portrait; left rail 60 wide on landscape; top tabs 48 high on desktop. Active = pressed plate with a lamp bar and `aria-current="page"`. Sing carries a tape label.

### 17. Mixer strip
Number, name on tape, optional badge (`duck -3 dB`, `-> Drums`, `group`, `return`), pan knob (sm) or a pan key, M and S keys (44 x 44), fader, FX slot count. Layouts: column (M/S stacked or side by side), side (controls left of fader). Selected strip: 3px amber top edge.

### 18. Insert slot (8 per strip)
2 x 4 grid of buttons, min 44 high. Number, name, and for sidechain `<- Kick`. Empty = dashed `alu-dim` with `+ Add`. Bypassed = 60 % opacity and struck-through name. **Duck indicator:** down-arrow, text `Ducked by Kick -3 dB`, gain-reduction meter (10 high). The number is always shown, never only the bar.

### 19. Knob menu
Sheet from a held knob: knob and tape name, then rows (min 52 high): **Create automation clip** (primary aluminium row), Add modulation, Map to macro, MIDI learn, then Type a value and Reset. Each row has a title and a one-line explanation.

### 20. Sections, minimap
Section flags: tape label with a padlock (closed and the word `LOCKED` when locked). Drawn 26 high, hit area 44 high extending into the ruler. Sticky flag with a left arrow when the section starts off-screen. **Minimap:** 30 high `well`, one thin line per lane in channel colour, section names, amber viewport box, playhead, loop tick. Tap jumps, drag the box scrolls. **Locked** = marker and blocks inside stay put when bars are inserted or deleted.

### 21. Roll notes
Note: channel colour fill, 2px ink edge, grips. **Unsure** (from singing): dashed ink edge, diagonal hatch, `?` badge (16px). **Ghost** (other channels): outline only in that channel's colour with the channel number inside. Selected: amber ring, tape with confidence (`A2 . 62 % sure`). Roll rows: 44 px on touch (via Fold to scale), 22 px on desktop.
**Note inspector** (tap alternative): Pitch, Start, Length, Velocity, each with `-` `+`.

### 22. Sing parts
Step indicator (four segments, done grey, current amber, label 11px). Big Record: 132 round `alu` key with a 64 `tally` dot (ready), a `Stop` key while recording, grey dot when the mic is blocked. Option card (radio, title, one line). Beat lamps (52 x 44, amber when current). Horizontal meter with a peak tick, a red zone at 90 % and the words `quiet`, `good`, `hot`, `CLIP`.

### 23. Bottom sheets
Radius `10 10 0 0`, top edge 2px `alu-dim`, `sheet` shadow, grab handle 44 x 5. Over a 62 % scrim (`rgba(16,22,26,.62)`). Close button 44 x 44 at the top right. Sheets never stack.


### 24. Morph (scene morph pad): the performance feature
**What it is:** four stored sounds ("scenes") of one instrument sit on the corners of a square pad: A Rolling, B Acid, C Open, D Scream. The puck's position blends all four; **every knob on the instrument follows live**. It is the psytrance build-up tool: one thumb takes a rolling bass from dark to screaming over 16 bars.

**Maths:** bilinear weights from puck x, y (0 to 1, y up): A = (1-x)(1-y), B = x(1-y), C = (1-x)y, D = xy. Each parameter = sum of scene value x weight. Stepped parameters (filter type, waveform switch) take the scene with the largest weight.

**Layout (phone portrait):** title row (Morph tape, one-line hint, **Ride** key); pad 396 x 272 (`well`, grid at thirds, faint diagonals); corner tapes are **buttons** (44 x 44 min) with the scene name and live weight %; the heavier scene's % turns amber; weight bars (A B C D, 6 px) under the pad; the instrument's knobs (sm) below; Glide segmented control; one line of help.

**Puck:** 28 px `alu` disc, 3 px ink ring, 3 px amber outer ring. Soft amber halo (r 22, r 30 while dragging).

**Knobs:** each shows its **range across the four scenes as the green ring**, so you can see which knobs a morph will move and how far before you touch the pad.

**Gestures and alternatives**
- Drag anywhere on the pad (pointer capture, `touch-action: none`).
- **Tap a corner** = glide to that scene over the Glide time: Jump, 1 beat, 1 bar, 4 bars (tempo-synced, ease-in-out).
- **Hold a corner** = store the current sound in that scene (confirm toast with Undo).
- Keyboard: pad is focusable; arrows move 5 % (Shift 1 %); keys 1 to 4 glide to A to D. `aria-label` announces the weights.
- Reduced motion: glides jump; nothing else animates.

**Ride (record):** a toggle with a tally lamp. While on, the transport record lamp lights, a `● RIDING · writing automation` badge shows, and the puck path is written as automation (one clip per changed parameter, or a single "Morph X/Y" pair). The trail is solid amber while riding, dotted when not.

**States:** idle; dragging (bigger halo); gliding (puck animates); riding (tally); disabled (instrument frozen: pad dimmed, reason shown).

**Seen in:** `screens/Morph.png`, `source/Morph.dc.html` (a working prototype: open the live canvas and use Play).


---

## Lite profile additions (see `lite.md`)

### 25. Profile badge (new state of a label, not a new control)
A 44 x 44 button holding a text badge: `LITE` (2px `alu-dim` outline) or `FULL` (2px `amber` outline, amber text), 11px/800, tracking .1em. Sits left of the project name in the phone header, replacing the "Synth" title. Tap opens "What's in Lite". Optional `frz-tag` beside it: "4 frozen". States: default, focus (amber ring), pressed (well fill).

### 26. Drum pad (new component)
**Why new:** nothing existing plays on touch-down while also showing a sample name, a choke group and a live hit lamp. Keys (on-screen keys) are pitched and have no per-pad settings; step cells are toggles, not triggers.
- **Size:** 4 per row on a phone: about 94 x 88. Min 44 x 44 in any layout.
- **Anatomy:** aluminium key (`alu`, `key` shadow, radius 6), 6px channel-colour stripe on top, lamp top-right (14 x 4), name (13px/800 upper, ink), optional choke badge (`ink` chip, `tape` text), sample file name (11px).
- **Play:** plays on pointer-down. Velocity from the vertical hit position (top louder). Multi-touch: every finger is a hit.
- **Select:** hold selects without playing; the selected pad's settings show below (tune, decay, level, pan, choke).
- **Keyboard:** Space plays, Enter selects, arrows move between pads.
- **States:** default; hit (key sinks, amber inner rim, lit lamp); selected (amber outline); hover/focus (amber ring); empty (dashed `alu-dim`, "+ Load sample"); disabled (42 %, the reason replaces the sample name).

### 27. Frozen state (new state on rows, blocks and strips)
- **Row:** diagonal hatch over the row, `frz-tag` (snowflake + FROZEN, 11px/800, `well` fill, `alu-dim` border). Mix controls stay live.
- **Pattern block:** hatch, kind tag reads `FROZEN`, preview is the audio waveform.
- **Freezing:** progress bar plus "bar 30 of 64".
- **Unfreeze:** a button with the CPU cost in words; Lite instruments only.
- **From Full:** "edit on desktop" sentence instead of a dead button; other Full-only features get a named tag (`Morph off`).

### 28. Budget line and voice meter (reuse)
Budget: horizontal meter (22) with a text summary, whole line is a 44 px button opening details. Voice meter: CPU segment style (15), 10 x 14 segments, the stolen voice outlined in ink.

### 29. Lite navigation (reuse)
The same nav component with `profile="lite"`: Rack, Playlist, Mixer, Roll, Drums, Keys. Patch, Mod and Sing are Full only (Sing: see open questions).

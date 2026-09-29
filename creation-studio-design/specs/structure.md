# Structure, behaviour and a suggested component map

You rebuild this in React with your own CSS variables, so this file is about **structure**: what regions exist, how they change with size, and what state each part needs.

## 1. App shell (all sizes)
```
SynthTab
  Header            project name (tape), undo, redo, revision badge   (phone 44 high)
  Transport         always visible: position, tempo, CPU, audio-lost, Play, Stop, Record, Loop, Panic
  ViewArea          one of Rack | Playlist | Mixer | Roll | Patch | Mod   (desktop can show two)
  ViewNav           Rack Playlist Mixer Roll Patch Mod + Sing
  Sheets / Popovers  bottom sheets, drawers, menus (never stacked)
```
Views are equal peers. **Sing** is not a view: it opens a four-step flow (Ready, Record, Review, Use as) and returns to the previous view.

## 2. Responsive rules
| Width | Nav | Transport | Notes |
|---|---|---|---|
| Phone portrait, 412 | bottom bar, 60 high, 7 items | two rows, 106 high | one view at a time, sheets from below, no horizontal page scroll |
| Phone landscape, 892 x 412 | left rail, 60 wide | one row, 56 high | Playlist, Roll and Mixer get the full width. Header is hidden, revision badge moves into the tool row. |
| Desktop, 1440 | top tabs, 48 high | one row, 56 high | side panels docked (Rack left, Inspector right), dock at the bottom. Two views at once. |
| Plugin, 900 x 600 | none | none | a single module faceplate |

Only the **timeline, piano roll and patch canvases** pan (and zoom). Everything else wraps or pages (mixer banks, strip selector).

Pointer density: on a coarse pointer every target is 44. On a fine pointer, step cells, note rows and slots may shrink (26 to 36). Choose with `@media (pointer: coarse)`, not by width.

## 3. Views and their regions
- **Rack:** ChannelList (rows), Add module sheet. Row = cap, number, name tape, instrument, M, S, volume mini-fader + dB button, 16-step preview. Tap row opens that channel's **InstrumentPanel**.
- **InstrumentPanel:** faceplate plates: Osc, Filter, Amp env, Glide/Drive/Output (Psy Bass); Kick has waveform preview and six knobs. Preset chip opens preset sheet.
- **Playlist:** ToolRow, Minimap, SectionsRow, Ruler, TrackHeaders + Lanes (blocks), PatternDrawer or tray, BlockActions.
- **Mixer:** StripBank + pinned Master; SelectedStripPanel (inserts, sends, pan, duck) in landscape and desktop; OneStrip mode on portrait phone with a Channels / Buses / Master selector.
- **Roll:** ToolRow, Keyboard, Grid (notes, ghost notes, playhead), VelocityLane, NoteInspector.
- **Patch:** ModuleBrowser (desktop), Canvas (nodes + cables), ConnectBanner.
- **Mod:** Macro plate, LFO, Env, Routes.
- **Sing:** step indicator + one of the four step screens.
- **Automation clip:** a sheet over the Playlist attached to one block.

## 4. Data the UI needs (names are suggestions)
```
Project { id, name, revision, dirty, tempo, meter, key, sections[], loop{from,to} }
Section { name, fromBar, toBar, locked }
Channel { id, number, name, color(red|yellow|blue|white|green), instrument, mute, solo, volume, pan, sends{busId: db}, inserts[8], group, steps? }
Instrument { type(kick|psyBass|polySynth|sampler|audioPlayer), preset, params{}, mods[], automations[] }
Pattern { id, kind(midi|audio|automation), name, length, notes[]|audio|points[] , usedBy[] }
Block { id, patternId, channelId, fromBar, bars, muted }   // linked copies share patternId; makeUnique clones the pattern
Note { pitch, start, length, velocity, uncertain?, confidence? }
Route { source, target, amount }  // modulation; Macro { id, targets[{param,min,max}] }
Take { id, kind(sing|ai), duration, confidence, notes[], audioRef, chosen }
Cost { action, priceUsd, sendsUserAudio }  // drives the price chip and the consent sheet
```
Two derived facts drive several states: `lost` (audio dropouts, shows the amber Lost key), `duckedBy` (sidechain reduction in dB).

## 5. Interaction model
- **Every drag has a tap alternative** (number entry, steppers, inspectors, tap A then B).
- **Tap** selects or toggles. **Hold** opens a menu (knobs) or starts a move (blocks, flags). **Double tap** resets (knob, fader) or sets accent (step cell).
- **Two fingers** on a canvas pans and pinch-zooms. A **second finger held** while dragging a knob = fine mode.
- **Undo and redo** apply to every edit in the project, including automation strokes. Revision numbers only advance when a change settles (autosave).
- **Panic** stops all sound immediately. It is never disabled.
- **Costs:** any action with a price shows the cost chip and needs its own clear tap. If it would send the user's own voice or family audio, the consent sheet is shown first, every time.
- **Locked section:** marker and blocks inside stay in place when bars are inserted or deleted (position lock only).

## 6. Suggested React component map
```
<SynthTab>
  <Header/> <Transport mode="phone|wide"/> <ViewNav mode="bottom|rail|top"/>
  <RackView/>        <ChannelRow/> <MiniFader/> <StepPreview/> <AddModuleSheet/>
  <InstrumentPanel/> <Plate/> <Knob/> <Segmented/> <EnvelopeEditor/> <WavePreview/>
  <PlaylistView/>    <Minimap/> <SectionFlag/> <Ruler/> <TrackHeader/> <PatternBlock kind/> <PatternDrawer/> <BlockActions/>
  <MixerView/>       <Strip layout="col|side"/> <Fader/> <Meter/> <InsertSlots/> <SendKnobs/> <DuckBadge/> <BankSwitch/> <StripSelector/>
  <RollView/>        <Keyboard/> <Grid/> <Note kind="normal|unsure|ghost"/> <VelocityLane/> <NoteInspector/>
  <PatchView/>       <Node/> <Port type/> <Cable type state/> <ConnectBanner/>
  <ModView/>         <MacroKnob/> <TargetChip/> <Lfo/> <Routes/>
  <SingFlow/>        <StepIndicator/> <BigRecord/> <BeatLamps/> <LiveTrace/> <TakeCard/> <TimingSlider/> <UseAsOptions/>
  <AutomationClip/>  <LaneCanvas/> <PointInspector/>
  <Sheet/> <Popover/> <Toast/> <CostChip/> <ConsentSheet/> <NumberEntry/> <RevisionBadge/>
```
Shared primitives: `Plate`, `Well`, `Key` (button with lamp), `Tape` (label), `Lamp`, `Segmented`, `Slider` (real `<input type=range>` underneath), `NumberEntry`.

## 7. Accessibility
- Knobs, faders, sliders: real `role="slider"` or an `<input type=range>` under the drawing, `aria-valuetext` = the tape text.
- Toggles: `button` with `aria-pressed`. Segmented: `role="group"`; nav uses `aria-current="page"`.
- Focus ring is amber, 2px, offset 2px, on everything. Never remove it.
- Canvases (timeline, roll, patch, envelope, EQ) need a non-pointer path: arrow keys move the selected item, and each item has an inspector with steppers.
- Live regions: toasts are `role="status"`, errors `role="alert"`. The audio-lost key announces `Audio dropped out N times`.
- Reduced motion: stop the recording lamp pulse, the rendering pulse and any zoom animation. The playhead still moves because it is the state.
- Contrast: body 9.0:1, secondary text 5.9:1 on the page (5.0:1 on plates). See the tokens file for pairs.

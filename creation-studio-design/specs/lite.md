# Lite profile: phone add-on

An add-on to the Synth tab design, not a redesign. Tokens (`--cs-*`), the console-and-scribble-strip identity, every component in `components.md` and the hard rules are unchanged. Lite screens are built from existing components; the only new component is the **drum pad** (26). New *states* are the **frozen** state (27) and the **profile badge** (25).

## Profiles
| | Lite | Full |
|---|---|---|
| Device | Android phone, Chrome, 412 portrait (892 x 412 landscape for the drum grid) | Desktop, 1440 |
| Nav | Rack · Playlist · Mixer · Roll · **Drums** · **Keys** | Rack · Playlist · Mixer · Roll · Patch · Mod · Sing |
| Header | `LITE` badge replaces the "Synth" title, left of the project name | `FULL` badge (amber) |
| Song format | same | same |

A Lite song opens in Full unchanged. A Full song opens in Lite with its heavy parts **frozen** (played as audio), and a sheet says which and why (`FromFull`).

## The Lite set, and where each item is designed
| Lite item | Screen | Built from |
|---|---|---|
| Tracks overview, budget | `LiteRack` | Rack rows (Main), mini-fader, dB button, step preview, horizontal meter (budget) |
| Add a track (only Lite items; Full items listed as "In Full, on desktop") | `LiteAdd` | Add-module sheet cards; Full-only cards dashed with a `FULL` badge |
| Kick | `KickPanel` (existing) | unchanged |
| **Drum Kit**: snare, closed hat, open hat, crash, ride, clap + 2 free pads | `DrumKit` | **drum pad (new)**, knobs (Tune, Decay, Filter, Level, Pan), waveform well with start/end handles + steppers, toggle + segmented (choke group), "Change sample" key |
| **Drum step grid** | `DrumGrid` (portrait: every row, 8 steps per page at 44 px, fill menu open), `LandDrumGrid` (landscape: every row x 16 steps at 44 px, velocity popover open) | step cells (7) with off / on / accent, mute key, fill menu (popover), segmented (bars 1-4, length 1/2/4, steps 1-8 / 9-16), swing |
| Psy Bass | `PsyBass` (existing) | unchanged |
| **Lite Synth**: 2 oscillators, filter, AHDSR, 1 LFO, 6 voices | `LiteSynth` | knobs, segmented (waveforms, filter type, LFO target), envelope editor (4), voice meter (CPU-segment style) |
| Lyria takes as audio tracks | `Generate` (existing), audio row in `LiteRack` | unchanged |
| Per track: volume, pan, mute, solo, 3-band EQ, reverb send, delay send, duck from kick | `LiteStrip` | one-strip mixer (MixerSingle), fader + meter, knobs, duck indicator (18). No insert slots: the Lite chain is fixed. |
| Shared reverb, delay, master limiter | `LiteShared` | knobs, segmented (delay time), LCD readouts, gain-reduction meter |
| Piano roll, playlist, on-screen keys | `Roll`, `Playlist`, `Keys` (existing) | unchanged; nav reaches Keys directly |
| CPU meter | Transport (existing) + budget line in `LiteRack` | CPU segment meter, horizontal meter |
| Voice limits | voice meter in `LiteSynth`; drum pad note | CPU segment style |
| Freeze | `Freeze` sheet, frozen rows in `LiteRack`, `Overload` (existing) | sheet, horizontal meter, frozen state (27) |
| Opening a Full song | `FromFull` | sheet, frozen state tags |

## Rules specific to Lite
- **Fixed chain per track:** EQ (3 band) then reverb send, delay send, then duck from Kick. Nothing can be inserted, so there is no insert grid and no effects group in Add track.
- **One reverb, one delay** for the song, each shown once in the mixer (`Reverb · Delay` tab).
- **Lite Synth limit:** 2 tracks (the Add card shows "1 of 2 left"; at 0 it is disabled with the reason "Phone limit: 2 Lite Synths. Freeze one to add another.").
- **Voices:** 6 per Lite Synth. When a 7th note starts, the oldest stops; the voice meter outlines the stolen voice in ink and says "oldest note stops".
- **Freeze:** records a track to audio. Mix controls (volume, pan, EQ, sends, duck) stay live; notes and sound are locked. Lite instruments can be unfrozen if the CPU allows (the cost is shown first). Parts from Full cannot be unfrozen on the phone: "edit on desktop".
- **Full-only features in a Full song:** frozen with a tag. Morph becomes `Morph off` (the recorded automation still plays).
- **Budget line** (Rack): CPU %, Lite Synths used, frozen count. At "busy" (about 75 %) it suggests freezing; at "full" (about 90 %) the Overload sheet opens. Thresholds are placeholders until measured.
- **Drum Kit panel:** 8 pads in a 4 x 2 grid, name on tape and channel colour stripe. Tap plays **and** selects. States: playing (lit lamp), selected (amber outline), muted (hatch + `M · MUTED`), empty (dashed, "+ Choose sample"), sample missing (amber rim, warning icon, words). The selected pad shows its waveform with **start (S) and end (E) handles** (44 px wide hit areas; `Start` / `End` steppers are the tap alternative), then Tune, Decay, Filter, Level, Pan, and a **Choke group** toggle with A/B: pads in the same group cut each other (closed hat cuts open hat). **Change sample** opens the sample picker (screen 3, not drawn yet).
- **Drum step grid:** one row per kit pad plus a **Kick** row (from the Kick track), 16 steps per bar, bars paged 1-4, pattern length 1, 2 or 4 bars, swing for the grid.
  - **Step:** tap cycles off, on, accent (accent = `>` mark and ink outline, as in the step cell). Hold opens the velocity popover (velocity slider with -/+ and Off/On/Acc); per-step settings will be added there later.
  - **Per row:** name tape, **M** (mute; the row's cells dim), and a **fill** key that opens a menu: Every 2nd step, Every 4th step, Off-beats, Clear row (tally text and trash icon, because it removes notes). Fills apply to the bar shown.
  - **Playhead:** the playing column is tinted amber with amber edges (landscape) or each playing cell is tinted (portrait), plus the step cell's own amber bar.
  - **Portrait:** 16 steps at 44 px do not fit in 412 px, so each row shows 8 steps with a `1-8 / 9-16` switch, and a read-only 16-step preview in the row header shows the whole bar. Rows scroll vertically.
  - **Landscape:** every row shows all 16 steps at 44 px (1 px gaps, a darker line every 4 steps). This screen is a focused editor: the nav rail is hidden to fit the width, and a back key returns to Drums. The 7th row scrolls into view.

## Accessibility notes
- Drum pads are `button`s: Space plays, Enter selects. Pads play on pointer-down; velocity from vertical hit position, with the pad settings as the tap alternative (Level knob).
- Frozen is never colour-only: snowflake icon, the word FROZEN, and a hatch.
- The profile badge is text (`LITE`/`FULL`), with a different outline colour only as a second cue.

## Numbers that are placeholders
Drum Kit hits at once (8), budget thresholds (75 %, 90 %), CPU freed by freezing (14 %), Full-song CPU (180 %). The brief gave: 6 voices, one or two Lite Synth tracks.

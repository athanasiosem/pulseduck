# PulseDuck

A stereo rhythmic volume shaper in JSFX format, with tempo synchronization,
audio and MIDI triggering, and optional low-band ducking.

## Features

- 16 built-in curves and a nine-node custom curve editor.
- Adjustable mix, curve duration, phase advance and gain smoothing.
- Tempo-synchronized cycles from a sixteenth note to eight beats.
- Audio sidechain triggering from input channels 3/4.
- Sample-offset MIDI note triggering with an optional note filter.
- Full-band or low-band-only processing.
- Stereo-linked gain and smoothed bypass.
- No external libraries, audio assets or intentional processing delay.

## Installation

1. Download `PulseDuck.jsfx`.
2. Open your JSFX host's resource directory and copy the file into `Effects`.
3. Refresh its effect list and insert **PulseDuck** on the signal to be ducked.

## Quick start

Select **Tempo Sync**, **1/4**, and **Deep Pocket**. Adjust Mix to taste.
For low-frequency ducking, select **Low Band Only** and set the cutoff.

For audio triggering, route the trigger signal to this effect's channels 3/4,
keep the main signal on channels 1/2, and select **Audio Trigger 3/4**.
Start with a threshold of -24 dBFS and an 80 ms holdoff, then adjust for your signal.
Hits during holdoff are discarded rather than delayed. The detector must fall
6 dB below threshold before it re-arms. Sustained sidechain signals may prevent
re-arming. Sidechain audio is not mixed into the main output.

For MIDI triggering, deliver note-on events to the effect and select
**MIDI Note Trigger**. Choose a note number or use -1 for all notes.
MIDI passes through; zero-velocity note-ons do not trigger ducking.

## Timing and curves

- **Cycle length:** a quarter note is one beat; bar labels mean four/eight beats.
- **Duration:** 25–200% in triggered modes. New triggers restart the curve.
  Tempo Sync caps duration at 100% to preserve the selected beat interval;
  choose a longer cycle for slower ducking.
- **Phase advance:** shifts the curve ahead within the cycle in Sync mode.
- **Smoothing:** reduces abrupt gain transitions; it also softens the initial dip.
- **Stopped transport:** ducking is bypassed by default; choose Keep Cycling
  for live monitoring. Output gain still applies.

Click and drag inside the graph to edit nine equally spaced gain nodes.
Editing a built-in curve converts it to an approximate custom curve. Choose
another built-in curve to leave Custom, or choose Custom to recall your edits.
The graph displays the raw shape before mix, duration scaling and smoothing.
The amber marker shows the scaled playback position.

Custom curves return to unity when a triggered or shortened cycle finishes.
Set the last node to 100% for a smooth endpoint. Parameters and custom nodes
are stored through the host's normal preset and project facilities.

## Low-band processing

The split uses a gentle one-pole low-pass and a complementary high-frequency
residual. It recombines to the dry signal at unity gain. This is a gradual
6 dB/octave split, not a steep frequency boundary. Partial ducking alters the
frequency and phase response; transient peaks can change. No limiter is included.

## Validation status

Source structure and mathematical DSP checks have been performed, including
404 numerical timing/state regression cases for the 1.1 changes. These checks
simulate the equations; they are not execution of the JSFX source in a host.
Host compilation, GUI interaction, project recall and audible playback remain
unverified. Gain smoothing introduces settling time, and audio triggering has
no lookahead, so the start of a transient can pass before full attenuation.

## License

MIT — see [LICENSE](LICENSE).

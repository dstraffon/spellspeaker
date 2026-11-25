# spellspeaker
Wavetable tool

**Spell Speaker² — Defragmenter Enhancement Suite** is an imaginary hardware synth box that actually works. Each knob reshapes the waveform on the scope, and when you flip the switch on it plays what you see.

![Spell Speaker design](design.png)

The panel started as a Figma mockup. This repo is that mockup rebuilt as a single self-contained web page: plain HTML, SVG, Canvas and the Web Audio API, with no build step and no dependencies.

## Run it

Open `index.html` in a modern browser. That's it. Sound starts when you flip the power switch, since browsers only allow audio after a click.

To serve it locally instead: `python3 -m http.server` and visit `http://localhost:8000`.

## Controls

| Control | What it does |
| --- | --- |
| **Power switch** | Starts the scope moving, the cables swaying and the sound playing. Dark when off, orange when on |
| **Dice button** (or press **R**) | Rolls random settings: every knob sweeps to a new value and a few slots get cast |
| **Blue knob** (amplitude) | Height of the wave and how loud it is |
| **Orange knob** (frequency) | Cycles on screen and the pitch. 35% = one cycle at A2 (110 Hz) |
| **Slots 1–14** | Click to add a harmonic (slot *n* adds harmonic ×*n*+1). Drag a slot to set its strength |
| **RUST** | Grit and static |
| **EARTH** | Sub-octave weight. Off at 50%, flipped either side of it |
| **AIR** | A high, shimmering overtone |
| **DUST** | Phosphor trails on the scope and a muffled sound |
| **WIDTH** | Stereo detune, drawn as a ghost trace |
| **FLUX** | Drift and vibrato. Still at 50%, drifts left or right from there |
| **BURN** | Saturation that squares the wave off |
| **Tuner** | Bottom-left of the scope: note, cents meter and exact pitch. Green within ±5¢ |

**Interaction:** drag or scroll any knob, hold Shift for fine control, double-click to reset. Every control also works from the keyboard: Tab to focus, arrow keys to adjust, Home and End for the extremes, Enter or Space to toggle a slot.

## Play it like a keyboard

Click the page once so it hears your keys, then use your computer keyboard as a little MIDI keyboard:

```
  W E   T Y U   O        ← sharps
 A S D F G H J K L       ← C D E F G A B C D
Z X                      ← octave down / up
```

- It starts at C3 and can go from C1 to C5. A small label on the scope shows the octave and every note sounding.
- **Up to 5 notes at once.** Hold keys to play chords; each note fades in and out on its own. A 6th key cuts off the oldest note.
- **Power off:** notes sound only while you hold them. **Power on:** the last chord you played keeps ringing after you let go (latched), and the next thing you play replaces it. With nothing played yet, it's the plain drone.
- Chords stay at about the same loudness as a single note. BURN works on the whole mix, so pushing it on a chord gets crunchy.
- The frequency knob and tuner follow your newest note. Turning the knob by hand shifts the whole chord; the next phrase starts back in tune.
- Some laptop keyboards can't register certain 3+ key combinations at once (ghosting). If a chord drops a note, that's the keyboard hardware.

## How it works

- **Scope:** a Canvas redraws the signal every frame. DUST sets how quickly old frames fade, which is where the trails come from.
- **Audio:** an AudioWorklet runs the synth on the browser's audio thread, so drawing never interrupts the sound. It has 8 voice slots (5 playable at once, the rest let released notes fade), each with its own pitch and envelope. Parameter changes glide sample by sample to avoid zipper noise.
- **Panel:** inline SVG, laid out at the Figma frame's proportions and scaled to fit the window.

## Credits

Design by Dylan Straffon. Built with Claude.

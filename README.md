# Fork Information
## What this Fork Changes
- removed Y-T mode
- set default color to amber
- moved gain to be a set value in configuration
### THIS IS INTENDED TO BE USED IN A WALLPAPER, IF YOU WANT A GENERAL OSCILLOSCOPE, PLEASE USE THE ORIGINAL SOFTWARE
## Quick Install
if you're here, you're probably coming from my wallpaper, in which case you just want a fairly quick installation

Requires [Nim](https://nim-lang.org/) 2.x.
```bash
git clone https://github.com/loglot/terminal-oscilloscope.git
cd terminal-oscilloscope
nim c -d:release --threads:on -o:osc-bg src/osc.nim
sudo cp ./osc-bg /bin/osc-bg
```

# terminal-oscilloscope

A terminal-based oscilloscope with CRT phosphor physics, written in Nim. Zero dependencies — 200KB binary, just libc.

## Braille renderer (amber palette)

![braille demo](braille_demo.gif)

## Half-block renderer

![demo](demo.gif)

## Features

- **CRT boot/shutdown animations** — phosphor ramp, beam sweep, vertical collapse, dot fade
- **Y-T and X-Y modes** — time-domain waveform or Lissajous figures
- **Phosphor persistence** — beam bloom, decay trails, intensity-based shading
- **Two renderers** — half-block (`▀▄█`) or braille dots for 4× resolution
- **Live audio capture** — direct libav bindings via dlopen, zero install
- **Threaded audio** — 60fps rendering, audio capture on separate thread
- **6 CRT phosphor palettes** — green, amber, cyan, blue, white, red

## Install

Requires [Nim](https://nim-lang.org/) 2.x.

```bash
git clone https://github.com/rolandnsharp/terminal-oscilloscope.git
cd terminal-oscilloscope
```

**Half-block version** (chunky CRT look):
```bash
nim c -d:release --threads:on -o:osc src/osc.nim
./osc
```

**Braille version** (high-resolution dots):
```bash
nim c -d:release --threads:on -o:osc_braille src/osc_braille.nim
./osc_braille
```

**Install globally:**
```bash
sudo ln -s $(pwd)/osc /usr/local/bin/osc
```

## Controls

| Key | Action |
|-----|--------|
| `]` / `[` | Zoom in / out time axis |
| `q` | Quit (with CRT shutdown effect) |

## Configuration

Edit the constants at the top of `src/osc.nim` or `src/osc_braille.nim`:

```nim
const
  # Phosphor physics
  Decay = 0.85           # persistence per frame (0.0–1.0)
  Beam = 0.4             # intensity at beam impact
  Bloom = 0.08           # horizontal glow spread

  # Phosphor glow thresholds
  HotGlow = 0.7          # white-hot beam core
  WarmGlow = 0.4         # bright phosphor
  CoolGlow = 0.15        # dim persistence trail

  # Palette: green, amber, cyan, blue, white, red
  Palette = "green"
```

### Palettes

| Name | Phosphor | Look |
|------|----------|------|
| `green` | P31 | classic oscilloscope |
| `amber` | P12 | warm retro terminal |
| `cyan` | P7 | Tektronix blue-green |
| `blue` | P11 | cool/modern |
| `white` | P4 | TV phosphor |
| `red` | P22-R | radar display |

## Audio

Captures system audio by opening the PulseAudio/PipeWire monitor of your default output sink directly via libavformat and libavdevice. Libraries are loaded at runtime with `dlopen` — no dev packages, no subprocess, no extra dependencies.

## Credits

CRT turn-on/off animations inspired by [AetherTune](https://github.com/nevermore23274/AetherTune).

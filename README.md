![gif](gif.gif)

# gloam

An internet radio and music player for the terminal, with a visualiser based on the animated background of [lpx.org.uk](https://lpx.org.uk).

It plays internet radio (Icecast, SHOUTcast and HLS streams, `.pls` and `.m3u` links) and any local audio file ffmpeg can read, and draws a shader in the terminal that reacts to the sound.

## Quick start

### Homebrew (macOS or Linux)

```sh
brew install londonpixelexchange/tap/gloam
gloam https://ice1.somafm.com/groovesalad-128-mp3
```

Homebrew handles ffmpeg, NumPy and the audio dependencies; no setup or environment
activation is needed.

### From source

Install Python 3.8+ and ffmpeg, then run setup once from this directory:

```sh
brew install python ffmpeg  # macOS; on Debian/Ubuntu: sudo apt install python3 python3-venv ffmpeg
./setup
./gloam https://ice1.somafm.com/groovesalad-128-mp3
```

Setup installs the Python dependencies in `.venv` inside this directory. Afterward,
just run `./gloam URL_OR_FILE`: it uses that environment automatically, with no
activation needed. You can launch it by its full path from any directory, too.
Run `./setup` again to update dependencies or retry an interrupted installation.
To choose a particular Python version for initial setup, use e.g. `python3.12 setup`.

## Requirements

- Python 3.8 or later
- [numpy](https://numpy.org)
- [ffmpeg](https://ffmpeg.org)
- macOS or Linux

Optional:

- [sounddevice](https://python-sounddevice.readthedocs.io) for sound, and the closest sync between picture and sound. Without it, gloam falls back to ffmpeg's macOS audio output, ffplay, paplay, pw-cat, aplay or SoX's `play`.
- [certifi](https://pypi.org/project/certifi/) to read song titles from https streams on Python builds without system certificates.

If you prefer to manage your own Python environment instead of using `./setup`:

```sh
python3 -m pip install numpy sounddevice certifi
```

## Usage

```
./gloam https://ice1.somafm.com/groovesalad-128-mp3
./gloam https://somafm.com/groovesalad.pls
./gloam song.flac other.mp3          # several sources: n / b to move between them
./gloam stations.m3u                 # a playlist of stations or files
./gloam URL --palette ember --hold
./gloam URL --react wild             # off, subtle, gentle, normal, lively, strong, wild, extreme, unhinged, or 0 to 10
./gloam URL --ahead 180              # run the picture 180 ms ahead of the sound (default 130)
./gloam URL --volume 60
./gloam URL --no-sound               # the picture only
./gloam URL --fps 15                 # cap redraws at 15 frames per second (default 30)
./gloam URL --vignette               # fade all edges into your terminal's background
./gloam URL --vignette 0.7           # a wider fade (0 to 1; default 0.45 when enabled)
./gloam URL --vignette --background '#1e2030'  # use an explicit background colour
./gloam URL --settings              # start with a panel of current settings
./gloam URL --no-now-playing        # hide the track display
```

Display options include `--full`, `--mini [C]`, `--num` and `--chunky` (cell layouts), `--256`, `--nocolour`, `--fps`, `--speed`, `--size WxH`, `--hue`, `--brightness`, `--colour` and `--white`. Run `./gloam --help` for the full list.

Gloam uses synchronized terminal updates by default to reduce horizontal tearing
(a visible seam between parts of different frames) in supporting terminals.
Use `--no-sync` if your terminal has trouble with this. A lower `--fps`, such as
15, also reduces terminal traffic; it accepts any finite number greater than zero.
Visible colour steps and the grid of terminal cells are separate from tearing;
changing the frame rate does not increase colour or cell resolution.

`--vignette` leaves the centre vivid and softly blends the edges into the terminal's
background colour, detected at startup using an OSC 11 query. If your terminal
does not report its background, the fade uses black; supply `--background '#RRGGBB'`
to match your theme. The width is a fraction of each half of the picture: larger
values bring the fade farther inward, and `0` disables it. The fade follows window
resizes. Transparent windows blend toward the reported theme colour; wallpaper or
content behind the window cannot be sampled.
Press `v` to toggle the vignette, keeping its last width. `(` and `)` narrow or
widen it by 0.05 and enable it if needed. The current setting appears briefly in
the status line. These controls work without starting with `--vignette`.

Press `?` for keyboard help or `s` for current settings. Both panels wrap to the
window width and paginate: Space advances a page, and `x` closes the panel.
Pressing `?` or `s` again closes its panel. Music and keyboard controls continue
working while a panel is open; settings update live. Changes briefly show the
adjusted value first, keeping it visible in narrow windows. Press `t` to show or
hide the now-playing track; `--no-now-playing` starts with it hidden. Adjustment
feedback and the panels remain available while the track display is hidden.

## Keys

| Key | Action |
| --- | --- |
| `n` / `b` | next / previous source |
| up / down (or `0` / `9`) | louder / quieter |
| `m` | mute |
| `r` / `R` | react more / less |
| `{` / `}` | picture earlier / later against the sound, 10 ms a press |
| `p` / `P` | next / previous palette now |
| `h` | hold the palette / let it change |
| `v` | toggle vignette, remembering its width |
| `(` / `)` | narrower / wider vignette, 0.05 a press |
| `+` / `-` | turn the colours round the colour wheel |
| `[` / `]` | darker / brighter |
| `<` / `>` (or `,` / `.`) | colours move slower / faster |
| `i` | show where the sound is going |
| `?` | toggle keyboard help |
| `s` | toggle current settings |
| Space / `x` | next panel page / close panel |
| `t` | show / hide now playing |
| `q` | quit (Ctrl-C works too) |

## Licence

MIT. See [LICENSE](LICENSE).

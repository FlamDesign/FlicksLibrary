# DM Se Date Tak · Episode 1 (interactive, 81 paths)

Live preview: https://flamdesign.github.io/FlicksLibrary/preview/dm-se-date-tak-ep1/

## What is in this folder
| Path | What it is |
|---|---|
| `v/` | 67 section clips, 540×960, H.264 + AAC, 24 fps. These are the web previews. |
| `music.mp3` | One continuous music track, 0–111 s, the same on every path. |
| `subtitles.json` | Hinglish subtitle cues on the episode clock. |
| `manifest.json` | Section timings, flick windows, choice codes and file naming. |
| `index.html` | The current browser player (two `<video>` elements in ping-pong, plus one `<audio>` for music). |
| `s/` | Choice thumbnails and the poster frame. |

The 720×1280 masters (same names, full mix with music baked in) are kept separately, not in this repo. Ask Arijeet for them.

## Structure
The episode is 111 s long, split into 5 sections. Each flick is open for the first 10 s of its section.

| Section | Episode time | File | Depends on |
|---|---|---|---|
| INTRO | 0–31.5 | `INTRO.mp4` | — |
| FD (Flick 1: outfit) | 31.5–45.5 | `FD_{o}.mp4` | outfit A/B/C |
| F1 (Flick 2: who is he) | 45.5–67.5 | `F1_{o}{m}.mp4` | + man K/N/R |
| F2 (Flick 3: gift) | 67.5–89.5 | `F2_{o}{m}{g}.mp4` | + gift S/R/T |
| F3 (Flick 4: date) | 89.5–111 | `F3_{o}{m}{d}.mp4` | + date A/B/C |

Choice codes:
- **o:** A = Peeli kurti, B = Black dress, C = White shirt & jeans
- **m:** K = Kabir, N = Neel, R = Rehan
- **g:** S = Sunflowers, R = Red roses, T = Pink tulips
- **d:** A = Marine Drive, B = Juhu fair, C = Chai in the rain

All versions of a section are frame-identical outside the swapped element, with the same timing and the same frame count.

## Switching rules
- A pick made during a flick window swaps to the matching version at the same offset (mid-flick switch).
- The next section always starts at its offset 0.
- The music must keep playing across every cut. It is a separate track for exactly this reason.

## Known cause of the lag
The current player swaps whole MP4 files between two `<video>` elements:
- At each section change, the next file is preloaded only 3 s before the cut.
- A mid-flick pick has to fetch and seek a new file before it can play.

Baking options:
- HLS or DASH with one rendition per version and aligned segments.
- Pre-buffering every version of the upcoming flick before its window opens.
- Combining the versions into one tiled or multi-track stream.

# games.aodhancoyne.com

An arcade. Dark room, neon carpet in perspective, one cabinet per game, and the
games run live inside the cabinet screens. **Four machines** stand in one rank:
Kedr, Missile, Doghole, Pacific Galleon.

Static HTML, no build step, no external requests, no tracking. Served by GitHub
Pages at `games.aodhancoyne.com` (see `CNAME`).

## How a cabinet works

Each `<article class="cab">` is marquee → bezel/screen → control panel → body.
The screen shows a still attract image until **Insert Coin** is pressed, then an
iframe of the real game is inserted and the still is hidden.

The games expect a desktop-sized viewport, so the iframe is given a full logical
resolution (960 × 720) and the whole frame is CSS-scaled down to fit the glass.
Handing the game a small viewport instead makes it crop. See `fit()` in the script.

While playing, the joystick and buttons hide and the panel becomes a control bar:
**Fullscreen**, **New tab**, **Stop**.

## Adding a game

Copy an `<article class="cab">` block and set:

- `.marquee b` / `.marquee i` — title and subtitle
- `.coin[data-src]` — the game URL
- `.screen img[src]` — an attract still in `assets/`
- a hue: add a rule like `.cab.yourgame{--hue:#RRGGBB}` and the class on the article
- `.cab.yourgame .art img{object-position:50% NN%}` to pick which band of the
  front panel art the cabinet shows

⚠ **The rank is sized to fit, so a fifth machine is not free.** `.hall` and
`.cab` both cut their width from the viewport so that `4W + 3G` fits (21vw and
3.2vw). Adding one means re-cutting both, and `.keys` has to shrink with them
or the longest control legend clips: that is why its font is
`clamp(6px,.70vw,8px)` rather than a fixed size.

## Audio — not wired up yet

Two `<audio>` elements are in place at the bottom of the page, pointing at files
that do not exist yet. Drop them in and they start working; nothing else to change.

| File | What it should be |
|---|---|
| `assets/room.mp3` | ambient arcade room tone, seamless loop |
| `assets/coin.mp3` | one-shot coin drop, plays on Insert Coin |

Nothing ever autoplays. Browsers block audio until a user gesture, so the speaker
button in the bottom right is the only thing that starts the room tone, and it is
off on load. The coin sound only plays if the room tone is already on.

## Known, still to check

- **Keyboard into the iframe.** Real keystrokes should reach a focused cross-origin
  iframe, but this has not been confirmed with a human at the keyboard. If it
  misbehaves, Fullscreen and New tab are the escape hatches.
- Both games are loaded only on demand, so opening the page costs nothing.

## Assets

`attract-kedr.jpg` and `attract-missile.jpg` are title-screen captures taken from
the live games on 2026-09-11. `still-pacific-galleon.jpg` likewise, 2026-10-07.

The Pacific Galleon marquee and front panel are generated, not hand-written:
see `~/pacific-galleon/cabinet-art/` (`build_marquee.py`, `build_art.py`), which
is versioned in the game repo. Both
set their lettering as OUTLINE PATHS via `text_to_paths.py`, because Trattatello
is a macOS system font and live `<text>` falls back to a plain serif everywhere
else. Edit the builders and re-run them, not the SVGs.

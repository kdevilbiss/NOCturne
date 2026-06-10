# NOCturne

**Alert noise you can dance to.**

NOCturne is a single-file LogicMonitor dashboard widget that plays your active alerts as a live, generative synth track. Severity sets the register and timbre (criticals growl low and buzzy, warnings chime up high), every resource keeps its own note, and the song gets busier as the alert count climbs, so you can hear an incident building before you have read a single alert. Everything is synthesized in the browser with the Web Audio API: no samples, no media files, no external calls.

NOCturne is an unofficial personal project, not an official LogicMonitor product, and is provided as-is under the MIT License. LogicMonitor is a trademark of LogicMonitor, Inc.

## What's in this repo

| File | What it is |
| --- | --- |
| [`nocturne.html`](nocturne.html) | The widget. One file. Paste it into a LogicMonitor Text widget and it runs. |
| [`lab-nocturne.html`](lab-nocturne.html) | The Sound Lab: the same engine on mock data. Opens from `file://` in any browser, no LogicMonitor, no setup. The no-portal way to hear it, demo it, and tune it. |
| [`docs/music-science.md`](docs/music-science.md) | Why the output sounds like music instead of a smoke detector: pentatonic scales, quantization, beat induction, and the generative-music payoff, explained for humans. |
| [`docs/box-art.md`](docs/box-art.md) | The back of the retail box, for those who remember software coming in boxes. |

## Quick start

> [!IMPORTANT]
> **An account admin must first enable Settings > Security > Allow Scripts in Dashboard Text Widget**, or nothing here runs. LogicMonitor ships it off because it lets Text widgets execute JavaScript. Enabling it is an account-level security decision: anyone who can edit a Text widget can run script in a viewer's browser. Enable it knowingly and keep dashboard edit access controlled.

1. Copy the entire contents of `nocturne.html`.
2. On a dashboard, add a **Text** widget and switch it to **HTML / code** mode.
3. Paste, save, press **Load**, then press **Play** once (browsers require one click before any sound).

No build, no hosting, no install. To update, paste the file again.

## Hear it without a portal

Download `lab-nocturne.html` and open it in a browser tab. It is the full synth engine and every control, running on a mock alert generator. Generate a set, press Play, push the Storm button. This is also the right file to open on a call when you want the demo without touching anyone's production dashboard.

## The four views

The alert pane renders the same alerts four ways, switchable live:

- **Chips**: named pills with severity dots. The readable one.
- **Hive**: a honeycomb of severity-colored hexagons, names on hover. Fits hundreds in one pane; big loads switch to it automatically until you pick a view by hand.
- **Cluster**: a night sky. Each device's alerts pack in a golden-angle rosette around its name-hashed home spot, so the same device always lives in the same patch of sky, and noisy devices grow into bright constellations.
- **Skyline**: one tower per device, alerts stacked as lit floors. The noisiest device is the tallest building on the block. Capped at the 250 noisiest devices; column width scales to fit.

In every view, ACK alerts are light blue, SDT alerts are purple, clicking an alert plays its note, and the NOW readout names the alert behind whatever you just heard.

## Performing

- **Transport**: Stop, Play/Pause, and Record, which captures the master output to a `.webm` clip (browser support varies; the button says so when unavailable).
- **Mix**: Storm drops a synthetic alert burst on the next bar with a boom. Calm clears it. Auto-DJ performs on its own, riding the Tone and Reverb sliders while you watch. Hush™ mutes the alert notes and keeps the bed grooving, perfect for pretending everything is fine.
- **Arrange**: Shuffle, Reverse, and Sort reorder the melody rotation.
- **Scale**: Minor, Major, or Dark (one deliberate half-step of menace).
- **Audio Synth**: Tempo, Tone (lowpass filter), Reverb, and Volume.
- **Footer panels**: Help explains how it works, Reference shows every live config value, and Settings exposes all of it as controls with copy/paste config and a reset.

## Dashboard tokens

Both optional, set on the widget or dashboard:

| Token | Range | Default | What it does |
| --- | --- | --- | --- |
| `AlertLimit` | 10 to 1000 | 50 | How many alerts Load pulls. |
| `Tempo` | 64 to 140 | 96 | Beats per minute. |

## Security model

- Talks to LogicMonitor only through the **viewer's existing dashboard session**, same-origin. No API tokens, no credentials, nothing embedded in the page.
- **Read-only**: it fetches active alerts and changes nothing. The Storm button is synthetic, browser-side noise.
- **No external requests**: every sound is synthesized locally, no analytics, no CDNs, no fonts fetched.
- Multi-instance safe and hardened against the LM v239 script-loading change.

## Browser notes

- One click of Play per session before any sound: autoplay rules, and fair ones.
- The track pauses itself when the tab is hidden. A throttled background tab may briefly play in slow motion, which sounds intentional and is not.
- Record needs `MediaRecorder` (best in Chrome and Edge).

## Lineage

Born in the LM Widget Lab as a rebirth of Song of Alerts, the sample-playback original, which stays behind in the Lab as an artifact. The sound here was prototyped in the lab bench. For the full story of why a stream of alerts comes out sounding like a song, read [the music science](docs/music-science.md).

The defaults are not arbitrary.

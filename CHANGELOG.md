# Changelog

## Unreleased

- Hardened Paste config: pasted JSON is untrusted input on the LogicMonitor origin, so it is now rebuilt against the shape of the defaults instead of deep-merged. Only known keys survive (which also blocks `__proto__` prototype-pollution tricks), numbers are coerced and clamped to the same ranges as the Settings UI (echo feedback in particular can no longer be pushed into runaway territory), waveforms must come from the known list, and the Reference panel escapes the one config-derived string it renders. Previously, a maliciously shared config could inject markup into the Reference panel.
- Removed dashboard token overrides for alert limit and tempo; those are now controlled only from the widget UI.
- Added the MIT license.
- README: removed the demo placeholder and a stale reference to the legacy widget, which stayed behind in the Lab.

## 1.0 - 2026-06-10

Initial release, moved into its own repo from the LM Widget Lab.

- The widget: live alerts become a generative, quantized synth track via the Web Audio API. Severity sets register, waveform, loudness, and length; each resource hashes to a fixed pentatonic scale degree; alert load drives note density; ACK and SDT play quieter. A bass, pad, kick, and hat bed loops a four-bar root pattern under everything.
- Four views of the alert pane: Chips, Hive (honeycomb hexagons), Cluster (golden-angle star rosettes per device), and Skyline (one tower per device, alerts as lit floors, capped at the 250 noisiest devices). Loads over 60 alerts switch to Hive until a view is picked by hand. ACK is light blue, SDT purple, in every view.
- Performance controls: transport with `.webm` Record, Storm and Calm, Auto-DJ (rides Tone and Reverb with the sliders animating), Hush™, Arrange (Shuffle / Reverse / Sort), three scales (Minor / Major / Dark), and live Tempo / Tone / Reverb / Volume.
- Reference panels: Help (how it works, glossary), Reference (every live config value), and Settings (everything as controls, with copy/paste config and reset to defaults).
- Dashboard tokens: `AlertLimit` (10 to 1000, default 50) and `Tempo` (64 to 140, default 96). Token values outside the select presets get their own option, so the UI shows what Load will pull.
- Hardening: claims its widget root with an init attribute, so two copies on one dashboard each wire their own DOM and a script re-run cannot double-wire listeners; tears the whole audio graph down if the host replaces the widget DOM mid-play; survives the LM v239 change where `document.currentScript` is null; the Reference panel's table resets everything host CSS could set.
- The Sound Lab (`lab-nocturne.html`): full-parity engine and controls on mock data, runs from `file://` anywhere.
- Default synth values: tempo 96, tone 42, reverb 11, volume 79. The numbers are not arbitrary.

# MySonar

Desktop audio player with real-time subtitle support.
**Windows · macOS** — v1.0.1 · Released 2026. 02. 22 · Developed by Hustlyn

[한국어](README.ko.md) · [日本語](README.ja.md)

---

![MySonar](docs/screenshot.png)

---

## Supported Formats

| Type | Formats |
|---|---|
| Audio | WAV · FLAC · MP3 · OGG · OPUS · AAC · M4A · AIFF · WebM |
| Subtitle | SRT · VTT · ASS / SSA · SMI · LRC · TXT |

---

## Subtitle File Naming

Place subtitle files in the same directory as the audio file.

### RAW (no language label)

| Pattern | Example (audio: `rain.flac`) |
|---|---|
| `<stem>.<subtitle-ext>` | `rain.srt` |
| `<stem>.<audio-ext>.<subtitle-ext>` | `rain.flac.srt` |

RAW subtitles are labeled **RAW** in the subtitle selector.

### Language-coded

| Pattern | Example (audio: `rain.flac`) |
|---|---|
| `<stem>.<lang>.<subtitle-ext>` | `rain.en.srt` · `rain.ja.vtt` |
| `<stem>.<audio-ext>.<lang>.<subtitle-ext>` | `rain.flac.ko.srt` |

`<lang>` must be a 2–3 letter lowercase ISO 639-1 code (e.g. `en`, `ko`, `ja`, `fra`).

**Example — directory for `rain.flac`:**

```
rain.flac
rain.srt            ← RAW
rain.en.srt         ← English
rain.ja.vtt         ← Japanese (different format)
rain.ko.ass         ← Korean
rain.flac.zh.srt    ← Chinese (full-filename form)
```

Multiple languages and formats per track are supported simultaneously. When a language has more than one subtitle file, a format selector (SRT / VTT / …) appears next to the language selector.

---

## Features

### Playback
- Waveform seekbar with real-time analyzer overlay
- Crossfade between tracks (configurable duration)
- Remember last playback position per file

### Audio Processing
- 10-band equalizer (32 Hz – 16 kHz) — Simple / Pro modes, built-in presets
- Dynamics compressor and bass boost
- Volume normalization (ReplayGain) (BETA)
- Spatial audio — HRTF binaural positioning (X / Y / Z axes)
- Left/right hearing balance correction with mono test tone

### Subtitles
- Overlay display — configurable font, size, color, outline, shadow, and position
- Script panel with current cue highlighted and auto-scroll
- Inline color tag rendering (SRT / VTT / SMI)
- Seek to previous / next subtitle cue

### Playlist & Collections
- Drag & drop files or folders anywhere onto the window
- Multi-select: Ctrl+click · Shift+click · mouse-drag lasso
- Collapsible track groups within the playlist, drag-reorderable
- Sort by name, duration, or file size (natural numeric order)
- Collections — named playlists saved as `.msc` files
  - Multiple cover images per collection, drag-reorderable
  - Rating (0–5 stars) and custom tags
  - Track pagination: 5 / 10 / 20 per page
- Search with tag filter and minimum rating filter
- Track info modal with full metadata view

### Appearance
- 7 themes: Dark · Light · Dark Rose · Light Rose · Light Marine · Dark Marine · Pink
- Rounded transparent window
- Album art: embedded metadata, same-name image file, or drag-and-drop override
- Action overlay — visual feedback on key presses
- Status bar — live playback state

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `Space` | Play / Pause |
| `←` / `→` | Seek ±1 s |
| `Shift` + `←` / `→` | Seek ±5 s |
| `Ctrl` + `←` / `→` | Seek ±0.2 s |
| `↑` / `↓` | Volume ±5% |
| `Z` / `X` | Previous / next subtitle line |
| `Alt` + `←` / `→` | Seek to previous / next cue |
| `[` / `]` | Subtitle font size −1 / +1 px |

All shortcuts (except `Ctrl+A` — select all tracks) are rebindable in **Settings → Controls**.
Seek amounts (±1 s / ±5 s / ±0.2 s) are also adjustable per category.

---

## Localization

Built-in: **English · Korean · Japanese**

To add a custom language, place a `.json` locale file in:

```
<exe-directory>/locales/<code>.json
```

User locale files override built-in ones with the same language code.

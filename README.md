<p align="right">
  <a href="README.ko.md">한국어</a> &nbsp;·&nbsp; <a href="README.ja.md">日本語</a>
</p>

<div align="center">

# MySonar

Desktop audio player with real-time subtitle support.

<br>

![Windows](https://img.shields.io/badge/Windows-Release-0078D4?style=flat-square)&nbsp;&nbsp;![macOS](https://img.shields.io/badge/macOS-QA_Testing-lightgrey?style=flat-square)

<br>

**v1.0.0** &nbsp;·&nbsp; Released 2026. 02. 22 &nbsp;·&nbsp; Hustlyn

<br>

<img src="preview/icon.png" width="72">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="preview/snrpack_icon.png" width="72">

</div>

<br>

<video src="preview/demo.mp4" controls width="100%"></video>

![MySonar](preview/00-page/00-track.png)

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

## Package Format (.snrpack)

A `.snrpack` file packages an entire collection — audio, subtitles, cover images, and all metadata — into a single file for easy sharing or backup. Internally it is a ZIP archive.

**Internal structure:**

```
collection.snrpack  (ZIP archive)
├── manifest.json        format version and app identity
├── collection.json      collection name, author, description, tags, track list, groups
├── audio/               audio files (stored uncompressed for seeking support)
│   ├── 0/  song.flac
│   └── 1/  another.flac  (index increments on filename collision)
├── images/              cover images (compressed)
│   └── 0/  cover.jpg
└── subtitles/           subtitle files, index-matched to audio (compressed)
    └── 0/  song.en.srt
             song.ja.vtt
```

`collection.json` stores the collection name, author, description, tags, cover image paths, and a full track list. Each track entry includes audio metadata (title, artist, album, duration, format, bitrate, etc.), rating, tags, and associated subtitle files with their language codes. Track groups and their order are also stored.

**To export:** Right-click a collection → **Export Package**
**To import:** Drag a `.snrpack` file onto the app window, or right-click a collection → **Import Package**

`.snrpack` files are registered with a custom icon in the system. Double-clicking a file or using **Open With → MySonar** opens it directly.

<table>
  <tr>
    <td width="50%" align="center"><img src="preview/03-packing/30_snrpack.png" width="100%"><br>.snrpack file in Explorer</td>
    <td width="50%" align="center"><img src="preview/03-packing/31_snrpack-open.png" width="100%"><br>Open directly via double-click or Open With</td>
  </tr>
</table>

---

## Features

### Playback
- Waveform seek bar with real-time analyzer overlay
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
- Collapsible track groups within the playlist, reorderable by drag
- Sort by name, duration, or file size (natural numeric order)
- Collections — named playlists saved as `.msc` files
  - Multiple cover images per collection, reorderable by drag
  - Rating (0–5 stars) and custom tags
  - Track pagination: 5 / 10 / 20 per page
- Search with tag and minimum rating filters
- Track info modal with full metadata view

### Appearance
- 7 themes: Dark · Light · Dark Rose · Light Rose · Light Marine · Dark Marine · Pink
- Rounded transparent window
- Album art: embedded metadata, same-name image file, or drag-and-drop override
- Action overlay — visual feedback on key presses
- Status bar — live playback state

---

## Screenshots

<table>
  <tr>
    <td align="center"><img src="preview/00-page/00-track.png"><br>Track view</td>
    <td align="center"><img src="preview/00-page/01-collection.png"><br>Collections</td>
    <td align="center"><img src="preview/00-page/02-edit_collection.png"><br>Collection editor</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/00-page/03-import-srnpack.png"><br>Import package</td>
    <td align="center"><img src="preview/00-page/04-eq.png"><br>Equalizer</td>
    <td align="center"><img src="preview/00-page/05-password.png"><br>Password lock</td>
  </tr>
</table>

### Themes

<table>
  <tr>
    <td align="center"><img src="preview/02-theme/20_dark.png"><br>Dark</td>
    <td align="center"><img src="preview/02-theme/21_light.png"><br>Light</td>
    <td align="center"><img src="preview/02-theme/22_dark-rose.png"><br>Dark Rose</td>
    <td align="center"><img src="preview/02-theme/23_light-rose.png"><br>Light Rose</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/02-theme/24_dark-marine.png"><br>Light Marine</td>
    <td align="center"><img src="preview/02-theme/25_dark-marine.png"><br>Dark Marine</td>
    <td align="center"><img src="preview/02-theme/26_pink.png"><br>Pink</td>
    <td></td>
  </tr>
</table>

### Settings

<table>
  <tr>
    <td align="center"><img src="preview/01-settings/10_setting.png"><br>General</td>
    <td align="center"><img src="preview/01-settings/11_setting.png"><br>Audio</td>
    <td align="center"><img src="preview/01-settings/12_setting.png"><br>Subtitle</td>
    <td align="center"><img src="preview/01-settings/13_setting.png"><br>Controls</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/01-settings/14_setting.png"><br>Tags</td>
    <td align="center"><img src="preview/01-settings/15_setting.png"><br>Language</td>
    <td align="center"><img src="preview/01-settings/16_setting.png"><br>Security</td>
    <td align="center"><img src="preview/01-settings/17_setting.png"><br>About</td>
  </tr>
</table>

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

All shortcuts (except `Ctrl+A` — select all tracks) can be reassigned in **Settings → Controls**.
Seek amounts (±1 s / ±5 s / ±0.2 s) are also adjustable per category.

---

## Localization

Built-in: **English · Korean · Japanese**

To add a custom language, download [sample_local.json](sample_local.json) as a starting template.
Rename it to `<code>.json` (e.g. `fr.json`), translate the values, then place it in:

```
<exe-directory>/locales/<code>.json
```

User locale files override built-in ones with the same language code.

---

## Roadmap

| | |
|---|---|
| Mac build & distribution | Build and publish a signed macOS release |
| Auto subtitle generation | Generate VTT subtitles from audio using a Whisper model |
| Custom themes | User-defined color themes via JSON or in-app editor |
| Manual & Docs | In-app help and online documentation |

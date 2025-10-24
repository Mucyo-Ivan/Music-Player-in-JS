# Music Player (JavaScript)

A lightweight, customizable music player built with plain JavaScript, HTML and CSS. This README covers setup, usage examples, API, integration tips, accessibility notes, and guidance for contributing.

## Table of contents
- [About](#about)
- [Features](#features)
- [Demo](#demo)
- [Requirements](#requirements)
- [Installation](#installation)
  - [Option A — Use via CDN / script include](#option-a---use-via-cdn--script-include)
  - [Option B — Install via npm](#option-b---install-via-npm)
- [Quick start](#quick-start)
  - [HTML example](#html-example)
  - [Creating a playlist (JS)](#creating-a-playlist-js)
- [Player API](#player-api)
  - [Constructor / initialization](#constructor--initialization)
  - [Methods](#methods)
  - [Events](#events)
- [Customization & Styling](#customization--styling)
- [Accessibility](#accessibility)
- [Browser support](#browser-support)
- [Development](#development)
  - [Run locally](#run-locally)
  - [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## About
This Music Player is intended to be simple to drop into any web project. It handles playing audio, playlists, seeking, volume control, and provides a small API to programmatically control playback.

## Features
- Play / pause / stop
- Playlist support (next / previous / shuffle / repeat)
- Seek bar and current time / duration display
- Volume control and mute
- Keyboard shortcuts (configurable)
- Accessible controls and ARIA attributes
- Small footprint, no framework required
- Events for integration (track change, time update, ended, etc.)

## Demo
Add the player to any HTML page and use the example below to get started.

## Requirements
- Modern browser with HTML5 Audio support
- Optional: Node.js + npm for local development and building

## Installation

### Option A - Use via CDN / script include
Include the CSS and JS files in your page:
```html
<link rel="stylesheet" href="music-player.css">
<script src="music-player.js"></script>
```

### Option B - Install via npm
```bash
npm install simple-js-music-player
```
Then import in your build:
```js
import MusicPlayer from 'simple-js-music-player';
import 'simple-js-music-player/dist/music-player.css';
```

## Quick start

### HTML example
Create a container and initialize the player in JavaScript:
```html
<!-- index.html -->
<div id="my-player" class="music-player"></div>

<script type="module">
  import MusicPlayer from './music-player.js'; // or from package

  const playlist = [
    { src: 'audio/track1.mp3', title: 'Track 1', artist: 'Artist A', cover: 'images/cover1.jpg', duration: 210 },
    { src: 'audio/track2.mp3', title: 'Track 2', artist: 'Artist B', cover: 'images/cover2.jpg', duration: 185 }
  ];

  const player = new MusicPlayer(document.getElementById('my-player'), {
    playlist,
    autoplay: false,
    loop: false,
    volume: 0.8,
  });

  // Programmatically play the first track
  player.play();
</script>
```

### Creating a playlist (JS)
```js
player.addTrack({
  src: 'audio/new-track.mp3',
  title: 'New Song',
  artist: 'Artist X',
  cover: 'images/new-cover.jpg'
});
```

## Player API

### Constructor / initialization
new MusicPlayer(containerElement, options)

Options (common):
- playlist: Array of track objects ({ src, title, artist, cover, duration })
- autoplay: boolean
- loop: boolean | 'track' | 'playlist'
- shuffle: boolean
- volume: number (0.0 - 1.0)
- muted: boolean
- keyboard: boolean (enable keyboard shortcuts)
- theme: object or className for custom styling

### Methods
- play(): Promise — start/resume playback
- pause(): void — pause playback
- stop(): void — pause and reset to start
- togglePlay(): void — toggle play/pause
- next(): void — play next track
- previous(): void — play previous track
- seek(seconds): void — jump to given time in seconds
- setVolume(value): void — set volume 0.0–1.0
- mute(): void
- unmute(): void
- addTrack(track): void — add to end of playlist
- insertTrack(track, index): void — insert at index
- removeTrack(index): void
- shuffle(on): void — enable/disable shuffle
- destroy(): void — teardown, remove listeners, and DOM if needed

Most methods return a boolean or a Promise when the action triggers playback that requires user gesture constraints.

### Events
The player dispatches DOM CustomEvents from the container element:
- 'mp:play' — { trackIndex, track }
- 'mp:pause'
- 'mp:timeupdate' — { currentTime, duration }
- 'mp:trackchange' — { oldIndex, newIndex, track }
- 'mp:ended' — fired when a track finishes
- 'mp:error' — { error }

Usage example:
```js
container.addEventListener('mp:trackchange', (e) => {
  console.log('Now playing:', e.detail.track.title);
});
```

## Customization & Styling
- The player exposes CSS variables for quick theming (e.g., --mp-primary, --mp-background, --mp-accent).
- You can supply a `theme` option or add a custom className to the container to override styles.
- The DOM structure is simple and documented in source comments for safe overrides.

Example CSS vars:
```css
.music-player {
  --mp-primary: #1db954;
  --mp-background: #fff;
  --mp-text: #111;
}
```

## Accessibility
- All controls use semantic HTML (buttons, inputs) and provide ARIA labels.
- Keyboard shortcuts (by default): Space = play/pause, ArrowRight = seek forward, ArrowLeft = seek backward, ArrowUp/Down = volume.
- Focus states are visible and meet contrast requirements.
- Announcements: when track changes or an error occurs, the player updates an aria-live region to inform screen reader users.

## Browser support
- Modern browsers with HTML5 Audio (Chrome, Firefox, Edge, Safari).
- On iOS Safari and some mobile browsers, auto-play is restricted; user interaction may be required to start audio.

## Development

### Run locally
If the project uses a simple tooling setup:
```bash
git clone <repo>
cd <repo>
npm install
npm run dev   # start dev server (e.g., Vite / webpack dev)
```

### Build
```bash
npm run build
```

### Testing
- Unit tests (if included) run with:
```bash
npm test
```
- Manual testing: test with a few audio formats (mp3, m4a, ogg) and ensure UI works at various viewport sizes.

## Troubleshooting
- Audio won't play automatically on mobile: ensure a user gesture initiated playback.
- Files not found: check audio src paths and server MIME types for audio files.
- CORS errors: ensure audio files are served with proper CORS headers if hosted on a different origin.

## Contributing
Contributions are welcome. Typical workflow:
1. Fork the repo
2. Create a branch: git checkout -b feat/add-feature
3. Make changes and add tests
4. Open a PR describing the change

Please follow the repository's code style and include tests for new features or bug fixes.

## License
Specify your license here (e.g., MIT). Replace below with a real license as appropriate.
Licensed under the MIT License — see LICENSE file for details.

## Contact
Maintainer: Your Name / your-email@example.com
Repository: (replace with project repo URL)

Happy building!

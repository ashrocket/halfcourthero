# Spotify Integration Guide — bandmusicgames.party Games

How to wire up the shared Spotify player used by **Grass Cutter 2003** (and the lobby) into any game on the network.

---

## How it works

Auth lives entirely at **bandmusicgames.party** (the lobby). When a player connects Spotify there, the lobby writes two shared cookies under `.bandmusicgames.party`:

| Cookie | Contents |
|---|---|
| `sp_token` | Access token (short-lived) |
| `sp_refresh` | Refresh token (30-day) |

Games never redirect to Spotify themselves — they just read these cookies on load. If the cookies aren't there, redirect the player back to the lobby.

On `localhost`, `sessionStorage` is used instead of cookies for easier development.

---

## Files to copy

Copy these two files from the `forcuttinggrass` repo verbatim:

- **`js/spotify.js`** — `SpotifyAuth`, `SpotifyPlayer`, bootstrap logic
- **`config.js`** — holds your `spotifyClientId` and the `trackUri` for this game

---

## config.js

```js
const CONFIG = {
  spotifyClientId: 'aa16f7f72c04485fb93d86d2f7ee33d1',  // shared client ID
  trackUri: 'spotify:track:YOUR_TRACK_ID_HERE',
};
```

Get the track URI from the Spotify desktop app: right-click a track → **Share → Copy Spotify URI**.

The `spotifyClientId` is shared across all games on the network — reuse it, don't create a new app.

---

## HTML setup

Add to `<head>`:
```html
<script>
  window.onSpotifyWebPlaybackSDKReady = function () {
    window._sdkReady = true;
    if (window._onSDKReady) window._onSDKReady();
  };
  function skipSpotify() {
    document.getElementById('spotify-overlay').classList.add('hidden');
    window._gameReady = true;
  }
</script>
```

Add before `</body>` (order matters — SDK must load before game scripts):
```html
<script src="https://sdk.scdn.co/spotify-player.js"></script>
<script src="config.js"></script>
<script src="js/spotify.js"></script>
<script src="js/game.js"></script>  <!-- your game -->
```

Add the overlay (style it however you want):
```html
<div id="spotify-overlay">
  <!-- your branding -->
  <button onclick="window.location.href='https://bandmusicgames.party'">
    Connect Spotify at Lobby
  </button>
  <button onclick="skipSpotify()">Play without music</button>
</div>
```

Add `.hidden { display: none; }` to your CSS.

---

## Gating game start on the overlay

The bootstrap in `spotify.js` sets `window._gameReady = true` once it's safe to start. Poll for it before launching your game loop:

```js
function waitForReady(cb) {
  if (window._gameReady) return cb();
  setTimeout(() => waitForReady(cb), 100);
}

waitForReady(() => startGame());
```

If you use Phaser, do this in a `BootScene` that polls and then starts your `MenuScene`.

---

## Playing music in-game

`spotify.js` exposes a global `SpotifyPlayer` object:

```js
// Start music (call once, e.g. on first user interaction)
if (window._spotifyConnected && !window._musicPlaying) {
  SpotifyPlayer.play();
  window._musicPlaying = true;
}

// Pause / resume
SpotifyPlayer.pause();
SpotifyPlayer.resume();

// Volume (0.0–1.0)
SpotifyPlayer.setVolume(0.5);

// Check if the device is ready before trying to play
SpotifyPlayer.isReady(); // boolean
```

`window._spotifyConnected` is set to `true` by the bootstrap only when a valid token was found and the SDK initialized. Always guard plays behind this flag.

Song looping is handled automatically in `spotify.js` — it listens for `player_state_changed` and calls `play()` again when the track ends.

---

## Local development

On `localhost`, cookies aren't set. The overlay's skip button calls `skipSpotify()` which sets `window._gameReady = true` without music — use this to test gameplay. To test music locally, manually set `sessionStorage.setItem('sp_token', '<token>')` in the browser console with a valid access token.

---

## Checklist

- [ ] `config.js` has the correct `trackUri` for this game
- [ ] Spotify SDK `<script>` tag is first, before `config.js` and `spotify.js`
- [ ] `#spotify-overlay` div exists with id exactly `spotify-overlay`
- [ ] Game loop waits for `window._gameReady` before starting
- [ ] Music play is gated behind `window._spotifyConnected`
- [ ] Spotify Premium is required — surface that clearly to players

# Songless (self-hosted)

A guess-the-song game that plays songs from a Spotify playlist. Clips unlock at 0.1s, 0.5s, 2s, 8s and 15s. Every wrong guess or skip moves to the next clip and plays it automatically. Rounds are random and unlimited.

It's a single static page (`index.html`) with no backend, so it runs on GitHub Pages or a local server.

## 1. Create the Spotify app (one time, by you)

1. Go to https://developer.spotify.com/dashboard and create an app. The account that owns it must have Premium.
2. Tick **Web API** and **Web Playback SDK**.
3. Add redirect URIs (these must match exactly, the page shows its own under the gear icon in Settings):
   - `https://YOUR-USERNAME.github.io/YOUR-REPO/`
   - `http://127.0.0.1:8000/` (for running locally; Spotify doesn't accept `localhost`)
4. Copy the **Client ID** and paste it into `CONFIG.CLIENT_ID` at the top of the script in `index.html`. The Client ID is public, so this is safe to commit.

## 2. Let your friends in

Spotify limits apps in development mode to **5 authorized users total** (including you). Add each friend under **User Management** in the dashboard using the name and email on their Spotify account. Anyone not on the list gets a 403 and can't play. Each friend also needs Premium.

## 3. Host it

1. Put `index.html` (and later `songs.json`) in a GitHub repo.
2. Repo **Settings → Pages**, deploy from the main branch.
3. Share the Pages link.

To run it only on your computer instead: `python3 -m http.server 8000` in this folder, then open http://127.0.0.1:8000/.

## 4. Set the default playlist

Since February 2026, development-mode apps may only be able to read playlists the logged-in user owns or collaborates on. So a friend probably can't read *your* playlist directly. The workaround:

1. Open the game, go to **Settings**, paste your playlist link and click **Load**.
2. Click **Export songs.json** and commit that file next to `index.html`.
3. Click **Use default**. The game now loads `songs.json` for everyone, with no playlist API call, and only uses Spotify to play the songs.

Anyone can still paste their own playlist link in Settings (it works for playlists they own or collaborate on).

Alternatively, set `CONFIG.DEFAULT_PLAYLIST` to a playlist link if you'd rather skip `songs.json`.

## Notes

- The first clip is 0.1s long, and browser audio has some latency, so very short clips can sound clipped or slightly off. That's a Web Playback SDK limit.
- Clips start at the beginning of each song. Turn on **random start** in Settings for a harder mode.
- Songs load muted and pause at the start point so clips begin as quickly as possible. hi

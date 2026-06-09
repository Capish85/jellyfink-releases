# Jellyfink

A Jellyfin server plugin that connects to one or more **remote Jellyfin servers** and shows their
libraries on **your** server, but **no media files are copied**. Only the
remote's metadata, artwork and playback links live locally; the remote stays the source of truth and
streams on demand.

This repository is the **public distribution** for the plugin (the catalog manifest and release
packages). The source code lives in a separate repository.

> **Requires Jellyfin 10.11+.**

## What it does

- Browse and play another Jellyfin server's **Movies** and **TV/Anime** from your own server.
- Uses the **remote's own metadata** (title, plot, year, genres, cast, ratings, artwork), written
  locally so there are **no internet metadata lookups**.
- **No file copying** — items stream from the remote on play.
- **Automatic** — after each sync the libraries are scanned for you, so titles appear on the home
  screen and are playable without any manual steps.

## Features

- **Two streaming modes** — *Direct* (your client streams straight from the remote) or *Proxy* (your
  server relays the stream, with seek support), plus optional auto-fallback to proxy when the remote
  isn't directly reachable.
- **Durable, credential-free links** — set your server URL and playback links become local,
  HMAC-signed, and resolve a fresh remote stream on play (no remote credential stored on disk).
  Optional link expiry.
- **Subtitles** — the remote file's embedded subtitle tracks are available to pick, like a direct
  connection (native on direct-play clients). Optional subtitle sidecar download.
- **Two-way watch state & favorites** — resume/played and favorites sync both ways with the remote.
- **"REMOTE" library badge** — optionally stamps a badge on each library tile so remote libraries are
  obvious on every client (TV, mobile, web).
- **Library grouping** — optional name prefix (e.g. `Remote ·`).
- **Content filters** — import by year range, minimum rating, or excluded genres.
- **Per-server limits** — cap items per library and maximum streaming bitrate.
- **Collections** — optionally mirror the remote's collections (box sets).
- **Live sync** — optionally sync shortly after the remote reports a change (in addition to the
  schedule).
- **Quiet hours**, **failure webhooks** (Discord/generic), a per-server **online/offline** indicator,
  and a dedicated **`jellyfink.log`**.

## Install

1. In Jellyfin: **Dashboard -> Plugins -> Repositories -> +** and add this URL:

   ```text
   https://raw.githubusercontent.com/Capish85/jellyfink-releases/main/manifest.json
   ```

2. **Dashboard -> Plugins -> Catalog -> Jellyfink -> Install**, then **restart** Jellyfin.

Updates appear in the catalog automatically when a new version is published here.

## Quick start

1. Open **Dashboard -> Plugins -> Jellyfink** (it also appears in the left Plugins menu).
2. **Add server** -> enter the remote server's URL and a login (username/password or API key) ->
   **Test connection**.
3. **Load libraries** -> tick the ones you want to import.
4. *(Recommended)* set **Local server URL** to your server's reachable address so playback links don't
   embed a remote credential.
5. **Save**, then **Save & sync now**. The first sync (plus its scan) can take a while for large
   libraries; after that, titles appear on the home screen and play normally.

## Notes

- **No internet metadata** is fetched — the libraries use the metadata the remote already has.
- Watch progress and favorites are recorded on the remote against the account you configured for that
  server (you can map a server's watch state to a specific local user).
- Prefer an **API key** or a dedicated low-privilege remote user over your main password.

## Versions

See [Releases](https://github.com/Capish85/jellyfink-releases/releases) for each version's changelog.

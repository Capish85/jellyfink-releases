# Jellyfink

![Jellyfink](jellyfink.png)

A J ellyfin server plugin that connects to one or  more **remote Jellyfin servers** and makes t heir libraries browsable and playable on **yo ur** server — without copying any media fil es. Metadata, artwork, and playback links are  written locally; the remote stays the source  of truth and streams on demand.

> **Require s Jellyfin 10.11 or later.**

---

## How it  works

Jellyfink writes lightweight `.strm` f iles (stream links) and `.nfo` metadata files  into a local folder you choose. Jellyfin tre ats these like any other library content: the y appear on the home screen, are searchable,  and play back through Jellyfink's proxy or di rectly from the remote.

**Nothing is downloa ded except metadata and artwork.** Video neve r touches your disk unless you are proxying t he stream, in which case it passes through me mory and is never stored.

---

## Features

 ### Libraries
- Imports **Movies** and **TV/A nime** libraries from any number of remote Je llyfin servers.
- Uses the **remote's own met adata** — title, plot, year, genres, cast,  ratings, artwork — so there are no internet  lookups and no scraping delay.
- **Merge wit h a local library** — instead of creating a  separate remote library, Jellyfink can add i ts content into an existing local library (se e [Library merging](#library-merging) below). 
- **Mirror remote collections** (box sets) a nd **playlists** locally.
- **Content filters ** — import by year range, minimum communit y rating, or excluded genres.
- Per-server ** item cap** and **streaming bitrate limit**.

 ### Playback
- **Proxy mode** — your server  relays the stream (recommended; supports see k, works through any firewall).
- **Direct mo de** — your client streams straight from th e remote (lower server load; requires the cli ent to reach the remote directly).
- **Auto-f allback** — optionally fall back to proxy w hen the remote is not directly reachable from  the client.
- **Signed, credential-free stre am links** — no remote password is ever wri tten to disk or sent to clients. Links are HM AC-signed and resolve a fresh remote stream o n play. Optional link expiry.

### Sync
- **S cheduled sync** — runs on a configurable in terval (default 12 hours).
- **Delta sync** � �� after the first full sync, only re-process es movies or TV series that changed since the  last run. Dramatically faster on large libra ries.
- **Live sync** — optionally triggers  a sync shortly after the remote reports a li brary change (WebSocket).
- **Targeted librar y scan** — after writing changes, only the  affected Jellyfink folders are re-indexed, no t your entire server.
- **Quiet hours** — s uppress scheduled syncs during a configured t ime window.

### Watch state and metadata
- * *Two-way watch state** — played status and  resume positions sync from the remote. The ne wer timestamp wins on conflict.
- **Favorites ** — optionally mirror favorite state from  the remote.
- Configurable **"played" thresho ld** (default 90%).
- Per-server **user mappi ng** — assign a remote server's watch state  to a specific local user account.

### Appea rance
- Optional **"REMOTE" badge** stamped o n library tiles so remote content is obvious  on every client.
- Optional **server name pre fix** or **custom prefix** for library names. 
- **Manage library metadata settings** — p revents Jellyfin from re-scraping metadata th at Jellyfink already wrote.

### Operations
-  **Failure webhooks** — POST a message to a  Discord or generic webhook on sync failure o r unreachable server.
- **Stream audit log**  — log every proxied stream request.
- **Con figurable log rotation** (size and backup cou nt).
- Per-server **online/offline health ind icator** on the config page.
- Dedicated **`j ellyfink.log`** in your Jellyfin log folder.
 
---

## Library merging

This is Jellyfink's  most powerful feature. Instead of a separate  "Remote Movies" library, you can point Jelly fink's imported content **into an existing lo cal library**.

**Movies** — if a movie exi sts both locally and on the remote, Jellyfin  presents them as **alternate versions** of th e same item. On playback the user can pick wh ich version to stream (e.g. local 1080p vs re mote 4K).

**TV shows** — Jellyfin's automa tic series grouping matches shows by provider  ID (TheTVDB/TMDB). If you have Scrubs S1–S 3 locally and the remote has S1–S6, the mer ged library shows all 6 seasons seamlessly. L ocal files back the episodes you own; `.strm`  files back the rest. No duplicates, no manua l cleanup.

To use merging, select a local li brary in the **"Merge into"** dropdown next t o each remote library on the Servers tab.

-- -

## Install

### Via plugin catalog (recomm ended)

1. In Jellyfin: **Dashboard → Plugi ns → Repositories → ＋** and add:

   `` `
   https://raw.githubusercontent.com/Capish 85/jellyfink-releases/main/manifest.json
   ` ``

2. **Dashboard → Plugins → Catalog � � Jellyfink → Install**, then **restart Jel lyfin**.

Updates appear in the catalog autom atically when a new version is published.

## # Manual install

Download the latest `.zip`  from [Releases](https://github.com/Capish85/j ellyfink-releases/releases), extract `Jellyfi n.Plugin.Jellyfink.dll` into your Jellyfin pl ugin folder, and restart.

---

## Quick star t

1. Open **Dashboard → Plugins → Jellyf ink**.
2. **Servers tab → Add server** —  enter the remote server's URL and credentials  (username/password or API key) → **Test co nnection**.
3. **Load libraries** — tick th e libraries you want to import. Optionally pi ck a local library to merge into.
4. **Librar y tab** — set the **Library root folder** ( where Jellyfink writes its files, e.g. `F:\Je llyfink`).
5. Set **This server's URL** to yo ur server's externally reachable address so p layback links resolve correctly for clients o utside your LAN.
6. **Save and sync now**. Th e first sync can take a while for large libra ries; after that, content appears on the home  screen and plays normally.

**Tip:** Enable  **Delta sync** (Advanced tab) after the first  full sync. Subsequent syncs will be much fas ter since only changed content is re-processe d.

---

## Notes

- Prefer an **API key** or  a dedicated low-privilege remote account ove r your main admin password.
- Watch state is  recorded on the remote against the account co nfigured for that server.
- **Manage library  metadata settings** is recommended — it pre vents Jellyfin from overwriting the metadata  Jellyfink wrote with internet lookups.
- When  new content is added on the remote, Jellyfin k scans only its own folders during the next  sync — not your entire local library.

---
 
## Versions

See [Releases](https://github.c om/Capish85/jellyfink-releases/releases) for  the full changelog. 
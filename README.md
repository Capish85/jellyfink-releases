# Jellyfink

<p align="center"><img src="jellyfink.png" alt="Jellyfink" width="480" /></p>

A J  ellyfin server plugin that connects to one o r  more **remote Jellyfin servers** and makes  t heir libraries browsable and playable on * *yo ur** server — without copying any media  fil es. Metadata, artwork, and playback link s are  written locally; the remote stays the  source  of truth and streams on demand.

> ** Require s Jellyfin 10.11 or later.**

---

##  How it  works

Jellyfink writes lightweight  `.strm` f iles (stream links) and `.nfo` meta data files  into a local folder you choose. J ellyfin tre ats these like any other library  content: the y appear on the home screen, are  searchable,  and play back through Jellyfink 's proxy or di rectly from the remote.

**Not hing is downloa ded except metadata and artwo rk.** Video neve r touches your disk unless y ou are proxying t he stream, in which case it  passes through me mory and is never stored.
 
---

## Features

 ### Libraries
- Imports * *Movies** and **TV/A nime** libraries from an y number of remote Je llyfin servers.
- Uses  the **remote's own met adata** — title, plo t, year, genres, cast,  ratings, artwork —  so there are no internet  lookups and no scra ping delay.
- **Merge wit h a local library**  — instead of creating a  separate remote l ibrary, Jellyfink can add i ts content into a n existing local library (se e [Library mergi ng](#library-merging) below). 
- **Mirror rem ote collections** (box sets) a nd **playlists ** locally.
- **Content filters ** — import  by year range, minimum communit y rating, or  excluded genres.
- Per-server ** item cap**  and **streaming bitrate limit**.

 ### Playba ck
- **Proxy mode** — your server  relays t he stream (recommended; supports see k, works  through any firewall).
- **Direct mo de** � � your client streams straight from th e remo te (lower server load; requires the cli ent t o reach the remote directly).
- **Auto-f allb ack** — optionally fall back to proxy w hen  the remote is not directly reachable from  t he client.
- **Signed, credential-free stre a m links** — no remote password is ever wri  tten to disk or sent to clients. Links are HM  AC-signed and resolve a fresh remote stream  o n play. Optional link expiry.

### Sync
- * *S cheduled sync** — runs on a configurable  in terval (default 12 hours).
- **Delta sync ** � �� after the first full sync, only  re-process es movies or TV series that chang ed since the  last run. Dramatically faster o n large libra ries.
- **Live sync** — optio nally triggers  a sync shortly after the remo te reports a li brary change (WebSocket).
- * *Targeted librar y scan** — after writing c hanges, only the  affected Jellyfink folders  are re-indexed, no t your entire server.
- ** Quiet hours** — s uppress scheduled syncs d uring a configured t ime window.

### Watch s tate and metadata
- * *Two-way watch state**  — played status and  resume positions sync  from the remote. The ne wer timestamp wins on  conflict.
- **Favorites ** — optionally mi rror favorite state from  the remote.
- Confi gurable **"played" thresho ld** (default 90%) .
- Per-server **user mappi ng** — assign a  remote server's watch state  to a specific l ocal user account.

### Appea rance
- Optiona l **"REMOTE" badge** stamped o n library tile s so remote content is obvious  on every clie nt.
- Optional **server name pre fix** or **c ustom prefix** for library names. 
- **Manage  library metadata settings** — p revents Je llyfin from re-scraping metadata th at Jellyf ink already wrote.

### Operations
-  **Failu re webhooks** — POST a message to a  Discor d or generic webhook on sync failure o r unre achable server.
- **Stream audit log**  — l og every proxied stream request.
- **Con figu rable log rotation** (size and backup cou nt) .
- Per-server **online/offline health ind ic ator** on the config page.
- Dedicated **`j e llyfink.log`** in your Jellyfin log folder.
  
---

## Library merging

This is Jellyfink's   most powerful feature. Instead of a separat e  "Remote Movies" library, you can point Jel ly fink's imported content **into an existing  lo cal library**.

**Movies** — if a movie  exi sts both locally and on the remote, Jell yfin  presents them as **alternate versions**  of th e same item. On playback the user can  pick wh ich version to stream (e.g. local 108 0p vs re mote 4K).

**TV shows** — Jellyfin 's automa tic series grouping matches shows b y provider  ID (TheTVDB/TMDB). If you have Sc rubs S1–S 3 locally and the remote has S1� �S6, the mer ged library shows all 6 seasons  seamlessly. L ocal files back the episodes yo u own; `.strm`  files back the rest. No dupli cates, no manua l cleanup.

To use merging, s elect a local li brary in the **"Merge into"* * dropdown next t o each remote library on th e Servers tab.

-- -

## Install

### Via plu gin catalog (recomm ended)

1. In Jellyfin: * *Dashboard → Plugi ns → Repositories →  ＋** and add:

   `` `
   https://raw.github usercontent.com/Capish 85/jellyfink-releases/ main/manifest.json
   ` ``

2. **Dashboard � � Plugins → Catalog � � Jellyfink → I nstall**, then **restart Jel lyfin**.

Update s appear in the catalog autom atically when a  new version is published.

## # Manual insta ll

Download the latest `.zip`  from [Release s](https://github.com/Capish85/j ellyfink-rel eases/releases), extract `Jellyfi n.Plugin.Je llyfink.dll` into your Jellyfin pl ugin folde r, and restart.

---

## Quick star t

1. Ope n **Dashboard → Plugins → Jellyf ink**.
2 . **Servers tab → Add server** —  enter t he remote server's URL and credentials  (user name/password or API key) → **Test co nnect ion**.
3. **Load libraries** — tick th e li braries you want to import. Optionally pi ck  a local library to merge into.
4. **Librar y  tab** — set the **Library root folder** ( w here Jellyfink writes its files, e.g. `F:\Je  llyfink`).
5. Set **This server's URL** to yo  ur server's externally reachable address so  p layback links resolve correctly for clients  o utside your LAN.
6. **Save and sync now**.  Th e first sync can take a while for large l ibra ries; after that, content appears on the  home  screen and plays normally.

**Tip:** E nable  **Delta sync** (Advanced tab) after th e first  full sync. Subsequent syncs will be  much fas ter since only changed content is re -processe d.

---

## Notes

- Prefer an **AP I key** or  a dedicated low-privilege remote  account ove r your main admin password.
- Wat ch state is  recorded on the remote against t he account co nfigured for that server.
- **M anage library  metadata settings** is recomme nded — it pre vents Jellyfin from overwriti ng the metadata  Jellyfink wrote with interne t lookups.
- When  new content is added on th e remote, Jellyfin k scans only its own folde rs during the next  sync — not your entire  local library.

---
 
## Versions

See [Relea ses](https://github.c om/Capish85/jellyfink-r eleases/releases) for  the full changelog.  
# Jellyfink — plugin releases

Public distribution for the **Jellyfink** Jellyfin plugin (remote library sharing). Source code lives
in a separate repository; this repo only hosts the catalog manifest and release packages.

## Install in Jellyfin

Dashboard → Plugins → Repositories → **+**, then add this URL:

```text
https://raw.githubusercontent.com/Capish85/jellyfink-releases/main/manifest.json
```

Then Dashboard → Plugins → Catalog → **Jellyfink** → Install, and restart. Updates appear in the
catalog automatically when a new version is published here.

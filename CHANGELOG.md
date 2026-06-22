# KN Weather Center — Changelog

## 2026-06-22 — PWA assets and logo fix

- **Added**: `profile_picture.png` (1024x1024) — bundled in the repo as the canonical logo/icon asset. Used for site header logo, iOS home screen icon (`apple-touch-icon`), and PWA manifest icons (192/512/1024 sizes).
- **Added**: `sw.js` service worker for offline caching of the app shell and PWA installability (Chrome "Install" prompt).
- **Fixed**: `logoUrl` previously pointed to an expiring CDN URL (`z-cdn-media.chatglm.cn`) — now uses the locally-bundled `./profile_picture.png` so the logo always loads.
- **Fixed**: `sw.js` precache list referenced the old filename `kn_weather_center.html` — updated to `index.html`. Cache version bumped `v1` → `v2` so existing users get a fresh cache on next load.
- **Improved**: PWA manifest icons switched from inline SVG data URI to the PNG — renders crisper on Android, Chrome OS, and iOS home screens.

## 2026-06-22 — Alert Polygon Map fix

- **Fixed**: Beach Hazards Statement (and other zone-based alerts) now render on the Alert Polygon Map. Previously, alerts with `geometry: null` were silently skipped. Now resolves UGC zone codes (MIZ###, MIC###, LMZ###, etc.) via the NWS zones API and draws all referenced polygons.
- **Added**: Tri-County Focus toggle to filter the map to alerts affecting Van Buren, Allegan, or Kalamazoo counties only.
- **Added**: Permanent purple dashed borders around the three monitored counties — always visible on top of any alert shading.
- **Added**: Purple pill-shaped labels ("Van Buren", "Allegan", "Kalamazoo") at each county centroid, toggleable via the "County Labels" checkbox.
- **Performance**: Zone geometries are cached in memory after first fetch — subsequent refreshes are instant.

## Source

Built and maintained from the KN Weather Center project.
Alert colors match the official NWS Hazards Map color codes (https://www.weather.gov/help-map).

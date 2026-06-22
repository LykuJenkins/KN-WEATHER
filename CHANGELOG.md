# KN Weather Center — Changelog

## 2026-06-22 — Alert Polygon Map fix

- **Fixed**: Beach Hazards Statement (and other zone-based alerts) now render on the Alert Polygon Map. Previously, alerts with `geometry: null` were silently skipped. Now resolves UGC zone codes (MIZ###, MIC###, LMZ###, etc.) via the NWS zones API and draws all referenced polygons.
- **Added**: Tri-County Focus toggle to filter the map to alerts affecting Van Buren, Allegan, or Kalamazoo counties only.
- **Added**: Permanent purple dashed borders around the three monitored counties — always visible on top of any alert shading.
- **Added**: Purple pill-shaped labels ("Van Buren", "Allegan", "Kalamazoo") at each county centroid, toggleable via the "County Labels" checkbox.
- **Performance**: Zone geometries are cached in memory after first fetch — subsequent refreshes are instant.

## Source

Built and maintained from the KN Weather Center project.
Alert colors match the official NWS Hazards Map color codes (https://www.weather.gov/help-map).

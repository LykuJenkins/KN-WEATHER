# KN Weather Center — Changelog

## 2026-06-22 — Top-level tab navigation + alert notifications

- **Added**: 4 top-level category tabs at the top of the page:
  - **Current Conditions** (8 panels) — Current, Hourly Forecast, 7-Day, Threats, WWA Map, Alert Polygon Map, WWA Summary, WWA Map Color Code
  - **NWS** (10 panels) — KGRR Radar & Loops, Hourly Forecast Graph, Forecast Discussion, NWS GRR Feed, NWS National Feed, NWS Gridpoint Forecast, GRR Office Headlines, Observations & Forecast Text, Tri-County Surface Obs Network, Precipitation & Temperature Outlook
  - **SPC** (3 panels) — SPC Severe Weather Outlooks, Observations Map, SPC Feed
  - **Storm Spotting** (7 panels) — Aviation, Surface Front Analysis, Forecasted SkewT Model Soundings, Storm Environment, Lightning Tracker, Local Storm Reports, Road Weather Cameras
- **Performance**: Only the active tab's panels are refreshed on each cycle. Tab 1 (Current Conditions) panels ALWAYS refresh regardless of which tab is active, so alerts and WWA info stay live in the background. This cuts the number of API calls per refresh cycle roughly in half when viewing non-Current tabs.
- **Added**: Floating alert notification banner. When a new NWS alert arrives while the user is on a non-Current-Conditions tab, a banner appears at the top of the viewport showing the alert event type and affected areas. Stacks up to 3 visible at once; auto-dismisses after 30 seconds. Tapping the banner switches to Current Conditions and dismisses all notifications. The EAS alert sound still plays (existing behavior).
- **Added**: Pulsing red alert dot on tab buttons. Visible on all tabs whenever there are active NWS alerts, so the user always knows to check Current Conditions. Clears automatically when alerts expire.
- **Added**: Active tab is saved to localStorage and restored on page refresh.
- **Changed**: Facebook panel is now a persistent footer visible on ALL tabs (not part of any tab's panel count). It lives in its own always-active tab pane at the bottom of the page.
- **Changed**: When switching to the Current Conditions tab, Leaflet's `alertMapInstance.invalidateSize()` is called to recompute the map layout after being hidden (Leaflet gets confused by `display:none` containers).
- **Changed**: `refreshAll()` rewritten to be tab-aware. Always refreshes Tab 1 panels (fWx, fAl, fAlertMap, fWWASummary, rWWA) plus only the active tab's other panels.

## 2026-06-22 — Panel layout & collapse-state persistence

- **Changed**: Moved Threats panel to slot 4 (immediately after 7-Day, before WWA Map). It now sits with the top at-a-glance panels instead of being buried at slot 7.
- **Added**: Panel collapse/expand state is now persisted to `localStorage` under `kn_weather_panel_state`. When a user collapses or expands any panel, the choice survives page refresh and return visits.
- **Added**: New `defaultCollapsed` flag on panel definitions. Panels with `defaultCollapsed: true` start collapsed on first visit (or any time the user has never toggled them). Once the user toggles a panel, their explicit choice takes precedence over the default.
- **Changed**: WWA Map Color Code panel now starts collapsed by default. The full color grid is large and was eating screen real estate; users who want it visible can expand it once and the choice will be remembered.
- **Fixed**: `resetPanelOrder()` no longer wipes the user's collapse preferences — it only resets the panel *order* (a separate concern). Collapse state is re-applied after the rebuild.

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

# TuXia (途匣)

[Chinese version](README.zh.md)

![TuXia Logo](resources/logo_256.png)

TuXia (Chinese: 途匣) is an offline-first rural bus route library and timetable transfer planner for county, township and village riders.

## Why TuXia

Rural passenger transport is poorly served by mainstream map products. Route notices are scattered across county portals and paper announcements, timetables change without warning, and a shortest-distance answer often cannot be completed on the day. TuXia turns reviewed public notices into a versioned route library and a downloadable route pack, so riders can search villages and stops and plan multi-leg journeys even when the network is unavailable.

## Features

- **Offline route packs** — checksummed regional packs installed on the device; search and planning keep working without a network
- **Place search** — villages, towns and stops with aliases, administrative scoping and upcoming departures
- **Route detail** — ordered stops, timetable, fare, direction, service type and suspension or seasonal status
- **Deterministic transfer planning** — walking transfers, minimum waiting time, last-departure checks and ranked plans
- **Map and walking hand-off** — route and plan overlays with an external map-app hand-off for the walking segment
- **Trust disclosure** — source note, confidence grade and last-updated date on every route, plus a plain-language disclaimer
- **Rider corrections** — evidence photos with checksums, offline drafts, idempotent submission and an audited review queue
- **Operator console** — source registry, collection runs, side-by-side review, versioned route library, pack build and audit trail

## Download

| Component | Download | Requirements |
|-----------|----------|--------------|
| Android rider app (APK) | [Latest Release](https://github.com/KylinLabAI/TuXia-App/releases/latest) | Android 8.0+ (arm64) |
| Backend bundle (Python, Linux x64) | [Latest Release](https://github.com/KylinLabAI/TuXia-App/releases/latest) | Python 3.14; the tarball ships the pilot seed fixtures |
| Operator console bundle (static) | [Latest Release](https://github.com/KylinLabAI/TuXia-App/releases/latest) | Evergreen browsers |
| Backend container image | `docker pull crpi-5w5kegfxurclu2lz.cn-hangzhou.personal.cr.aliyuncs.com/kylinlab2026/tuxia:latest` | Linux container host |

The Android App Bundle (AAB) is published alongside the APK for store submission. Each release asset has a sibling `.sha256` file. The same release assets are mirrored on [GitHub](https://github.com/KylinLabAI/TuXia-App/releases/latest) and [Gitee](https://gitee.com/KylinLab/TuXia-App/releases/latest).

## Quick Start

### Riders

1. Download the APK from the latest release and allow installation from this source.
2. Open TuXia. The top bar shows **未安装线路包** (no route pack installed).
3. Open the **线路包** (Packs) tab and install the pilot county pack.
4. Search a village, town or stop, or plan a journey — both work offline after the pack is installed.

### Operators

1. Unpack the backend bundle (or pull the container image) and start the API on port 8421.
2. Set `TUXIA_SEED_PASSWORD` before first boot so the pilot console accounts are created, and keep `TUXIA_SECURE_COOKIES=true` behind HTTPS.
3. Serve the operator console as a static bundle behind the same reverse proxy, routing `/api/v1` to the backend.
4. Sign in as `reviewer`, `operator`, `owner` or `admin`, work the review queue, and build a route pack once accepted changes exist.

Local development over plain HTTP must use `TUXIA_SECURE_COOKIES=false` explicitly; the default stays secure.

## Documentation

- [User Manual (English)](../user_manual/user_manual.md)
- [User Manual (Chinese)](../user_manual/user_manual.zh.md)
- [Release Notes](https://github.com/KylinLabAI/TuXia-App/releases/latest)

## Architecture

| Component | Runtime | Responsibility |
|-----------|---------|----------------|
| Rider app | Flutter (Dart), Android | Offline search, route detail, transfer planner, packs, corrections, map |
| Backend | Python 3.14, FastAPI | Source registry, collection, change detection, review, route library, pack builder, public API at `/api/v1` |
| Console | React, Vite, TypeScript | Review queue, candidate comparison, source registry, refresh health, pack status, audit trail |
| Data | SQLite, canonical JSON + gzip packs | Route library state and deterministic, checksummed release packs |

The route pack schema is the only backend-app contract. Search and journey planning run entirely on the device.

## Known Limitations

- The current release targets one pilot county (`pilot-county`).
- Phone-reservation (on-demand) services are shown for information but are not planned as fixed departures.
- Map tiles require a network connection; route and plan text remain available offline.

## Contact

kylinlab.tech

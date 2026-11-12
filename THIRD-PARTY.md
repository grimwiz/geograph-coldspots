# Third-party components and data

Everything in `lib/` is bundled so the page works from a local file without a build step.

| File | What | Licence |
|---|---|---|
| `lib/exifr.full.umd.js` | [exifr](https://github.com/MikeKovarik/exifr) 7.1.3 — EXIF reader | MIT, `lib/LICENSE-exifr.txt` |
| `lib/proj4.js` | [proj4js](https://github.com/proj4js/proj4js) 2.22.0 — coordinate transformation | MIT, `lib/LICENSE-proj4.md` |
| `lib/geotiff.js` | [geotiff.js](https://github.com/geotiffjs/geotiff.js) 3.0.5 — reads the OSTN15 grid | MIT, `lib/LICENSE-geotiff.txt` |
| `lib/leaflet/` | [Leaflet](https://leafletjs.com/) 1.9.4 — map | BSD 2-Clause, `lib/LICENSE-leaflet.txt` |
| `lib/ostn15-grid.js` | Ordnance Survey's OSTN15 transformation grid (`uk_os_OSTN15_NTv2_OSGBtoETRS.tif` from [PROJ-data](https://github.com/OSGeo/PROJ-data/tree/master/uk_os)), base64-encoded | BSD licence, as published by Ordnance Survey with its [OSTN15 developer resources](https://www.ordnancesurvey.co.uk/products/os-net/for-developers) |
| `lib/subjects.js` | Offline fallback copy of Geograph's subject tags (taken 2026-11-08); the page normally uses the live list from `tags/subject.json.php` | Data from [Geograph Britain and Ireland](https://www.geograph.org.uk/) |

Services used at run time (not bundled): Geograph's API, tag lists and writing guidance (`skill.php`), OpenStreetMap's Overpass API (data © OpenStreetMap
contributors, ODbL), Esri World Imagery tiles (© Esri, Maxar, Earthstar Geographics) and, optionally,
OpenStreetMap tiles.

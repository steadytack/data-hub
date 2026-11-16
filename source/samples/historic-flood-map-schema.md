# Historic Flood Map: real file inspected (2026-11-14)

Downloaded key-less from the Environment Agency link in the build brief and read read-only with SQLite. The file itself is not kept here (20 MB zip, 46 MB GeoPackage); `historic-flood-map-data-version.txt` beside this file is the source's own version file, saved as given.

| Item | Found |
|---|---|
| Data version file | "This is the September 2026 release." (free text, not a structured date) |
| Zip | `Historic_Flood_Map.gpkg.zip`, 20,128,338 bytes; one entry `Historic_Flood_Map.gpkg`, 46,129,152 bytes |
| Layers | One feature table, `Historic_Flood_Map`; `gpkg_contents.last_change` 2026-09-07T13:46:20Z; description empty |
| Rows | 21,578 |
| Coordinate system | EPSG:27700 (British National Grid); extent x 90,094 to 655,506, y 10,072 to 653,502 |
| Geometry | Column `shape`, declared `GEOMETRY`; every row is a MultiPolygon (WKB type 6); no null geometries |
| Columns | `objectid` INTEGER not null (1 to 21,578, unique); `shape_leng` REAL (perimeter, 0.05 to 968,667, no nulls); `layer` TEXT(30) (one value, "Historic Flood Map", no nulls) |
| Not in the file | No event date, flood source, or event name columns; no column descriptions |

What this means for the spec: the attribute data is almost empty, so the data dictionary is mostly owner-supplied or harvested from the EA's catalogue record; freshness must parse the version text ("September 2026 release") rather than a date field; geometry validity has not been checked yet (slice 3 does that).

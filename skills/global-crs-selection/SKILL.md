---
name: global-crs-selection
description: "Use when choosing a coordinate reference system for world-scale or near-global geospatial work, including global maps, world choropleths, antimeridian issues, polar coverage, global grids, or deciding between EPSG:4326, EPSG:3857, Equal Earth, LAEA, Robinson, or other world projections."
---

# Global CRS Selection

Use this skill when the spatial extent is continental, global, transoceanic, or crosses the antimeridian and ordinary local projection advice is not enough.

## Goals

- Choose a defensible CRS for world or near-world work.
- Match the projection to the task: storage, display, measurement, thematic mapping, or modelling.
- Avoid common global CRS failures such as Web Mercator misuse, antimeridian splits, and polar distortion.
- State tradeoffs clearly when no single projection preserves everything.

## Core Facts

- `EPSG:4326` is a geographic CRS in degrees. It is a good interchange format, but not a projected measurement surface.
- `EPSG:3857` is for web mapping and visualisation. It is not appropriate for precision distance, area, or global statistical analysis.
- For world thematic maps, equal-area projections are usually the safest default.
- No single world CRS preserves area, shape, distance, and direction at the same time. The correct choice depends on the task.
- Polar and antimeridian-spanning data need explicit handling even when the CRS itself is valid.

## Task-Based Selection

### 1. Data exchange or API storage
- Default to `EPSG:4326` when you need broad interoperability.
- Keep in mind that coordinates are angular units in degrees.
- Do not compute area, buffer distance, or planar nearest-neighbour analysis directly in `EPSG:4326` unless the tooling is explicitly geodesic.

### 2. Slippy maps and tiled web display
- Use `EPSG:3857` only when the target is a standard web map stack.
- State clearly that the choice is for display compatibility, not analytical correctness.

### 3. World choropleths or global thematic maps
- Prefer an equal-area projection.
- Good defaults:
  - Equal Earth: good general-purpose world thematic map with preserved relative area.
  - Lambert Azimuthal Equal Area: strong choice for hemisphere-focused or pole-centred views.
  - Cylindrical Equal Area variants: useful when you need a simple global equal-area frame and accept shape distortion.

### 4. World reference maps for presentation
- Use a compromise projection when visual balance matters more than metric properties.
- Typical candidates: Robinson, Winkel Tripel, Natural Earth.
- Do not treat these as measurement-safe.

### 5. Global distance or area analysis
- Prefer geodesic calculations on the ellipsoid when possible.
- If a projected CRS is required, pick one that matches the region and metric of interest rather than forcing one world CRS onto all tasks.
- For hemisphere-scale analysis, consider a custom `+proj=laea` centred on the study area.

### 6. Polar work
- Avoid equatorial or generic world projections for Arctic and Antarctic analysis.
- Use a pole-centred azimuthal or stereographic projection appropriate to the task.

## Common Country Defaults For Meters Or Feet

Use this section when the user is really asking for a practical projected CRS with linear units for local analysis, buffering, area, or engineering-style measurements.

| Country / region | Common projected CRS choice | Units | Notes |
|---|---|---|---|
| United States | State Plane zone for the state or county | Feet or meters, depending on zone definition | Best default for local engineering and parcel work. Do not use one national CRS for precise local measurement everywhere. |
| United States | UTM zone covering the study area | Meters | Good practical default for regional analysis when State Plane is unnecessary. |
| Canada | UTM zone for the province or study area | Meters | Common default for local and regional work. Some provinces also use their own standard systems. |
| United Kingdom | British National Grid (`EPSG:27700`) | Meters | Standard default for Great Britain. |
| Ireland | Irish Transverse Mercator (`EPSG:2157`) | Meters | Good default for modern work in Ireland. |
| France | Lambert-93 (`EPSG:2154`) | Meters | Standard metropolitan France default. |
| Germany | ETRS89 / UTM zone 32N or 33N | Meters | Pick the UTM zone that actually covers the study area. |
| Spain | ETRS89 / UTM zone 28N, 29N, 30N, or 31N | Meters | Use the correct zone; Spain spans multiple zones. |
| Italy | ETRS89 / UTM zone 32N or 33N | Meters | Use the appropriate zone for the region. |
| Netherlands | Amersfoort / RD New (`EPSG:28992`) | Meters | Standard Dutch projected CRS. |
| Belgium | Belgian Lambert 2008 (`EPSG:3812`) | Meters | Common modern national default. |
| Switzerland | CH1903+ / LV95 (`EPSG:2056`) | Meters | Standard Swiss projected CRS. |
| Portugal | ETRS89 / Portugal TM06 (`EPSG:3763`) | Meters | Good mainland Portugal default. |
| Australia | GDA2020 / MGA zone for the area | Meters | Use the correct MGA zone for local analysis; use Australian Albers for nationwide area summaries. |
| New Zealand | NZGD2000 / New Zealand Transverse Mercator 2000 (`EPSG:2193`) | Meters | Standard national default for NZ. |
| Japan | Japan Plane Rectangular CS zone for the prefecture or project | Meters | Best for precise local work; Japan uses many zones. |
| South Korea | Korea 2000 / Unified CS (`EPSG:5179`) | Meters | Common default for national and regional work. |
| Brazil | SIRGAS 2000 / UTM zone for the area | Meters | Brazil spans many UTM zones; choose locally. |
| Mexico | UTM zone for the area | Meters | Better than forcing one CRS across the whole country for local analysis. |
| South Africa | Lo zone or national projected system used by the data provider | Meters | Verify the source CRS carefully; local systems are common. |

### Fast Rules For Units

- If the user says "I need meters," recommend a local projected CRS, usually a national grid or the correct UTM zone.
- If the user says "I need feet" in the United States, prefer the appropriate State Plane definition and confirm whether the workflow expects US survey feet or international feet.
- If the country spans several UTM zones, do not recommend one zone blindly unless the study area is confined to that zone.
- For countrywide statistical mapping, a national equal-area CRS may be better than UTM even when the user still wants meter-based units.
- If the task is legal, cadastral, or engineering work, prefer the CRS used by the authoritative local agency over a generic fallback.

### What To Say When Unsure

- "For precise measurement, I need the actual study area within the country before I pick the best projected CRS."
- "For local work, use the relevant national grid or UTM zone; for countrywide mapping, use the country's standard national projection if one exists."
- "If the source data already comes from an authoritative local agency, keep its projected CRS unless there is a strong reason to transform it."

## Workflow

1. Establish the spatial extent.
   - Single country, continent, hemisphere, global, or global with polar emphasis.
   - Note whether geometries cross the antimeridian.
2. Identify the real task.
   - Storage and exchange.
   - Tile display.
   - Thematic mapping.
   - Area or distance analysis.
   - Interpolation or modelling.
3. Check whether the current CRS is geographic or projected.
   - If `EPSG:4326`, confirm whether downstream operations are geodesic or planar.
   - If `EPSG:3857`, verify whether it was chosen only for web display.
4. Pick the projection family.
   - Equal-area for area comparison.
   - Compromise for presentation.
   - Azimuthal for hemisphere or pole-centred views.
   - Geographic only for storage, exchange, or geodesic workflows.
5. Handle edge cases.
   - Antimeridian crossing.
   - Polar extent.
   - Mixed CRS layers.
   - Axis-order confusion.
6. State the tradeoff explicitly.
   - What is preserved.
   - What is distorted.
   - Why that distortion is acceptable for this task.

## Decision Rules

- If the user says "world map", ask whether the goal is visual presentation or quantitative comparison.
- If the user needs global area comparison, do not recommend `EPSG:3857`.
- If the user needs a broad, safe global default and no other constraint dominates, start with Equal Earth for thematic world maps.
- If the user is publishing to a standard web map, `EPSG:3857` is acceptable for rendering only.
- If the workflow crosses the antimeridian, mention geometry splitting, wrapping, or longitude normalization before downstream overlay work.
- If the workflow is polar, do not leave the data in a generic world projection without justification.

## Common Failure Modes

- Treating `EPSG:4326` as if degrees were metres.
- Running buffer or area calculations in `EPSG:3857` because the coordinates look metric.
- Choosing a world projection because it is familiar rather than because it matches the task.
- Ignoring the antimeridian, causing polygons or raster bounds to wrap incorrectly.
- Assuming one CRS can serve storage, visualisation, and measurement equally well.

## Output Format

```text
Spatial extent:
Current CRS:
Actual task:
Recommended CRS or projection family:
Why this choice fits:
What distortion or limitation remains:
Edge cases to handle:
Validation checks:
```

## Guidelines

- Prefer named projection families over random EPSG guessing when working at global scale.
- Mention geodesic alternatives when the user asks for accurate world distances.
- Be explicit when a recommendation is a PROJ definition rather than a single EPSG code.
- Separate storage CRS recommendations from analysis CRS recommendations.
- For world maps, explain the tradeoff in plain language instead of assuming the user knows projection theory.

## Example Triggers

- "Which CRS should I use for a world choropleth?"
- "I need a projection for global climate polygons that cross the antimeridian"
- "Should I use EPSG:4326 or EPSG:3857 for a world dataset?"
- "What is a good CRS for Arctic and Antarctic coverage in the same workflow?"
- "I need a global equal-area projection for analysis"
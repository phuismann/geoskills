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
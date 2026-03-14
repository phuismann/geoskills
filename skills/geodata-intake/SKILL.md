---
name: geodata-intake
description: "Use when inspecting a new geospatial dataset for the first time, including GeoJSON, Shapefile, GeoPackage, raster, CSV with coordinates, or mixed geo data. Helpful for schema review, CRS detection, geometry summary, dataset extent, and first-pass analysis planning."
---

# Geodata Intake

Use this skill for the first structured pass over unfamiliar geospatial data.

## Goals

- Identify what the dataset is and how it is organized.
- Surface the coordinate reference system or note that it is missing.
- Summarize geometry or raster characteristics.
- Detect obvious analysis blockers early.
- Produce a short, decision-ready intake summary.

## Workflow

1. Determine the dataset type.
   - Vector, raster, tabular-with-location, or mixed package.
   - Note format details such as GeoJSON, Shapefile sidecars, GeoPackage layers, GeoTIFF bands, or CSV latitude and longitude fields.
2. Identify spatial structure.
   - For vector data: geometry type, feature count, layer names, and core attributes.
   - For raster data: band count, cell size, nodata handling, and likely value meaning.
3. Check reference system information.
   - Record CRS, EPSG if available, units, and whether the CRS is explicit or inferred.
   - If missing or ambiguous, state that clearly instead of guessing.
4. Check geographic footprint.
   - Report dataset extent, coverage region, and whether coordinates look plausible.
   - Flag suspicious extents such as degrees treated as meters or swapped latitude and longitude.
5. Identify data readiness issues.
   - Missing metadata, null geometry, invalid geometry, duplicate features, broken field names, unknown units, inconsistent encodings, or mixed precision.
6. End with a concise recommendation.
   - Ready for analysis.
   - Needs cleanup first.
   - Needs CRS clarification before any spatial operation.

## Output Format

Use this structure when reporting results:

```text
Dataset type:
Spatial structure:
CRS:
Extent / coverage:
Key attributes or bands:
Immediate issues:
Recommended next step:
```

## Guidelines

- Do not assume a CRS from coordinates unless you say it is a hypothesis.
- Separate verified facts from inferred interpretation.
- Prefer simple, concrete language over GIS jargon when summarizing for non-specialists.
- If multiple layers are present, summarize each briefly and then give a package-level conclusion.

## Example Triggers

- "Inspect this GeoPackage before we use it"
- "What is in this shapefile and is it analysis-ready?"
- "Summarize this raster dataset for a geospatial workflow"
- "Check whether this CSV can be mapped safely"
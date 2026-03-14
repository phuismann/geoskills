---
name: crs-and-reprojection
description: "Use when working with coordinate reference systems, EPSG codes, datum mismatches, reprojection choices, projected versus geographic CRS, unit confusion, or map alignment problems in geospatial data."
---

# CRS And Reprojection

Use this skill when the core problem is spatial reference correctness.

## Goals

- Determine whether the current CRS information is known, missing, or wrong.
- Explain the operational impact of the CRS on measurements, overlays, and map alignment.
- Recommend a safe target CRS for the task.
- Distinguish assigning a CRS from reprojecting data.

## Workflow

1. Establish the current state.
   - What CRS is declared?
   - Is it geographic or projected?
   - What units are implied?
2. Check whether the CRS matches the task.
   - Web mapping.
   - Distance or area measurement.
   - Local engineering workflow.
   - Regional or global analysis.
3. Identify the failure mode.
   - Missing CRS metadata.
   - Wrong CRS assignment.
   - Mixed layers in different CRS.
   - Datum mismatch.
   - Coordinates with suspicious magnitude or axis order.
4. Recommend the correction path.
   - Assign the correct source CRS if metadata is missing but known.
   - Reproject only after the source CRS is trustworthy.
   - Choose a target CRS based on the analysis objective, not habit.
5. State validation checks.
   - Overlay known reference layers.
   - Confirm units.
   - Sanity-check extent and feature alignment.

## Decision Rules

- Use a local or regional projected CRS for accurate distance and area work.
- Use geographic CRS only when the workflow truly operates in angular coordinates.
- Avoid Web Mercator for precision measurement analysis.
- If the source CRS is uncertain, stop and resolve that before downstream processing.

## Output Format

```text
Observed CRS situation:
Why it is a problem or not:
Recommended source CRS action:
Recommended target CRS:
Validation checks:
Risks / assumptions:
```

## Example Triggers

- "These layers do not line up, help me diagnose the projection issue"
- "Which EPSG should I use for area calculations in this region?"
- "Do I assign a CRS or reproject this dataset?"
- "Why are the coordinates huge and the map is offset?"
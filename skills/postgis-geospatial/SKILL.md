---
name: postgis-geospatial
description: "Use when working with PostGIS in PostgreSQL for geospatial SQL, including geometry or geography design, SRID handling, spatial indexing, ST_Intersects or ST_DWithin queries, raster or vector analysis patterns, and query tuning for PostGIS workloads."
---

# PostGIS Geospatial

Use this skill for spatial database work that specifically targets PostgreSQL with PostGIS.

## Goals

- Choose the right PostGIS types and functions for the task.
- Avoid SRID and unit mistakes that lead to wrong answers.
- Use spatial indexes and query structure effectively.
- Produce SQL that is correct first and performant second.

## Workflow

1. Define the data model.
   - Determine whether the table should use `geometry` or `geography`.
   - Record the expected SRID and geometry type.
2. Match the type to the analysis.
   - Use `geometry` for most projected, local, or topology-heavy workflows.
   - Use `geography` when great-circle distance on lon or lat data is the actual requirement.
3. Plan the query.
   - Load or transform inputs.
   - Align SRIDs before comparison.
   - Use the appropriate spatial predicate or measurement function.
   - Return a result shaped for application use.
4. Add performance controls.
   - Make sure a spatial index exists where it matters.
   - Use index-friendly predicates such as `ST_DWithin` when appropriate.
   - Avoid repeated transforms inside large joins when the source can be normalized earlier.
5. Validate behavior.
   - Check SRIDs.
   - Sanity-check distance units.
   - Confirm that query results match expected spatial relationships.

## PostGIS-Specific Rules

- Never compare geometries in different SRIDs without making the conversion explicit.
- Do not use `ST_Distance` alone for proximity filtering when `ST_DWithin` is more appropriate.
- Prefer storing data in a consistent analysis-ready SRID rather than transforming ad hoc in every query.
- State whether distances and areas are in meters, feet, or projected map units.
- If a query is slow, inspect indexing and data model choices before rewriting the whole SQL shape.

## Common Tasks

- Create a PostGIS table for point, line, or polygon data.
- Add SRIDs and clean up imported spatial tables.
- Perform point-in-polygon or nearest-neighbor analysis.
- Compute buffers, distances, areas, or intersections.
- Tune large spatial joins.
- Review whether `geometry` or `geography` is the correct choice.

## Output Format

```text
PostGIS objective:
Recommended spatial type and SRID strategy:
Key SQL approach:
Index or performance considerations:
Validation checks:
Risks / assumptions:
```

## Example Triggers

- "Write a PostGIS query for nearest features within 500 meters"
- "Should this table use geometry or geography?"
- "Help fix an SRID mismatch in PostGIS"
- "Tune this PostGIS spatial join"
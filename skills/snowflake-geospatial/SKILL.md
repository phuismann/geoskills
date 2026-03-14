---
name: snowflake-geospatial
description: "Use when working with geospatial data in Snowflake, including GEOGRAPHY columns, WKT or GeoJSON loading, spatial joins, ST_INTERSECTS or ST_DISTANCE queries, geohash workflows, and performance-aware SQL design for geo analytics in Snowflake."
---

# Snowflake Geospatial

Use this skill for geospatial workflows that specifically target Snowflake.

## Goals

- Choose Snowflake geospatial patterns that are correct and practical.
- Help load, transform, and query geospatial data using Snowflake SQL.
- Avoid common mistakes around types, CRS assumptions, and expensive spatial joins.
- Produce query plans that are explicit about correctness and cost.

## Workflow

1. Clarify the storage shape.
   - Is the source data arriving as GeoJSON, WKT, WKB, coordinate columns, or an upstream table?
   - Identify the target geospatial column and whether the data should be stored as `GEOGRAPHY`.
2. Confirm spatial semantics.
   - Snowflake geospatial work is commonly centered on `GEOGRAPHY` with longitude and latitude semantics.
   - If the task needs projected analysis or strict planar behavior, state that Snowflake may need preprocessing outside the warehouse.
3. Design the SQL path.
   - Loading and parsing.
   - Cleaning and normalizing.
   - Spatial predicate or measurement.
   - Output summarization.
4. Guard the query for scale.
   - Reduce rows before expensive spatial predicates.
   - Prefer bounding or prefilter logic when possible.
   - Avoid unnecessary repeated geospatial construction inside joins.
5. Validate results.
   - Spot-check counts.
   - Sanity-check known distances or containment cases.
   - Confirm coordinate ordering and null handling.

## Snowflake-Specific Rules

- Be explicit when creating `GEOGRAPHY` values from raw text or JSON.
- Treat axis order carefully: longitude first, then latitude, unless the source format guarantees otherwise.
- Do not assume Snowflake is the right place for every projection-heavy workflow.
- Separate correctness guidance from performance guidance.
- When writing SQL, prefer readable CTEs over dense single-block queries for spatial logic.

## Common Tasks

- Load GeoJSON or WKT into a Snowflake table.
- Build point geometries from longitude and latitude columns.
- Run point-in-polygon joins.
- Filter records by distance from a point.
- Aggregate events by administrative boundary.
- Prepare geospatial outputs for BI or downstream analytics.

## Output Format

```text
Snowflake geospatial objective:
Recommended data type and storage pattern:
Key SQL approach:
Performance considerations:
Validation checks:
Risks / assumptions:
```

## Example Triggers

- "Write a Snowflake query to join points to polygons"
- "How should I store GeoJSON in Snowflake for geo analytics?"
- "Help optimize this Snowflake spatial join"
- "Use Snowflake geography functions to filter by distance"
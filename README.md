# geoskills

Minimal agent skills for geodata science and geospatial data work.

This repository follows the same basic pattern used by Anthropic's skills repository: each skill lives in its own folder and includes a `SKILL.md` file with YAML frontmatter plus task-specific instructions.

## Repository Layout

```text
skills/
  geodata-intake/
    SKILL.md
  crs-and-reprojection/
    SKILL.md
  geodata-qaqc/
    SKILL.md
  snowflake-geospatial/
    SKILL.md
  postgis-geospatial/
    SKILL.md
template/
  geo-skill-template/
    SKILL.md
```

## Starter Skills

### `geodata-intake`
Use for first-pass inspection of new geospatial datasets. Focuses on format, CRS, schema, extent, geometry type, and immediate risks.

### `crs-and-reprojection`
Use when the task involves projections, EPSG codes, datum mismatches, unit confusion, or reprojection decisions.

### `geodata-qaqc`
Use for basic quality control of vector or raster datasets before analysis, delivery, or publication.

### `snowflake-geospatial`
Use for geospatial SQL workflows in Snowflake, including `GEOGRAPHY`, spatial joins, distance filters, and warehouse-aware query planning.

### `postgis-geospatial`
Use for geospatial SQL workflows in PostgreSQL/PostGIS, including geometry versus geography choices, indexing, SRID handling, and spatial query tuning.

## Design Rules

- Keep skills narrow and task-shaped.
- Make the `description` concrete so an agent can discover the skill reliably.
- Prefer checklists and output formats over long prose.
- Put reusable domain guidance in the skill, not only in examples.
- Add scripts or reference assets only when the workflow actually needs them.

## How To Add A Skill

1. Copy `template/geo-skill-template` to a new folder under `skills/`.
2. Rename the folder to a lowercase, hyphenated identifier.
3. Set `name` to match the folder name exactly.
4. Rewrite `description` so it includes the phrases a user would naturally say.
5. Replace the template sections with a concrete workflow, examples, and constraints.

## Next Good Additions

- `spatial-join-planning`
- `raster-preprocessing`
- `remote-sensing-index-selection`
- `openstreetmap-feature-extraction`
- `geodata-documentation`

## Notes

These starter skills are intentionally lightweight. They are meant to establish a clean structure and a consistent writing style before adding more advanced geospatial workflows.
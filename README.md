```
  __ _  ___  ___  ___  _  _(_) | |___
 / _` |/ _ \/ _ \/ __|| |/ / | | / __|
| (_| |  __/ (_) \__ \|   <| | | \__ \
 \__, |\___|\___/|___/|_|\_\_|_|_|___/
 |___/
```

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
  global-crs-selection/
    SKILL.md
  geospatial-ml/
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

### `global-crs-selection`
Use when choosing projections for world-scale or near-global work. Covers task-based selection across `EPSG:4326`, `EPSG:3857`, Equal Earth, LAEA, compromise projections, antimeridian handling, and polar edge cases.

### `snowflake-geospatial`
Use for geospatial SQL workflows in Snowflake, including `GEOGRAPHY`, spatial joins, distance filters, and warehouse-aware query planning.

### `postgis-geospatial`
Use for geospatial SQL workflows in PostgreSQL/PostGIS, including geometry versus geography choices, indexing, SRID handling, and spatial query tuning.

### `geospatial-ml`
Use when applying machine learning to geospatial data. Covers spatial feature engineering with geopandas, momepy, osmnx, and pysal; raster feature extraction with rasterio and torchgeo; spatial cross-validation; and common pitfalls like autocorrelation-driven data leakage.

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

## Using These Skills

These folders are source material for agent skills. They are not auto-discovered by Claude Code or Codex from this repo layout alone. To use a skill, copy or symlink the specific skill folder into the tool's skill directory structure.

### Claude Code

- Project scope: copy a skill folder to `.claude/skills/<skill-name>/SKILL.md`
- Personal scope: copy a skill folder to `~/.claude/skills/<skill-name>/SKILL.md`
- Claude can load a skill automatically when the `description` matches your request.
- You can also invoke it directly with `/skill-name`

Example:

```text
.claude/
  skills/
    global-crs-selection/
      SKILL.md
```

Then ask a natural question such as:

```text
Which CRS should I use for a world choropleth?
```

Or invoke it directly:

```text
/global-crs-selection world choropleth of country-level emissions
```

### Codex

- Repository scope: copy a skill folder to `.agents/skills/<skill-name>/SKILL.md`
- Codex can invoke skills implicitly when the `description` matches the task.
- You can inspect or invoke skills explicitly from the CLI or IDE skill picker using `/skills` or by mentioning the skill with `$`
- Use `AGENTS.md` for broad repository instructions, and use `SKILL.md` folders for focused, reusable workflows like CRS selection, QAQC, or feature engineering.

Example:

```text
.agents/
  skills/
    global-crs-selection/
      SKILL.md
```

Example prompt:

```text
Use the global-crs-selection skill to recommend a projection for a world population map.
```

If you want these skills available everywhere, place them in your personal skill directory for the relevant tool instead of keeping them only in a single repository.

## Next Good Additions

- `spatial-join-planning`
- `raster-preprocessing`
- `remote-sensing-index-selection`
- `openstreetmap-feature-extraction`
- `geodata-documentation`

## Contributing

Contributions are welcome! Here's how to get involved:

1. **New skill** — copy `template/geo-skill-template`, follow the design rules above, and open a pull request with a clear description of the skill's scope.
2. **Improve an existing skill** — edit the relevant `SKILL.md` and explain what gap or inaccuracy the change addresses.
3. **Report an issue** — open a GitHub issue describing the problem, the context (tool, model, dataset type), and what behaviour you expected.

### Guidelines

- One skill per pull request keeps reviews focused.
- Keep the `description` front-matter concrete and trigger-phrase rich — this is what agents use for discovery.
- Prefer checklists and structured output formats over free-form prose.
- Do not add external dependencies unless they are truly unavoidable.
- All contributions are released under the [MIT License](LICENSE).

## Notes

These starter skills are intentionally lightweight. They are meant to establish a clean structure and a consistent writing style before adding more advanced geospatial workflows.
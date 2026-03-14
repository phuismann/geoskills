# AGENTS.md

## Purpose

This repository stores reusable geospatial skill definitions.

Use these instructions whenever creating or editing Markdown files in this repo, especially:

- `README.md`
- `skills/*/SKILL.md`
- `template/geo-skill-template/SKILL.md`

## Repository Rules

- Each skill must live in `skills/<skill-name>/SKILL.md`.
- The folder name and the frontmatter `name` must match exactly.
- Skill names must be lowercase and hyphenated.
- Skills in this repository are source material, not auto-installed runtime skills.
- If a new skill is added, update `README.md` so the repository layout and starter skill list stay accurate.
- Prefer extending an existing skill when the new request is a sub-workflow of that skill rather than creating overlapping skills.

## Markdown Validation Checklist

Before finishing any Markdown change, validate all of the following:

1. Headings are clear and follow the existing style in the repo.
2. Bullets and numbering are consistent and easy to scan.
3. Fenced code blocks include a language or `text` where helpful.
4. Wording is concrete and practical, not vague or promotional.
5. Examples are relevant to geospatial workflows.
6. New content does not duplicate another section unnecessarily.
7. ASCII is preferred unless there is a strong reason to use other characters.

## SKILL.md Validation Checklist

Every `SKILL.md` should pass this checklist:

1. YAML frontmatter is present and valid.
2. Frontmatter includes at minimum:
   - `name`
   - `description`
3. The `description` starts with a practical trigger such as `Use when ...` and includes phrases a user would naturally say.
4. The body defines a narrow task, not a broad GIS textbook topic.
5. The body contains a usable workflow, checklist, or decision structure.
6. The skill provides an explicit output format when the task benefits from structured answers.
7. The skill includes guardrails, caveats, or common failure modes where mistakes are likely.
8. Example triggers are concrete and aligned with the `description`.
9. Claims about CRS, SQL behavior, or library usage should be broadly correct and defensible.
10. If the skill recommends a CRS, unit, or workflow, it should explain when that recommendation does and does not apply.

## Content Style For This Repo

- Keep skills task-shaped and operational.
- Prefer checklists, decision rules, and copyable output formats over long prose.
- Distinguish storage, display, and analysis workflows when they differ.
- Separate verified facts from heuristics or defaults.
- Avoid overfitting instructions to one tool unless the skill is explicitly tool-specific.
- When listing CRS recommendations, say whether they are national defaults, common practice, or task-specific fallbacks.
- When listing libraries or SQL behavior, avoid implying support or semantics that vary by version unless that caveat is stated.

## Geospatial Fact-Check Rules

Before finalizing a geospatial Markdown addition, validate these domain-specific risks:

1. CRS advice does not confuse geographic coordinates in degrees with projected coordinates in meters or feet.
2. `EPSG:3857` is not recommended for precise measurement analysis except where the limitation is stated explicitly.
3. UTM recommendations mention zone choice when the country or study area spans multiple zones.
4. Country-level CRS recommendations are framed as common defaults, not universal legal truth.
5. SQL guidance distinguishes between storage type choices, indexing, and measurement semantics when those differ.
6. ML guidance accounts for spatial leakage, CRS-dependent features, or scale effects when relevant.
7. Any "best" recommendation is scoped to the task, not written as a universal rule.

## README Update Rules

When adding a new skill, update `README.md` in these places if applicable:

1. Repository layout tree.
2. Starter skills summary.
3. Usage guidance, if the addition changes how users install or invoke skills.
4. Contributing guidance, if the new pattern changes how contributors should validate or place content.

## Final Validation Workflow

Before finishing a Markdown change, do this pass in order:

1. Validate structure.
   - Correct file path.
   - Valid YAML frontmatter if applicable.
   - Headings and lists render cleanly.
2. Validate scope.
   - The file solves one clear task.
   - It does not substantially duplicate another skill.
3. Validate geospatial correctness.
   - CRS, units, SQL, and library claims are defensible.
   - Caveats are included where misuse is common.
4. Validate discoverability.
   - `description` includes phrases a user would actually say.
   - Example triggers match the real scope of the file.
5. Validate repository consistency.
   - `README.md` is updated if a new skill was added or renamed.
   - The wording and formatting match the rest of the repo.

## Review Standard

When reviewing a Markdown addition, check for:

- Broken or misleading trigger descriptions.
- YAML mistakes in frontmatter.
- Overly broad scope.
- Missing examples.
- Advice that could cause incorrect geospatial analysis.
- README drift after adding or renaming a skill.
- Missing caveats where a recommendation is only conditionally correct.
- New sections that are too generic to be operationally useful.

## Preferred Outcome

Future Markdown additions should be concise, technically defensible, and directly useful to an agent or user trying to complete a geospatial task.
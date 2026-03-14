---
name: geodata-qaqc
description: "Use when performing quality assurance or quality control on geo data before analysis, sharing, or publishing. Covers vector validity, attribute consistency, raster sanity checks, completeness, and basic geospatial data fitness review."
---

# Geodata QAQC

Use this skill to perform a practical geospatial quality check before analysis or delivery.

## Goals

- Detect issues that could invalidate spatial analysis.
- Separate critical blockers from minor cleanup items.
- Assess whether the data is fit for the stated purpose.

## QAQC Checklist

### Vector Data

- Geometry exists for all expected features.
- Geometry type is consistent with the declared layer purpose.
- Invalid geometry is identified.
- Duplicate features or IDs are flagged.
- Required fields are present.
- Key attributes have plausible ranges and units.
- Null rates are noted for important fields.

### Raster Data

- Raster dimensions and band count match expectations.
- Cell size and extent are plausible.
- CRS is present and usable.
- Nodata values are known.
- Pixel value ranges look reasonable for the product.
- Obvious tiling seams, clipping artifacts, or empty bands are flagged.

### Cross-Cutting Checks

- Metadata exists and is internally consistent.
- Units are explicit.
- Time fields have a known timezone or date standard if relevant.
- Naming is understandable enough for downstream users.
- The dataset matches the intended analysis scale.

## Severity Model

Classify issues as:

- Blocker: analysis should not proceed until fixed.
- Major: analysis can proceed only with clear caveats.
- Minor: cleanup recommended but not immediately blocking.

## Output Format

```text
Fitness for use:
Blockers:
Major issues:
Minor issues:
Recommended fixes:
Go / no-go recommendation:
```

## Guidelines

- Tie QAQC to the actual use case when one is known.
- Avoid generic warnings without explaining the likely impact.
- Distinguish data defects from missing documentation.
- If the data appears usable, still state the remaining uncertainty.

## Example Triggers

- "Run a QAQC pass on this geospatial layer"
- "Is this raster clean enough for analysis?"
- "What issues should we fix before publishing this dataset?"
- "Review this geo data for obvious quality risks"
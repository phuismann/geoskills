---
name: geospatial-ml
description: "Use when applying machine learning to geospatial data, including spatial feature engineering, morphometric indicators, urban analysis, spatial cross-validation, library selection, or integrating geopandas, momepy, pysal, osmnx, torchgeo, or rasterio into an ML pipeline."
---

# Geospatial ML

Use this skill when the task involves building or evaluating a machine learning pipeline that consumes geospatial data as input, or when spatial structure must be respected during modelling.

## Library Landscape

### Vector and geometry
| Library | Role |
|---|---|
| **geopandas** | GeoDataFrame with CRS awareness; primary table for vector ML features |
| **shapely** | Geometry primitives and operations behind geopandas |
| **pyproj** | CRS and coordinate transforms |
| **osmnx** | Download and analyse OpenStreetMap road and place networks |
| **momepy** | Urban morphometrics from building footprints, street networks, and city blocks |
| **pysal / libpysal** | Spatial weights matrices, spatial statistics |
| **esda** | Moran's I, local spatial autocorrelation (LISA), spatial clustering |
| **mgwr** | Multiscale Geographically Weighted Regression |
| **spopt** | Spatial optimisation (p-median, regionalisation) |
| **h3 / h3-pandas** | Uber Hexagonal Hierarchical Spatial Index for grid aggregation |

### Raster and remote sensing
| Library | Role |
|---|---|
| **rasterio** | Read/write rasters, window reads, masking |
| **xarray / rioxarray** | N-dimensional arrays with spatial metadata and lazy loading |
| **numpy** | Array arithmetic for raster band math |
| **torchgeo** | PyTorch datasets, samplers, and transforms for geospatial rasters |
| **segmentation-models-pytorch** | Pre-built encoder-decoder architectures for semantic segmentation |

### Spatial ML and statistics
| Library | Role |
|---|---|
| **scikit-learn** | Standard ML; works with geopandas DataFrames |
| **mlxtend** | Spatial feature selection helpers |
| **lightgbm / xgboost** | Gradient boosting; handles spatial tabular features well |
| **pytorch / lightning** | Deep learning backbone for point clouds, rasters, graphs |
| **torch-geometric** | Graph neural networks over spatial neighbourhood graphs |

---

## Workflow

### 1. Define the target and geometry
- What is being predicted? (building type, land cover class, property value, urban density, flood risk)
- What geometry type holds labels? (point, polygon, pixel, hexagon)
- Is the label spatially continuous or discrete?

### 2. Construct spatial features

**From building footprints / urban blocks (momepy)**
- `momepy.Area`, `momepy.Perimeter`, `momepy.CircularCompactness`, `momepy.Elongation`
- `momepy.CoveredArea`, `momepy.NeighboringStreetOrientationDeviation`
- `momepy.BlocksCount`, `momepy.Reached` — network-based reach metrics
- Build a queen/rook `libpysal.weights.Queen` tessellation before computing context metrics

**From street networks (osmnx)**
- Download with `osmnx.graph_from_place` or bounding box
- Compute betweenness, closeness, edge density
- `osmnx.stats.basic_stats` for global network measures
- Project graph with `osmnx.project_graph` before any distance-dependent metric

**From vector attributes (geopandas)**
- Spatial join (`gpd.sjoin`) to attach context from neighbouring layers
- Buffer + dissolve for density ring features
- Distance to nearest feature with `GeoSeries.distance`
- H3 aggregation for grid-level summary features

**From raster bands (rasterio / xarray)**
- Zonal statistics: mean, std, median per polygon with `rasterstats.zonal_stats`
- Band indices: NDVI `(NIR - Red) / (NIR + Red)`, NDWI, NDBI
- Texture features from windowed reads (GLCM via `skimage.feature.graycomatrix`)

### 3. Assemble the feature matrix
- Merge all features into a single GeoDataFrame keyed on a stable geometry ID
- Drop geometry column before passing to sklearn estimators: `df.drop(columns='geometry')`
- Check for infinity, extreme outliers, and CRS-dependent features computed in wrong units

### 4. Handle spatial autocorrelation
- Compute Moran's I on the target variable (`esda.Moran`) before modelling
- If Moran's I is significantly positive, standard random splits will leak spatial signal
- Use spatial cross-validation to avoid this (see section below)

### 5. Spatial cross-validation
- **Block CV** (`sklearn-spatial` or manual): divide study area into geographic blocks, treat each block as a fold
- **Buffered leave-one-out (BLOO)**: exclude neighbours within a distance buffer from training
- **H3 tile CV**: assign H3 cell as fold key; shuffle cells, not rows
- Never use `train_test_split` with `shuffle=True` when geometry exhibits autocorrelation

### 6. Train and evaluate
- Baseline: spatial naive model (predict nearest-neighbour label or spatial lag mean)
- Compare model RMSE / F1 on spatial CV folds vs random CV folds — large gap indicates leakage
- Feature importance: SHAP values highlight which spatial indicators drive predictions
- Map residuals back onto geometry to diagnose spatial bias

### 7. Raster / deep learning path (torchgeo)
- Use `torchgeo.datasets` for standard benchmarks (LandCover.AI, EuroSAT, SpaceNet)
- `torchgeo.samplers.RandomGeoSampler` for patch-based training
- `torchgeo.transforms` for normalisation, augmentation respecting CRS
- Store predictions as GeoTIFF with `rasterio.open(..., 'w')` and matching `transform` + `crs`

---

## Common Pitfalls

| Pitfall | Risk | Fix |
|---|---|---|
| Random train/test split on autocorrelated data | Inflated metrics; model fails in new areas | Block or buffered spatial CV |
| momepy metrics before CRS projection | Wrong metric units (degrees not meters) | Always project to a local CRS first |
| Mixing CRS across joined layers | Geometry misalignment | Reproject all layers to one CRS before any join |
| Including coordinates as raw ML features | Model memorises study-area location | Use derived features (distance, density) not raw lat/lon |
| Nodata as a valid pixel value | Corrupts raster statistics | Confirm `nodata` mask before band math |
| Graph distance in angular CRS | Incorrect network centrality | Project graph before computing metrics |

---

## Output Format

When reporting on a geospatial ML pipeline:

```text
Target variable:
Geometry type and source:
Feature groups constructed:
  - Morphometrics:
  - Network:
  - Raster / remote sensing:
  - Context / aggregated:
Spatial autocorrelation check (Moran's I):
CV strategy:
Baseline model and metric:
Candidate model and metric:
Spatial bias in residuals:
Recommended next step:
```

---

## Guidelines

- Always project data to a metric CRS before computing any distance, area, or density feature.
- Treat spatial autocorrelation as a data leakage problem, not a curiosity.
- Keep geometry and feature matrix in sync via a stable ID column; do not rely on row order.
- Prefer interpretable features (density, compactness, connectivity) over raw coordinates.
- Document which spatial scale each feature operates at (parcel, block, street, neighbourhood).
- For raster segmentation, report IoU per class, not just mean accuracy.

## Example Triggers

- "Build a feature set from building footprints using momepy for a land use classifier"
- "How do I do spatial cross-validation to avoid data leakage?"
- "Which Python libraries should I use for urban ML with OpenStreetMap data?"
- "Compute street network centrality features for a regression model"
- "Train a semantic segmentation model on satellite imagery with torchgeo"
- "I have geopandas polygons and want to feed them into scikit-learn — what features should I extract?"
- "How do I check for and handle spatial autocorrelation before modelling?"

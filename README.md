# Czarr -- a local catalogue for zarr datasets.


Czarr is a lightweight catalog and access layer for climate and geospatial datasets distributed as Zarr, NetCDF (.nc/.nc4), and related multidimensional formats. Its main objective is to standardize ingestion, cataloging, and access to multidimensional climate data, enabling users to easily extract variables from large reanalysis and geospatial datasets.

Czarr is designed for academic and on-premise environments, where datasets are stored locally and must be managed without cloud infrastructure or dedicated DevOps teams.

The project is built on top of the Kedro framework, which provides reproducible, declarative ETL pipelines. Data ingestion pipelines convert heterogeneous climate data sources into Zarr-based datasets, register their metadata in a local catalog, and expose variables for downstream applications.


## Core design goals
- Treat Canonical storage format (netcd, zarr) for multidimensional datasets.
- Separate data ingestion, transformation, and access concerns.
- Provide reproducibility and lineage via Kedro pipelines and DVC.
- Remain simple, readable, and maintainable by future researchers.
- Expose dataset variables through explicit Python access methods.
- (Optional) Develop an API for variable collection.

## Non-Goals:
- Czarr is not a visualization tool
- Czarr is not a distributed compute engine
- Czarr does not replace Xarray

## Functionalities:
- Declarative dataset ingestion using Kedro pipelines, tracked with DVC.
- Preset ingestion configurations for common climate data sources: MERRA-2, ERA5, STRM DEMs, EDGAR, etc.
- Local catalog of datasets and variables, including:
    - Spatial and temporal coverage
    - Resolution and coordinate systems
    - Available variables and attributes
- Integrated spatial interpolator (e.g. nearest, bilinear, kdn).



Example 1: Get SO2 atmospheric surface concentration for a location in São Paulo, 2020.

```python
cz_catalog = czarr.Catalog('path/to/data/folder/')
cz_catalog.get(
    source='merra2.monthly',
    variable='SO2MASS',
    lat=-23.534252923029104, 
    lon=-46.633654683786097,
    start-date=datetime('2020-01-01'),
    months=12,
    interpolation='bilinear',
    aggregation= 'average',
)
```

Example 2: Create a subset of data containing datasets for 2020 and a-temporal datasets:
```python
cz_catalog.subset(
    years=[2020],
    include_atemporal=rue,
    output_path="/path/to/subset_catalog",
)
```

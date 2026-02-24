# stac-backends

There's a lot of things you can do with a STAC catalog, both as a maintainer/provider and as a user:

- **Search (aka "needle in a haystack")**: Find one or a few items
- **Full scan**: Find most or all items
- **Aggregations**: Statistics on the STAC items
- **Ingestion**: Add one or more items to a catalog
- **Dumps**: Take all items out of a catalog into another format
- **Migrations**: Update a catalog to a new version or schema

Here's a rough comparison of each backend for each of these uses, as well as their maturity.

|              | [pgstac](#pgstac) | [elasticsearch](#elasticsearchopensearch) | [stac-geoparquet](#stac-geoparquet) | [static](#static) |
| ------------ | :---------------: | :---------------------------------------: | :---------------------------------: | :---------------: |
| Search       |        ✅         |                    ✅                     |                ⚠️\*                 |        ❌         |
| Full scan    |        ❌         |                    ⚠️                     |                 ✅                  |        ⚠️         |
| Aggregations |        ⚠️         |                    ✅                     |                 ✅                  |        ❌         |
| Ingest       |        ⚠️         |                    ⚠️                     |                ⚠️\*                 |        ✅         |
| Maturity     |        ✅         |                    ⚠️                     |                 ⚠️                  |        ✅         |
| Dumps        |        ✅         |                    ✅                     |                 ✅                  |        ✅         |
| Migrations   |        ✅         |                    ⚠️                     |                 ❌                  |        ❌         |

## Backends

Here's a short description of each backend, with some links.

### pgstac

A Postgres/PostGIS schema for STAC.
Excellent for large scale deployments, or if you already have a Postgres instance.

- [Homepage](https://stac-utils.github.io/pgstac/pgstac/)
- [Repo](https://github.com/stac-utils/pgstac)
- [pypgstac](https://stac-utils.github.io/pgstac/pypgstac/)
- [stac-fastapi-pgstac](https://github.com/stac-utils/stac-fastapi-pgstac)

### elasticsearch/opensearch

There's an API implementation that uses an elasticsearch/opensearch backend.
There's no published schema for the \*search indices, those are up to the instance.

- [Homepage](https://stac-utils.github.io/stac-fastapi-elasticsearch-opensearch/)
- [Repo](https://github.com/stac-utils/stac-fastapi-elasticsearch-opensearch)

### stac-geoparquet

A light specification for writing STAC into [GeoParquet](https://geoparquet.org/).

- [Specification homepage](https://radiantearth.github.io/stac-geoparquet-spec/)
- [Specification repo](https://github.com/radiantearth/stac-geoparquet-spec)
- [stac-geoparquet splash page](https://geoparquet.org/)
- [stac-utils/rustac](https://stac-utils.github.io/rustac-py/) (low-level Python package)
- [stac-utils/stac-geoparquet](https://stac-utils.github.io/stac-geoparquet/) (higher-level Python package)

Some notes/caveats about the **stac-geoparquet** ratings in the tier list:

- **stac-geoparquet**'s search performance is heavily influenced by the presence or absence of datetime sorting (e.g via a [hash](https://github.com/radiantearth/stac-spec/discussions/1378))
- Ingestion will be awkward until we build something, e.g. with [iceberg](https://iceberg.apache.org/)

### Static

STAC catalogs can be statically hosted on blob storage as a collection of JSON files.

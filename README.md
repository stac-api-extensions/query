# STAC API - Query Extension

- **OpenAPI specification:** [openapi.yaml](openapi.yaml)
- **Conformance Classes:**
  - **STAC API - Item Search** binding: <https://api.stacspec.org/v1.0.0-rc.1/item-search#query>
  - **STAC API - Features** binding: <https://api.stacspec.org/v1.0.0-rc.1/ogcapi-features#query>
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.1/README.md#maturity-classification):** Candidate
- **Dependencies:**
  - [Features](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.1/ogcapi-features)
  - [Item Search](https://github.com/radiantearth/stac-api-spec/tree/v1.0.0-rc.1/item-search)

The Query Extension adds a `query` parameter that allows additional filtering based on the properties of Item objects. The JSON syntax for
these filters is known as "STACQL" (pronounced `stack-cue-el`).

It is recommended to implement the [Filter Extension](https://github.com/stac-api-extensions/filter)
instead of the Query Extension. Filter Extension is more well-defined, more expressive, and
uses the standardized CQL2 query language instead of the proprietary language defined here.
There is no plan to deprecate this extension, but it is also unlikely to see any further
refinement or changes.

The extension can be applied to either the **STAC API - Item Search** endpoint `/search`
(advertised with the conformance class <https://api.stacspec.org/v1.0.0-rc.1/item-search#query>) or to the 
**STAC API - Features** endpoint `/collections/{collection_id}/items` (advertised with the conformance class <https://api.stacspec.org/v1.0.0-rc.1/ogcapi-features#query>)

The syntax for the `query` parameter is:

```json
{
  "query": {
    "<property_name>": {
      "<operator>": <value>
    }
  }
}
```

Each property to search is an entry in the `query` filter. `<operator>` can be one of: `eq`, `neq`, `lt`, `lte`, `gt`, `gte`, `startsWith`, `endsWith`, `contains`, `in`. 
Multiple operators may be provided for each property and are treated as a logical AND, where all conditions must be met.

## Examples

Find scenes with cloud cover between 0 and 10%:

```json
{
  "query": {
    "eo:cloud_cover": {
      "gte": 0,
      "lte": 10
    },
    "stringAttr1": {
      "startsWith": "abc",
      "endsWith": "xyz"
    },
    "stringAttr2": {
      "contains": "mnop"
    },
    "stringAttr3": {
      "in": ["landsat", "modis", "naip"]
    }
  }
}
```

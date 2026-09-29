# Interactive Dashboards (eodash) Extension Specification

- **Title:** Interactive Dashboards (eodash)
- **Field Name Prefix:** eodash
- **Scope:** Collection, Item, Link, Asset
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @eodash
- **Identifier:** <https://eodash.github.io/eodash-extension/v0.2.0/schema.json>

This document explains the Interactive Dashboards (eodash) Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
The extension provides a set of fields to enrich STAC Collections, Items, Assets and Links with metadata
that enables clients to offer interactive visualization and data processing capabilities, including:

- Dynamically generated user-interface forms for data processing.
- Client-side rendering of charts and data visualizations.
- Support for custom map projections.
- Application of custom color legends, and dynamic user-configurable styling.

The extension originates from the [eodash](https://github.com/eodash/eodash) dashboard, which is one implementation of it.
Nevertheless, the fields are not tied to eodash and can be used and implemented by any other client or tool.

For additional concepts used by eodash that are not covered by this extension,
refer to the [eodash STAC documentation](https://eodash.github.io/eodash/STAC.html).

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields below can be used in these parts of STAC documents:

- [ ] Catalogs
- [x] Collections
- [x] Items
- [x] Assets
- [x] Links

### Collection Fields

These fields can be applied to the top-level of a STAC Collection object.

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:mapProjection | [Projection Object](#projection-object) | Defines a custom map projection that clients can register and use for displaying data. This is essential for visualizing data in non-standard coordinate reference systems (e.g., polar stereographic). |
| eodash:jsonform | string | A URL pointing to a JSON Schema file. Clients can use this schema to dynamically generate a user interface form, allowing users to input parameters for data processing services. |
| eodash:rasterform | string | A URL pointing to a JSON Schema and legend configuration file. Clients can use this schema to dynamically generate a user interface form, allowing users to change the parameters of tile URLs. |
| eodash:vegadefinition | string | A URL pointing to a [Vega](https://vega.github.io/vega/) or [Vega-Lite](https://vega.github.io/vega-lite/) JSON definition. Clients can use this to render charts from data returned by a service. |
| eodash:colorlegend | [Color Legend Object](#color-legend-object) | Defines a custom color legend for client-side styling of rendered data |

### Link Fields

These fields can be applied to STAC Links (in Collections, Items, or Catalogs).

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:flatstyle | string \| object | A URL pointing to a JSON object that extends [OpenLayers Flat Styles](https://openlayers.org/en/latest/apidoc/module-ol_style_flat.html), or the style object itself. Used for dynamic styling of web map links (WMS, WMTS, XYZ) and service links. |

**Note**: To describe the coordinate reference system of the linked resource (e.g., for WMS links),
use the fields of the [Projection Extension](https://github.com/stac-extensions/projection),
i.e. `proj:code`, `proj:wkt2` or `proj:projjson`.

### Asset Fields

These fields can be applied to STAC Assets (in Collections or Items).

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:flatstyle | string \| object | A URL pointing to a style JSON object or the style object itself for dynamic asset styling. |

**Note**: For data assets, styling is typically provided through links with `rel: "style"` rather than directly on the asset.
The style link's `href` points to an OpenLayers Flat Style definition, and `asset:keys` specifies which assets the style applies to.

### Projection Object

The `eodash:mapProjection` field uses the following object structure:

| Field Name | Type | Description |
| :---- | :---- | :---- |
| name | string | **REQUIRED**. A unique identifier for the projection (e.g., "EPSG:3031" or a custom name like "ORTHO:320500"). |
| def | string | **REQUIRED**. The Proj4 definition string specifying the projection parameters. |
| extent | \[number\] | **OPTIONAL**. The valid coordinate bounds as \[minX, minY, maxX, maxY\] in the projection's units. |

### **Color Legend Object**

The `eodash:colorlegend` field uses the following object structure:

| Field Name | Type | Description |
| :---- | :---- | :---- |
| domain | \[number\] | **REQUIRED**. Array of numeric values defining the input domain for the color scale. |
| range | \[string\] | **REQUIRED**. Array of color values (hex codes, CSS colors) corresponding to the domain values. |
| scaleType | string | **OPTIONAL**. Type of scale to use. Valid values: `"linear"`, `"log"`, `"pow"`, `"sqrt"`, `"symlog"`, `"continuous"`, `"discrete"`. Default is `"linear"`. |
| title | string | **OPTIONAL**. Title text displayed with the color legend. |
| tickFormat | string | **OPTIONAL**. Format string for tick labels (e.g., `".0f"` for integers, `".2f"` for two decimal places). |
| width | number | **OPTIONAL**. Width of the color legend in pixels. |
| ticks | number | **OPTIONAL**. Approximate number of ticks to display on the legend. |
| tickValues | \[number\] | **OPTIONAL**. Explicit array of values where ticks should be placed, overriding automatic tick generation. |
| markType | string | **OPTIONAL**. Visual style of the legend marks. Implementation-specific values. |

## Related Extensions and Standards

This extension is designed to work with several other STAC extensions and standards:

- **[Projection Extension](https://github.com/stac-extensions/projection)**: For describing the coordinate reference system
  of Items, Assets and Links using `proj:code`, `proj:wkt2` or `proj:projjson`
- **[Web Map Links Extension](https://github.com/stac-extensions/web-map-links)**: For map service links such as WMS, WMTS, XYZ, etc.
- **[Render Extension](https://github.com/stac-extensions/render)**: For visualization and styling metadata

For additional metadata properties used by the eodash implementation (such as `locations`, service configuration, and observation point handling), see the [eodash STAC documentation](https://eodash.github.io/eodash/STAC.html).

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```

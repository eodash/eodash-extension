# **eodash STAC Extension Specification**

- **Title:** eodash STAC extension
- **Field Name Prefix:** eodash, eox  
- **Scope:** Collection, Item, Link, Asset  
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @eodash
<!-- - **Identifier:** <https://stac-extensions.github.io/template/v1.0.0/schema.json> -->

This document explains the eodash STAC Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
The extension provides a set of properties to enrich STAC Collections and Items with metadata necessary for the [eodash](https://github.com/eodash/eodash) visualization client. These properties enable advanced, interactive features including:

- Dynamically generated user-interface forms for data processing.  
- Client-side rendering of charts and data visualizations.  
- Support for custom map projections.  
- Application of custom color legends, and dynamic user-configurable styling.

For more extensions and concepts not covered by this extension specification and needed by eodash, refer to **[eodash STAC Documentation](https://github.com/eodash/eodash/blob/main/docs/STAC.md)**.


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

### **Collection Fields**

These fields can be applied to the top-level of a STAC Collection object.

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:mapProjection | [Projection Object](#projection-object) | Defines a custom map projection that the client can register and use for displaying data. This is essential for visualizing data in non-standard coordinate reference systems (e.g., polar stereographic). |
| eodash:jsonform | string | A URL pointing to a JSON Schema file. eodash uses this schema to dynamically generate a user interface form, allowing users to input parameters for data processing services. |
| eodash:vegadefinition | string | A URL pointing to a [Vega](https://vega.github.io/vega/) or [Vega-Lite](https://vega.github.io/vega-lite/) JSON definition. eodash uses this to render charts from data returned by a service. |
| eox:colorlegend | [Color Legend Object](#color-legend-object) | Defines a custom color legend for client-side styling of rendered data |

### **Item Fields**

These fields can be applied at the top level of STAC Items.

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:proj4\_def | [Projection Object](#projection-object) | Defines a custom Proj4 projection for the item data. |

### **Link Fields**

These fields can be applied to STAC Links (in Collections, Items, or Catalogs).

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:proj4\_def | [Projection Object](#projection-object) | Defines a custom Proj4 projection for the linked resource. Commonly used in WMS links to specify the coordinate reference system. |
| eox:flatstyle | string \| object | A URL pointing to a JSON object that extends [OpenLayers Flat Styles](https://openlayers.org/en/latest/apidoc/module-ol_style_flat.html), or the style object itself. Used for dynamic styling of web map links (WMS, WMTS, XYZ) and service links. |

### **Asset Fields**

These fields can be applied to STAC Assets (in Collections or Items).

| Field Name | Type | Description |
| :---- | :---- | :---- |
| eodash:proj4\_def | [Projection Object](#projection-object) | Defines a custom Proj4 projection for the asset data. |
| eox:flatstyle | string \| object | A URL pointing to a style JSON object or the style object itself for dynamic asset styling. |

**Note**: For data assets, styling is typically provided through links with `rel: "style"` rather than directly on the asset. The style link's `href` points to an OpenLayers Flat Style definition, and `asset:keys` specifies which assets the style applies to.

### **Projection Object**

Both `eodash:mapProjection` and `eodash:proj4_def` use the same object structure:

| Field Name | Type | Description |
| :---- | :---- | :---- |
| name | string | **REQUIRED**. A unique identifier for the projection (e.g., "EPSG:3031" or a custom name like "ORTHO:320500"). |
| def | string | **REQUIRED**. The Proj4 definition string specifying the projection parameters. |
| extent | \[number\] | **OPTIONAL**. The valid coordinate bounds as \[minX, minY, maxX, maxY\] in the projection's units. |

### **Color Legend Object**

The `eox:colorlegend` property uses the following object structure:

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

- **[Projection Extension](https://github.com/stac-extensions/projection)**: For standard EPSG coordinate reference systems using `proj:epsg`
- **[Web Map Links Extension](https://github.com/stac-extensions/web-map-links)**: For map service links (WMS, WMTS, XYZ) with additional properties like `endpoint`, `method`, and `wmts:layer`
- **[Render Extension](https://github.com/stac-extensions/render)**: For visualization and styling metadata

For additional metadata properties used by eodash (such as `locations`, service configuration, and observation point handling), see the [eodash STAC documentation](https://github.com/eodash/eodash/blob/main/docs/STAC.md).

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

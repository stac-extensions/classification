# Classification Extension Specification

- **Title:** Classification
- **Identifier:** <https://stac-extensions.github.io/classification/v1.0.0/schema.json>
- **Field Name Prefix:** classification
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @drwelby @mmohr @pjhartzell 

This document explains the Classification Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

- Examples:
  - [Asset example](examples/asset-single-band.json): Shows the basic usage of the extension in a STAC Item (TODO)
- [JSON Schema](json-schema/schema.json) (TODO)
- [Changelog](./CHANGELOG.md)

## Classification Types

| Field Name              | Type               | Description |
| ----------------------- | ------------------ | ----------- |
| classification:classes  | \[Class Object]    | Classes in the dataset (including bands or property fields) |
| classification:bitmask  | \[BitRange Object] | Classes stored in bit ranges in the dataset |

The following fields are defined for use in Item properties and as an extension
to the [Raster Band Object](https://github.com/stac-extensions/raster#raster-band-object)
in [`raster:bands`](https://github.com/stac-extensions/raster#item-asset-fields)
as defined by the [raster extension](https://github.com/stac-extensions/raster).
As it's defined for Item properties, STAC implcitly also allows its use in Item Assets, 
and it can be part of Collections in [Item Asset Definitions](https://github.com/stac-extensions/item-assets)
and [Summaries](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md#summaries).

If the extension is given in the `stac_extensions` list, at least one of the fields must be specified in any of the given places listed above.
Please note that the JSON Schema is not able to validate the values of Collection summaries.

### BitRange Object

*Describes multiple classes stored in a bit range*

| Field Name      | Type             | Description |
| --------------- | ---------------- | ----------- |
| name            | string           | Short name of the class for machine readibility. |
| description     | string           | A short description of the classification. [CommonMark 0.29](https://commonmark.org/) syntax MAY be used for rich text representation. |
| bits            | \[integer]       | **REQUIRED** Bits used to generate class values |
| classes         | \[Class Object]  | **REQUIRED** Classes in the classification |
| roles           | \[string]        | Roles assigned to this bit range, similar to Item Asset roles. |

### Class Object

*Describes a data class*

| Field Name     | Type   | Description |
| -------------- | ------ | ----------- |
| name           | string | Short name of the class for machine readibility. |
| description    | string | **REQUIRED** Description of class. [CommonMark 0.29](https://commonmark.org/) syntax MAY be used for rich text representation. |
| value          | *any*  | **REQUIRED** Value of class. |
| color_hint     | string | Suggested color for rendering as RGB hex code in upper-case without leading `#`, e.g. `FF0000`. |

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

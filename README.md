# Template Extension Specification

- **Title:** Classification
- **Identifier:** <https://stac-extensions.github.io/classification/v1.0.0/schema.json>
- **Field Name Prefix:** classification
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @drwelby @mmohr @pjhartzell 

This document explains the Classification Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

Classification stores metadata that clarifies the values within a dataset. Common uses would be:

- Examples:
  - [Asset example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)


| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| classification:distinct   | [Distinct object]        | **REQUIRED**. Classes in the dataset |

| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| classification:bitmask   | [Bitmask object]        | **REQUIRED**. Classes in the dataset |

### Distinct Object

| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| classes | [Value] | **REQUIRED** Classes in the classification |
| role| string | see https://github.com/radiantearth/stac-spec/pull/989 |
| description | string                | A short description of the value(s). |

### Bitmask Object

_This could be combined with the simpler "Distinct" object with the bitmask specific fields optional, but this makes for easier discussion._

| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| bits | [integer] | **REQUIRED** Bits used to generate class values|
| endianess | 'big'/'little'?? | Byte order (HALP)| 
| role | string | see https://github.com/radiantearth/stac-spec/pull/989 |
| classes | [Class] | **REQUIRED** Classes in the classification |
| description | string                | A short description of the value(s). |

### Class Object

| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| value   | Any| **REQUIRED** Value of class |
| description | string                | **REQUIRED** Description of class |
| name   | string                    | Short name of the class for machine readibility, optional|
| color-hint | RGB string | suggested color for semantic meaning or to force consistent rendering|


_`value: any` leaves it open for ranges but hopefully that can be discouraged!_



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

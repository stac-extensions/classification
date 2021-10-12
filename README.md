# Template Extension Specification

- **Title:** Classification
- **Identifier:** <https://stac-extensions.github.io/classification/v1.0.0/schema.json>
- **Field Name Prefix:** classification
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @drwelby @mmohr

This document explains the Classification Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

Classification stores metadata that clarifies the values within a dataset. Common uses would be:

- Describing classes of data, and the values belonging to the class
- The reverse of the above, as a lookup table mapping values to classes


- Examples:
  - [Asset example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Asset Properties 

For single-band rasters

## Raster Band (Raster Extension)

For multiband rasters

## Table Column (Table Extension)

For tabular or vector datasets

## Collection Fields

In `summaries` field


_naming the field "classes" seems more descriptive but ??_

| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| classification:classes   | [Class object]        | **REQUIRED**. Classes in the dataset |


_Current fields from file:values_
### Class Object
| Field Name           | Type                      | Description |
| -------------------- | ------------------------- | ----------- |
| values  | [Any] | **REQUIRED** Values in the class |
| summary | string                | REQUIRED. A short description of the value(s). |


_"values" is shown as a list of any object. I think that could include Range objects for continuous data_

_@mmohr picked "summary" as distinctive from the "description" field commonly used elsewhere with the expectation that CommonMark would not be used and these descriptions would be short in nature_

_"Summaries" are mentioned elsewhere in the STAC spec (https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md#summaries) to summarize data statistically so maybe "description" is OK_


### _Common fields used elsewhere_

| Field Name           | Type                      | Description |
| name   | string                    | Name of the class |
| description | string                | Description of class |
| title | string | Like "name" but formatted for display|

_"Description" vs "Summary" is addressed above_

_"name" could be useful as a field to identify a class. If classes are mapped to names ((instead of a list)) it would make classes more machine readable. A field named "name" as optional would be useful when classes don't have names._

_"title" in other formats like SLD are useful for generating legends and such, but fall into the category of styling and display and probably should be left to styling files which could be linked as assets._

### _Other fields suggested in discussions_

| Field Name           | Type                      | Description |
| color   | string                    | Color to display this class |

_This is another case of using a metadata field for styling and probably should be discouraged._






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

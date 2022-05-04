# Classification Extension Specification

- **Title:** Classification
- **Identifier:** <https://stac-extensions.github.io/classification/v1.0.0/schema.json>
- **Field Name Prefix:** classification
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Pilot
- **Owner**: @drwelby @mmohr @pjhartzell 

This document explains the Classification Extension to the 
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

- Examples:
  - Asset level:
    - [Classes example](examples/item-classes-maxar.json): STAC Item with classified raster bands in asset (Maxar)
    - [Bitfields example](examples/item-bitfields-landsat.json): STAC Item with bitfields in asset (Landsat) 
  - Collection level:
    - [Item Assets example](examples/collection-item-assets.json): STAC Collection using Item Assets for classed child Items
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Classification Types

| Field Name              | Type                | Description |
| ----------------------- | ------------------- | ----------- |
| classification:classes  | `[Class]`         | Classes stored in raster or bands |
| classification:bitfields   | `[Bit Field]`        | Classes stored in bit fields in the raster |

`classification:classes` is for when one or more unique coded integer values are present within a raster asset 
or band therein. These coded values translate to classes of data with verbose descriptions.

An example would be a cloud mask raster that stores values that represent image conditions in each pixel.

`classification:bitfields` is for classes that are stored in fields of continuous bits within the pixel's value. 
Files using this strategy are commonly given the name 'bitmask' or 'bit index'. The values stored are the integer 
representation of the bits in the field when summed as an isolated string. Bits are always read right to left. The 
position of the first bit in a field is given by its offset. Therefore the first (rightmost) bit is at offset zero.

These classification objects can be used in the following places:

- In a raster Asset object if single band.
- For multiband rasters, use [`raster:bands`](https://github.com/stac-extensions/raster) and store the classes in 
  each Band Object.
- As an [`item-assets`](https://github.com/stac-extensions/item-assets) field in a Collection object, to indicate 
  that the classification is used across child Items.

### Bit Field Object

*Describes multiple classes stored in a field of a continuous range of bits*

| Field Name      | Type           | Description |
| --------------- | -------------- | ----------- |
| offset          | `integer`    | **REQUIRED** Offset to first bit in the field |
| length          | `integer`    | **REQUIRED** Number of bits in the field |
| classes         | `[Class]`      | **REQUIRED** Classes represented by the field values |
| roles           | `[string]`       | see [Asset Roles](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#asset-roles) |
| description     | `string`       | A short description of the classification. |
| name           | `string`             | Short name of the class for machine readibility, optional |

A Bit Field stores classes within a range of bits in a data value. The range is described by the offset of the first 
bit from the rightmost position, and the length of bits used to store the class values.

Since bit fields are often used to store data masks, they can also use optional STAC roles to identify their purpose 
to clients.

Following is a simplified example a bitfield scheme for cloud data using 4 bits. The bits are broken into 3 bit fields.

```{.txt}
3210
||||
...X   - 1 here means "no data", 0 means "valid data"
..X.   - 1 here means "cloud pixel", 0 means "clear pixel"
XX..   - these two bits are "cloud confidence" and give 4 classes (binary 00, 01, 10, and 11)
```

Working right to left, these four bits represent 3 bit fields:

- `...X` is `offset:0, length:1`
- `..X.` is `offset:1, length:1`
- `XX..` is `offset:2, length:2`

To extract the values in a bit field from a value called `data`, you typically would use the expression:

`data >> offset & (1<<length) - 1`

This does:

- `data >> offset` right-shifts the bits in the `data` value `offset` number of times
- `(1 << length) - 1` gives us `length` number of 1 bits going right to left, which gives us a bitmask
- ANDing (`&`) the values together gives the binary representation of the class value

An example of finding the cloud confidence class value from the 4 bit example above for the data value of integer `6`:

- Integer 6 is `0110` in binary
- We want to extract the field at `offset:2, length:2`
- First, right-shift twice (`0110 >> 2`), which results in `1001`
- Next, make the bitmask, which is 2 left shifts of `0001` to get `0100` (integer 4), then subtract 1 to 
  get `0011` (integer 3)
- AND these two values together `1001 & 0011` and you get `0001`, or integer 1
- Therefore at this pixel the cloud confidence field is storing class 1

The key distinction with bit fields from other types of bitmasks is that the bits in the field are summed 
as standalone bits. Therefore `01..` cloud confidence class uses the value of 1, not 4 (binary `0100`)

For a real world example, see [Landsat 8's Quality raster](https://www.usgs.gov/media/images/landsat-1-8-collection-1-level-1-quality-bit-designations).

### Class Object

*Describes a data class*

| Field Name     | Type                 | Description |
| -------------- | -------------------- | ----------- |
| value          | `integer`                | **REQUIRED** Value of class |
| description    | `string`             | **REQUIRED** Description of class |
| name           | `string`             | Short name of the class for machine readibility, optional |
| color_hint     | `RGB string` | suggested color for rendering (Hex RGB code in upper-case without leading #) |

Class objects enumerate data values and their corresponding classes. A cloud mask raster could contain the following 
 four classes:

- 0: "No data"
- 1: "Clear"
- 2: "Cloud"
- 3: "Cloud shadow"

`color_hint` only is intended to *hint* a reasonable color for clients to use and is not intended to define styling. For example, the ESA landcover datasets use `"color_hint":"006400"` to suggest using a green color for a class of `"Tree cover"`.


For conveying styling see the [Raster Extension](https://github.com/stac-extensions/raster/issues/17) and [cpt2json](https://github.com/zakjan/cpt2json) for discussion on passing styling as an item asset instead.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) 
Instructions for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes 
are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any 
[node.js installation](https://nodejs.org/en/download/).

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

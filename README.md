# DataPerú Tesseract

## Schema Update

To add a new cube to the schema, add a new XML file to the frags folder and define the a new `<Cube/>` object. Make sure to wrap that new object with a `<Schema/>` object with the appropriate schema name.

## Schema Concatenation

Install [moncat](https://github.com/hwchen/mondrian-schema-cat#installation) and run

```
just cat
```

The contents of the `frags/` folder should now be concatenated into the `schema.xml` file.

*NOTE:* Currently, `moncat` does not support the new `<SharedDimension/>` object introduced by Tesseract. So make sure to copy the shared dimensions from the `frags/shared.xml` file to the top of the `schema.xml` file as a temporary workaround.

# json-schema-testing


Working notes:

The Python package `jsonschema` doesn't seem capable enough of resolving `"$ref"`-ed schemas.  

Testing `check-jsonschema` python package instead.  Also testing Microsoft VS code built-in linting.



1. `check-jsonschema`
  - Pointing the option `--schemafile` to the public `https` link for the schema, `check-jsonschema` is capable of fetching (1) relative `"$ref"`-ed schemas to the primary `https` schema URL and (2) fetching absolute `"$ref"`-ed schemas given their own `https` URL.
  - `check-jsonschema` also capable of interpreting the `format` of `uuid` and `date-time` and catching errors.
  - When checking `format:uuid`, `check-jsonschema` enforces 8-4-4-4-12 format
  - When checking `format:uuid`, `check-jsonschema` does not care if hex characters are lowercase or uppercase letters.
  - When checking `format:date-time`, `check-jsonschema` works.  It requires timezone.  It is ok with `+00:00` or `Z` postfix for zulu time.
  


2. Microsoft VS code
  - Given the `"$schema"` field populated in the `test.json` file, Microsoft VS code is capable of fetching the primary schema and resolving relative as well as absolute `"$ref"`-ed schemas.
  - Microsoft VS code does *NOT* catch some invalid values when `format` of `uuid`.
  - When checking `format:date-time`, Microsoft VS codes is OK.  The behavior is the same as `check-jsonschema`.


Conclusion 1:  Microsoft VS code does not catch `uuid` `format` restrictions.  Validation scripts should be written to use the Python package `check-jsonschema`.

Conclusion 2:  Should also include a `pattern` in the UUID schemas.  This is primarily to help out VS code users quickly identify UUID issues.

Conclusion 3:  It's ok to write a schema with relative `"$ref"`-ed schemas.  I.e., sub-objects with their own schema do not need the full URL.


Example of `check-jsonschema` command:

`check-jsonschema ./files/test.json --no-cache --schemafile https://raw.githubusercontent.com/jp7745/json-schema-testing/main/schema/schema.json`



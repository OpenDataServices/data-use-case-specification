Notes
==========

This file contains notes pertaining to the specification which may make it into documentation in the future.

## Notes on JSON Pointer syntax and arrays

The specification relies on defining paths of a target JSON dataset via JSON Pointer Syntax ([RFC 6901](https://datatracker.ietf.org/doc/html/rfc6901)). This is well supported in various programming languages, as well as in [JSON Schema validation](https://json-schema.org/draft/2020-12/json-schema-validation.html#name-json-pointers) but it, ironically, presents a problem for the main use-case of this specification: machine-checking of use-cases in target datasets.

Since some use-cases may require properties which are nested under one or more levels of arrays, it becomes a challenge to mechanically evaluate to what extent the dataset supports the use-case.

For example in a comprehensive [OCDS](https://standard.open-contracting.org/latest/en/) dataset, we may want to track the number of unique awarded suppliers ([U004](https://docs.google.com/spreadsheets/d/1j-Y0ktZiOyhZzi-2GSabBCnzx6fF5lv8h1KYwi_Q9GM/edit#gid=1075539355)). We need:

* The identifier for each Award
* The identifier for each Supplier
* The name for each supplier
* The status of each Award

The problem is that in an [OCDS release](https://standard.open-contracting.org/latest/en/schema/release/), `awards` is an array, as is `awards.suppliers`. There is no way in JSON pointer syntax to refer to the `awards.status` or `awards.suppliers.id` across an array, we have to declare the exact index e.g. `/awards/0/status`.

While this is supported in [JSON Path Syntax](https://goessner.net/articles/JsonPath/), JSON Path does not (yet) have a formal specification while JSON Pointer *does*. Further, JSON Schema provides functionality to support validation of strings as JSON Path.

Therefore, all arrays should be denoted using the `0` index, and tools should spot this and expand the array to contain the full list of JSON Pointers for the data set. This should be repeated for multiple arrays.

Using the above example, we would specify the paths required for U004 as:

* `/awards/0/id`
* `/awards/0/suppliers/0/id`,
* `/awards/0/suppliers/0/name`,
* `/awards/0/status`



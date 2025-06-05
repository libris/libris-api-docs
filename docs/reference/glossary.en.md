---
title: Introduction and glossary
---

Libris XL uses [JSON-LD](https://json-ld.org/) as its data format and we provide an API for creating, reading, updating, and deleting records. Read operations can be performed without authentication, while all other types of operations require an access token.

The APIs documented here are not versioned and may change in the future.

## Glossary

Some terms that appear in the documentation:

`embellished` is a term that describes that a record is delivered not only with its own data, but also with relevant data that the record links to. The boundaries for exactly which data is included in a certain type of record, and which is broken out to be separate (and linked to), vary over time. Generally, more and more data is being broken out, as this makes the data reusable for multiple records. It is often good to request records as `embellished` so that you are not affected by these shifting boundaries.

`framed` is a term that describes that the extra data that comes with a record due to linking should be inserted at the places where the data is used in the JSON-LD structure. If the data is not `framed`, it is instead delivered as a list of entities.

`lens` is a term that describes a mechanism for filtering out everything except the most important information in a record. Exactly what is filtered out depends on the type of record/entity you request. There are currently three values for `lens` to choose from:

* `chip` is the most filtered variant. If used, often the only thing left is, for example, the entity's name (if such exists)
* `card` is the intermediate level. Here, everything included in the `chip` level is included, plus some additional information. This can be, for example, variant names or identifiers.
* `none` means that no data is filtered out. 
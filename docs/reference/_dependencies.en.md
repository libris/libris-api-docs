---
title: List a record's dependencies
---

```
GET https://libris.kb.se/_dependencies
```

This request lists which other records a record depends on.

## Parameters

* `id` - The bibliographic record's ID (e.g. http://libris.kb.se/bib/1234)
* `relation` - Type of relation (this parameter is optional and can be excluded)
* `reverse` - Boolean to indicate reverse relation (default is `false`)

## Example

Request:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_dependencies?id=http://libris.kb.se/bib/1234&relation=language'
```

Response:

```json title="JSON"
["https://libris.kb.se/zfpl8t8310mn9s2m"]
``` 
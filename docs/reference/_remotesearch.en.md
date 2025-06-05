---
title: Search in external databases
---
!!! warning "NOTE"
    This request requires [authentication](../howto/auth.md).

```
GET https://libris.kb.se/_remotesearch
```

This request allows you to search external databases.

## Parameters

* `q` - The search query
* `start` - Number of results to skip in the result, used for pagination. Default is 0.
* `n` - Max number of results to include in the result, used for pagination. Default is 10.
* `databases` - A comma-separated list of databases to search in. Use this parameter alone to list available databases.

## Examples

### List available databases

Request:

```bash title="Shell"
curl -XGET -H "Authorization: Bearer xxxx" 'https://libris.kb.se/_remotesearch?databases=true'
```

Response:

```json title="JSON"
[{"database":"AGRALIN","name":"Wageningen UR",
  "alternativeName":"Wageningen UR Library",
  "country":"Netherlands",
  "comment":"Bibliographic information about books and journals.",
  ...},
 ...
]
```

### Search

Request, search in two external databases for the phrase "tove":

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_remotesearch?q=tove&databases=BIBSYS,DANBIB'
```

Response:

```json title="JSON"
{
  "totalResults": {
    "BIBSYS":7336,
    "DANBIB":11991
  },
  "items": [
    ...
  ]
}
``` 
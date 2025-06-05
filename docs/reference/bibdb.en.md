---
title: Biblioteksdatabasen (library database)
---

```
GET http://bibdb.libris.kb.se/api/lib
```

Via an API, you can retrieve information from [Biblioeksdatabsen](https://biblioteksdatabasen.libris.kb.se/), the library database. You can get information about a single library or listings of all libraries.

All data in the database is available via the API. However, contact information requires you to send an API key as an extra header in the request. Contact Libris customer service if you need an API key.

## Parameters

* `sigel` - The sigel for the library to be displayed, e.g. `S`.
* `ocode` - Organization code for the collection of sigels to be displayed, e.g. `KB`.
* `dump` - `true` or `false`. If the value is `true`, all libraries are listed.
* `updatedDatum` - Loads all libraries updated since the specified date. Format: YYYY-MM-DD, e.g. `2024-05-23`.
* `start` - Start the listing from this position (integer).
* `n` - Show up to this number of libraries in the listing. Default: `200`.
* `level` - `full` or `brief`. Determines how much information is shown per library. Default: `full`.

If you want to further limit the list, you can specify key-value pairs as parameters to the query (see below).

## Examples

### Information about a specific library
To get information about a specific library, specify its sigel as a parameter:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?sigel=S'
```

### Information about several libraries
Specify multiple sigels to retrieve information about several libraries:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?sigel=S,H'
```

### Information about all libraries
To retrieve all libraries within an organization, specify the organization code as a parameter.

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?ocode=KB'
```

The parameters "ocode" and "sigel" can also be combined.

### List of updated libraries
If you want a list of libraries updated since a certain date, use the "updated" parameter:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?updated=2023-06-01'
```

Or, if you want all libraries, you can use the "dump" parameter:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true'
```

### Limit selection

To limit your selection to a certain value, e.g. only Swedish libraries, you can use the key for country (country_code) in the query:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true&country_code=se'
```

Another example, to limit to libraries in Stockholm:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true&address.city=stockholm'
``` 
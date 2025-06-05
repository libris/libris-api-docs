---
title: List holdings records for a bibliographic record
---

```
GET https://libris.kb.se/_findhold
```

This request lists the holdings records that the specified sigel has for the specified bibliographic record.

## Parameters

* `id` - The bibliographic record's ID (e.g. https://libris.kb.se/s93ns5h436dxqsh or http://libris.kb.se/bib/1234)
* `library` - Sigel ID (e.g. https://libris.kb.se/library/SEK)

## Example

Request:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_findhold?id=https://libris.kb.se/s93ns5h436dxqsh&library=https://libris.kb.se/library/SEK'
```

Response:

```json title="JSON"
["https://libris.kb.se/48h9kp894jm8kzz"]
```

Request:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_findhold?id=http://libris.kb.se/bib/1234&library=https://libris.kb.se/library/SEK'
```

Response:

```json title="JSON"
["https://libris.kb.se/48h9kp894jm8kzz"]
``` 
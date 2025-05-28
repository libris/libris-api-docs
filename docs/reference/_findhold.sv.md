---
title: Lista beståndsposter för bibliografisk post
---

```
GET https://libris.kb.se/_findhold
```

Detta anrop listar beståndsposterna som angivet sigel har för den specificerade
bibliografiska posten.

## Parametrar

* `id` - Den bibliografiska postens ID (t.ex. https://libris-qa.kb.se/s93ns5h436dxqsh eller http://libris.kb.se/bib/1234)
* `library` - Sigel-ID (t.ex. https://libris.kb.se/library/SEK)

## Exempel

Förfrågan:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_findhold?id=https://libris.kb.se/s93ns5h436dxqsh&library=https://libris.kb.se/library/SEK'
```

Svar:

```json title="JSON"
["https://libris.kb.se/48h9kp894jm8kzz"]
```

Förfrågan:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_findhold?id=http://libris.kb.se/bib/1234&library=https://libris.kb.se/library/SEK'
```

Svar:

```json title="JSON"
["https://libris.kb.se/48h9kp894jm8kzz"]
```

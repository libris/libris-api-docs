---
title: Lista en posts beroenden
---

```
GET https://libris.kb.se/_dependencies
```

Detta anrop listar vilka andra poster som en post har beroende till.

## Parametrar

* `id` - Den bibliografiska postens ID (t.ex. http://libris.kb.se/bib/1234)
* `relation` - Typ av relation (den här parametern är valfri och kan exkluderas)
* `reverse` - Bool för att indikera omvänd relation (standardvärdet är `false`)


## Exempel

Förfrågan:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_dependencies?id=http://libris.kb.se/bib/1234&relation=language'
```

Svar:

```json title="JSON"
["https://libris.kb.se/zfpl8t8310mn9s2m"]
```

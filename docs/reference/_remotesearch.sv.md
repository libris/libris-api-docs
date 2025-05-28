---
title: Sök i externa databaser
---
!!! warning "OBS"
    Detta anrop kräver [autentisering](../howto/auth.md).

```
GET https://libris.kb.se/_remotesearch
```

Detta anrop låter dig slå mot externa databaser.

## Parametrar

* `q` - Sökfrågan
* `start` - Antal träffar att hoppa över i resultatet, används för paginering. Standardvärdet är 0.
* `n` - Max antal träffar att inkludera i resultatet, används för paginering. Standardvärdet är 10.
* `databases` - En kommaseparerad lista av databaser att söka i. Använd denna parameter ensam för att lista tillgängliga databaser.

## Exempel

### Lista tillgängliga databaser

Förfrågan:

```bash title="Shell"
curl -XGET -H "Authorization: Bearer xxxx" 'https://libris.kb.se/_remotesearch?databases=true'
```

Svar:

```json title="JSON"
[{"database":"AGRALIN","name":"Wageningen UR",
  "alternativeName":"Wageningen UR Library",
  "country":"Nederländerna",
  "comment":"Bibliografiska uppgifter om böcker och tidskrifter.",
  ...},
 ...
]
```

### Sök

Förfrågan, sökning i två externa databaser efter frasen "tove":

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_remotesearch?q=tove&databases=BIBSYS,DANBIB'
```

Svar:

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

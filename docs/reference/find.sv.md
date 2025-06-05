---
title: Libris sök-API (/find)
---

Detta anrop låter dig söka i Libris.

```
GET https://libris.kb.se/find
```

## Parametrar

* `q` - Sökfrågan.
* `o` - Hitta endast poster som länkar till detta ID.
* `_limit` - Max antal träffar att inkludera i resultatet, används för
  paginering. Standardvärdet är 200.
* `_offset` - Antal träffar att hoppa över i resultatet, används för
  paginering. Standardvärdet är 0.

Standardoperatorn är `+` (`OCH`), vilket innebär att en sökning på `tove jansson` är
samma sak som en sökning på `tove +jansson`. `-` exkluderar söktermer, `|`
innebär `ELLER`, `*` används för prefixsökningar, `""` matchar hela frasen och
`()` används för att påverka operatorprioritet.

Sökningen kan filtreras på värdet på egenskaper i posten. Samma egenskap kan anges flera gånger genom
att upprepa parametern eller genom att komma-separera värdena. Om olika egenskaper anges innebär det 
`OCH`. Om samma egenskap anges flera gånger innebär det `ELLER`, såvida inte prefixet `and-` används.
Det går att kombinera olika egenskaper med `ELLER` genom att använda prefixet `or-`.

| Filter | Beskrivning |
| ------ | ----------- | 
| `<egenskap>`          | Egenskapen har värdet.
| `not-<egenskap>`      | Egenskapen har inte värdet.
| `or-<egenskap>`       | Kombinera filter för olika egenskaper med `ELLER` istället för `OCH`.
| `and-<field>`         | Kombinera filter för _samma_ egenskap med `OCH` istället för `ELLER`.
| `exists-<egenskap>`   | Egenskapen existerar. Ange ett booleskt värde, d.v.s. `true` eller `false`.
| `min-<egenskap>`      | Värdet är större eller lika med.
| `minEx-<egenskap>`    | Värdet är större än (Ex står för "Exclusive").
| `max-<egenskap>`      | Värdet är mindre eller lika med.
| `maxEx-<egenskap>`    | Värdet är mindre än.
| `matches-<egenskap>`  | Värdet matchar (se datumsökning och "inkludera underordnade termer"-sökning nedan).
| `and-matches-<field>` | Kombinera flera `matches`-filter för _samma_ fält med `OCH` istället för `ELLER`.

### Filteruttryck

 Filteruttryck                                        | Filter-parametrar    
-------------------------------------------------------|-----------------------                 
 a är x `ELLER` a är y...                              | `a=x&a=y...`
 a är x `OCH` a är `INTE` y...                         | `a=x&not-a=y...`
 a är x `OCH` a är y...                                | `and-a=x&and-a=y...`
 a är x `OCH` b är y `OCH` c är z...                   | `a=x&b=y&c=z...`
 a är x `ELLER` b är y...                              | `or-a=x&or-b=y...`          
 (a är x `ELLER` b är y) `OCH` c är z...               | `or-a=x&or-b=y&c=z...`  
 (a är x `ELLER` b är y) `OCH` (c är z `ELLER` d är q) | Inte möjligt. Kan bara ange en grupp med `or-`.                    

För egenskaper som är av typen datum (`meta.created`, `meta.modified` och `meta.generationDate`)
kan värdet anges på följande format:

| Format                  | Upplösning | Exempel               |
|-------------------------|------------|-----------------------|
| `ÅÅÅÅ`                  | År         | `2020`                |
| `ÅÅÅÅ-MM`               | Månad      | `2020-04`             |
| `ÅÅÅÅ-MM-DD`            | Dag        | `2020-04-01`          |
| `ÅÅÅÅ-MM-DD'T'HH`       | Timme      | `2020-04-01T12`       |
| `ÅÅÅÅ-MM-DD'T'HH:mm`    | Minut      | `2020-04-01T12:15`    |
| `ÅÅÅÅ-MM-DD'T'HH:mm:ss` | Sekund     | `2020-04-01T12:15:10` |
| `ÅÅÅÅ-'W'VV`            | Vecka      | `2020-W04`            |

## Exempel

Sökning efter `tove jansson` och `tove lindgren`, max 2 träffar:
```bash
curl -XGET -H "Accept: application/ld+json" \
    https://libris.kb.se/find\?q\=tove%20\(jansson\|lindgren\)\&_limit=2
```

Länkar till country/Vietnam:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find?o=https://id.kb.se/country/vm&_limit=2'
```

Har medietyp (mediaType) men inte bärartyp (carrierType):
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find?exists-mediaType=true&exists-carrierType=false'
```

Utgiven på 1760-talet:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?min-publication.year=1760&maxEx-publication.year=1770&_limit=5'
```

Noterad musik utgiven på 1930- eller 1950-talet:
```bash
curl -XGET -H "Accept: application/ld+json" -G \
    'https://libris.kb.se/find.jsonld' \
    -d instanceOf.@type=NotatedMusic \
    -d min-publication.year=1930 \
    -d max-publication.year=1939 \
    -d min-publication.year=1950 \
    -d max-publication.year=1959 \
    -d _limit=5
```

Katalogiserad av sigel "S" vecka åtta eller tio 2018:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?meta.descriptionCreator.@id=https://libris.kb.se/library/S&matches-meta.created=2018-W08,2018-W10&_limit=2'
```

Innehåller 'Aniara' och har ett bestånd med sigel APP1:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?q=Aniara&@reverse.itemOf.heldBy.@id=https://libris.kb.se/library/APP1'
```

Har ämne term/sao/Monster:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?instanceOf.subject.@id=https://id.kb.se/term/sao/Monster'
```

Har ämne term/sao/Monster _eller_ term/sao/Magi:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?instanceOf.subject.@id=https://id.kb.se/term/sao/Monster&instanceOf.subject.@id=https://id.kb.se/term/sao/Magi'
```

Har ämne term/sao/Monster _och_ term/sao/Magi:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?and-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster&and-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi'
```

Har ämne term/sao/Monster _och_ term/sao/Magi men _inte_ term/sao/Trollkarlar,
och har genre/form term/saogf/Fantasy:
```bash
curl -XGET -H "Accept: application/ld+json" -G \
    'https://libris.kb.se/find.jsonld' \
    -d and-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster \
    -d and-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi \
    -d not-instanceOf.subject.@id=https://id.kb.se/term/sao/Trollkarlar \
    -d instanceOf.genreForm.@id=https://id.kb.se/term/saogf/Fantasy
```

Har ämne term/sao/Monster (eller en underordnad term, t.ex. Drakar, Gorgoner):
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster'
```

Har ämne term/sao/Monster (eller en underordnad term, t.ex. Drakar, Gorgoner), _och_ term/sao/Magi (eller en underordnad term,
t.ex. Amuletter, Häxeri):
```bash
curl -XGET -H "Accept: application/ld+json" -G \
    'https://libris.kb.se/find.jsonld' \
    -d and-matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster \
    -d and-matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi
```
---
title: Libris search API (/find)
---

This endpoint allows you to search in Libris.

```
GET https://libris.kb.se/find
```

## Parameters

* `q` - The search query.
* `o` - Find only records that link to this ID.
* `_limit` - Max number of results to include in the result, used for pagination. Default is 200.
* `_offset` - Number of results to skip in the result, used for pagination. Default is 0.

The default operator is `+` (`AND`), which means a search for `tove jansson` is the same as a search for `tove +jansson`. `-` excludes search terms, `|` means `OR`, `*` is used for prefix searches, `""` matches the whole phrase, and `()` is used to affect operator precedence.

The search can be filtered on the value of properties in the record. The same property can be specified multiple times by repeating the parameter or by comma-separating values. If different properties are specified, it means `AND`. If the same property is specified multiple times, it means `OR`, unless the prefix `and-` is used. You can combine different properties with `OR` by using the prefix `or-`.

| Filter | Description |
| ------ | ----------- |
| `<property>`          | The property has the value.
| `not-<property>`      | The property does not have the value.
| `or-<property>`       | Combine filters for different properties with `OR` instead of `AND`.
| `and-<field>`         | Combine filters for _the same_ property with `AND` instead of `OR`.
| `exists-<property>`   | The property exists. Specify a boolean value, i.e. `true` or `false`.
| `min-<property>`      | The value is greater than or equal to.
| `minEx-<property>`    | The value is greater than (Ex stands for "Exclusive").
| `max-<property>`      | The value is less than or equal to.
| `maxEx-<property>`    | The value is less than.
| `matches-<property>`  | The value matches (see date search and "include subordinate terms" search below).
| `and-matches-<field>` | Combine multiple `matches` filters for _the same_ field with `AND` instead of `OR`.

### Filter Expressions

 Filter Expression                                       | Filter Parameters    
---------------------------------------------------------|-----------------------                 
 a is x `OR` a is y...                                   | `a=x&a=y...`
 a is x `AND` a is `NOT` y...                            | `a=x&not-a=y...`
 a is x `AND` a is y...                                  | `and-a=x&and-a=y...`
 a is x `AND` b is y `AND` c is z...                     | `a=x&b=y&c=z...`
 a is x `OR` b is y...                                   | `or-a=x&or-b=y...`          
 (a is x `OR` b is y) `AND` c is z...                    | `or-a=x&or-b=y&c=z...`  
 (a is x `OR` b is y) `AND` (c is z `OR` d is q)         | Not possible. You can only specify one group with `or-`.                    

For properties of type date (`meta.created`, `meta.modified`, and `meta.generationDate`), the value can be specified in the following formats:

| Format                  | Resolution | Example               |
|-------------------------|------------|-----------------------|
| `YYYY`                  | Year       | `2020`                |
| `YYYY-MM`               | Month      | `2020-04`             |
| `YYYY-MM-DD`            | Day        | `2020-04-01`          |
| `YYYY-MM-DD'T'HH`       | Hour       | `2020-04-01T12`       |
| `YYYY-MM-DD'T'HH:mm`    | Minute     | `2020-04-01T12:15`    |
| `YYYY-MM-DD'T'HH:mm:ss` | Second     | `2020-04-01T12:15:10` |
| `YYYY-'W'WW`            | Week       | `2020-W04`            |

## Examples

Search for `tove jansson` and `tove lindgren`, max 2 results:
```bash
curl -XGET -H "Accept: application/ld+json" \
    "https://libris.kb.se/find?q=tove%20(jansson|lindgren)&_limit=2"
```

Links to country/Vietnam:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find?o=https://id.kb.se/country/vm&_limit=2'
```

Has media type (mediaType) but not carrier type (carrierType):
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find?exists-mediaType=true&exists-carrierType=false'
```

Published in the 1760s:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?min-publication.year=1760&maxEx-publication.year=1770&_limit=5'
```

Notated music published in the 1930s or 1950s:
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

Catalogued by sigel "S" week eight or ten 2018:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?meta.descriptionCreator.@id=https://libris.kb.se/library/S&matches-meta.created=2018-W08,2018-W10&_limit=2'
```

Contains 'Aniara' and has a holding with sigel APP1:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?q=Aniara&@reverse.itemOf.heldBy.@id=https://libris.kb.se/library/APP1'
```

Has subject term/sao/Monster:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?instanceOf.subject.@id=https://id.kb.se/term/sao/Monster'
```

Has subject term/sao/Monster _or_ term/sao/Magi:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?instanceOf.subject.@id=https://id.kb.se/term/sao/Monster&instanceOf.subject.@id=https://id.kb.se/term/sao/Magi'
```

Has subject term/sao/Monster _and_ term/sao/Magi:
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?and-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster&and-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi'
```

Has subject term/sao/Monster _and_ term/sao/Magi but _not_ term/sao/Trollkarlar, and has genre/form term/saogf/Fantasy:
```bash
curl -XGET -H "Accept: application/ld+json" -G \
    'https://libris.kb.se/find.jsonld' \
    -d and-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster \
    -d and-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi \
    -d not-instanceOf.subject.@id=https://id.kb.se/term/sao/Trollkarlar \
    -d instanceOf.genreForm.@id=https://id.kb.se/term/saogf/Fantasy
```

Has subject term/sao/Monster (or a subordinate term, e.g. Drakar, Gorgoner):
```bash
curl -XGET -H "Accept: application/ld+json" \
    'https://libris.kb.se/find.jsonld?matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster'
```

Has subject term/sao/Monster (or a subordinate term, e.g. Drakar, Gorgoner), _and_ term/sao/Magi (or a subordinate term, e.g. Amulets, Witchcraft):
```bash
curl -XGET -H "Accept: application/ld+json" -G \
    'https://libris.kb.se/find.jsonld' \
    -d and-matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Monster \
    -d and-matches-instanceOf.subject.@id=https://id.kb.se/term/sao/Magi
``` 
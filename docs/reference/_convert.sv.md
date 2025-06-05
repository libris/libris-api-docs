---
title: Förhandsgranska MARC21-konvertering
---

```
POST https://libris.kb.se/_convert
```

Detta anrop låter dig förhandsgrandska hur ett JSON-LD-dokument kommer se ut efter konvertering till MARC21.

Förfrågan måste se ut på följande vis:

* body måste bestå av ett JSON-LD-dokument.
* `Content-Type` måste vara `application/ld+json`.
* `Accept` måste vara `application/x-marc-json`.

## Exempel
Förfrågan:

```bash title="Shell"
curl -XPOST -H "Content-Type: application/ld+json" \
    -H "Accept: application/x-marc-json" \
    -d '
      {
        "@graph": [
          {
            "@id": "https://TEST",
            "@type": "Record",
            "mainEntity": {
              "@id": "https://TEST#it"
            }
          },
          {
            "@id": "https://TEST#it",
            "@type": "Print",
            "hasTitle": [
              {
                "@type": "Title",
                "mainTitle": "Sent i november"
              }
            ],
            "instanceOf": {
              "@type": "Text",
              "subject": [
                {
                  "@id": "https://libris.kb.se/31fjrv9m50fkz43#it"
                }
              ],
              "language": [
                {
                  "@id": "https://id.kb.se/language/swe"
                }
              ],
              "contentType": [
                {
                  "@id": "https://id.kb.se/term/rda/Text"
                }
              ]
            }
          }
        ],
        "@context": "/context.jsonld"
      }
    ' \
    https://libris.kb.se/_convert
```

Svar:
```title="MARC21"
{"fields":[{"008":"|     |        |                   swe| "},{"041":{"ind1":" ","ind2":" ","subfields":[{"a":"swe"}]}},{"245":{"ind1":"1","ind2":"0","subfields":[{"a":"Sent i november"}]}},{"336":{"ind1":" ","ind2":" ","subfields":[{"a":"text"},{"b":"txt"},{"2":"rdacontent"}]}},{"600":{"ind1":"3","ind2":"4","subfields":[{"a":"Mumintrollen"},{"c":"(fiktiva gestalter)"},{"0":"https://libris.kb.se/31fjrv9m50fkz43#it"}]}}],"leader":"     |a| a          4500"}
```

Alternativt, om du sparat ner posten i filen `my_post.jsonld`:


```bash title="Shell"
curl -XPOST -H "Content-Type: application/ld+json" \
    -H "Accept: application/x-marc-json" \
    -d@my_post.jsonld https://libris.kb.se/_convert
```



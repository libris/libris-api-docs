---
title: Preview MARC21 conversion
---

```
POST https://libris.kb.se/_convert
```

This request allows you to preview how a JSON-LD document will look after conversion to MARC21.

The request must be structured as follows:

* The body must consist of a JSON-LD document.
* `Content-Type` must be `application/ld+json`.
* `Accept` must be `application/x-marc-json`.

## Example
Request:

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

Response:
```title="MARC21"
{"fields":[{"008":"|     |        |                   swe| "},{"041":{"ind1":" ","ind2":" ","subfields":[{"a":"swe"}]}},{"245":{"ind1":"1","ind2":"0","subfields":[{"a":"Sent i november"}]}},{"336":{"ind1":" ","ind2":" ","subfields":[{"a":"text"},{"b":"txt"},{"2":"rdacontent"}]}},{"600":{"ind1":"3","ind2":"4","subfields":[{"a":"Mumintrollen"},{"c":"(fiktiva gestalter)"},{"0":"https://libris.kb.se/31fjrv9m50fkz43#it"}]}}],"leader":"     |a| a          4500"}
```

Alternatively, if you have saved the record in the file `my_post.jsonld`:

```bash title="Shell"
curl -XPOST -H "Content-Type: application/ld+json" \
    -H "Accept: application/x-marc-json" \
    -d@my_post.jsonld https://libris.kb.se/_convert
```
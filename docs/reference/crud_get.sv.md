---
title: Läsa en post
---

```
GET https://libris.kb.se/<post-id>
```

En post kan läsas genom att skicka ett `GET`-anrop till postens URI (t.ex.
`https://libris.kb.se/<post-id>`) med `Accept`-headern satt till t.ex.
`application/ld+json`. Standardinnehållstypen är `text/html` vilket ger en
renderad HTML-vy av posten och inte enbart underliggande data.

Formatet på data i svaret kan styras via parametrar.

## Parametrar
Alla parametrar är valfria och kan utelämnas. Se [ordlistan](glossary.md) för vad termerna betyder.

* `embellished` - `true` eller `false`. Standardvärdet är `true`.
* `framed` - `true` eller `false`. 
    * Standardvärdet är `false` för `Content-Type: application/ld+json`. 
    * Standardvärdet är `true` för  `Content-Type: application/json`.
* `lens` - `chip`, `card` eller `none`. Standardvärdet är `none`. Om `lens` är `chip` eller `card` blir `framed` alltid `true`.
* `computedLabel` - `sv` eller `en`. Ej satt som standard (dvs: om parametern utelämnas kommer `computedLabel`-etiketter inte att visas). Kräver att `framed` är satt till `true`.

## Exempel
Förfrågan:

```bash title="Shell"
curl -XGET -H "Accept: application/ld+json" https://libris.kb.se/s93ns5h436dxqsh
```

Svar:
```json title="JSON-LD"
{
  "@context": "/context.jsonld",
  "@graph": [
    ...
  ]
}
```

Förfrågan:

```bash title="Shell"
$ curl -XGET -H "Accept: application/ld+json" "https://libris.kb.se/s93ns5h436dxqsh?embellished=false&lens=chip"
```

Svar:

```json title="JSON-LD"
{
  "@context": "/context.jsonld",
  "@graph": [
    ...
  ]
}
```

## Profile Negotiation

För att få data i en annan variant (genom att använda ett specifikt
urval av RDF-vokabulär) stöder vi en form av "profile negotiation".

(Observera att Profile Negotiation ännu (2024) inte är en standard. Se
[IETF Internet Draft on Profile Negotiation](https://profilenegotiation.github.io/I-D-Profile-Negotiation/I-D-Profile-Negotiation.html)
och
[W3C Working Draft on Content Negotiation by Profile](https://www.w3.org/TR/dx-prof-conneg/)
för detaljer.)

### Profiler tillgängliga just nu

Se [målkontexterna i libris-definitions-repot](https://github.com/libris/definitions/tree/develop/sys/context/target).

### Profilerade datavyer

Med content negotiation plus parametrar:

```bash title="Shell"
curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/sdo-w3c"

curl -s -H'Accept: text/turtle' "https://id.kb.se/relator/contributor?profile=https://id.kb.se/sys/context/target/bibo-w3c"

curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/bibo-w3c"
```

Kombination av parametrar:

```bash title="Shell"
curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/sdo-w3c&embellished=false"
```

Genom att använda enbart headers för negotiation:

```bash title="Shell"
curl -s -H 'Accept: application/trig' -H 'Accept-Profile: <https://id.kb.se/sys/context/target/loc-w3c-sdo>' \
      http://libris.kb.se/fxql7jqr38b1dkf
```

## Content Negotiation

För att få data i olika representationer stöder vi [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation).
Just nu stöds följande:

* `application/ld+json` ([JSON-LD](https://www.w3.org/TR/json-ld11/))
* `application/json` (JSON)
* `text/turtle` ([Turtle](https://www.w3.org/TR/turtle/))
* `application/trig` ([TriG](https://www.w3.org/TR/trig/))
* `application/rdf+xml` ([RDF/XML](https://www.w3.org/TR/rdf-syntax-grammar/))

```bash title="Shell"
curl -s -H'Accept: application/ld+json' https://libris.kb.se/s93ns5h436dxqsh

curl -s -H'Accept: application/json' https://libris.kb.se/s93ns5h436dxqsh

curl -s -H'Accept: text/turtle' https://libris.kb.se/s93ns5h436dxqsh

curl -s -H'Accept: application/trig' https://libris.kb.se/s93ns5h436dxqsh

curl -s -H'Accept: application/rdf+xml' https://libris.kb.se/s93ns5h436dxqsh
```

För bekvämlighetens skull lägga till `/data.<filändelse>` för att få en viss representation
utan att behöva specificera den med `Accept`-headern (i det här fallet ignoreras `Accept`-headern):

```bash title="Shell"
# JSON-LD
curl -s https://libris.kb.se/s93ns5h436dxqsh/data.jsonld

# JSON
curl -s https://libris.kb.se/s93ns5h436dxqsh/data.json

# Turtle
curl -s https://libris.kb.se/s93ns5h436dxqsh/data.ttl

# TriG
curl -s https://libris.kb.se/s93ns5h436dxqsh/data.trig

# RDF/XML
curl -s https://libris.kb.se/s93ns5h436dxqsh/data.rdf
```
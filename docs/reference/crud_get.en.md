---
title: Read a record
---

```
GET https://libris.kb.se/<id>
```

A record can be read by sending a `GET` request to the record's URI (e.g. `https://libris.kb.se/<id>`) with the `Accept` header set to e.g. `application/ld+json`. The default content type is `text/html`, which provides a rendered HTML view of the record and not just the underlying data.

The format of the data in the response can be controlled via parameters.

## Parameters
All parameters are optional and can be omitted. See the [glossary](glossary.md) for the meaning of the terms.

* `embellished` - `true` or `false`. Default is `true`.
* `framed` - `true` or `false`.
    * Default is `false` for `Content-Type: application/ld+json`.
    * Default is `true` for `Content-Type: application/json`.
* `lens` - `chip`, `card` or `none`. Default is `none`. If `lens` is `chip` or `card`, `framed` is always `true`.
* `computedLabel` - `sv` or `en`. Not set by default, so if it is not specified, `computedLabel` labels will not be shown. Requires that `framed` is set to `true`.

## Examples
Request:

```bash title="Shell"
curl -XGET -H "Accept: application/ld+json" https://libris.kb.se/s93ns5h436dxqsh
```

Response:

```json title="JSON-LD"
{
  "@context": "/context.jsonld",
  "@graph": [
    ...
  ]
}
```

Request:

```bash title="Shell"
$ curl -XGET -H "Accept: application/ld+json" "https://libris.kb.se/s93ns5h436dxqsh?embellished=false&lens=chip"
```

Response:

```json title="JSON-LD"
{
  "@context": "/context.jsonld",
  "@graph": [
    ...
  ]
}
```

## Profile Negotiation

To get data in another variant (by using a specific selection of RDF vocabularies), we support a form of "profile negotiation".

(Note that Profile Negotiation is not yet (2024) a standard. See the [IETF Internet Draft on Profile Negotiation](https://profilenegotiation.github.io/I-D-Profile-Negotiation/I-D-Profile-Negotiation.html) and the [W3C Working Draft on Content Negotiation by Profile](https://www.w3.org/TR/dx-prof-conneg/) for details.)

### Profiles currently available

See [target contexts in the libris-definitions repo](https://github.com/libris/definitions/tree/develop/sys/context/target).

### Profiled data views

With content negotiation plus parameters:

```bash title="Shell"
curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/sdo-w3c"

curl -s -H'Accept: text/turtle' "https://id.kb.se/relator/contributor?profile=https://id.kb.se/sys/context/target/bibo-w3c"

curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/bibo-w3c"
```

Combination of parameters:

```bash title="Shell"
curl -s -H'Accept: text/turtle' "https://libris.kb.se/fxql7jqr38b1dkf?profile=https://id.kb.se/sys/context/target/sdo-w3c&embellished=false"
```

Using only headers for negotiation:

```bash title="Shell"
curl -s -H 'Accept: application/trig' -H 'Accept-Profile: <https://id.kb.se/sys/context/target/loc-w3c-sdo>' \
      http://libris.kb.se/fxql7jqr38b1dkf
```

## Content Negotiation

To get data in different representations, we support [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation). Currently, the following are supported:

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

For convenience, you can add `/data.<extension>` to get a certain representation without having to specify it with the `Accept` header (in this case, the `Accept` header is ignored):

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
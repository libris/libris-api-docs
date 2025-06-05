---
title: Skapa en post
---
!!! warning "OBS"
    Detta anrop kräver [autentisering](../howto/auth.md).

```
POST https://libris.kb.se/data
```

En ny post kan skapas genom att skicka ett `POST`-anrop till `/data`.

Förfrågans body ska innehålla posten i JSON-LD-format.

Följande headers måste vara satta:

* `Content-Type` ska vara `application/ld+json`.
* `Authorization` ska innehålla en aktiv och giltig bearer-token. Exempel: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` ska vara en sigel som din OAuth-klient är knuten till (t.ex. Doro).

API:et implementerar ett antal valideringssteg, till exempel för att förhindra
att duplicerade beståndsposter skapas. Om någon validering misslyckas svarar
API:et `400 Bad Request` med ett felmeddelande som förklarar problemet.

Observera att ett tillfälligt `@id` **måste** specificeras på flera ställen i
dokumentet du skickar:

* Tillfälligt `@id` i Record
* Tillfälligt `mainEntity.@id` i Record
* Tillfälligt `@id` i Thing ("saken")

Till exempel:

```json title="JSON-LD"
{"@graph":[{"@id":"https://id.kb.se/TEMPID","@type":"Record","mainEntity":{"@id":"https://id.kb.se/TEMPID#it"}},{"@id":"https://id.kb.se/TEMPID#it","@type":"Person","familyName":"Testing"}]}
```

Ett lyckat anrop kommer resultera i ett `201 Created`-svar med den nya postens
URI satt i `Location`-headern.

## Exempel

```bash title="Shell"
curl -XPOST -H 'Content-Type: application/ld+json' \
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    -d@min_post.jsonld \
    https://libris.kb.se/data
```

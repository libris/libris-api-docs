---
title: Uppdatera en post
---

!!! warning "OBS"
    Detta anrop kräver [autentisering](../howto/auth.md).

```
PUT https://libris.kb.se/<post-id>
```

En befintlig post kan uppdateras genom att skicka ett `PUT`-anrop till
postens URI (t.ex. `https://libris.kb.se/s93ns5h436dxqsh`).

Förfrågans body ska innehålla posten i JSON-LD-format.

Följande headers måste vara satta:

* `Content-Type` ska vara `application/ld+json`.
* `Authorization` ska innehålla en aktiv och giltig bearer-token. Exempel: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` ska vara en sigel som din OAuth-klient är knuten till (t.ex. S).
* `If-Match` innehållandes en `ETag`, t.ex. `1821177452` (se nedan).

För att göra ett `PUT`-anrop behöver man skicka med en `If-Match`-header som
innehåller en`ETag`. Detta för att förhindra samtidiga uppdateringar av ett
dokument. Det värde som ska fyllas i kan hämtas från `ETag`-headern i det svar
som returneras när man gör ett `GET`-anrop mot en viss post. Om posten hunnit
ändras av någon annan efter det att `ETag`:en hämtats kommer API:et svara med
`409 Conflict`.

Det går inte att uppdatera ID:t för en given post. Om ett nytt ID önskas behöver
posten tas bort och en ny skapas i dess ställe.

Ett lyckat anrop kommer resultera i ett `204 No Content`-svar, medan
misslyckade anrop kommer resultera i ett `400 Bad Request`-svar med medföljande
felmeddelande.

## Exempel

```bash title="Shell"
curl -XPUT -H 'Content-Type: application/ld+json' \
    -H 'If-Match: <etag>' \
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    -d@min_uppdaterade_post.jsonld \
    https://libris.kb.se/<id>
```

---
title: Ta bort en post
---
!!! warning "OBS"
    Detta anrop kräver [autentisering](../howto/auth.md).

```
DELETE https://libris.kb.se/<id>
```

Du kan radera en post genom att skicka ett `DELETE`-anrop till postens URI
(t.ex. `https://libris.kb.se/s93ns5h436dxqsh`).

Följande headers måste vara satta:

* `Authorization` ska innehålla en aktiv och giltig bearer-token. Exempel: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` ska vara en sigel som din OAuth-klient är knuten till (t.ex. T).

Du kan enbart radera poster som inga andra poster länkar till. Det innebär att
om du till exempel vill radera en bibliografisk post kan du bara göra det om
det inte finns några beståndsposter kopplade till den. Om så är fallet kommer
API:et svara med `403 Forbidden`.

En lyckad borttagning kommer resultera i ett `204 No Content`-svar. Ytterligare
anrop till postens URI kommer resultera i ett `410 Gone`-svar.


## Exempel

```bash title="Shell"
curl -XDELETE
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    https://libris.kb.se/<id>
```

---
title: Delete a record
---
!!! warning "NOTE"
    This request requires [authentication](../howto/auth.md).

```
DELETE https://libris.kb.se/<id>
```

You can delete a record by sending a `DELETE` request to the record's URI (e.g. `https://libris.kb.se/s93ns5h436dxqsh`).

The following headers must be set:

* `Authorization` must contain an active and valid bearer token. Example: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` must be a sigel that your OAuth client is linked to (e.g. T).

You can only delete records that no other records link to. This means that, for example, if you want to delete a bibliographic record, you can only do so if there are no holdings records linked to it. If this is the case, the API will respond with `403 Forbidden`.

A successful deletion will result in a `204 No Content` response. Further requests to the record's URI will result in a `410 Gone` response.

## Example

```bash title="Shell"
curl -XDELETE \
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    https://libris.kb.se/<id>
``` 
---
title: Update a record
---

!!! warning "NOTE"
    This request requires [authentication](../howto/auth.md).

```
PUT https://libris.kb.se/<id>
```

An existing record can be updated by sending a `PUT` request to the record's URI (e.g. `https://libris.kb.se/s93ns5h436dxqsh`).

The body of the request should contain the record in JSON-LD format.

The following headers must be set:

* `Content-Type` must be `application/ld+json`.
* `Authorization` must contain an active and valid bearer token. Example: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` must be a sigel that your OAuth client is linked to (e.g. S).
* `If-Match` containing an `ETag`, e.g. `1821177452` (see below).

To make a `PUT` request, you need to include an `If-Match` header containing an `ETag`. This is to prevent concurrent updates to a document. The value to use can be obtained from the `ETag` header in the response when you make a `GET` request for a specific record. If the record has been changed by someone else after you retrieved the `ETag`, the API will respond with `409 Conflict`.

It is not possible to update the ID of a given record. If a new ID is desired, the record must be deleted and a new one created in its place.

A successful request will result in a `204 No Content` response, while failed requests will result in a `400 Bad Request` response with an accompanying error message.

## Example

```bash title="Shell"
curl -XPUT -H 'Content-Type: application/ld+json' \
    -H 'If-Match: <etag>' \
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    -d@my_updated_record.jsonld \
    https://libris.kb.se/<id>
``` 
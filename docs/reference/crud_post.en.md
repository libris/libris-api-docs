---
title: Create a record
---
!!! warning "NOTE"
    This request requires [authentication](../howto/auth.md).

```
POST https://libris.kb.se/data
```

A new record can be created by sending a `POST` request to `/data`.

The body of the request should contain the record in JSON-LD format.

The following headers must be set:

* `Content-Type` must be `application/ld+json`.
* `Authorization` must contain an active and valid bearer token. Example: `Bearer: hW3IHc9PexxxFP2IkAAbqKvjLbW4thQ`
* `XL-Active-Sigel` must be a sigel that your OAuth client is linked to (e.g. Doro).

The API implements a number of validation steps, for example to prevent duplicate holdings records from being created. If any validation fails, the API responds with `400 Bad Request` and an error message explaining the problem.

Note that a temporary `@id` **must** be specified in several places in the document you send:

* Temporary `@id` in Record
* Temporary `mainEntity.@id` in Record
* Temporary `@id` in Thing ("the thing")

For example:

```json title="JSON-LD"
{"@graph":[{"@id":"https://id.kb.se/TEMPID","@type":"Record","mainEntity":{"@id":"https://id.kb.se/TEMPID#it"}},{"@id":"https://id.kb.se/TEMPID#it","@type":"Person","familyName":"Testing"}]}
```

A successful request will result in a `201 Created` response with the new record's URI set in the `Location` header.

## Example

```bash title="Shell"
curl -XPOST -H 'Content-Type: application/ld+json' \
    -H 'Authorization: Bearer <token>' \
    -H 'XL-Active-Sigel: <sigel>' \
    -d@my_record.jsonld \
    https://libris.kb.se/data
``` 
---
title: Authentication and authorization
---
APIs that provide write access require authentication. Authentication is done through [Libris Login](https://login.libris.kb.se/) which is linked to a personal account. Rights management is handled using OAuth2.

To register a client, you need to contact Libris customer service, who will provide the new client's ID and client secret, as well as instructions on how to use these to obtain a bearer token. This bearer token is referenced in the examples below and should be included as a parameter in all requests that require authentication.

The credentials provided apply either to the test environments or the production environment, depending on the nature of the integration and the user's needs.

If a request is answered with "401 Unauthorized", it means that the rights to perform the task are missing or invalid.

To write data to Libris, there are two different levels of rights:

* Level 1 for managing only holdings records.
* Level 2 for managing both bibliographic records and holdings records.

The client is linked to one or more sigels, and only holdings records belonging to those sigels can be created, changed, or deleted in the examples below.

## Rules

To use Libris write APIs, you must have an API account for Libris to be able to generate a key (token) when using it.

* When usage ceases, this must be reported to KB, who will deactivate the API account.
* In case of misuse or repeated incorrect use, KB reserves the right to immediately revoke authorization for the API client.
* All types of bulk changes to data in Libris (including creating new records and deleting records) via APIs may only be performed in consultation or agreement with KB and must be run in the test environment before implementation.
* Bulk changes to records in Libris may only be performed on your own holdings unless otherwise agreed with the National Library of Sweden. A system supplier, media supplier, or cataloguing service provider may perform such bulk changes on behalf of a Libris-connected library.

## Retrieve a bearer token

Once you have received a client ID and client secret from customer service, do the following to retrieve a bearer token:

```bash title="Shell"
curl -X POST -d 'client_id=<your client id>&client_secret=<your client secret>&grant_type=client_credentials' https://login.libris.kb.se/oauth/token'
```
To retrieve a token for the test environments, use `https://login-stg.libris.kb.se/oauth/token` instead.

The response should look something like this:

```json title="JSON"
{"access_token": "tU77KXIxxxxxxxKh5qxqgxsS", "expires_in": 36000, "token_type": "Bearer", "scope": "read write", "app_version": "1.5.0"}
```

From the response, you are expected to extract your `access_token` and include it in every request that requires authentication. 
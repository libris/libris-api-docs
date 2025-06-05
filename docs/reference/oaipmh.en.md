---
title: OAI-PMH
---
```
GET https://libris.kb.se/api/oaipmh
```

The Libris OAI-PMH server follows the [official OAI-PMH specification](https://www.openarchives.org/OAI/openarchivesprotocol.html).

The purpose of this document is to provide Libris-specific information on how OAI-PMH can be used to retrieve Libris metadata.

The address for the OAI-PMH interface is https://libris.kb.se/api/oaipmh/

## The Set parameter (subsets)

The OAI-PMH specification describes how harvesting of subsets works in general, but in Libris these are used for certain specific purposes.

The Libris OAI-PMH implementation uses three main subsets for its metadata. These are `auth`, `bib`, and `hold`, which represent authority records, bibliographic records, and holdings records. There are also a couple of smaller subsets, namely `sao` (Swedish subject headings) and `nb` (the National Bibliography).

In addition to the basic subsets, there are sub-subsets for bibliographic and holdings records for specific sigels.
For example, the subset `set=bib:S` includes all bibliographic records for which there is a holdings record with sigel S.
The subset `set=hold:S` consists of all holdings records with sigel S.

## The MetadataPrefix parameter (format)

OAI-PMH uses the parameter "metadataPrefix" to select the export format. The Libris OAI-PMH implementation mainly supports four formats: `marcxml`, `rdfxml`, `jsonld`, and `ttl`. Because the OAI-PMH specification requires it, a very simple form of `oai_dc` (Dublin Core) is also offered, but in practice, records in `oai_dc` lack any real metadata.

To meet Libris needs without violating the OAI-PMH specification, three additional "forms" of these formats are offered (strictly speaking, according to the specification, these are independent formats themselves). These are:

* `[mainformat]_expanded`
* `[mainformat]_includehold`
* `[mainformat]_includehold_expanded`

`[mainformat]_expanded` for a record `A` means that records `B,C ..` that `A` links to are also, as far as possible, included in `A`.
For example: If `metadataPrefix=marcxml_expanded` is used for a bibliographic record, this means that relevant authority information is included in the record.

`[mainformat]_includehold` differs from `[mainformat]` only for bibliographic records. These records are then followed by a list of holdings records for the bibliographic record in question (the list is in the `<about>` part of the response for the record).

`[mainformat]_includehold_expanded` combines `[mainformat]_includehold` and `[mainformat]_expanded`.

## Libris-specific parameters
Libris' implementation of OAI-PMH offers an extra parameter that is not included in the OAI-PMH specification and can be used with the verbs `GetRecord` and `ListRecords`. The parameter is called `x-withDeletedData` and if set to `true`, the response will include metadata for records that have been marked as deleted. This is contrary to the OAI-PMH specification, which prohibits both extra parameters and the inclusion of metadata for deleted records. Nevertheless, this choice has been made to enable certain necessary Libris functionality.

## Example
To retrieve all bibliographic records (with associated holdings records), for which there are holdings records with sigel `S` and which have been updated between February 13 and 12:00 on February 14:

```bash title="Shell"
curl "https://libris.kb.se/api/oaipmh/?verb=ListRecords&metadataPrefix=marcxml_includehold_expanded&from=2018-02-13&until=2018-02-14T12:00:00Z&x-withDeletedData=true&set=bib:S"
```

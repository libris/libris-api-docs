---
title: Libris API Documentation
---
Libris is Sweden's national library catalogue. Libris is a collaboration between more than 600 Swedish libraries, which together build up the content. Libris consists of many different systems, including a search service, interlibrary loan, and cataloguing tools.

Libris' technical platform is called Libris XL. The Libris data store is based on open, linked data in the BIBFRAME/RDF format and the KBV (Kungliga bibliotekets basvokabulär) vocabulary.

With Libris APIs you can -- for example -- integrate Libris data to and from your organization's local systems. You can use the APIs to search, write to, and retrieve data from Libris. The APIs work via HTTP.

## Terms of use for public Libris APIs

Libris as a data platform includes a number of publicly available APIs. Most of these APIs provide read access to open, publicly available data. Some APIs also provide write access and therefore require authentication and an agreement with KB for use.

Please note that the APIs do not have separate versioning but share the same version as the entire Libris system. This means that with each new version of Libris, the individual APIs may also have changed. The APIs are backward compatible as long as the major version number of the Libris system is the same. This means that any changes in minor versions are additions.

## Availability and limitations

In general, the APIs are available around the clock, but KB delivers the service on a best-effort basis, in other words, without guarantees. The availability of the APIs also depends on available server and network capacity. Any service windows are communicated via the notification function in the Libris cataloguing interface, or in the event of major outages on kb.se and other channels.

The National Library of Sweden may, if necessary, impose restrictions regarding the number of API calls (Rate Limiting). If an individual API user utilizes available resources to such an extent that it impairs the availability of the National Library's services for other users, the National Library may block that client until further notice, while an investigation and dialogue about the usage is ongoing.

## Open data in Libris

The Swedish national bibliography and Swedish authority records are freely available without restrictions. The license form is [Creative Commons Zero](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en). They can be accessed using [OAI-PMH](https://www.kb.se/4.2705879d169b8ba882a43ef.html). The data contains links to [Wikipedia](https://www.wikipedia.org/), [DBPedia](https://wiki.dbpedia.org/), [LC Linked Data Service](http://id.loc.gov/) (Authorities and Vocabularies and subject headings), and [VIAF](http://viaf.org/).

## Contact us

For questions regarding Libris systems and services, you can send an email to libris@kb.se.

For further contact information, see https://www.kb.se/om-oss/kontakta-oss.html.
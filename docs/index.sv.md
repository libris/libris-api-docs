---
title: Libris API-dokumentation
---
Libris är Sveriges nationella bibliotekskatalog. Libris är ett samarbete mellan mer än 600 svenska bibliotek, som gemensamt bygger upp innehållet. Libris består av många olika system, bland annat söktjänst, fjärrlån och katalogiseringsverktyg.

Libris tekniska plattform kallas för Libris XL. Libris datalager är baserad på öppna, länkade data ([RDF](https://www.w3.org/TR/rdf11-primer/)) i modellen [BIBFRAME](https://www.loc.gov/bibframe/docs/bibframe2-model.html) och vokabuläret [KBV](https://id.kb.se/vocab/) (Kungliga bibliotekets basvokabulär).

Med Libris API:er kan du — till exempel — integrera med Librisdata från och till din organisations lokala system. Du kan använda API:er för att både söka i, skriva in till och hämta data från Libris. API:erna fungerar via HTTP.

## Användarvillkor för Libris publika API:er

Libris som dataplattform omfattar ett antal publikt tillgängliga API:er. De flesta av dessa API:er ger lästillgång till öppna, allmänt tillgängliga data. Vissa API:er ger också skrivrättigheter och kräver därmed autentisering och överenskommelse med KB för användning.

Observera att API:erna inte har separat versionering utan har samma version som hela Librissystemet. Detta innebär att vid varje ny version av Libris kan även de enskilda API:erna ha ändrats. API:erna är bakåtkompatibla så länge version-numret av Librissystemet har samma helnummer (major). Detta betyder att alla eventuella förändringar i minor-versioner är tillägg.

## Tillgänglighet och begränsningar

I allmänhet är API:erna tillgängliga dygnet runt, men KB levererar tjänsten efter bästa förmåga, med andra ord utan garantier. API:ernas tillgänglighet beror även på tillgänglig server- och nätverkskapacitet. Eventuella servicefönster delas via notifieringsfunktionen i Libris katalogiseringsgränssnitt, eller vid större avbrott på kb.se och i andra kanaler.

Kungliga biblioteket kan vid behov sätta begränsningar rörande antalet anrop till API:er (Rate Limiting). Om en enskild API-användare utnyttjar tillgängliga resurser i sådan utsträckning att det försämrar tillgängligheten till Kungliga bibliotekets tjänster för andra användare har Kungliga biblioteket möjlighet att blockera den klienten tills vidare, medan en utredning och dialog om nyttjandet pågår.

## Öppna data i Libris

Svensk nationalbibliografi och svenska auktoritetsposter är fritt tillgängliga utan restriktioner. Licensformen är [Creative Commons nivå 0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.sv). De går att komma åt med hjälp av [OAI-PMH](https://www.kb.se/4.2705879d169b8ba882a43ef.html). Datat innehåller länkar till [Wikipedia](https://www.wikipedia.org/), [DBPedia](https://wiki.dbpedia.org/), [LC Linked Data Service](http://id.loc.gov/) (Authorities and Vocabularies och ämnesord) samt [VIAF](http://viaf.org/).

## Kontakta oss

För frågor som rör Libris system och tjänster kan du skicka epost till libris@kb.se.

För ytterligare kontaktinformation, se https://www.kb.se/om-oss/kontakta-oss.html.
---
title: Ange en User-Agent-header
---

När du gör anrop till Libris API:er ber vi dig att inkludera en beskrivande `User-Agent`-header. Det gör det möjligt för oss att identifiera förfrågningar från dig i våra loggar och kontakta dig vid behov.

Utan en meningsfull `User-Agent` ser dina anrop ut som anonym trafik. Om din användning av våra API:er orsakar oväntad belastning eller fel har vi inget sätt att meddela dig. Vid upprepade problem kan anonyma klienter anropsbegränsas eller blockeras utan förvarning.

## Vad som ska ingå

En bra `User-Agent`-sträng identifierar din applikation och ger oss ett sätt att nå dig:

```
User-Agent: min-biblioteksapp/1.0 (https://example.com/; kontakt@example.com)
```

Inkludera minst:

- Namn på din applikation och eventuellt versionsnummer
- En URL och/eller e-postadress så att vi kan kontakta dig vid behov.

## Exempel

### curl

```bash
curl -H "User-Agent: min-biblioteksapp/1.0 (kontakt@example.com)" \
     https://libris.kb.se/find.jsonld?q=...
```

### Python (requests)

```python
import requests

headers = {"User-Agent": "min-biblioteksapp/1.0 (kontakt@example.com)"}
response = requests.get("https://libris.kb.se/find.jsonld", params={"q": "..."}, headers=headers)
```

### JavaScript (fetch)

```js
fetch("https://libris.kb.se/find.jsonld?q=...", {
  headers: { "User-Agent": "min-biblioteksapp/1.0 (kontakt@example.com)" },
});
```

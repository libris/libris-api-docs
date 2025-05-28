---
title: Biblioteksdatabasen
---

```
GET http://bibdb.libris.kb.se/api/lib
```

Via ett API går det att hämta information ur [Biblioteksdatabasen](https://biblioteksdatabasen.libris.kb.se/). Du kan få information om ett enskilt bibliotek eller listningar på samtliga bibliotek.

All data som finns i databasen är tillgänglig via API:et. Data om kontaktuppgifter kräver dock att man skickar med en API-nyckel som extra header i anropet. Kontakta Libris kundservice om du behöver en API-nyckel.

## Parametrar

* `sigel` - Sigler för den/det bibliotek som visas, t.ex. `S`.
* `ocode` - Organisationskod för den samling sigler som ska visas, t.ex. `KB`.
* `dump` - `true` eller `false`. Om värdet är `true` listas samtliga bibliotek.
* `updatedDatum` - Laddar samtliga bibliotek som uppdaterats sedan angivet datum. Anges på formen YYYY-MM-DD, t.ex. `2024-05-23`.
* `start` - Börja listningen från denna position (heltal).
* `n` - Visa max detta antal bibliotek i listningen. Default: `200`.
* `level` - `full` eller `brief`. Avgör hur mycket information som ska visas per bibliotek. Default: `full`.

Vill man begränsa listan ytterligare kan man ange nyckelvärde-par som parametrar till frågan (se nedan).

## Exempel

### Information om ett särskilt bibliotek
För att få information om ett särskilt bibliotek anger man dess sigel som parameter:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?sigel=S'
```

### Information om flera bibliotek
Ange flera sigler för att hämta information om flera bibliotek:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?sigel=S,H'
```

### Information om samtliga bibliotek
För att hämta samtliga bibliotek inom en organisation kan man ange organisationskoden som parameter.

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?ocode=KB'
```

Parametrarna “ocode” och “sigel” går också att kombinera.

### Lista på uppdaterade bibliotek
Vill man få en lista på de bibliotek som uppdaterats sedan ett visst datum kan man använda parametern “updated”:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?updated=2023-06-01'
``` 

Eller vill man ha alla bibliotek så kan man använda “dump”-parametern:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true'
```

### Begränsa urval

För att begränsa sitt urval till ett visst värde, t.ex. enbart svenska bibliotek kan man använda nyckeln för land (country_code), i frågan:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true&country_code=se`
```

Ett annat exempel, för att begränsa till bibliotek i Stockholm:

```bash title="Shell"
curl -XGET 'http://bibdb.libris.kb.se/api/lib?dump=true&address.city=stockholm'
```

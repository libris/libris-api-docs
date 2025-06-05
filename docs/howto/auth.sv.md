---
title: Autentisering och auktorisering
---
API:er som ger skrivtillgång kräver autentisering. Autentiseringen sker genom [Libris Login](https://login.libris.kb.se/) som är kopplat till ett personligt konto. Rättighetshanteringen sköts med hjälp av OAuth2.

För att registrera en klient behöver ni kontakta Libris kundservice som återkommer med den nya klientens id och klienthemlighet samt en instruktion för hur dessa ska användas för att erhålla en bearer-token. Det är denna bearer-token som det hänvisas till i exemplen nedan, och som ska följa med som en parameter i alla anrop som kräver autentisering.

Utdelade uppgifter gäller antingen för testmiljöerna eller för produktionsmiljön beroende på integrationens karaktär och användarens behov.

Om ett anrop besvaras med ”401 Unauthorized“ så innebär det att rättigheter för att utföra uppgiften saknas eller är ogiltiga.

För att skriva data till Libris finns två olika nivåer av rättigheter:

* Nivå 1 för hantering av enbart beståndposter.
* Nivå 2 för hantering av både bibliografiska poster och beståndsposter.

Klienten är knuten till en eller flera sigel, och det är enbart beståndsposter tillhörande den eller dessa sigel som kan skapas, ändras eller tas bort i exemplen nedan..

## Regelverk

För att använda Libris skriv-API:er krävs att man innehar ett API-konto till Libris för att kunna generera en nyckel (token) vid användning.

* Då nyttjande upphör ska detta meddelas KB som inaktiverar API-kontot.
* Vid missbruk eller upprepad felanvändning förbehåller sig KB rätten att utan dröjsmål dra tillbaka behörighet för API-klienten.
* Alla typer av mängdändringar av data i Libris (inklusive skapande av nya poster och borttagande av poster) via API:er får endast utföras i samråd eller efter överenskommelse med KB och ska köras i testmiljö före implementation.
* Mängdändringar av poster i Libris får endast utföras på det egna beståndet om inte annat är överenskommet med Kungliga biblioteket. En systemleverantör, medieleverantör eller leverantör av katalogiseringstjänst kan utföra sådana mängdändringar på uppdrag av ett Librisanslutet biblioteket.

## Hämta ut en bearer-token

När man har fått ett klientid och klienthemlighet ifrån kundtjänst gör man följande för att hämta
en bearer-token:

```bash title="Shell"
curl -X POST -d 'client_id=<ert klientid>&client_secret=<er klienthemlighet>&grant_type=client_credentials' https://login.libris.kb.se/oauth/token'
```
För att hämta ut en token som gäller för testmiljöerna anropas istället `https://login-stg.libris.kb.se/oauth/token`.

Svaret på anropet bör se ut ungefär som följande:

```json title="JSON"
{"access_token": "tU77KXIxxxxxxxKh5qxqgxsS", "expires_in": 36000, "token_type": "Bearer", "scope": "read write", "app_version": "1.5.0"}
```

Utifrån svaret på anropet förväntas ni ta ut er access_token och skicka med den vid varje anrop som kräver autentisering.
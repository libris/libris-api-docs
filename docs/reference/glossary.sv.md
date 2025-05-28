---
title: Introduktion och ordlista
---

Libris XL använder [JSON-LD](https://json-ld.org/) som dataformat och vi tillhandahåller ett API för att skapa, läsa, uppdatera och radera poster. Läsoperationer kan utföras utan autentisering, medan alla andra typer av operationer kräver en access-token.

De API:er som är dokumenterade här är inte versionerade och kan komma att ändras i framtiden.

## Ordlista

Några termer som förekommer i dokumentationen:

`embellished` är en term som beskriver att en post levereras med inte bara sin egen data, utan även med relevant data som posten länkar till. Gränserna för precis vilka data som ligger i en viss sorts post, och vilka som brutits ut för att ligga för sig självt (och länkas till) varierar över tid. Generellt sett så bryts mer och mer data ut, eftersom datan då blir återanvändbar för flera poster. Det är ofta bra att be om poster `embellished` eftersom man då inte påverkas av dessa flyktiga gränsdragningar.

`framed` är en term som beskriver att den extra data som följer med en post på grund av länkningar ska monteras in på de ställen där datan används i JSON-LD-strukturen. Om data inte är `framed` så levereras den i stället som en lista av entiter.

`lens` är en term som beskriver en mekanism för att filtrera bort allt utom den viktigaste informationen i en post. Precis vad som filtreras bort beror på vilken typ av post/entitet man ber om. Det finns för tillfället tre värden för `lens` att välja mellan:

* `chip` är den mest filtrerade varianten. Används den, så är ofta det enda som blir kvar t ex entitetens namn (om ett sådant finns)
* `card` är mellan-nivån. Här inkluderas allting som finns på `chip`-nivån och viss ytterligare information. Det kan t ex vara variant-namn, eller identifikatorer.
* `none` innebär att ingen data filtreras bort.
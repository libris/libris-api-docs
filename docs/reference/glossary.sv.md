---
title: Introduktion och ordlista
---

Libris XL använder [JSON-LD](https://json-ld.org/) som dataformat och vi tillhandahåller ett API för att skapa, läsa, uppdatera och radera poster. Läsoperationer kan utföras utan autentisering, medan alla andra typer av operationer kräver en access-token.

De API:er som är dokumenterade här är inte versionerade och kan komma att ändras i framtiden.

## Ordlista

Några termer som förekommer i dokumentationen:

### embellished
`embellished` är en term som beskriver att en post levereras med inte bara sin egen data, utan även med relevant data som posten länkar till. Gränserna för precis vilka data som ligger i en viss sorts post, och vilka som brutits ut för att ligga för sig självt (och länkas till) varierar över tid. Generellt sett så bryts mer och mer data ut, eftersom datan då blir återanvändbar för flera poster. Det är ofta bra att be om poster `embellished` eftersom man då inte påverkas av dessa flyktiga gränsdragningar.

Till exempel kan en språkegenskap i en post se ut så här med `embellished=false`:

```json
"language": [
  {
    "@id": "https://id.kb.se/language/swe"
  }
]
```

Med `embellished=true`:

```json
"language": [
  {
    "@id": "https://id.kb.se/language/swe",
    "code": "swe",
    "@type": "Language",
    "sameAs": [
      {
        "@id": "https://id.kb.se/i18n/lang/sv"
      }
    ],
    "langTag": "sv",
    "prefLabelByLang": {
      "de": "Schwedisch",
      "en": "Swedish",
      "fr": "suédois",
      "sv": "Svenska"
    },
    ...
  }
```

### framed
`framed` är en term som beskriver att den extra data som följer med en post på grund av länkningar ska monteras in på de ställen där datan används i JSON-LD-strukturen. Om data inte är `framed` så levereras den i stället som en lista av entiteter.


### lens
`lens` är en term som beskriver en mekanism för att filtrera bort allt utom den viktigaste informationen i en post. Precis vad som filtreras bort beror på vilken typ av post/entitet man ber om. Det finns för tillfället tre värden för `lens` att välja mellan:

* `chip` är den mest filtrerade varianten. Används den, så är ofta det enda som blir kvar t ex entitetens namn (om ett sådant finns)
* `card` är mellan-nivån. Här inkluderas allting som finns på `chip`-nivån och viss ytterligare information. Det kan t ex vara variant-namn, eller identifikatorer.
* `none` innebär att ingen data filtreras bort.


### computedLabel
`computedLabel` kan sättas till `sv` eller `en`. Då berikas posterna med färdiga uträknade etiketter med värden från flera egenskaper.

Utan `computedLabel`:

```json
"agent": {
  "@id": "https://libris.kb.se/wt79bh6f2j46dtr#it",
  "@type": "Person",
  "lifeSpan": "1914-2001",
  "givenName": "Tove",
  "familyName": "Jansson",
}
```

Med `computedLabel=sv`:

```json
"agent": {
  "@id": "https://libris.kb.se/wt79bh6f2j46dtr#it",
  "@type": "Person",
  "lifeSpan": "1914-2001",
  "givenName": "Tove",
  "familyName": "Jansson",
  "computedLabel": "Jansson, Tove, 1914-2001"
}
```

Utan `computedLabel`:

```json
"hasTitle": [
  {
    "@type": "Title",
    "subtitleByLang": {
      "ja": "山藤宗山作品集.",
      "ja-Latn-t-ja": "Yamafuji Sōzan sakuhinshū."
    },
    "mainTitleByLang": {
      "ja": "茶花のいれ方",
      "ja-Latn-t-ja": "Chabana no irekata"
    }
  }
]
```

Med `computedLabel=sv`:

```json
"hasTitle": [
  {
    "@type": "Title",
    "subtitleByLang": {
      "ja": "山藤宗山作品集.",
      "ja-Latn-t-ja": "Yamafuji Sōzan sakuhinshū."
    },
    "mainTitleByLang": {
      "ja": "茶花のいれ方",
      "ja-Latn-t-ja": "Chabana no irekata"
    },
    "computedLabel": "茶花のいれ方 : 山藤宗山作品集. ’Chabana no irekata : Yamafuji Sōzan sakuhinshū.’"
  }
]
```

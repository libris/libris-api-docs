## Libris API-dokumentation

Detta repo innehåller dokumentationen för Libris olika API:er. Färdigbyggd dokumentationssajt finns på https://libris.kb.se/api/docs/.

Vi har viss ambition att följa https://diataxis.fr/ vad gäller hur dokumentationen är uppdelad och skriven.

Dokumentationen ligger under `docs/` och är i Markdown-format.

För att generera dokumentationssajten används [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), ett slags framework ovanpå [MkDocs](https://www.mkdocs.org/).

Se:

* https://squidfunk.github.io/mkdocs-material/setup/ för övergripande inställningar
* https://squidfunk.github.io/mkdocs-material/reference/ för saker som kan användas i dokumenten

## Flerspråkighet

[MkDocs static i18n plugin](https://github.com/ultrabug/mkdocs-static-i18n) används för att göra det möjligt att ha sidor på olika språk. Just nu är svenska (`sv`) och engelska (`en`) aktiverade. Svenska är default.

När du skapar en ny sida, skriv den på svenska först. Lägg den på lämpligt ställe under `docs/` och döp filen till `<nånting>.sv.md`. Om du gör en översättning ska den heta `<nånting>.en.md`.

**Vid ändringar: se till att ändra både den svenska och den engelska versionen samtidigt.**

## Navigation

Alla sidor under `docs/` kompileras automatiskt till HTML, men endast de som är explicit listade i `mkdocs.yml` hamnar i navigationsmenyn.

## Bygga och testa sajten lokalt

Allt du behöver är att ha https://github.com/astral-sh/uv installerat, samt en klon av detta repo.

För köra den inbyggda utvecklingsservern:
```bash
uv run mkdocs serve
```
Ovanstående bygger sajten och tillgängliggör den på http://127.0.0.1:8000/api/docs/ som du kan besöka i din webbläsare. Så fort du gör en ändring laddas sajten om automatiskt.

För att bygga sajten och placera resulterande filerna i katalogen `site/`:
```bash
uv run mkdocs build
```

## Steg för steg: skapa en ny sida

Säg att du vill skapa en ny sida om _Vubbelförknysare_ i sektionen **Förklaringar**. Se först till att `uv run mkdocs serve` är igång. Öppna http://127.0.0.1:8000/api/docs/ i en webbläsare.

Skapa sedan filen `docs/explanation/vubbel.sv.md` med följande innehåll:

```
---
title: Vubbelförknysare
---
Under konstruktion.
```

Sedan kan du titta på sidan på http://127.0.0.1:8000/api/docs/explanation/vubbel/.

Hur sökvägen i URL:en ser ut avgörs alltså direkt av filnamnet och dess placering i hierarkin:

* docs/**explanation/vubbel**.sv.md => /api/docs/**explanation/vubbel**/
* docs/**howto/sparql_examples**.sv.md => /api/docs/**howto/sparql_examples**/

För att göra en engelsk version, skapa filen `docs/explanation/vubbel.en.md` med följande innehåll:

```
---
title: Veeblefetzer
---
Under construction.
```

Därefter ska du kunna byta mellan svenska och engelska versionerna av sidan med hjälp av språkväljaren till vänster om sökrutan. Sökvägen för den engelska varianten är identisk, förutom att `en/` automatiskt läggs till före `docs/`, så docs/**explanation/vubbel**.en.md => /api/docs/en/explanation/vubbel/.

Sidan syns inte i menyn per automatik. Det är för att sidorna som ska ingå i menyn just nu är manuellt specificerade. För att lägga till sidan till menyn under "Förklaringar" behöver du redigera `mkdocs.yml`. Lägg till det nyss skapade filnamnet -- utan `.sv`-suffixet -- under raden `- Förklaringar:`. Det vill säga, om det tidigare såg ut så här:

```
  - Förklaringar:
    - explanation/rdf.md
    - explanation/libris.md
```

...så ska du ändra till följande:

```
  - Förklaringar:
    - explanation/rdf.md
    - explanation/libris.md
    - explanation/vubbel.md
```

Efter att du sparat `mkdocs.yml` kommer sidan att laddas om och "Vubbelförknysare" bör synas i menyn under "Förklaringar".

## Att jobba i repot

Det här repot har en enda branch, `master`. Ändringar i master syns normalt inom 10-20 sekunder på https://libris-dev.kb.se/api/docs/ om sajten kompilerar korrekt. Det kan hända att något strular med notifieringen från GitHub; i så fall syns ändringar inom fem minuter.

För större ändringar som behöver granskning: gör ändringarna i en feature-branch och skapa en PR.

Om en sida ska vara tillgänglig för andra men inte synas i menyn (än så länge): se bara till att den är bortkommenterad i `mkdocs.yml`.

All dokumentation ska finnas på svenska, och allt bör även finnas i engelsk översättning.
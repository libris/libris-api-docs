## Libris API-dokumentation

OBS! WIP EXPERIMENTAL osv etc.

Detta repo innehåller dokumentationen för Libris olika API:er.

Dokumentationen ligger under `docs/` och är i Markdown-format.

För att generera dokumentationssajten används [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), ett slags framework ovanpå [MkDocs](https://www.mkdocs.org/).

Se:

* https://squidfunk.github.io/mkdocs-material/setup/ för övergripande inställningar
* https://squidfunk.github.io/mkdocs-material/reference/ för saker som kan användas i dokumenten

## Flerspråkighet

[MkDocs static i18n plugin](https://github.com/ultrabug/mkdocs-static-i18n) används för att göra det möjligt att ha sidor på olika språk. Just nu är svenska (`sv`) och engelska (`en`) aktiverade. Svenska är default.

När du skapar en ny sida, skriv den på svenska först. Lägg den på lämpligt ställe under `docs/` och döp filen till `<nånting>.sv.md`. Om du gör en översättning ska den heta `<nånting>.en.md`.

## Navigation

Alla sidor under `docs/` kompileras automatiskt till HTML, men endast de som är explicit listade i `mkdocs.yml` hamnar i navigationsmenyn.

## Installera, bygg, testa

```bash
# Skapa en venv och aktivera
python3 -m venv venv
source venv/bin/activate
# Installera beroenden
pip3 install -r requirements.txt

# Bygg sajten
mkdocs build # output hamnar i site/

# Kör inbyggd server, sajten laddas om automatiskt vid ändringar
mkdocs serve
```

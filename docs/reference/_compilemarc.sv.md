---
title: Bibliografisk post i MARCXML med bestånds- och auktoritetsinformation
---

```
GET https://libris.kb.se/_compilemarc
```

Detta anrop låter dig ladda ner en komplett bibliografisk post med bestånds- och auktoritetsinformation i MARC21 XML-format. Om den kompletta posten ska innehålla beståndsinformation så måste en exportprofil först ha registrerats för ditt sigel-ID. Kontakta kundservice i fall du behöver registrera en sådan profil.

## Parametrar

* `id` - Den bibliografiska postens ID (t.ex. http://libris.kb.se/bib/1234)
* `library` - Sigel-ID (t.ex. https://libris.kb.se/library/SEK)

## Exempel

Förfrågan:

```bash title="Shell"
curl -XGET 'https://libris.kb.se/_compilemarc?id=http://libris.kb.se/bib/1234&library=https://libris.kb.se/library/SEK'
```

Svar:

```xml title="MARCXML"
<?xml version="1.0" encoding="utf-8"?>
<collection xmlns="http://www.loc.gov/MARC21/slim">
  <record xmlns="http://www.loc.gov/MARC21/slim" type="Bibliographic"><leader>     cam a       7  4500</leader><controlfield tag="001">1234</controlfield><controlfield tag="003">SE-LIBR</controlfield><controlfield tag="005">20110309100026.0</controlfield><controlfield tag="008">011211s1971    xxu|||||||||||000 0|eng|c</controlfield><datafield ind1=" " ind2=" " tag="020"><subfield code="a">0824711165</subfield></datafield><datafield ind1=" " ind2=" " tag="035"><subfield code="9">9900990307</subfield></datafield><datafield ind1=" " ind2=" " tag="040"><subfield code="a">Li*</subfield></datafield><datafield ind1=" " ind2=" " tag="041"><subfield code="a">eng</subfield></datafield><datafield ind1=" " ind2=" " tag="084"><subfield code="a">Ubb</subfield><subfield code="2">kssb/5</subfield></datafield><datafield ind1=" " ind2=" " tag="084"><subfield code="a">Ucef</subfield><subfield code="2">kssb/5</subfield></datafield><datafield ind1=" " ind2=" " tag="084"><subfield code="a">Ue.05</subfield><subfield code="2">kssb/5</subfield></datafield><datafield ind1=" " ind2=" " tag="084"><subfield code="a">Ppdc</subfield><subfield code="2">kssb/5</subfield></datafield><datafield ind1="1" ind2="0" tag="245"><subfield code="a">Water and water pollution handbook</subfield><subfield code="n">Vol. 2</subfield></datafield><datafield ind1=" " ind2="1" tag="264"><subfield code="a">New York :</subfield><subfield code="b">Marcel Dekker,</subfield><subfield code="c">1971</subfield></datafield><datafield ind1=" " ind2=" " tag="300"><subfield code="a">S. 451-800</subfield><subfield code="b">ill.</subfield></datafield><datafield ind1="1" ind2=" " tag="700"><subfield code="a">Ciaccio, Leonard L.</subfield><subfield code="4">edt</subfield></datafield><datafield ind1="0" ind2="0" tag="772"><subfield code="t">Water and water pollution handbook [ed. by L.L. Ciaccio]</subfield><subfield code="d">New York : Marcel Dekker, cop. 1971-1973</subfield><subfield code="w">9900006496</subfield></datafield><datafield ind1=" " ind2=" " tag="887"><subfield code="a">{"@id":"s93ns5h436dxqsh","modified":"2011-03-09T10:00:26+01:00"}</subfield><subfield code="2">librisxl</subfield></datafield></record>
```

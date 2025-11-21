---
title: Typnormalisering
---
Arbetet med typnormaliseringen är pågående och instruktionen kommer att justeras därefter.
Läs mer om typnormaliseringen
(Länk kommer)

De egenskaperna som påverkas är: verkstyp, instanstyp, innehållstyp, utgivningssätt, bärartyp, mediatyp, genre/form


# Verk
####	Nya verkstyper


```json
   "instanceOf": {
        "@type": "Monograph"
                 "Serial"
                 "Collection"
                 "Integrating"
                 
}
```
Gamla verkstyperna uttrycks med contentType (RDA-termlista) eller genreForm (SAOGF-termlista)
####	Ny egenskap Kategori på verket. Hit flyttas gamla verkstyperna, genreForm och innehållstyp
```json
   "instanceOf": {
        "category": [
          {
            "@id": "https://id.kb.se/term/rda/Text"
          },
         {
            "@id": "https://id.kb.se/saogf/Romaner"
          }
        ],
                 
}

```





<details>

<summary>Mappningen mellan gamla verktyperna och contentType (RDA-termlista) eller genreForm (SAOGF-termlista)</summary>

|gamla verktyper  | contentType/genreForm |

| :------------- | -------------: |
| ManuscriptText  |https://id.kb.se/term/saogf/Handskrifter  |
| Text  | https://id.kb.se/term/rda/Text  |
|Audio| https://id.kb.se/term/rda/SpokenWord  |
|NotatedMusic|https://id.kb.se/term/rda/NotatedMusic  |
|MixedMaterial|https://id.kb.se/term/ktg/MixedMaterial|
|Cartography|https://id.kb.se/term/rda/CartographicImage|
|Object|https://id.kb.se/term/rda/ThreeDimensionalForm  |
|Multimedia|https://id.kb.se/term/rda/ComputerProgram|
|Visual|se subklasser MovingImage, StillImage |
|Dataset|https://id.kb.se/term/ktg/Dataset|
|NotatedMovement|https://id.kb.se/term/rda/NotatedMovement|
|Software|https://id.kb.se/term/rda/ComputerProgram|
|Music|https://id.kb.se/term/saogf/Music|
|MusicAudio|https://id.kb.se/term/rda/PerformedMusic|
|NonMusicAudio|https://id.kb.se/term/rda/Sounds|
|ManuscriptNotatedMusic| https://id.kb.se/term/saogf/Handskrifter + https://id.kb.se/term/rda/NotatedMusic |
|Kit|https://id.kb.se/term/ktg/Kit |
|ManuscriptCartography|https://id.kb.se/term/saogf/Handskrifter|  
|MovingImage|https://id.kb.se/term/rda/TwoDimensionalMovingImage  |
|StillImage|https://id.kb.se/term/rda/StillImage |
  
</details>

# Instans

####	Nya instanstyper


```json
   
        "@type": "PhysicalResource"
                 "DigitalResource"
                 
                 
```

####	Ny egenskap Kategori på instansen. Hit flyttas MediaType, CarrierType (och genre/form om den fanns i instansdelen och är olänkad)
```json
   
        "category": [
        {
          "@id": "https://id.kb.se/term/ktg/PrintedVolume"
        }
      ]
                 


```

####	egenskapen IssuanceType utgår. Uppgifterna finns i nya versktyperna  

<details>

<summary>Mappningen mellan gamla IssuanceType och versktyperna</summary>  

 |Issuance Type  | ny versktyp |
 
| ------------- | ------------- |
| Monografisk resurs |"instanceOf": {"@type": "Monograph"}  |
| Integrerande  | "instanceOf": { "@type": "Integrating"}  |
|Samling|"instanceOf": {"@type": "Collection"} |
|Seriell resurs |"instanceOf": {"@type": "Serial"}   |


</details>



<details>
<summary>Exempel i JSON efter typnormaliseringen</summary>   
   
```json
{
  "@graph": [
    {
      "@id": "https://id.kb.se/TEMPID",
      "@type": "Record",
      "mainEntity": {
        "@id": "https://id.kb.se/TEMPID#it"
      },
      "descriptionConventions": [
        {
          "@id": "https://id.kb.se/marc/Isbd"
        },
        {
          "@id": "https://id.kb.se/term/enum/Rda"
        }
      ],
      "descriptionLanguage": {
        "@id": "https://id.kb.se/language/swe"
      },
      "encodingLevel": "marc:MinimalLevel",
      "recordStatus": "",
      "marc:catalogingSource": {
        "@id": "https://id.kb.se/marc/CooperativeCatalogingProgram"
      },
      "descriptionCreator": {
        "@id": "https://libris.kb.se/library/SEK"
      }
    },
    {
      "@id": "https://id.kb.se/TEMPID#it",
     #   "NY INSTANSTYP"
      "@type": "PhysicalResource",  
      "hasTitle": [
        {
          "@type": "Title",
          "mainTitle": "",
          "subtitle": ""
        }
      ],
      "responsibilityStatement": "",
      "identifiedBy": [
        {
          "@type": "ISBN",
          "value": "",
          "qualifier": ""
        }
      ],
      "editionStatement": [
        ""
      ],
      "publication": [
        {
          "@type": "PrimaryPublication",
          "year": "",
          "date": "",
          "country": [
            {
              "@id": "https://id.kb.se/country/sw"
            }
          ],
          "place": {
            "@type": "Place",
            "label": [
              ""
            ]
          },
          "agent": {
            "@type": "Agent",
            "label": [
              ""
            ]
          }
        }
      ],
      "manufacture": [
        {
          "@type": "Manufacture",
          "agent": [
            {
              "@type": "Agent",
              "label": [
                ""
              ]
            }
          ],
          "date": "",
          "place": [
            {
              "@type": "Place",
              "label": [
                ""
              ]
            }
          ]
        }
      ],
      "copyright": [
        {
          "@type": "Copyright",
          "date": ""
        }
      ],
      "extent": [
        {
          "@type": "Extent",
          "label": [
            ""
          ]
        }
      ],
      "physicalDetailsNote": "",
      "hasDimensions": {
        "@type": "Dimensions",
        "label": [
          ""
        ]
      },
      "seriesMembership": [
        {
          "@type": "SeriesMembership",
          "inSeries": {
            "@type": "Instance",
            "identifiedBy": [
              {
                "@type": "ISSN",
                "value": ""
              }
            ],
            "instanceOf": {
              "@type": "Work",
              "hasTitle": [
                {
                  "@type": "Title",
                  "mainTitle": ""
                }
              ]
            }
          },
          "marc:seriesTracingPolicy": "",
          "seriesEnumeration": "",
          "seriesStatement": [
            ""
          ]
        }
      ],
      "hasNote": [
        {
          "@type": "Note",
          "label": [
            ""
          ]
        }
      ],
      "instanceOf": {
     # "NY VERKSTYP" 
        "@type": "Monograph",    
        "language": [
          {
            "@id": "https://id.kb.se/language/swe"
          }
        ],
 #  "NY EGENSKAP för Innehållstyp och GenreForm"
        "category": [
          {   
            "@id": "https://id.kb.se/term/rda/Text"     
          }
        ],
        "classification": [
          {
            "@type": "ClassificationDdc",
            "code": "",
            "edition": "full",
            "editionEnumeration": "23/swe"
          },
          {
            "@type": "Classification",
            "code": "",
            "inScheme": {
              "@type": "ConceptScheme",
              "@id": "https://id.kb.se/term/kssb/8",
              "code": "kssb",
              "version": "8"
            }
          }
        ],
        "contribution": [
          {
            "@type": "PrimaryContribution",
            "agent": null,
            "role": [
              {
                "@id": "https://id.kb.se/relator/author"
              }
            ]
          },
          {
            "@type": "Contribution",
            "agent": null,
            "role": []
          }
        ],
        "hasNote": [
          {
            "@type": "marc:LanguageNote",
            "label": [
              ""
            ]
          }
        ],
        "subject": [],
        "intendedAudience": []
      },
      "contribution": [
        {
          "@type": "Contribution",
          "agent": null,
          "role": []
        }
      ],
 # "NY EGENSKAP för MediaType, CarrierType"
      "category": [
        {  
          "@id": "https://id.kb.se/term/ktg/PrintedVolume"   
        }
      ]
    }
  ]
}
```

</details>


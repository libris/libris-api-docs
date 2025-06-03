---
title: Libris utvecklingsmiljöer
---

Libris har ett antal olika miljöer, med olika bas-URI:er.

Produktionsmiljö: https://libris.kb.se 

* Innehåller all funktionalitet som är satt i produktion och den mest aktuella datan. 
* Kommande release kommuniceras via Nytt inom Libris ca en vecka före releasedatum. Preliminär versionsinformation publiceras en halv vecka före och fullständig versionsinformation efter genomförd release.

Stagingmiljö: https://libris-stg.kb.se 

* Stagingmiljön är en spegling av produktionsmiljön. Den används för att säkerställa att införda ändringar fungerar innan produktionsmiljön uppdateras med dem. 
* Stagingmiljön uppdateras ca 1 vecka före releasedatum.

QA-miljö (Quality Assurance): https://libris-qa.kb.se 

* Används för testning av införd funktionalitet. 
* Uppdateras oregelbundet, vid behov.

EDU-miljö (Education): https://libris-edu.kb.se 

* Används för utbildningar av katalogisering.
* Uppdateras första fredagen varje månad kl. 15.

DEV-miljö (Development): Två miljöer

* Används för utveckling internt.
* Uppdateras oregelbundet, vid behov.
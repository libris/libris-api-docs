---
title: Libris environments
---

Libris has several different environments, each with different base URIs.

Production environment: https://libris.kb.se

* Contains all functionality that is in production and the most current data.
* Upcoming releases are communicated via "Nytt inom Libris" about a week before the release date. Preliminary version information is published half a week before, and full version information after the release is completed.

Staging environment: https://libris-stg.kb.se

* The staging environment mirrors the production environment. It is used to ensure that implemented changes work before the production environment is updated with them.
* The staging environment is updated about 1 week before the release date.

QA environment (Quality Assurance): https://libris-qa.kb.se

* Used for testing implemented functionality.
* Updated irregularly, as needed.

EDU environment (Education): https://libris-edu.kb.se

* Used for cataloguing training.
* Updated on the first Friday of every month at 15:00.

DEV environment (Development): Two environments

* Used for internal development.
* Updated irregularly, as needed. 
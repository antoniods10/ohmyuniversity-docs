---
title: API Index | OhMyUniversity!
description: Comprehensive index of all API sources used in the OhMyUniversity! ecosystem, including endpoints, authentication methods, rate limits, and integration status.
head:
  - - meta
    - property: og:title
      content: API Index | OhMyUniversity!
  - - meta
    - property: og:description
      content: Explore the full list of API sources integrated in OhMyUniversity!, with details on endpoints, authentication, rate limits, and current status.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/api/overview
  - - meta
    - name: keywords
      content: ohmyuniversity api, university api index, cineca ans api, mim open data api, european data portal api, student data api, university integration, api documentation
  - - meta
    - name: twitter:title
      content: API Index | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Full overview of APIs used in OhMyUniversity!, including endpoints, authentication methods, and integration status.
---

# OhMyUniversity! - API Index

Overview of all API sources documented in this repository. Update this table whenever a source is add or modified.

| Source                                | Base URL                                                  | Auth               | Status | Implemented |
| ------------------------------------- | --------------------------------------------------------- | ------------------ | ------ | ----------- |
| European Data Portal                  | `https://data.europa.eu/en`                               | API Key (optional) | Active | Yes         |
| MUR (Università - dati pubblici)      | `https://dati-ustat.mur.gov.it/api/3/action/package_list` | CKAN API standard  | Active | Yes         |
| MIM (Ministero Istruzione) / OpenData | `https://dati.istruzione.it/opendata/esploraidati/`       | N/A                | Active | Yes         |

## Public Universities

| Source             | University                                                                 | Base URL                                            | Auth         | Status | Implemented |
| ------------------ | -------------------------------------------------------------------------- | --------------------------------------------------- | ------------ | ------ | ----------- |
| CINECA/Esse3       | Centro Alti Studi per la Difesa (CASD) di Roma                             | `https://casd.esse3.cineca.it/e3rest/docs`          | Bearer Token | Active | No          |
| ???                | Istituto Italiano di Scienze Umane di Firenze                              | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Istituto universitario di studi superiori di Pavia (IUSS)                  | `https://iusspavia.esse3.cineca.it/e3rest/docs/`    | Bearer Token | Active | No          |
| CINECA/Esse3       | Politecnico di Bari                                                        | `https://poliba.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| Proprietary System | Politecnico di Milano                                                      | ???                                                 | Bearer Token | Active | No          |
| ???                | Politecnico di Torino                                                      | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Scuola IMT Alti Studi di Lucca                                             | `https://imtlucca.esse3.cineca.it/e3rest/docs`      | Bearer Token | Active | No          |
| CINECA/Esse3       | Scuola internazionale superiore di studi avanzati di Trieste               | `https://sissa.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Scuola normale superiore di Pisa                                           | `https://sns.esse3.cineca.it/e3rest/docs`           | Bearer Token | Active | No          |
| CINECA/Esse3       | Scuola Superiore Meridionale di Napoli                                     | `https://ssm.esse3.cineca.it/e3rest/docs`           | Bearer Token | Active | No          |
| CINECA/Esse3       | Scuola superiore di studi universitari e perfezionamento "S. Anna" di Pisa | `https://sssup.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università Ca' Foscari Venezia                                             | `https://unive.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi "Carlo Bo" di Urbino                                | `https://uniurb.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Ancona - Politecnica delle Marche                | `https://univpm.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Bari                                             | `https://uniba.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Bergamo                                          | `https://unibg.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| Proprietary System | Università degli studi di Bologna                                          | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Brescia                                          | `https://unibs.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Cagliari                                         | `https://unica.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Camerino                                         | `https://unicam.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| GOMP/Besmart       | Università degli studi di Cassino e del Lazio Meridionale                  | ???                                                 | Bearer Token | Active | No          |
| ???                | Università degli studi di Catania                                          | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Catanzaro - Magna Grecia                         | `https://unicz.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Ferrara                                          | `https://unife.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Firenze                                          | `https://unifi.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Foggia                                           | `https://unifg.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| ???                | Università degli studi di Genova                                           | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di L'Aquila                                         | `https://univaq.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Macerata                                         | `https://unimc.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Messina                                          | `https://unime.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Milano                                           | Not Found                                           | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Milano-Bicocca                                   | `https://unimib.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Modena e Reggio Emilia                           | `https://unimore.esse3.cineca.it/e3rest/docs`       | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Napoli - Parthenope                              | `https://uniparthenope.esse3.cineca.it/e3rest/docs` | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Napoli Federico II                               | Not Found                                           | Bearer Token | Active | No          |
| ???                | Università degli studi di Padova                                           | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Palermo                                          | `https://unipa.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Parma                                            | `https://unipr.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Pavia                                            | `https://unipv.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Perugia                                          | `https://unipg.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Roma "Foro Italico"                              | `https://iusm.esse3.cineca.it/e3rest/docs`          | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Roma La Sapienza                                 | `https://uniroma1.esse3.cineca.it/e3rest/docs`      | Bearer Token | Active | No          |
| ???                | Università degli studi di Roma Tor Vergata                                 | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Salerno                                          | `https://unisa.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Sassari                                          | `https://uniss.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Teramo                                           | `https://unite.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Torino                                           | `https://unito.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Trento                                           | `https://unitn.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Trieste                                          | `https://units.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Udine                                            | `https://uniud.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Verona                                           | `https://univr.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi dell' Insubria                                      | `https://uninsubria.esse3.cineca.it/e3rest/docs`    | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi del Molise                                          | `https://unimol.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | Yes         |
| CINECA/Esse3       | Università degli studi del Piemonte orientale "Amedeo Avogadro"            | `https://unipmn.esse3.cineca.it/e3rest/docs/`       | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli Studi del Sannio                                          | `https://unisannio.esse3.cineca.it/e3rest/docs`     | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi della Campania "Luigi Vanvitelli"                   | `https://unicampania.esse3.cineca.it/e3rest/docs`   | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi della Basilicata                                    | `https://unibas.esse3.cineca.it/e3rest/docs/`       | Bearer Token | Active | No          |
| Besmart/GOMP       | Università degli Studi della Tuscia                                        | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi Gabriele D'Annunzio di Chieti e Pescara             | `https://unich.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi L'Orientale di Napoli                               | `https://unior.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi Mediterranea di Reggio Calabria                     | `https://unirc.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| Besmart/GOMP       | Università degli studi Roma Tre                                            | ???                                                 | Bearer Token | Active | No          |
| CINECA/Esse3       | Università del Salento                                                     | `https://unisalento.esse3.cineca.it/e3rest/docs`    | Bearer Token | Active | No          |
| CINECA/Esse3       | Università della Calabria                                                  | `https://unical.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3       | Università di Pisa                                                         | `https://unipi.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università degli studi di Siena                                            | `https://unisi.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università Iuav di Venezia                                                 | `https://iuav.esse3.cineca.it/e3rest/docs`          | Bearer Token | Active | No          |
| CINECA/Esse3       | Università per stranieri di Perugia                                        | `https://unipg.esse3.cineca.it/e3rest/docs`         | Bearer Token | Active | No          |
| CINECA/Esse3       | Università per stranieri di Siena                                          | `https://unistrasi.esse3.cineca.it/e3rest/docs`     | Bearer Token | Active | No          |

## Private Universities

| Source               | University                                                                       | Base URL                                         | Auth         | Status | Implemented |
| -------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------ | ------------ | ------ | ----------- |
| CINECA/Esse3         | Gran Sasso Science Institute - Scuola di dottorato internazionale dell'Aquila    | `https://gssi.esse3.cineca.it/e3rest/docs`       | Bearer Token | Active | No          |
| CINECA/Esse3         | Humanitas University                                                             | `https://humanitas.esse3.cineca.it/e3rest/docs/` | Bearer Token | Active | No          |
| UNIMIA               | Istituto Superiore di Educazione Fisica della Lombardia                          | ???                                              | Bearer Token | Active | No          |
| UNIBO                | Istituto Superiore di Educazione Fisica di Bologna                               | ???                                              | Bearer Token | Active | No          |
| UNIFI                | Istituto Superiore di Educazione Fisica di Firenze                               | ???                                              | Bearer Token | Active | No          |
| UNIVAQ               | Istituto Superiore di Educazione Fisica di L'Aquila                              | ???                                              | Bearer Token | Active | No          |
| UNIMIA               | Istituto Superiore di Educazione Fisica di Milano                                | ???                                              | Bearer Token | Active | No          |
| CINECA/Esse3         | Istituto Superiore di Educazione Fisica di Napoli                                | ???                                              | Bearer Token | Active | No          |
| UNIPA                | Istituto Superiore di Educazione Fisica di Palermo                               | ???                                              | Bearer Token | Active | No          |
| UNIPG                | Istituto Superiore di Educazione Fisica di Perugia                               | ???                                              | Bearer Token | Active | No          |
| Campusnet            | Istituto Superiore di Educazione Fisica di Torino                                | ???                                              | Bearer Token | Active | No          |
| UNIURB               | Istituto Superiore di Educazione Fisica di Urbino                                | ???                                              | Bearer Token | Active | No          |
| ???                  | Libera Università di Bolzano                                                     | ???                                              | Bearer Token | Active | No          |
| CINECA/Esse3         | Libera Università di Lingue e Comunicazione (IULM) di Milano                     | `https://iulm.esse3.cineca.it/e3rest/docs`       | Bearer Token | Active | No          |
| CINCECA/Esse3        | Libera Università Maria SS.Assunta - (LUMSA) di Roma                             | `https://lumsa.esse3.cineca.it/e3rest/docs`      | Bearer Token | Active | No          |
| CINECA/Esse3         | Libera Università Mediterranea "Giuseppe Degennaro"                              | `https://lum.esse3.cineca.it/e3rest/docs`        | Bearer Token | Active | No          |
| CINECA/Esse3         | Libera Università della Sicilia Centrale "KORE" di Enna                          | `https://unikore.esse3.cineca.it/e3rest/docs/`   | Bearer Token | Active | No          |
| CINECA/Esse3         | Libera Università, Vita-Salute San Raffaele di Milano                            | `https://unisr.esse3.cineca.it/e3rest/docs/`     | Bearer Token | Active | No          |
| CINECA/Esse3         | Link Campus University di Roma                                                   | `https://unilink.esse3.cineca.it/e3rest/docs/`   | Bearer Token | Active | No          |
| CINCECA/Esse3        | Luiss - Libera Università internazionale degli studi sociali Guido Carli di Roma | `https://luiss.esse3.cineca.it/e3rest/docs/`     | Bearer Token | Active | No          |
| Besmart/Besmart/GOMP | Saint Camillus International University of Health                                | ???                                              | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università "Carlo Cattaneo" (LIUC)                                               | `https://liuc.esse3.cineca.it/e3rest/docs/`      | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università Campus Bio-Medico di Roma                                             | `https://unicampus.esse3.cineca.it/e3rest/docs/` | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università Cattolica del "Sacro Cuore"                                           | `https://unicatt.esse3.cineca.it/e3rest/docs/`   | Bearer Token | Active | No          |
| you@B                | Università commerciale Luigi Bocconi di Milano                                   | ???                                              | Bearer Token | Active | No          |
| Besmart/GOMP         | Università degli studi Suor Orsola Benincasa di Napoli                           | ???                                              | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università della Valle d'Aosta                                                   | `https://univda.esse3.cineca.it/e3rest/docs/`    | Bearer Token | Active | No          |
| ???                  | Università degli Studi Internazionali di Roma – UNINT                            | ???                                              | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università di Scienze Gastronomiche                                              | `https://unisg.esse3.cineca.it/e3rest/docs/`     | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università Europea di Roma                                                       | `https://unier.esse3.cineca.it/e3rest/docs/`     | Bearer Token | Active | No          |
| CINCECA/Esse3        | Università per stranieri "Dante Alighieri" di Reggio Calabria                    | `https://unist.esse3.cineca.it/e3rest/docs/`     | Bearer Token | Active | No          |

## Telematic Universities

| Source        | University                                                 | Base URL                                          | Auth         | Status | Implemented |
| ------------- | ---------------------------------------------------------- | ------------------------------------------------- | ------------ | ------ | ----------- |
| CINCECA/Esse3 | Università telematica "e-Campus" di Novedrate (CO)         | `https://uniecampus.esse3.cineca.it/e3rest/docs`  | Bearer Token | Active | No          |
| Besmart/GOMP  | Università telematica "Giustino Fortunato" di Benevento    | ???                                               | Bearer Token | Active | No          |
| Besmart/GOMP  | Università telematica "Italian University line" di Firenze | ???                                               | Bearer Token | Active | No          |
| Multiversity  | Università telematica "Pegaso" di Napoli                   | ???                                               | Bearer Token | Active | No          |
| Multiversity  | Università telematica "San Raffaele" di Roma               | ???                                               | Bearer Token | Active | No          |
| CINECA/Esse3  | Università telematica Guglielmo Marconi di Roma            | `https://unimarconi.esse3.cineca.it/e3rest/docs`  | Bearer Token | Active | No          |
| CINECA/Esse3  | Università telematica internazionale UNINETTUNO di Roma    | `https://uninettuno.esse3.cineca.it/e3rest/docs`? | Bearer Token | Active | No          |
| Sophia        | Università telematica Niccolò Cusano di Roma               | ???                                               | Bearer Token | Active | No          |
| CINECA/Esse3  | Università telematica non statale "Leonardo da Vinci"      | `https://unidav.esse3.cineca.it/e3rest/docs`      | Bearer Token | Active | No          |
| ???           | Università telematica Unitelma Sapienza di Roma            | ???                                               | Bearer Token | Active | No          |
| Multiversity  | Universitas telematica Mercatorum di Roma                  | ???                                               | Bearer Token | Active | No          |

### Status legend

- active - endpoints verified and working
- to be verified - documentation available but endpoints not recently tested
- deprecated - do not use, see the source changelog

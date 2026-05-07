---
title: Logistica API V1 | OhMyUniversity!
description: REST API documentation for the Logistica service (logistica-service-v1) - CINECA ESSE3.
head:
  - - meta
    - property: og:title
      content: Logistica API V1 | OhMyUniversity!
  - - meta
    - property: og:description
      content: REST API documentation for the Logistica service (logistica-service-v1) - CINECA ESSE3.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/api/cineca/api-unimol/logistica-api-v1
  - - meta
    - name: keywords
      content: logistica v1 api, esse3 rest api, cineca api, ohmyuniversity api, logistica-service-v1
  - - meta
    - name: twitter:title
      content: Logistica API V1 | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: REST API documentation for the Logistica service (logistica-service-v1) - CINECA ESSE3.
---

# OhMyUniversity! - Unimol: Logistica API V1

**Version:** `1.0.0` · **Base URL:** `/logistica-service-v1`

API REST di ESSE3 per l'accesso alla parte logistica dell'offerta didattica

---

## Endpoints - Easystaff

### `GET /easystaff/logistica/{aaOffId}` - Logistics information for easystaff.

```java
/**
 * # ATTENZIONE
 *
 * @param aaOffId              integer (path, required)            - Offering year ID
 * @param dipCod               string (query, optional)            - Department code
 * @param listaCds             string (query, optional)            - List of course codes to extract separated by ,
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<EasystaffAdLogConDettagli> on success,
 *         DettaglioErrore on failure
 */
GET /easystaff/logistica/{aaOffId}
```

**Auth:** `EasyStaff` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {}
]
```

**`422 Unprocessable Entity` - error**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /easystaff/struttura/{aaOffId}` - Structure information for easystaff.

```java
/**
 * # ATTENZIONE
 *
 * @param aaOffId              integer (path, required)            - Offering year ID
 * @param dipCod               string (query, optional)            - Department code
 * @param listaCds             string (query, optional)            - List of course codes to extract separated by ,
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<EasystaffCdsordConDettagli> on success,
 *         DettaglioErrore on failure
 */
GET /easystaff/struttura/{aaOffId}
```

**Auth:** `EasyStaff` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {}
]
```

**`422 Unprocessable Entity` - error**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

## Endpoints - Logistica

### `GET /logistica` - Retrieves the list of records from the P09_AD_LOG header table, filtered by optional parameters. Parameters only work on the physical AD of the sharing.

```java
/**
 * Retrieves the list of AdLog objects with logistics header information.
 * The optional start and limit parameters allow pagination of results.
 *
 * @param aaOffId              integer (query, optional)           - Offering year ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adId                 integer (query, optional)           - Educational activity ID
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param linguaDidCod         string (query, optional)            - ISO6392 code of the teaching language
 * @param sedeDes              string (query, optional)            - Description of the location (if the character * is used, the like operator is applied)
 * @param dataModLog           string (query, optional)            - Last modification date of the logistics
 * @param start                integer (query, optional)           - Used together with `limit` to indicate record pagination
 * @param limit                integer (query, optional)           - Used together with `start` to indicate record pagination, `limit` indicates the number of ...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<AdLog> on success
 */
GET /logistica
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione (required)
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015" // Last modification date of the entire logistics. Format: DD/MM/YYYY
  }
]
```

<br>

---

<br>

### `GET /logistica/copertura/cancellabile` - Indicates whether a coverage is cancellable.

```java
/**
 * Indicates if a coverage is cancellable by retrieving an object of type
 * 'CoperturaCancellabile'. It is necessary to specify at least one parameter. #####
 * Return codes of the function Return Code | Description - | - 0 |
 * coverage not present in ESSE3, therefore cancellable 1 | coverage
 * cancellable 2 | coverage NOT cancellable because it is present in a teacher
 * register (P09_REG_DOC) 3 | coverage NOT cancellable because it is present in a
 * questionnaire
 *
 * @param coperId              integer (query, optional)           - Coverage identifier
 * @param matricola            string (query, optional)            - Student matriculation code
 * @param aaOffId              integer (query, optional)           - Offering year identifier
 * @param cdsCod               string (query, optional)            - Study course code
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param adCod                string (query, optional)            - Educational activity code
 * @param udCod                string (query, optional)            - Teaching unit code
 * @param domPartCod           string (query, optional)            - Partition domain code of students within a partition factor
 * @return CoperturaCancellabile on success,
 *         DettaglioErrore on failure
 */
GET /logistica/copertura/cancellabile
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
{
  "retCode": 1, // Code indicating whether the coverage is cancellable
  "msg": "Non è possibile cancellare la copertura in quanto è presente nel registro docenti di ESSE3" // Any message with additional information
}
```

**`422 Unprocessable Entity` - error**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `PUT /logistica/syllabusAD` - Updates the syllabus information associated with the keyed logistics (complete or partial). If domain and/or partition codes are not specified, the provided information will be used to identify the entities to update. In the case of UTENTE_TECNICO, the docenteMatricola field must be valued.

```java
/**
 * Updates syllabus information associated with the keyed logistics
 * (complete or partial).
 *
 * @param docenteMatricola     string (query, optional)            - Teacher matriculation
 * @param body                 SyllabusADPatch (body, required)    - Syllabus information to be updated. Optional fields not specified will not be considered ...
 * @return SyllabusADPatchResult on success,
 *         DettaglioErrore on failure
 */
PUT /logistica/syllabusAD
```

**Auth:** `DOCENTE`, `UTENTE_TECNICO` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Request body

```json
{
  "aaOffId": 2001, // Offering year providing the educational activity (required)
  "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity (required)
  "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
  "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity (required)
  "adCod": "PDS_AD_1", // Educational activity code (required)
  "fatPartCod": "ALF", // Codice del fattore di partizione
  "domPartCod": "PARI", // Codice del dominio di partizione
  "obiettiviSvilSostenibileList": "1,3,13", // List of associated sustainable development goals
  "rimuoviObiettiviSvilSostenibileList": 0, // If set to 1 updates the content of obiettiviSvilSostenibileList to NULL regardless of the field value
  "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili
  "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
  "campiSyllabus": [
    {
      "nomeCampo": "CONTENUTI", // Name of the field to be updated
      "valore": "valore", // Value to update in the field indicated in `nomeCampo`
      "iso6392": "ita" // ISO6392 code for the language associated with the field to be modified (required)
    }
  ] // CampiSyllabus
}
```

#### Response

**`200 OK`**

```json
{
  "righeAdLogPdsAggiornate": 5, // Number of rows updated in P09_AD_LOG_PDS
  "righeAdLogPdsDesLinAggiornate": [
    {
      "righeAggiornate": 5, // Number of rows updated in P09_AD_LOG_PDS_DES_LIN associated to the ISO6392 code
      "righeEliminate": 5, // Number of rows updated in P09_AD_LOG_PDS_DES_LIN associated to the ISO6392 code
      "righeInserite": 5, // Number of rows updated in P09_AD_LOG_PDS_DES_LIN associated to the ISO6392 code
      "iso6392": "5" // ISO6392 code of the linguistic updates performed
    }
  ] // RigheAdLogPdsDesLinAggiornate
}
```

**`422 Unprocessable Entity` - Operation not allowed**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /logistica/{adLogId}/adLogConSyllabus` - Retrieves the syllabus of the keyed logistics. Optional parameters filter any AD of the sharing.

```java
/**
 * Retrieves the list of AdLogConSyllabus objects.
 *
 * @param adLogId              integer (path, required)            - Logistics ID
 * @param aaOffId              integer (query, optional)           - Offering year ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<AdLogConSyllabus> on success
 */
GET /logistica/{adLogId}/adLogConSyllabus
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015", // Last modification date of the entire logistics. Format: DD/MM/YYYY
    "SyllabusAD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveADContestualizzata": {
          "cdsId": 1, // Key of the study course providing the educational activity (required)
          "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
          "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
          "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
          "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
          "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
          "pdsId": 1, // Key of the study path providing the educational activity (required)
          "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
          "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
          "aaOffId": 1, // Offering year providing the educational activity (required)
          "adId": 1, // Educational activity key (required)
          "adCod": "PDS_AD_1", // Educational activity code
          "adDes": "Esempio di PDS AD", // Educational activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveADContestualizzata (required)
        "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili (required)
        "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
        "realFisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing. The flag is taken directly from DB.
        "tipoCatalogoCod": "C", // Code of the study course catalog type (optional)
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "metodiDidattici": "metodi didattici", // Teaching methods
        "metodiDidatticiEng": "metodi didattici", // Teaching methods in English
        "modalitaVerificaApprendimento": "modalita verifica apprendimento", // Assessment methods
        "modalitaVerificaApprendimentoEng": "modalita verifica apprendimento in inglese", // Assessment methods in English
        "altreInfo": "altre informazioni", // Other information
        "altreInfoEng": "altre informazioni", // Other information in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "adLogOpz": [
          {
            "chiaveADContestualizzata": {}
            "chiavePartizione": {}
            "facId": 1, // Unique faculty ID
            "facCod": "FAC_1", // Faculty code
            "facDes": "Esempio di FAC", // Faculty description
            "facDesEng": "Esempio di FAC", // Faculty description (in English)
            "areaDiscCod": "AREA_1", // Disciplinary area code related to the faculty/department
            "areaDiscDes": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department
            "areaDiscDesEng": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department (in English)
            "integratoFlg": 1, // Integrated
            "tipoCorsoCod": "SP4", // Course type code
            "tipoCorsoDes": "Specializzazione 4 anni", // Course description
            "tipoCorsoDesEng": "Residency Program (4 years)" // Course description (in English)
          }
        ], // AdLogOpz (optional)
        "syllabusOpt1": "campo opzionale 1", // Optional field 1 (optional)
        "syllabusOpt1Eng": "optional field 1", // Optional field 1 in English (optional)
        "syllabusOpt2": "campo opzionale 2", // Optional field 2 (optional)
        "syllabusOpt2Eng": "optional field 2", // Optional field 2 in English (optional)
        "syllabusOpt3": "campo opzionale 3", // Optional field 3 (optional)
        "syllabusOpt3Eng": "optional field 3", // Optional field 3 in English (optional)
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "metodiDidatticiSpa": "metodi didattici", // Teaching methods in Spanish (optional)
        "modalitaVerificaApprendimentoSpa": "modalita verifica apprendimento in spagnolo", // Assessment methods in Spanish (optional)
        "altreInfoSpa": "altre informazioni", // Other information in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "syllabusOpt1Spa": "optional field 1", // Optional field 1 in Spanish (optional)
        "syllabusOpt2Spa": "optional field 2", // Optional field 2 in Spanish (optional)
        "syllabusOpt3Spa": "optional field 3", // Optional field 3 in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "metodiDidatticiFra": "metodi didattici", // Teaching methods in French (optional)
        "modalitaVerificaApprendimentoFra": "modalita verifica apprendimento in francese", // Assessment methods in French (optional)
        "altreInfoFra": "altre informazioni", // Other information in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "syllabusOpt1Fra": "optional field 1", // Optional field 1 in French (optional)
        "syllabusOpt2Fra": "optional field 2", // Optional field 2 in French (optional)
        "syllabusOpt3Fra": "optional field 3", // Optional field 3 in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "metodiDidatticiDeu": "metodi didattici", // Teaching methods in German (optional)
        "modalitaVerificaApprendimentoDeu": "modalita verifica apprendimento in tedesco", // Assessment methods in German (optional)
        "altreInfoDeu": "altre informazioni", // Other information in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "syllabusOpt1Deu": "optional field 1", // Optional field 1 in German (optional)
        "syllabusOpt2Deu": "optional field 2", // Optional field 2 in German (optional)
        "syllabusOpt3Deu": "optional field 3", // Optional field 3 in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ] // SyllabusAD
  }
]
```

<br>

---

<br>

### `GET /logistica/{adLogId}/udLogConDettagli` - Retrieves the details of the keyed logistics UDs, namely the syllabus of the UDs and the teaching load. Optional parameters filter any UD of the sharing.

```java
/**
 * Retrieves the list of UdLogConDettagli objects.
 *
 * @param adLogId              integer (path, required)            - Logistics ID
 * @param aaOffId              integer (query, optional)           - Offering year ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param udCod                string (query, optional)            - Teaching unit code
 * @param udDes                string (query, optional)            - descrizione dell'unit� didattica (se viene utilizzato il carattere * viene applicato il like)
 * @param docenteMatricola     string (query, optional)            - Teacher matriculation
 * @param docenteCognome       string (query, optional)            - Teacher surname
 * @param docenteNome          string (query, optional)            - Teacher name
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<UdLogConDettagli> on success
 */
GET /logistica/{adLogId}/udLogConDettagli
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveUDMaster": {
      "chiaveAdContestualizzata": {
        "cdsId": 1, // Key of the study course providing the educational activity (required)
        "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
        "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
        "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
        "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
        "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
        "pdsId": 1, // Key of the study path providing the educational activity (required)
        "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
        "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
        "aaOffId": 1, // Offering year providing the educational activity (required)
        "adId": 1, // Educational activity key (required)
        "adCod": "PDS_AD_1", // Educational activity code
        "adDes": "Esempio di PDS AD", // Educational activity description
        "afId": 1 // afId from U-Gov Didattica
      }, // ChiaveAdContestualizzata (required)
      "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
      "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
      "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
      "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
    }, // ChiaveUDMaster
    "udLogId": 1, // Non-unique ID that identifies a logistics sharing at module level
    "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
    "SyllabusUD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveUDContestualizzata": {
          "chiaveAdContestualizzata": {
            "cdsId": 1, // Key of the study course providing the educational activity (required)
            "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
            "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
            "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
            "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
            "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
            "pdsId": 1, // Key of the study path providing the educational activity (required)
            "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
            "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
            "aaOffId": 1, // Offering year providing the educational activity (required)
            "adId": 1, // Educational activity key (required)
            "adCod": "PDS_AD_1", // Educational activity code
            "adDes": "Esempio di PDS AD", // Educational activity description
            "afId": 1 // afId from U-Gov Didattica
          }, // ChiaveAdContestualizzata (required)
          "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
          "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
          "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
          "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
        }, // ChiaveUDContestualizzata (required)
        "desUdPubblFlg": 0, // Flag che indica se le descrizioni delle unit� didattiche sono pubblicabili (required)
        "masterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing
        "realMasterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing. Flag taken directly from DB.
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
        "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ], // SyllabusUD
    "CaricoDocenti": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
        "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
        "tipoCreDesEng": "Lezione", // Credit type description in English
        "docenteId": 1, // Key of the teacher giving lessons (required)
        "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
        "docenteNome": "Mario", // Name of the teacher giving lessons
        "docenteCognome": "Rossi", // Surname of the teacher giving lessons
        "ore": 4.0, // Lesson hours
        "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
        "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
        "oreAttSuppDid": 8.0, // Teaching support activity hours
        "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
        "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
        "tipoCoperturaDes": "Ordinario", // Description of the type of teacher coverage
        "CaricoDocentiOpz": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
            "docenteId": 1, // Key of the teacher giving lessons (required)
            "codFis": "LNZFCR67R07A944E", // Teacher fiscal code
            "eMail": "pr@ateneo.it", // Teacher email
            "settCod": "settCod", // Scientific disciplinary sector code
            "titolareFlg": 1, // Indicates if the teacher is titular of the student partition for the AD at PDS level
            "respDidFlg": 1, // Indicates if the teacher is educational manager of the student partition for the UD
            "userId": "userId", // User ID
            "dataModDoc": "15/10/2015", // Modification date of teacher information
            "dataMod": "15/10/2015", // Modification date of information related to the distribution of teaching load among teachers
            "ugovCoperId": 12345, // Unique ID identifying a coverage
            "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
            "tipoCoperturaDes": "Ordinario" // Description of the type of teacher coverage
          }
        ] // CaricoDocentiOpz (optional)
      }
    ] // CaricoDocenti
  }
]
```

<br>

---

<br>

### `GET /logisticaFull/{adLogId}/` - Retrieves all information of the keyed logistics. Optional parameters filter any AD of the sharing.

```java
/**
 * Retrieves the list of LogisticaConDettagli objects with the same
 * AD_LOG_ID key. The optional start and limit parameters allow paginating the
 * results.
 *
 * @param adLogId              integer (path, required)            - Logistics ID
 * @param aaOffId              integer (query, optional)           - Offering year ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<LogisticaConDettagli> on success
 */
GET /logisticaFull/{adLogId}/
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015", // Last modification date of the entire logistics. Format: DD/MM/YYYY
    "SyllabusAD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveADContestualizzata": {
          "cdsId": 1, // Key of the study course providing the educational activity (required)
          "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
          "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
          "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
          "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
          "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
          "pdsId": 1, // Key of the study path providing the educational activity (required)
          "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
          "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
          "aaOffId": 1, // Offering year providing the educational activity (required)
          "adId": 1, // Educational activity key (required)
          "adCod": "PDS_AD_1", // Educational activity code
          "adDes": "Esempio di PDS AD", // Educational activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveADContestualizzata (required)
        "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili (required)
        "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
        "realFisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing. The flag is taken directly from DB.
        "tipoCatalogoCod": "C", // Code of the study course catalog type (optional)
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "metodiDidattici": "metodi didattici", // Teaching methods
        "metodiDidatticiEng": "metodi didattici", // Teaching methods in English
        "modalitaVerificaApprendimento": "modalita verifica apprendimento", // Assessment methods
        "modalitaVerificaApprendimentoEng": "modalita verifica apprendimento in inglese", // Assessment methods in English
        "altreInfo": "altre informazioni", // Other information
        "altreInfoEng": "altre informazioni", // Other information in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "adLogOpz": [
          {
            "chiaveADContestualizzata": {}
            "chiavePartizione": {}
            "facId": 1, // Unique faculty ID
            "facCod": "FAC_1", // Faculty code
            "facDes": "Esempio di FAC", // Faculty description
            "facDesEng": "Esempio di FAC", // Faculty description (in English)
            "areaDiscCod": "AREA_1", // Disciplinary area code related to the faculty/department
            "areaDiscDes": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department
            "areaDiscDesEng": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department (in English)
            "integratoFlg": 1, // Integrated
            "tipoCorsoCod": "SP4", // Course type code
            "tipoCorsoDes": "Specializzazione 4 anni", // Course description
            "tipoCorsoDesEng": "Residency Program (4 years)" // Course description (in English)
          }
        ], // AdLogOpz (optional)
        "syllabusOpt1": "campo opzionale 1", // Optional field 1 (optional)
        "syllabusOpt1Eng": "optional field 1", // Optional field 1 in English (optional)
        "syllabusOpt2": "campo opzionale 2", // Optional field 2 (optional)
        "syllabusOpt2Eng": "optional field 2", // Optional field 2 in English (optional)
        "syllabusOpt3": "campo opzionale 3", // Optional field 3 (optional)
        "syllabusOpt3Eng": "optional field 3", // Optional field 3 in English (optional)
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "metodiDidatticiSpa": "metodi didattici", // Teaching methods in Spanish (optional)
        "modalitaVerificaApprendimentoSpa": "modalita verifica apprendimento in spagnolo", // Assessment methods in Spanish (optional)
        "altreInfoSpa": "altre informazioni", // Other information in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "syllabusOpt1Spa": "optional field 1", // Optional field 1 in Spanish (optional)
        "syllabusOpt2Spa": "optional field 2", // Optional field 2 in Spanish (optional)
        "syllabusOpt3Spa": "optional field 3", // Optional field 3 in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "metodiDidatticiFra": "metodi didattici", // Teaching methods in French (optional)
        "modalitaVerificaApprendimentoFra": "modalita verifica apprendimento in francese", // Assessment methods in French (optional)
        "altreInfoFra": "altre informazioni", // Other information in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "syllabusOpt1Fra": "optional field 1", // Optional field 1 in French (optional)
        "syllabusOpt2Fra": "optional field 2", // Optional field 2 in French (optional)
        "syllabusOpt3Fra": "optional field 3", // Optional field 3 in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "metodiDidatticiDeu": "metodi didattici", // Teaching methods in German (optional)
        "modalitaVerificaApprendimentoDeu": "modalita verifica apprendimento in tedesco", // Assessment methods in German (optional)
        "altreInfoDeu": "altre informazioni", // Other information in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "syllabusOpt1Deu": "optional field 1", // Optional field 1 in German (optional)
        "syllabusOpt2Deu": "optional field 2", // Optional field 2 in German (optional)
        "syllabusOpt3Deu": "optional field 3", // Optional field 3 in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ], // SyllabusAD
    "UdLogConDettagli": [
      {
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione
        "chiaveUDMaster": {
          "chiaveAdContestualizzata": {
            "cdsId": 1, // Key of the study course providing the educational activity (required)
            "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
            "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
            "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
            "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
            "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
            "pdsId": 1, // Key of the study path providing the educational activity (required)
            "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
            "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
            "aaOffId": 1, // Offering year providing the educational activity (required)
            "adId": 1, // Educational activity key (required)
            "adCod": "PDS_AD_1", // Educational activity code
            "adDes": "Esempio di PDS AD", // Educational activity description
            "afId": 1 // afId from U-Gov Didattica
          }, // ChiaveAdContestualizzata (required)
          "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
          "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
          "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
          "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
        }, // ChiaveUDMaster
        "udLogId": 1, // Non-unique ID that identifies a logistics sharing at module level
        "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
        "SyllabusUD": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "chiaveUDContestualizzata": {}
            "desUdPubblFlg": 0, // Flag che indica se le descrizioni delle unit� didattiche sono pubblicabili (required)
            "masterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing
            "realMasterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing. Flag taken directly from DB.
            "contenuti": "contenuti del corso", // Course contents
            "contenutiEng": "contenuti del corso", // Course contents in English
            "obiettiviFormativi": "obiettivi formativi", // Educational objectives
            "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
            "prerequisiti": "prerequisiti", // Prerequisites
            "prerequisitiEng": "prerequisiti", // Prerequisites in English
            "testiRiferimento": "testi riferimento", // Reference texts
            "testiRiferimentoEng": "testi riferimento", // Reference texts in English
            "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
            "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
            "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
            "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
            "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
            "obiettiviSvilSostenibile": ["..."]
            "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
            "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
            "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
            "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
            "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
            "contenutiFra": "contenuti del corso", // Course contents in French (optional)
            "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
            "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
            "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
            "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
            "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
            "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
            "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
            "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
            "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
          }
        ], // SyllabusUD
        "CaricoDocenti": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
            "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
            "tipoCreDesEng": "Lezione", // Credit type description in English
            "docenteId": 1, // Key of the teacher giving lessons (required)
            "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
            "docenteNome": "Mario", // Name of the teacher giving lessons
            "docenteCognome": "Rossi", // Surname of the teacher giving lessons
            "ore": 4.0, // Lesson hours
            "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
            "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
            "oreAttSuppDid": 8.0, // Teaching support activity hours
            "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
            "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
            "tipoCoperturaDes": "Ordinario", // Description of the type of teacher coverage
            "CaricoDocentiOpz": ["..."]
          }
        ] // CaricoDocenti
      }
    ] // UdLogConDettagli
  }
]
```

<br>

---

<br>

### `GET /logisticaPerAdFull/{aaOffId}` - Logistics retrieval by study course.

```java
/**
 * Retrieves the list of LogisticaConDettagli objects with the same offering
 * year and study course identifier or code (one of the two must be specified).
 *
 * @param aaOffId              integer (path, required)            - Offering year ID
 * @param cdsId                integer (query, optional)           - Study course identifier
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param dataModLog           string (query, optional)            - Last modification date of the logistics
 * @param desAdPubblFlg        integer (query, optional)           - Flag indicating if the descriptions of the educational activities are publishable
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<LogisticaConDettagli> on success,
 *         DettaglioErrore on failure
 */
GET /logisticaPerAdFull/{aaOffId}
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015", // Last modification date of the entire logistics. Format: DD/MM/YYYY
    "SyllabusAD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveADContestualizzata": {
          "cdsId": 1, // Key of the study course providing the educational activity (required)
          "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
          "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
          "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
          "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
          "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
          "pdsId": 1, // Key of the study path providing the educational activity (required)
          "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
          "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
          "aaOffId": 1, // Offering year providing the educational activity (required)
          "adId": 1, // Educational activity key (required)
          "adCod": "PDS_AD_1", // Educational activity code
          "adDes": "Esempio di PDS AD", // Educational activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveADContestualizzata (required)
        "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili (required)
        "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
        "realFisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing. The flag is taken directly from DB.
        "tipoCatalogoCod": "C", // Code of the study course catalog type (optional)
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "metodiDidattici": "metodi didattici", // Teaching methods
        "metodiDidatticiEng": "metodi didattici", // Teaching methods in English
        "modalitaVerificaApprendimento": "modalita verifica apprendimento", // Assessment methods
        "modalitaVerificaApprendimentoEng": "modalita verifica apprendimento in inglese", // Assessment methods in English
        "altreInfo": "altre informazioni", // Other information
        "altreInfoEng": "altre informazioni", // Other information in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "adLogOpz": [
          {
            "chiaveADContestualizzata": {}
            "chiavePartizione": {}
            "facId": 1, // Unique faculty ID
            "facCod": "FAC_1", // Faculty code
            "facDes": "Esempio di FAC", // Faculty description
            "facDesEng": "Esempio di FAC", // Faculty description (in English)
            "areaDiscCod": "AREA_1", // Disciplinary area code related to the faculty/department
            "areaDiscDes": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department
            "areaDiscDesEng": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department (in English)
            "integratoFlg": 1, // Integrated
            "tipoCorsoCod": "SP4", // Course type code
            "tipoCorsoDes": "Specializzazione 4 anni", // Course description
            "tipoCorsoDesEng": "Residency Program (4 years)" // Course description (in English)
          }
        ], // AdLogOpz (optional)
        "syllabusOpt1": "campo opzionale 1", // Optional field 1 (optional)
        "syllabusOpt1Eng": "optional field 1", // Optional field 1 in English (optional)
        "syllabusOpt2": "campo opzionale 2", // Optional field 2 (optional)
        "syllabusOpt2Eng": "optional field 2", // Optional field 2 in English (optional)
        "syllabusOpt3": "campo opzionale 3", // Optional field 3 (optional)
        "syllabusOpt3Eng": "optional field 3", // Optional field 3 in English (optional)
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "metodiDidatticiSpa": "metodi didattici", // Teaching methods in Spanish (optional)
        "modalitaVerificaApprendimentoSpa": "modalita verifica apprendimento in spagnolo", // Assessment methods in Spanish (optional)
        "altreInfoSpa": "altre informazioni", // Other information in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "syllabusOpt1Spa": "optional field 1", // Optional field 1 in Spanish (optional)
        "syllabusOpt2Spa": "optional field 2", // Optional field 2 in Spanish (optional)
        "syllabusOpt3Spa": "optional field 3", // Optional field 3 in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "metodiDidatticiFra": "metodi didattici", // Teaching methods in French (optional)
        "modalitaVerificaApprendimentoFra": "modalita verifica apprendimento in francese", // Assessment methods in French (optional)
        "altreInfoFra": "altre informazioni", // Other information in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "syllabusOpt1Fra": "optional field 1", // Optional field 1 in French (optional)
        "syllabusOpt2Fra": "optional field 2", // Optional field 2 in French (optional)
        "syllabusOpt3Fra": "optional field 3", // Optional field 3 in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "metodiDidatticiDeu": "metodi didattici", // Teaching methods in German (optional)
        "modalitaVerificaApprendimentoDeu": "modalita verifica apprendimento in tedesco", // Assessment methods in German (optional)
        "altreInfoDeu": "altre informazioni", // Other information in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "syllabusOpt1Deu": "optional field 1", // Optional field 1 in German (optional)
        "syllabusOpt2Deu": "optional field 2", // Optional field 2 in German (optional)
        "syllabusOpt3Deu": "optional field 3", // Optional field 3 in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ], // SyllabusAD
    "UdLogConDettagli": [
      {
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione
        "chiaveUDMaster": {
          "chiaveAdContestualizzata": {
            "cdsId": 1, // Key of the study course providing the educational activity (required)
            "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
            "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
            "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
            "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
            "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
            "pdsId": 1, // Key of the study path providing the educational activity (required)
            "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
            "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
            "aaOffId": 1, // Offering year providing the educational activity (required)
            "adId": 1, // Educational activity key (required)
            "adCod": "PDS_AD_1", // Educational activity code
            "adDes": "Esempio di PDS AD", // Educational activity description
            "afId": 1 // afId from U-Gov Didattica
          }, // ChiaveAdContestualizzata (required)
          "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
          "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
          "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
          "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
        }, // ChiaveUDMaster
        "udLogId": 1, // Non-unique ID that identifies a logistics sharing at module level
        "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
        "SyllabusUD": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "chiaveUDContestualizzata": {}
            "desUdPubblFlg": 0, // Flag che indica se le descrizioni delle unit� didattiche sono pubblicabili (required)
            "masterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing
            "realMasterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing. Flag taken directly from DB.
            "contenuti": "contenuti del corso", // Course contents
            "contenutiEng": "contenuti del corso", // Course contents in English
            "obiettiviFormativi": "obiettivi formativi", // Educational objectives
            "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
            "prerequisiti": "prerequisiti", // Prerequisites
            "prerequisitiEng": "prerequisiti", // Prerequisites in English
            "testiRiferimento": "testi riferimento", // Reference texts
            "testiRiferimentoEng": "testi riferimento", // Reference texts in English
            "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
            "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
            "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
            "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
            "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
            "obiettiviSvilSostenibile": ["..."]
            "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
            "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
            "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
            "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
            "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
            "contenutiFra": "contenuti del corso", // Course contents in French (optional)
            "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
            "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
            "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
            "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
            "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
            "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
            "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
            "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
            "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
          }
        ], // SyllabusUD
        "CaricoDocenti": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
            "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
            "tipoCreDesEng": "Lezione", // Credit type description in English
            "docenteId": 1, // Key of the teacher giving lessons (required)
            "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
            "docenteNome": "Mario", // Name of the teacher giving lessons
            "docenteCognome": "Rossi", // Surname of the teacher giving lessons
            "ore": 4.0, // Lesson hours
            "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
            "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
            "oreAttSuppDid": 8.0, // Teaching support activity hours
            "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
            "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
            "tipoCoperturaDes": "Ordinario", // Description of the type of teacher coverage
            "CaricoDocentiOpz": ["..."]
          }
        ] // CaricoDocenti
      }
    ] // UdLogConDettagli
  }
]
```

**`422 Unprocessable Entity` - invalid parameters**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /logisticaPerDocente` - Logistics retrieval by teacher.

```java
/**
 * Retrieves logistics by teacher
 *
 * @param docenteId            integer (query, optional)           - id del docente
 * @param docenteMatricola     string (query, optional)            - Teacher matriculation
 * @param docenteCognome       string (query, optional)            - Teacher surname
 * @param userId               string (query, optional)            - Unique ID that allows identifying the user account
 * @param aaOffId              integer (query, optional)           - Offering year ID
 * @param adLogId              integer (query, optional)           - Logistics ID
 * @param dataModTCDoc         string (query, optional)            - Last modification date of the logistics by teacher (retrieves changes to the anagraphics or...
 * @param start                integer (query, optional)           - Used together with `limit` to indicate record pagination
 * @param limit                integer (query, optional)           - Used together with `start` to indicate record pagination, `limit` indicates the number of ...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<LogisticaPerDocente> on success
 */
GET /logisticaPerDocente
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "aaOffId": 1, // Partition offering year
    "cdsId": 1, // Key of the study course providing the educational activity
    "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
    "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
    "cdsDesEng": "Esempio descrizione in inglese", // English description of the study course providing the educational activity
    "aaOrdId": 2016, // Regulation year of the study course providing the educational activity
    "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
    "aaOrdDes": "Esempio di ord", // Regulation description providing the educational activity
    "pdsId": 1, // Key of the study path providing the educational activity
    "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
    "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
    "adId": 1, // Educational activity key
    "adCod": "PDS_AD_1", // Educational activity code
    "adDes": "Esempio di PDS AD", // Educational activity description
    "adDesEng": "Esempio descrizione in inglese", // English description of the educational activity
    "udId": 1, // Key of the teaching unit (module) of the educational activity
    "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
    "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
    "udDesEng": "Esempio descrizione in inglese", // English description of the teaching unit (module) of the educational activity
    "masterFlg": 0, // Flag che indica se la UD corrente � la UD fisica della condivisione
    "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
    "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
    "adLogId": 1, // Unique ID identifying a logistics sharing
    "udLogId": 1, // Unique ID that identifies a logistics sharing at module level
    "fatPartCod": "ALF", // Codice del fattore di partizione
    "fatPartDes": "Alfabetico", // Partition factor description
    "domPartCod": "PARI", // Codice del dominio di partizione
    "domPartDes": "PARI", // Partition domain description
    "partCod": "S1", // Codice della partizione dell'anno accademico
    "partDes": "Primo semestre", // Academic year partition description
    "tipoCreCod": "LEZ", // Codice identificativo del tipo di credito del segmento corrente
    "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
    "docenteId": 1, // Key of the teacher giving lessons
    "docenteCodFis": "LNZFCR67R07A944E", // Teacher fiscal code
    "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
    "docenteNome": "Mario", // Name of the teacher giving lessons
    "docenteCognome": "Rossi", // Surname of the teacher giving lessons
    "docenteEMail": "pr@ateneo.it", // Teacher email
    "settCod": "settCod", // Scientific disciplinary sector code
    "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
    "ore": 4.0, // Lesson hours
    "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
    "oreAttSupDid": 4.0, // OreAttSupDid
    "tipoCoperturaCod": "ORD", // Code of the type of teacher coverage
    "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
    "titolareFlg": 0, // Flag che indica se il docente � titolare per la AD corrente
    "respDidFlg": 0, // Flag che indica se il docente � responsabile didattico per la UD corrente
    "userId": "userId", // User ID
    "dataModDoc": "15/10/2015", // Modification date of teacher information
    "dataMod": "15/10/2015", // Modification date of information related to the distribution of teaching load among teachers
    "fisicaFlg": 0, // Physical AD of sharing flag
    "tipoCorsoCod": "L2", // Typology code of the course providing the educational activity
    "tipoCorsoDes": "Esempio descrizione", // Typology description of the course providing the educational activity
    "tipoCorsoDesEng": "Esempio descrizione in inglese", // English description of the typology of the course providing the educational activity
    "claMCod": "L-8" // Description
  }
]
```

<br>

---

<br>

### `GET /logisticaPerOdFull/streamed/{aaOffId}/{cdsOffId}/` - Logistics retrieval by study course. Version with transfer-encoding.

```java
/**
 * Retrieves the list of LogisticaConDettagli objects with the same offering
 * year and master study course. The response is returned partially using
 * transfer-encoding.
 *
 * @param aaOffId              integer (path, required)            - Offering year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<LogisticaConDettagli> on success
 */
GET /logisticaPerOdFull/streamed/{aaOffId}/{cdsOffId}/
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015", // Last modification date of the entire logistics. Format: DD/MM/YYYY
    "SyllabusAD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveADContestualizzata": {
          "cdsId": 1, // Key of the study course providing the educational activity (required)
          "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
          "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
          "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
          "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
          "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
          "pdsId": 1, // Key of the study path providing the educational activity (required)
          "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
          "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
          "aaOffId": 1, // Offering year providing the educational activity (required)
          "adId": 1, // Educational activity key (required)
          "adCod": "PDS_AD_1", // Educational activity code
          "adDes": "Esempio di PDS AD", // Educational activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveADContestualizzata (required)
        "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili (required)
        "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
        "realFisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing. The flag is taken directly from DB.
        "tipoCatalogoCod": "C", // Code of the study course catalog type (optional)
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "metodiDidattici": "metodi didattici", // Teaching methods
        "metodiDidatticiEng": "metodi didattici", // Teaching methods in English
        "modalitaVerificaApprendimento": "modalita verifica apprendimento", // Assessment methods
        "modalitaVerificaApprendimentoEng": "modalita verifica apprendimento in inglese", // Assessment methods in English
        "altreInfo": "altre informazioni", // Other information
        "altreInfoEng": "altre informazioni", // Other information in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "adLogOpz": [
          {
            "chiaveADContestualizzata": {}
            "chiavePartizione": {}
            "facId": 1, // Unique faculty ID
            "facCod": "FAC_1", // Faculty code
            "facDes": "Esempio di FAC", // Faculty description
            "facDesEng": "Esempio di FAC", // Faculty description (in English)
            "areaDiscCod": "AREA_1", // Disciplinary area code related to the faculty/department
            "areaDiscDes": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department
            "areaDiscDesEng": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department (in English)
            "integratoFlg": 1, // Integrated
            "tipoCorsoCod": "SP4", // Course type code
            "tipoCorsoDes": "Specializzazione 4 anni", // Course description
            "tipoCorsoDesEng": "Residency Program (4 years)" // Course description (in English)
          }
        ], // AdLogOpz (optional)
        "syllabusOpt1": "campo opzionale 1", // Optional field 1 (optional)
        "syllabusOpt1Eng": "optional field 1", // Optional field 1 in English (optional)
        "syllabusOpt2": "campo opzionale 2", // Optional field 2 (optional)
        "syllabusOpt2Eng": "optional field 2", // Optional field 2 in English (optional)
        "syllabusOpt3": "campo opzionale 3", // Optional field 3 (optional)
        "syllabusOpt3Eng": "optional field 3", // Optional field 3 in English (optional)
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "metodiDidatticiSpa": "metodi didattici", // Teaching methods in Spanish (optional)
        "modalitaVerificaApprendimentoSpa": "modalita verifica apprendimento in spagnolo", // Assessment methods in Spanish (optional)
        "altreInfoSpa": "altre informazioni", // Other information in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "syllabusOpt1Spa": "optional field 1", // Optional field 1 in Spanish (optional)
        "syllabusOpt2Spa": "optional field 2", // Optional field 2 in Spanish (optional)
        "syllabusOpt3Spa": "optional field 3", // Optional field 3 in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "metodiDidatticiFra": "metodi didattici", // Teaching methods in French (optional)
        "modalitaVerificaApprendimentoFra": "modalita verifica apprendimento in francese", // Assessment methods in French (optional)
        "altreInfoFra": "altre informazioni", // Other information in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "syllabusOpt1Fra": "optional field 1", // Optional field 1 in French (optional)
        "syllabusOpt2Fra": "optional field 2", // Optional field 2 in French (optional)
        "syllabusOpt3Fra": "optional field 3", // Optional field 3 in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "metodiDidatticiDeu": "metodi didattici", // Teaching methods in German (optional)
        "modalitaVerificaApprendimentoDeu": "modalita verifica apprendimento in tedesco", // Assessment methods in German (optional)
        "altreInfoDeu": "altre informazioni", // Other information in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "syllabusOpt1Deu": "optional field 1", // Optional field 1 in German (optional)
        "syllabusOpt2Deu": "optional field 2", // Optional field 2 in German (optional)
        "syllabusOpt3Deu": "optional field 3", // Optional field 3 in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ], // SyllabusAD
    "UdLogConDettagli": [
      {
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione
        "chiaveUDMaster": {
          "chiaveAdContestualizzata": {
            "cdsId": 1, // Key of the study course providing the educational activity (required)
            "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
            "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
            "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
            "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
            "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
            "pdsId": 1, // Key of the study path providing the educational activity (required)
            "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
            "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
            "aaOffId": 1, // Offering year providing the educational activity (required)
            "adId": 1, // Educational activity key (required)
            "adCod": "PDS_AD_1", // Educational activity code
            "adDes": "Esempio di PDS AD", // Educational activity description
            "afId": 1 // afId from U-Gov Didattica
          }, // ChiaveAdContestualizzata (required)
          "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
          "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
          "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
          "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
        }, // ChiaveUDMaster
        "udLogId": 1, // Non-unique ID that identifies a logistics sharing at module level
        "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
        "SyllabusUD": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "chiaveUDContestualizzata": {}
            "desUdPubblFlg": 0, // Flag che indica se le descrizioni delle unit� didattiche sono pubblicabili (required)
            "masterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing
            "realMasterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing. Flag taken directly from DB.
            "contenuti": "contenuti del corso", // Course contents
            "contenutiEng": "contenuti del corso", // Course contents in English
            "obiettiviFormativi": "obiettivi formativi", // Educational objectives
            "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
            "prerequisiti": "prerequisiti", // Prerequisites
            "prerequisitiEng": "prerequisiti", // Prerequisites in English
            "testiRiferimento": "testi riferimento", // Reference texts
            "testiRiferimentoEng": "testi riferimento", // Reference texts in English
            "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
            "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
            "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
            "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
            "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
            "obiettiviSvilSostenibile": ["..."]
            "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
            "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
            "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
            "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
            "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
            "contenutiFra": "contenuti del corso", // Course contents in French (optional)
            "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
            "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
            "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
            "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
            "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
            "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
            "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
            "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
            "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
          }
        ], // SyllabusUD
        "CaricoDocenti": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
            "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
            "tipoCreDesEng": "Lezione", // Credit type description in English
            "docenteId": 1, // Key of the teacher giving lessons (required)
            "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
            "docenteNome": "Mario", // Name of the teacher giving lessons
            "docenteCognome": "Rossi", // Surname of the teacher giving lessons
            "ore": 4.0, // Lesson hours
            "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
            "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
            "oreAttSuppDid": 8.0, // Teaching support activity hours
            "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
            "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
            "tipoCoperturaDes": "Ordinario", // Description of the type of teacher coverage
            "CaricoDocentiOpz": ["..."]
          }
        ] // CaricoDocenti
      }
    ] // UdLogConDettagli
  }
]
```

<br>

---

<br>

### `GET /logisticaPerOdFull/{aaOffId}/{cdsOffId}/` - Logistics retrieval by study course.

```java
/**
 * Retrieves the list of LogisticaConDettagli objects with the same offering
 * year and master study course.
 *
 * @param aaOffId              integer (path, required)            - Offering year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Description of the study course (if the character * is used, the like operator is applied)
 * @param aaOrdId              integer (query, optional)           - Regulation ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Description of the study path (if the character * is used, the like operator is applied)
 * @param adCod                string (query, optional)            - Educational activity code
 * @param adDes                string (query, optional)            - Description of the educational activity (if the character * is used, the like operator is applied)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<LogisticaConDettagli> on success
 */
GET /logisticaPerOdFull/{aaOffId}/{cdsOffId}/
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "chiavePartizione": {
      "aaOffId": 1, // Partition offering year (required)
      "fatPartCod": "ALF", // Partition factor code (required)
      "fatPartDes": "Alfabetico", // Partition factor description
      "fatPartDesEng": "Alphabetic", // Partition factor description in English
      "domPartCod": "PARI", // Partition domain code (required)
      "domPartDes": "PARI", // Partition domain description
      "domPartDesEng": "ODD", // Partition domain description in English
      "partCod": "S1", // Academic year partition code (required)
      "partDes": "Primo semestre", // Academic year partition description
      "partDesEng": "First Semester", // Academic year partition description in English
      "adLogId": 1 // Logistics grouping ID (required)
    }, // ChiavePartizione
    "chiaveADFisica": {
      "cdsId": 1, // Key of the study course providing the educational activity (required)
      "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
      "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
      "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
      "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
      "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
      "pdsId": 1, // Key of the study path providing the educational activity (required)
      "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
      "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
      "aaOffId": 1, // Offering year providing the educational activity (required)
      "adId": 1, // Educational activity key (required)
      "adCod": "PDS_AD_1", // Educational activity code
      "adDes": "Esempio di PDS AD", // Educational activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveADFisica
    "dataInizio": "15/10/2015", // Start date of the educational period. Format: DD/MM/YYYY
    "dataFine": "15/10/2015", // End date of the educational period. Format: DD/MM/YYYY
    "dataIniValDid": "15/10/2015", // Start date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "dataFinValDid": "15/10/2015", // End date for filling out teaching evaluation questionnaires. Format: DD/MM/YYYY
    "fatPartEffCod": "ALF", // Actual partition factor code
    "fatPartEffDes": "Alfabetico", // Actual partition factor description
    "domPartEffCod": "PARI", // Actual partition domain code
    "domPartEffDes": "PARI", // Actual partition domain description
    "partEffCod": "S1", // Actual academic year partition code
    "partEffDes": "Primo semestre", // Actual academic year partition description
    "partEffDesEng": "First semester", // Actual academic year partition description in English
    "linguaDidId": 4, // Teaching language ID
    "linguaDidCod": "eng", // ISO6392 code of the teaching language
    "linguaDidDes": "Inglese", // Teaching language description
    "tipoDidCod": "C", // Code of the study course teaching type
    "tipoDidDes": "Convenzionale", // Description of the study course teaching type
    "tipoDidDesEng": "Convenzionale", // Description of the study course teaching type in English
    "sedeId": 1, // Key ID of the location
    "sedeDes": "Roma", // Description of the location
    "sedeDesEng": "Rome", // Description of the location in English
    "dataModLog": "15/10/2015", // Last modification date of the entire logistics. Format: DD/MM/YYYY
    "SyllabusAD": [
      {
        "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione (required)
        "chiaveADContestualizzata": {
          "cdsId": 1, // Key of the study course providing the educational activity (required)
          "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
          "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
          "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
          "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
          "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
          "pdsId": 1, // Key of the study path providing the educational activity (required)
          "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
          "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
          "aaOffId": 1, // Offering year providing the educational activity (required)
          "adId": 1, // Educational activity key (required)
          "adCod": "PDS_AD_1", // Educational activity code
          "adDes": "Esempio di PDS AD", // Educational activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveADContestualizzata (required)
        "desAdPubblFlg": 0, // Flag che indica se le descrizioni delle attivit� didattiche sono pubblicabili (required)
        "fisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing
        "realFisicaFlg": 0, // Flag indicating if the current AD is the physical AD of the sharing. The flag is taken directly from DB.
        "tipoCatalogoCod": "C", // Code of the study course catalog type (optional)
        "contenuti": "contenuti del corso", // Course contents
        "contenutiEng": "contenuti del corso", // Course contents in English
        "obiettiviFormativi": "obiettivi formativi", // Educational objectives
        "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
        "prerequisiti": "prerequisiti", // Prerequisites
        "prerequisitiEng": "prerequisiti", // Prerequisites in English
        "metodiDidattici": "metodi didattici", // Teaching methods
        "metodiDidatticiEng": "metodi didattici", // Teaching methods in English
        "modalitaVerificaApprendimento": "modalita verifica apprendimento", // Assessment methods
        "modalitaVerificaApprendimentoEng": "modalita verifica apprendimento in inglese", // Assessment methods in English
        "altreInfo": "altre informazioni", // Other information
        "altreInfoEng": "altre informazioni", // Other information in English
        "testiRiferimento": "testi riferimento", // Reference texts
        "testiRiferimentoEng": "testi riferimento", // Reference texts in English
        "adLogOpz": [
          {
            "chiaveADContestualizzata": {}
            "chiavePartizione": {}
            "facId": 1, // Unique faculty ID
            "facCod": "FAC_1", // Faculty code
            "facDes": "Esempio di FAC", // Faculty description
            "facDesEng": "Esempio di FAC", // Faculty description (in English)
            "areaDiscCod": "AREA_1", // Disciplinary area code related to the faculty/department
            "areaDiscDes": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department
            "areaDiscDesEng": "Esempio di AREA_1", // Disciplinary area description related to the faculty/department (in English)
            "integratoFlg": 1, // Integrated
            "tipoCorsoCod": "SP4", // Course type code
            "tipoCorsoDes": "Specializzazione 4 anni", // Course description
            "tipoCorsoDesEng": "Residency Program (4 years)" // Course description (in English)
          }
        ], // AdLogOpz (optional)
        "syllabusOpt1": "campo opzionale 1", // Optional field 1 (optional)
        "syllabusOpt1Eng": "optional field 1", // Optional field 1 in English (optional)
        "syllabusOpt2": "campo opzionale 2", // Optional field 2 (optional)
        "syllabusOpt2Eng": "optional field 2", // Optional field 2 in English (optional)
        "syllabusOpt3": "campo opzionale 3", // Optional field 3 (optional)
        "syllabusOpt3Eng": "optional field 3", // Optional field 3 in English (optional)
        "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
        "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
        "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
        "obiettiviSvilSostenibile": [
          {
            "obiettiviSvilSosCod": "13", // Sustainable development goal code
            "obiettiviSvilSosDes": "Agire per il clima", // Sustainable development goal description
            "obiettiviSvilSosDesEstesa": "Promuovere azioni, a tutti i livelli, per combattere il cambiamento climatico", // Extended description of the sustainable development goal
            "obiettiviSvilSosDesEng": "Climate action", // Sustainable development goal description in English
            "obiettiviSvilSosDesEngEstesa": "Take urgent action to combat climate change and its impacts" // Extended description of the sustainable development goal in English
          }
        ], // ObiettiviSvilSostenibile
        "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
        "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
        "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
        "metodiDidatticiSpa": "metodi didattici", // Teaching methods in Spanish (optional)
        "modalitaVerificaApprendimentoSpa": "modalita verifica apprendimento in spagnolo", // Assessment methods in Spanish (optional)
        "altreInfoSpa": "altre informazioni", // Other information in Spanish (optional)
        "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
        "syllabusOpt1Spa": "optional field 1", // Optional field 1 in Spanish (optional)
        "syllabusOpt2Spa": "optional field 2", // Optional field 2 in Spanish (optional)
        "syllabusOpt3Spa": "optional field 3", // Optional field 3 in Spanish (optional)
        "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
        "contenutiFra": "contenuti del corso", // Course contents in French (optional)
        "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
        "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
        "metodiDidatticiFra": "metodi didattici", // Teaching methods in French (optional)
        "modalitaVerificaApprendimentoFra": "modalita verifica apprendimento in francese", // Assessment methods in French (optional)
        "altreInfoFra": "altre informazioni", // Other information in French (optional)
        "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
        "syllabusOpt1Fra": "optional field 1", // Optional field 1 in French (optional)
        "syllabusOpt2Fra": "optional field 2", // Optional field 2 in French (optional)
        "syllabusOpt3Fra": "optional field 3", // Optional field 3 in French (optional)
        "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
        "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
        "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
        "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
        "metodiDidatticiDeu": "metodi didattici", // Teaching methods in German (optional)
        "modalitaVerificaApprendimentoDeu": "modalita verifica apprendimento in tedesco", // Assessment methods in German (optional)
        "altreInfoDeu": "altre informazioni", // Other information in German (optional)
        "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
        "syllabusOpt1Deu": "optional field 1", // Optional field 1 in German (optional)
        "syllabusOpt2Deu": "optional field 2", // Optional field 2 in German (optional)
        "syllabusOpt3Deu": "optional field 3", // Optional field 3 in German (optional)
        "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
      }
    ], // SyllabusAD
    "UdLogConDettagli": [
      {
        "chiavePartizione": {
          "aaOffId": 1, // Partition offering year (required)
          "fatPartCod": "ALF", // Partition factor code (required)
          "fatPartDes": "Alfabetico", // Partition factor description
          "fatPartDesEng": "Alphabetic", // Partition factor description in English
          "domPartCod": "PARI", // Partition domain code (required)
          "domPartDes": "PARI", // Partition domain description
          "domPartDesEng": "ODD", // Partition domain description in English
          "partCod": "S1", // Academic year partition code (required)
          "partDes": "Primo semestre", // Academic year partition description
          "partDesEng": "First Semester", // Academic year partition description in English
          "adLogId": 1 // Logistics grouping ID (required)
        }, // ChiavePartizione
        "chiaveUDMaster": {
          "chiaveAdContestualizzata": {
            "cdsId": 1, // Key of the study course providing the educational activity (required)
            "cdsCod": "CDS_AD_1", // Code of the study course providing the educational activity
            "cdsDes": "Esempio di CDS AD", // Description of the study course providing the educational activity
            "aaOrdId": 2016, // Regulation year of the study course providing the educational activity (required)
            "aaOrdCod": "CDS_AD_1", // Regulation code providing the educational activity
            "aaOrdDes": "Esempio di CDS AD", // Regulation description providing the educational activity
            "pdsId": 1, // Key of the study path providing the educational activity (required)
            "pdsCod": "PDS_AD_1", // Code of the study path providing the educational activity
            "pdsDes": "Esempio di PDS AD", // Description of the study path providing the educational activity
            "aaOffId": 1, // Offering year providing the educational activity (required)
            "adId": 1, // Educational activity key (required)
            "adCod": "PDS_AD_1", // Educational activity code
            "adDes": "Esempio di PDS AD", // Educational activity description
            "afId": 1 // afId from U-Gov Didattica
          }, // ChiaveAdContestualizzata (required)
          "udId": 1, // Key of the teaching unit (module) of the educational activity (required)
          "udCod": "CDS_AD_1", // Code of the teaching unit (module) of the educational activity
          "udDes": "Esempio di CDS AD", // Description of the teaching unit (module) of the educational activity
          "udDesEng": "Example of Description" // Description of the teaching unit (module) of the educational activity in English
        }, // ChiaveUDMaster
        "udLogId": 1, // Non-unique ID that identifies a logistics sharing at module level
        "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
        "SyllabusUD": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "chiaveUDContestualizzata": {}
            "desUdPubblFlg": 0, // Flag che indica se le descrizioni delle unit� didattiche sono pubblicabili (required)
            "masterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing
            "realMasterFlg": 0, // Flag indicating if the current UD is the physical UD of the sharing. Flag taken directly from DB.
            "contenuti": "contenuti del corso", // Course contents
            "contenutiEng": "contenuti del corso", // Course contents in English
            "obiettiviFormativi": "obiettivi formativi", // Educational objectives
            "obiettiviFormativiEng": "obiettivi formativi", // Educational objectives in English
            "prerequisiti": "prerequisiti", // Prerequisites
            "prerequisitiEng": "prerequisiti", // Prerequisites in English
            "testiRiferimento": "testi riferimento", // Reference texts
            "testiRiferimentoEng": "testi riferimento", // Reference texts in English
            "uGovAfId": 12345, // Id univoco che consente di individuare una attivit� didattica offerta >- id della afId proveniente da U-Gov Didattica
            "uGovArId": 12345, // Unique ID that identifies a logistics sharing at module level
            "obiettiviSvilSostenibileDes": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals
            "obiettiviSvilSostenibileDesEng": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in English
            "obiettiviSvilSostenibileList": "1-3-13", // List of associated sustainable development goals
            "obiettiviSvilSostenibile": ["..."]
            "contenutiSpa": "contenuti del corso", // Course contents in Spanish (optional)
            "obiettiviFormativiSpa": "obiettivi formativi", // Educational objectives in Spanish (optional)
            "prerequisitiSpa": "prerequisiti", // Prerequisites in Spanish (optional)
            "testiRiferimentoSpa": "testi riferimento", // Reference texts in Spanish (optional)
            "obiettiviSvilSostenibileDesSpa": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in Spanish (optional)
            "contenutiFra": "contenuti del corso", // Course contents in French (optional)
            "obiettiviFormativiFra": "obiettivi formativi", // Educational objectives in French (optional)
            "prerequisitiFra": "prerequisiti", // Prerequisites in French (optional)
            "testiRiferimentoFra": "testi riferimento", // Reference texts in French (optional)
            "obiettiviSvilSostenibileDesFra": "obiettivi per lo sviluppo sostenibile", // Sustainable development goals in French (optional)
            "contenutiDeu": "contenuti del corso", // Course contents in German (optional)
            "obiettiviFormativiDeu": "obiettivi formativi", // Educational objectives in German (optional)
            "prerequisitiDeu": "prerequisiti", // Prerequisites in German (optional)
            "testiRiferimentoDeu": "testi riferimento", // Reference texts in German (optional)
            "obiettiviSvilSostenibileDesDeu": "obiettivi per lo sviluppo sostenibile" // Sustainable development goals in German (optional)
          }
        ], // SyllabusUD
        "CaricoDocenti": [
          {
            "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
            "udLogId": 1, // Unique ID that identifies a logistics sharing at module level (required)
            "chiavePartizione": {}
            "tipoCreCod": "LEZ", // Credit type identifying code for the current segment (required)
            "tipoCreDes": "Lezione", // Credit type identifying description for the current segment
            "tipoCreDesEng": "Lezione", // Credit type description in English
            "docenteId": 1, // Key of the teacher giving lessons (required)
            "docenteMatricola": "mrrs01", // Matriculation of the teacher giving lessons
            "docenteNome": "Mario", // Name of the teacher giving lessons
            "docenteCognome": "Rossi", // Surname of the teacher giving lessons
            "ore": 4.0, // Lesson hours
            "frazioneCarico": 25.0, // Load fraction as a percentage of total hours for the credit type
            "valDidFlg": 0, // Flag che indica se la il docente � valutabile con i questionari
            "oreAttSuppDid": 8.0, // Teaching support activity hours
            "uGovCoperId": 12345, // Unique ID identifying a coverage >- ID of DI_COPER table in U-Gov Didattica
            "tipoCoperturaCod": "ORD", // Code identifying the type of teacher coverage
            "tipoCoperturaDes": "Ordinario", // Description of the type of teacher coverage
            "CaricoDocentiOpz": ["..."]
          }
        ] // CaricoDocenti
      }
    ] // UdLogConDettagli
  }
]
```

<br>

---

<br>

## Endpoints - Struttura

### `GET /logisticheEliminate` - Retrieves deleted logistics.

```java
/**
 * Retrieves LogisticaEliminata objects with their deletion date
 *
 * @param dataModLog           string (query, optional)            - Last modification date of the logistics
 * @param start                integer (query, optional)           - Used together with `limit` to indicate record pagination
 * @param limit                integer (query, optional)           - Used together with `start` to indicate record pagination, `limit` indicates the number of ...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<LogisticaEliminata> on success
 */
Retrieves deleted logistics.
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "adLogId": 1, // Unique ID that identifies a logistics sharing (required)
    "dataModLog": "15/10/2015" // Last modification date of Logistics and details. Format: DD/MM/YYYY
  }
]
```

<br>

---

<br>

## Endpoints - Risfisse

### `GET /risFisse/aule` - List of buildings

```java
/**
 * List of buildings
 *
 * @param aulaCod              string (query, optional)            - Classroom code (if the character * is used, the like operator is applied)
 * @param extAulaCod           string (query, optional)            - Classroom code in the external system (if the character * is used, the like operator is applied)
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param fields               string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<Aula> on success,
 *         DettaglioErrore on failure
 */
GET /risFisse/aule
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "aulaId": 10, // ESSE3 classroom ID, primary key of esse3 together with edificioId
    "edificioId": 100, // ESSE3 building ID, primary key of esse3 together with aulaId
    "aulaCod": "aula_cod", // ESSE3 classroom code, alternative key together with building code (required)
    "edificioCod": "ed_cod", // ESSE3 classroom code, alternative key together with classroom code (required)
    "extAulaCod": "ext_aula_cod", // Classroom code of the external logistics system, unique key in the external system (required)
    "aulaDes": "descrizione aula", // Classroom description
    "capienza": 100, // Classroom capacity
    "abilCbt": "N", // Indicates the classroom type in relation to CBT (Computer Base Test). Values (N => Not enabled, C => enabled only for CBT, T => both) (optional)
    "abilFlg": 1, // Flag indicating whether the classroom is enabled
    "dipartimenti": [
      {
        "facId": 1, // Department ID
        "facCod": "1", // Codice del dipartimento
        "facDes": "INGEGNERIA" // Description del dipartimento
      }
    ] // Dipartimenti (optional)
  }
]
```

**`422 Unprocessable Entity` - invalid parameters**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Description dell'errore
  "errDetails": [
    {
      "errorType": "stackTrace", // Description del tipo di errore aggiuntivo
      "value": "SocketTimeoutException....", // Description dell'errore
      "rawValue": "SocketTimeoutException...." // Description dell'errore
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /risFisse/edifici` - List of buildings

```java
/**
 * List of buildings
 *
 * @param filter               string (query, optional)            - The parameter allows applying filters to the model class using the RS language...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param fields               string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<Edificio> on success
 */
GET /risFisse/edifici
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "edificioId": 100, // ESSE3 building ID, primary key of esse3
    "edificioCod": "ed_cod", // ESSE3 building code, alternative key of esse3
    "extEdificioCod": "ext_aula_cod", // Building code of the external logistics system, unique key in the external system
    "edificioDes": "descrizione aula", // Building description
    "nazioneId": 34, // ID of the country to which the building belongs (optional)
    "nazioneCod": "ita", // Country code of the building
    "nazioneDes": "Italia", // Country description of the building (optional)
    "comuneId": 34, // For default country resources, represents the comuneId of the building (optional)
    "comuneCod": "234T", // For default country resources, represents the municipality code of the building
    "comuneDes": "Bologna", // For default country resources, represents the municipality description of the building (optional)
    "via": "via dante alighieri 10", // Classroom address (optional)
    "citstra": "Rieka", // Foreign city (optional)
    "nota": "nota libera", // Free note
    "urlWeb": "http://www.google.com" // Web page URL
  }
]
```

<br>

---

<br>

### `GET /risFisse/edifici/{edificioId}` - Detail of the single building

```java
/**
 * Detail of the single building
 *
 * @param edificioId           integer (path, required)            - Building ID
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param fields               string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return Edificio on success
 */
GET /risFisse/edifici/{edificioId}
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
{
  "edificioId": 100, // ESSE3 building ID, primary key of esse3
  "edificioCod": "ed_cod", // ESSE3 building code, alternative key of esse3
  "extEdificioCod": "ext_aula_cod", // Building code of the external logistics system, unique key in the external system
  "edificioDes": "descrizione aula", // Building description
  "nazioneId": 34, // ID of the country to which the building belongs (optional)
  "nazioneCod": "ita", // Country code of the building
  "nazioneDes": "Italia", // Country description of the building (optional)
  "comuneId": 34, // For default country resources, represents the comuneId of the building (optional)
  "comuneCod": "234T", // For default country resources, represents the municipality code of the building
  "comuneDes": "Bologna", // For default country resources, represents the municipality description of the building (optional)
  "via": "via dante alighieri 10", // Classroom address (optional)
  "citstra": "Rieka", // Foreign city (optional)
  "nota": "nota libera", // Free note
  "urlWeb": "http://www.google.com" // Web page URL
}
```

<br>

---

<br>

### `GET /risFisse/edifici/{edificioId}/aule` - List of classrooms of a building

```java
/**
 * List of classrooms of a building
 *
 * @param edificioId           integer (path, required)            - Building ID
 * @param filter               string (query, optional)            - The parameter allows applying filters to the model class using the RS language...
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param fields               string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<Aula> on success
 */
GET /risFisse/edifici/{edificioId}/aule
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "aulaId": 10, // ESSE3 classroom ID, primary key of esse3 together with edificioId
    "edificioId": 100, // ESSE3 building ID, primary key of esse3 together with aulaId
    "aulaCod": "aula_cod", // ESSE3 classroom code, alternative key together with building code (required)
    "edificioCod": "ed_cod", // ESSE3 classroom code, alternative key together with classroom code (required)
    "extAulaCod": "ext_aula_cod", // Classroom code of the external logistics system, unique key in the external system (required)
    "aulaDes": "descrizione aula", // Classroom description
    "capienza": 100, // Classroom capacity
    "abilCbt": "N", // Indicates the classroom type in relation to CBT (Computer Base Test). Values (N => Not enabled, C => enabled only for CBT, T => both) (optional)
    "abilFlg": 1, // Flag indicating whether the classroom is enabled
    "dipartimenti": [
      {
        "facId": 1, // Department ID
        "facCod": "1", // Codice del dipartimento
        "facDes": "INGEGNERIA" // Description del dipartimento
      }
    ] // Dipartimenti (optional)
  }
]
```

<br>

---

<br>

### `GET /risFisse/edifici/{edificioId}/aule/{aulaId}` - Detail of the single classroom

```java
/**
 * Detail of the single classroom
 *
 * @param aulaId               integer (path, required)            - Classroom ID
 * @param edificioId           integer (path, required)            - Building ID
 * @param optionalFields       string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @param fields               string (query, optional)            - Specifies the list of optional fields (which are not retrieved by default)
 * @return List<Aula> on success
 */
GET /risFisse/edifici/{edificioId}/aule/{aulaId}
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "aulaId": 10, // ESSE3 classroom ID, primary key of esse3 together with edificioId
    "edificioId": 100, // ESSE3 building ID, primary key of esse3 together with aulaId
    "aulaCod": "aula_cod", // ESSE3 classroom code, alternative key together with building code (required)
    "edificioCod": "ed_cod", // ESSE3 classroom code, alternative key together with classroom code (required)
    "extAulaCod": "ext_aula_cod", // Classroom code of the external logistics system, unique key in the external system (required)
    "aulaDes": "descrizione aula", // Classroom description
    "capienza": 100, // Classroom capacity
    "abilCbt": "N", // Indicates the classroom type in relation to CBT (Computer Base Test). Values (N => Not enabled, C => enabled only for CBT, T => both) (optional)
    "abilFlg": 1, // Flag indicating whether the classroom is enabled
    "dipartimenti": [
      {
        "facId": 1, // Department ID
        "facCod": "1", // Codice del dipartimento
        "facDes": "INGEGNERIA" // Description del dipartimento
      }
    ] // Dipartimenti (optional)
  }
]
```

<br>

---

<br>

### `PUT /risFisse/syncConSistLog` - Synchronization with the external logistics system

```java
/**
 * Performs synchronization with the external logistics system
 *
 * @return ImportSistLogResult on success
 */
PUT /risFisse/syncConSistLog
```

**Auth:** `UTENTE_TECNICO` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
{
  "sistLog": "UP_1", // External logistics system
  "ret": 1, // Codice di ritorno
  "errMsg": "Esecuzione corretta", // Error message
  "logs": [
    {
      "level": "INFO", // Message level
      "msg": "messaggio di log" // Log message
    }
  ] // Logs
}
```

<br>

---

<br>

---

## References

- **Swagger UI:** [Logistica Api V1 - ESSE3 REST Docs](<https://unimol.esse3.cineca.it/e3rest/docs/?urls.primaryName=Logistica%20Api%20V1%20(https%3A%2F%2Funimol.esse3.cineca.it%2Fe3rest%2Fapi%2Flogistica-service-v1)>)
- **Spec YAML:** [logisticaApiV1.yaml](https://unimol.esse3.cineca.it/e3rest/api/swagger-service-v1/swagger/specs/p09-logisticaApiV1.yaml)
- **ESSE3 REST API General Documentation:** [wiki.u-gov.it](https://wiki.u-gov.it/confluence/display/ESSE3/Servizi+REST+su+ESSE3)
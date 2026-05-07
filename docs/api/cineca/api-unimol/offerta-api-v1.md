---
title: Offerta API V1 | OhMyUniversity!
description: REST API documentation for the Offerta service (offerta-service-v1) - CINECA ESSE3.
head:
  - - meta
    - property: og:title
      content: Offerta API V1 | OhMyUniversity!
  - - meta
    - property: og:description
      content: REST API documentation for the Offerta service (offerta-service-v1) - CINECA ESSE3.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/api/cineca/api-unimol/offerta-api-v1
  - - meta
    - name: keywords
      content: offerta v1 api, esse3 rest api, cineca api, ohmyuniversity api, offerta-service-v1
  - - meta
    - name: twitter:title
      content: Offerta API V1 | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: REST API documentation for the Offerta service (offerta-service-v1) - CINECA ESSE3.
---

# OhMyUniversity! - Unimol: Offerta API V1

**ENG:** `Offer`

**Version:** `1.2.0` · **Base URL:** `/offerta-service-v1`

ESSE3 REST API for accessing the academic offer
## ChangeLog
Version        | ESSE3 Release Version         | Interventions 
 -             | -                             | -
    1.1.0      | 19.03.03.00                   | added method /offerte/{aaOffId}/{cdsOffId}/attivita/cancellabile 
    1.1.0      | 19.03.03.00                   | added method /offerte/{aaOffId}/{cdsOffId}/moduli/cancellabili 
    1.2.0      | 19.04.01.00                   | deprecated methods /docenti and /docentiFull use the analogs on anagrafica-service-v2

---

## Endpoints - Groupings (Raggruppamenti)

### `GET /ad-raggruppate` - Retrieve AD groupings

```java
/**
 * Retrieves the AD groupings with the details of the 'parent' activity and
 * its 'child' activities. The annoCoorte parameter is mandatory, while
 * cdsCod and adCod are optional, refer to the fields of the 'parent' activity
 * and a LIKE filter can be applied.
 *
 * @param annoCoorte           integer (query, required)           - Cohort year
 * @param cdsCod               string (query, optional)            - Study course code
 * @param adCod                string (query, optional)            - Academic activity code
 * @return List<ADGruppoPadre> on success,
 *         DettaglioErrore on failure
 */
GET /ad-raggruppate
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "adragoffId": 1, // Grouped academic activity key
    "cdsId": 1, // Academic activity delivery study course key
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
    "adId": 1, // Academic activity key
    "adCod": "AD_1", // Academic activity code
    "adDes": "Esempio di AD", // Academic activity description
    "adDesEng": "Esempio di AD", // Academic activity description in English
    "tipoRagCod": "TIPO_RAG_1", // Grouping type code
    "tipoRagDes": "Esempio di tipo di raggruppamento", // Grouping type description
    "tipoRagDesEng": "Esempio di tipo di raggruppamento in inglese", // Grouping type description in English
    "annoCoorte": 2020, // Cohort year
    "adFiglie": [
      {
        "adragoffId": 1, // Grouped academic activity key
        "adfiglioProgId": 1, // Child grouped academic activity key
        "cdsFiglioId": 1, // Child academic activity study course key
        "cdsFiglioCod": "CDS_AD_1", // Child academic activity study course code
        "cdsFiglioDes": "Esempio di CDS AD", // Child academic activity course description
        "cdsFiglioDesEng": "Esempio di CDS AD", // Child academic activity course description in English
        "adFiglioId": 1, // Child academic activity key
        "adFiglioCod": "AD_1", // Child academic activity code
        "adFiglioDes": "Esempio di AD", // Child academic activity description
        "adFiglioDesEng": "Esempio di AD" // Child academic activity description in English
      }
    ] // AdFiglie
  }
]
```

**`422 Unprocessable Entity` - Error**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Error description
  "errDetails": [
    {
      "errorType": "stackTrace", // Additional error type description
      "value": "SocketTimeoutException....", // Error description
      "rawValue": "SocketTimeoutException...." // Error description
    }
  ] // ErrDetails
}
```

<br>

---

<br>

## Endpoints - Offer (Offerta)

### `GET /attivitaGeneriche` - Retrieve information about generic academic activities

```java
/**
 * Retrieves the list of 'ADGenerica' objects. The optional start and
 * limit parameters allow paginating the results.
 *
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<ADGenerica> on success
 */
GET /attivitaGeneriche
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "adId": 1, // Generic academic activity key (required)
    "adCod": "AD_1", // Generic academic activity code
    "adDes": "Esempio di AD", // Generic academic activity description
    "offertaExistsFlg": 1 // Flag indicating if the offer exists for the generic academic activity
  }
]
```

<br>

---

<br>

### `GET /dominiPartizione` - Retrieve information about partition domains

```java
/**
 * Retrieves the list of partition domains. The optional start and
 * limit parameters allow paginating the results.
 *
 * @param fatPartCod           string (query, optional)            - Partition factor code
 * @param fatPartDes           string (query, optional)            - Partition factor description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<PartizioniFull> on success
 */
GET /dominiPartizione
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "fatPartCod": "ALF", // Partition factor code
    "fatPartDes": "Alfabetico", // Partition factor description
    "fatPartDesEng": "Alphabetic", // Partition factor description in English
    "tipoFatt": "ALF", // Partition factor type
    "DominioDiPartizione": [
      {
        "fatPartCod": "ALF", // Partition factor code (required)
        "domPartCod": "PARI", // Partition domain code (required)
        "domPartDes": "PARI", // Partition domain description
        "domPartDesEng": "PAIR" // Partition domain description in English
      }
    ] // DominioDiPartizione
  }
]
```

<br>

---

<br>

### `GET /fattoriPartizione` - Retrieve information about partition factors

```java
/**
 * Retrieves the list of partition factors. The optional start and
 * limit parameters allow paginating the results.
 *
 * @param fatPartCod           string (query, optional)            - Partition factor code
 * @param fatPartDes           string (query, optional)            - Partition factor description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<FattoreDiPartizione> on success
 */
GET /fattoriPartizione
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "fatPartCod": "ALF", // Partition factor code (required)
    "fatPartDes": "Alfabetico", // Partition factor description
    "fatPartDesEng": "Alphabetic", // Partition factor description in English
    "tipoFatt": "ALF" // Partition factor type
  }
]
```

<br>

---

<br>

### `GET /offerte` - Retrieve information about academic offer headers

```java
/**
 * Retrieves the list of 'Offerta' objects with the offer header information.
 * The optional start and limit parameters allow paginating the results.
 *
 * @param aaOffId              integer (query, optional)           - Offer year ID
 * @param cdsCod               string (query, optional)            - Study course code
 * @param cdsDes               string (query, optional)            - Study course description (using * applies a LIKE filter)
 * @param dipCod               string (query, optional)            - Code of the administrative department of the study course
 * @param dipDes               string (query, optional)            - Description of the administrative department of the study course (using * applies a LIKE filter)
 * @param statoAttCod          string (query, optional)            - Offer status, if not set all offers are retrieved
 * @param tipiCorsoCod         string (query, optional)            - Study course type code
 * @param dataModOd            string (query, optional)            - Last modification date of the academic offer
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<Offerta> on success
 */
GET /offerte
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
[
  {
    "aaOffId": 1, // Academic activity delivery offer year (required)
    "cdsOffId": 1, // Academic activity delivery study course key (required)
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "statoAttCod": "A", // Academic offer status
    "tipoCorsoCod": "L2", // Academic activity delivery study course type code
    "tipiCorsoCod": "L2", // Academic activity delivery study course type code
    "tipoCorsoDes": "Scuola di specializzazione", // Study course type description
    "tipoCorsoDesEng": "School of Specialization", // Study course type description in English
    "dataModOd": "15/10/2015", // Last modification date of the entire academic offer. Format: DD/MM/YYYY
    "dipCod": "DIP_COD", // Code of the administrative department of the study course
    "dipDes": "DIP_COD", // Description of the administrative department of the study course
    "offertaExistsFlg": 1, // Flag indicating if the offer exists
    "logisticaExistsFlg": 0 // Flag indicating if logistics exists
  }
]
```

<br>

---

<br>

### `GET /offerte/attivita/cancellabile` - Indicates if an AD is cancellable

```java
/**
 * Indicates if an AD is cancellable by retrieving an object of type
 * 'ADCancellabile'. It is necessary to specify at least one parameter.
 * Return Codes | Description:
 * 0 | AD not present, therefore cancellable
 * 1 | AD cancellable
 * 2 | AD present in booklets (P11_AD_SCE)
 * 3 | AD/UD present in exam definition (P10_APP)
 * 4 | AD present in professor authorization for exams (P10_ABIL_DOC)
 * 5 | AD present in professor register (P09_REG_DOC)
 * 6 | AD present in AD groupings used to plan exam calendar (P10_INSIEME_AD)
 * 7 | AD present in plans A,P,V and the ad is delivered (P11_AD_PIANI)
 * 8 | AD present in the registry of theses and term papers presented by students (P12_TESI)
 * 9 | Parent AD of a common exam (P09_AD_ESACOM)
 *
 * @param afId                 integer (query, optional)           - af_id from U-Gov
 * @param aaOffId              integer (query, optional)           - Offer year identifier
 * @param cdsCod               string (query, optional)            - Study course code
 * @param aaOrdId              integer (query, optional)           - Sort year ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param adCod                string (query, optional)            - Academic activity code
 * @param udCod                string (query, optional)            - Didactic unit code
 * @return ADCancellabile on success,
 *         DettaglioErrore on failure
 */
GET /offerte/attivita/cancellabile
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
{
  "retCode": 1, // Code indicating if the academic activity is cancellable
  "msg": "Non è possibile cancellare la AD in quanto è presente nel registro docenti" // Possible message with additional information
}
```

**`422 Unprocessable Entity` - Error**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Error description
  "errDetails": [
    {
      "errorType": "stackTrace", // Additional error type description
      "value": "SocketTimeoutException....", // Error description
      "rawValue": "SocketTimeoutException...." // Error description
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `PUT /offerte/attivita/conteggio-piani` - Information on plan counts linked to a generic AD

```java
/**
 * Information on plan counts linked to a generic AD
 *
 * @param body                 AttivitaConteggioPianiFiltri (body, required) - Object with data for managing AD filters
 * @return AttivitaConteggioPiani on success,
 *         DettaglioErrore on failure
 */
PUT /offerte/attivita/conteggio-piani
```

**Auth:** `UTENTE_TECNICO` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Request body

```json
{
  "cdsCod": "CDS_1", // Code of the academic activity course
  "aaOrdId": 2025, // Sorting year of the academic activity delivery study course
  "pdsCod": "PDS_1", // Academic activity delivery path code
  "aaOffId": 2025, // Offer year of the activity
  "adCod": "AD_1" // Codice dell'attività didattica
}
```

#### Response

**`200 OK`**

```json
{
  "cdsCod": "CDS_1", // Code of the academic activity course
  "aaOrdId": 2025, // Sorting year of the academic activity delivery study course
  "pdsCod": "PDS_1", // Academic activity delivery path code
  "aaOffId": 2025, // Offer year of the activity
  "adCod": "AD_1", // Codice dell'attività didattica
  "conteggioPiani": 3 // Count of plans linked to the activity
}
```

**`404 Not Found` - no data present**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Error description
  "errDetails": [
    {
      "errorType": "stackTrace", // Additional error type description
      "value": "SocketTimeoutException....", // Error description
      "rawValue": "SocketTimeoutException...." // Error description
    }
  ] // ErrDetails
}
```

**`422 Unprocessable Entity` - invalid input data**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Error description
  "errDetails": [
    {
      "errorType": "stackTrace", // Additional error type description
      "value": "SocketTimeoutException....", // Error description
      "rawValue": "SocketTimeoutException...." // Error description
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/attivita` - Retrieve information about activities

```java
/**
 * Retrieves the list of 'ADContestualizzata' objects. The optional parameters
 * start and limit allow paginating the results.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Study path description (using * applies a LIKE filter)
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<ADContestualizzata> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/attivita
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiaveAdContestualizzata": {
      "cdsId": 1, // Academic activity delivery study course key (required)
      "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
      "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
      "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
      "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
      "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
      "pdsId": 1, // Academic activity delivery study path key (required)
      "pdsCod": "PDS_AD_1", // Academic activity delivery path code
      "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
      "aaOffId": 1, // Academic activity delivery offer year (required)
      "adId": 1, // Academic activity key (required)
      "adCod": "PDS_AD_1", // Academic activity code
      "adDes": "Esempio di PDS AD", // Academic activity description
      "afId": 1 // afId from U-Gov Didattica
    }, // ChiaveAdContestualizzata (required)
    "adDesEng": "Esempio di PDS AD", // Academic activity description in English
    "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
    "aaOrdDesEng": "Esempio di CDS AD", // Academic activity delivery sorting description in English
    "pdsDesEng": "Esempio di PDS AD", // Academic activity delivery path description
    "linguaInsDes": "inglese", // Teaching language of the academic activity used for ECTS
    "linguaInsDesEng": "inglese", // Teaching language of the academic activity in English
    "nonErogabileOdFlg": 0, // Non-deliverable activity. If = 1 indicates that for this activity the partitions (classes) of p09_ad_log should NOT be considered in career processes, i.e., in the student booklet the partition (class) is never assigned for this activity
    "tipoEsaCod": "S", // Exam type code
    "tipoEsaDes": "scritto", // Exam type description
    "tipoEsaDesEng": "wrote", // Exam type description in English
    "tipoValCod": "V", // Evaluation type code
    "tipoValDes": "voto", // Evaluation type description
    "tipoValDesEng": "grade", // Evaluation type description in English
    "tipoInsCod": "FON, CAR", // Teaching type code. Valid only for pre-reform CDS
    "tipoInsDes": "fondamentale", // Teaching type description. Valid only for pre-reform CDS
    "gruppoGiudCod": "1", // Where the evaluation type is judgment, i.e., TIPO_VAL_COD = G, indicates the judgment group used
    "gruppoGiudDes": "Esempio di descrizione del gruppo di giudizio", // Judgment group description
    "reiterabile": 1, // Indicates if an activity can be repeated more than once within the student's career (e.g., literature courses). Contains the maximum number of possible repetitions.
    "urlSitoWeb": "Esempio di url", // URL of the structure's website
    "urlCorsoMoodle": "www.moodle.com", // URL of the MOODLE course linked to the academic activity
    "adCapogruppo": {
      "cdsId": 1, // Academic activity delivery study course key
      "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
      "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
      "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
      "adId": 1, // Academic activity key
      "adCod": "AD_1", // Academic activity code
      "adDes": "Esempio di AD", // Academic activity description
      "adDesEng": "Esempio di AD", // Academic activity description in English
      "tipoRagCod": "TIPO_RAG_1", // Grouping type code
      "tipoRagDes": "Esempio di tipo di raggruppamento", // Grouping type description
      "tipoRagDesEng": "Esempio di tipo di raggruppamento in inglese", // Grouping type description in English
      "annoCoorte": 2020 // Cohort year
    }, // AdCapogruppo
    "capoGruppoFlg": 0, // Indicates if it is a head AD.
    "adWebViewFlg": 0 // Indicates if the current AD is visible on the web
  }
]
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/attivita/cancellabile` - Retrieve an integer indicating if the AD is cancellable

```java
/**
 * Retrieves an 'ADOffertaCancellabile' object. Return Codes | Description:
 * 1 | AD cancellable
 * 2 | AD present in booklets (P11_AD_SCE)
 * 3 | AD/UD present in exam definition (P10_APP)
 * 4 | AD present in professor authorization for exams (P10_ABIL_DOC)
 * 5 | AD present in professor register (P09_REG_DOC)
 * 6 | AD present in AD groupings used to plan exam calendar (P10_INSIEME_AD)
 * 7 | AD present in plans A,P,V and the ad is delivered (P11_AD_PIANI)
 * 8 | AD present in the registry of theses and term papers presented by students (P12_TESI)
 * 9 | Parent AD of a common exam (P09_AD_ESACOM)
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<ADOffertaCancellabile> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/attivita/cancellabile
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "aaOffId": 1, // Academic Offer Year ID (required)
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code (required)
    "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
    "pdsCod": "PDS_AD_1", // Academic activity delivery path code (required)
    "adCod": "AD_1", // Generic academic activity code (required)
    "cancellabile": 1 // Integer indicating the offered academic activity is cancellable
  }
]
```

<br>

---

<br>

### `PATCH /offerte/{aaOffId}/{cdsOffId}/attivita/{aaOrdOffId}/{pdsOffId}/{adOffId}` - Modify a contextualized academic activity

```java
/**
 * Allows modifying a contextualized academic activity
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdOffId           integer (path, required)            - Sorting ID of the study course
 * @param pdsOffId             integer (path, required)            - Study path ID
 * @param adOffId              integer (path, required)            - Academic activity ID
 * @param body                 AggiornamentoADContestualizzata (body, required) - Object containing the parameters for modifying the contextualized academic activity
 * @return ADContestualizzata on success,
 *         DettaglioErrore on failure
 */
PATCH /offerte/{aaOffId}/{cdsOffId}/attivita/{aaOrdOffId}/{pdsOffId}/{adOffId}
```

**Auth:** `UTENTE_TECNICO` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Request body

```json
{
  "urlCorsoMoodle": "www.moodle.com" // URL of the MOODLE course linked to the academic activity
}
```

#### Response

**`200 OK`**

```json
{
  "chiaveAdContestualizzata": {
    "cdsId": 1, // Academic activity delivery study course key (required)
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
    "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
    "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
    "pdsId": 1, // Academic activity delivery study path key (required)
    "pdsCod": "PDS_AD_1", // Academic activity delivery path code
    "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
    "aaOffId": 1, // Academic activity delivery offer year (required)
    "adId": 1, // Academic activity key (required)
    "adCod": "PDS_AD_1", // Academic activity code
    "adDes": "Esempio di PDS AD", // Academic activity description
    "afId": 1 // afId from U-Gov Didattica
  }, // ChiaveAdContestualizzata (required)
  "adDesEng": "Esempio di PDS AD", // Academic activity description in English
  "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
  "aaOrdDesEng": "Esempio di CDS AD", // Academic activity delivery sorting description in English
  "pdsDesEng": "Esempio di PDS AD", // Academic activity delivery path description
  "linguaInsDes": "inglese", // Teaching language of the academic activity used for ECTS
  "linguaInsDesEng": "inglese", // Teaching language of the academic activity in English
  "nonErogabileOdFlg": 0, // Non-deliverable activity. If = 1 indicates that for this activity the partitions (classes) of p09_ad_log should NOT be considered in career processes, i.e., in the student booklet the partition (class) is never assigned for this activity
  "tipoEsaCod": "S", // Exam type code
  "tipoEsaDes": "scritto", // Exam type description
  "tipoEsaDesEng": "wrote", // Exam type description in English
  "tipoValCod": "V", // Evaluation type code
  "tipoValDes": "voto", // Evaluation type description
  "tipoValDesEng": "grade", // Evaluation type description in English
  "tipoInsCod": "FON, CAR", // Teaching type code. Valid only for pre-reform CDS
  "tipoInsDes": "fondamentale", // Teaching type description. Valid only for pre-reform CDS
  "gruppoGiudCod": "1", // Where the evaluation type is judgment, i.e., TIPO_VAL_COD = G, indicates the judgment group used
  "gruppoGiudDes": "Esempio di descrizione del gruppo di giudizio", // Judgment group description
  "reiterabile": 1, // Indicates if an activity can be repeated more than once within the student's career (e.g., literature courses). Contains the maximum number of possible repetitions.
  "urlSitoWeb": "Esempio di url", // URL of the structure's website
  "urlCorsoMoodle": "www.moodle.com", // URL of the MOODLE course linked to the academic activity
  "adCapogruppo": {
    "cdsId": 1, // Academic activity delivery study course key
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
    "adId": 1, // Academic activity key
    "adCod": "AD_1", // Academic activity code
    "adDes": "Esempio di AD", // Academic activity description
    "adDesEng": "Esempio di AD", // Academic activity description in English
    "tipoRagCod": "TIPO_RAG_1", // Grouping type code
    "tipoRagDes": "Esempio di tipo di raggruppamento", // Grouping type description
    "tipoRagDesEng": "Esempio di tipo di raggruppamento in inglese", // Grouping type description in English
    "annoCoorte": 2020 // Cohort year
  }, // AdCapogruppo
  "capoGruppoFlg": 0, // Indicates if it is a head AD.
  "adWebViewFlg": 0 // Indicates if the current AD is visible on the web
}
```

**`422 Unprocessable Entity` - Operation not allowed**

```json
{
  "statusCode": 200, // Http Status Code
  "retCode": -1, // Error code
  "retErrMsg": "Parametri non corretti", // Error description
  "errDetails": [
    {
      "errorType": "stackTrace", // Additional error type description
      "value": "SocketTimeoutException....", // Error description
      "rawValue": "SocketTimeoutException...." // Error description
    }
  ] // ErrDetails
}
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/docentiPerUD` - Retrieve information about professors linked to the UD

```java
/**
 * Retrieves the list of DocentiPerUD objects.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Study path description (using * applies a LIKE filter)
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param udCod                string (query, optional)            - Didactic unit code
 * @param udDes                string (query, optional)            - Didactic unit description (using * applies a LIKE filter)
 * @param docenteMatricola     string (query, optional)            - Professor registration number
 * @param docenteCognome       string (query, optional)            - Professor surname (using * applies a LIKE filter)
 * @param docenteNome          string (query, optional)            - Professor name (using * applies a LIKE filter)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<DocentiPerUD> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/docentiPerUD
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiaveUdContestualizzata": {
      "chiaveAdContestualizzata": {
        "cdsId": 1, // Academic activity delivery study course key (required)
        "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
        "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
        "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
        "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
        "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
        "pdsId": 1, // Academic activity delivery study path key (required)
        "pdsCod": "PDS_AD_1", // Academic activity delivery path code
        "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
        "aaOffId": 1, // Academic activity delivery offer year (required)
        "adId": 1, // Academic activity key (required)
        "adCod": "PDS_AD_1", // Academic activity code
        "adDes": "Esempio di PDS AD", // Academic activity description
        "afId": 1 // afId from U-Gov Didattica
      }, // ChiaveAdContestualizzata (required)
      "udId": 1, // Unit (module) key of the academic activity (required)
      "udCod": "CDS_AD_1", // Unit (module) code of the academic activity
      "udDes": "Esempio di CDS AD", // Unit (module) description of the academic activity
      "udDesEng": "Example of Description" // Unit (module) description of the academic activity in English
    }, // ChiaveUdContestualizzata (required)
    "tipoCoperturaCod": "ORD", // Professor coverage type code
    "tipoCoperturaDes": "ORDINARIO", // Professor coverage type description
    "noTraspFlg": 0, // Flag indicating if the professor should not be reported for SUA
    "fatPartCod": "ALF", // Partition factor code
    "fatPartDes": "Alfabetico", // Partition factor description
    "domPartCod": "PARI", // Partition domain code
    "domPartDes": "PARI", // Partition domain description
    "docenteId": 1, // Key of the professor teaching (required)
    "docenteMatricola": "mrrs01", // Registration number of the professor teaching
    "docenteNome": "Mario", // Name of the professor teaching
    "docenteCognome": "Rossi", // Surname of the professor teaching
    "lezioneFlg": 0, // Flag indicating if the professor teaches for the current UD
    "titolareFlg": 0, // Flag che indica se il docente � titolare per la AD corrente
    "respDidFlg": 0 // Flag che indica se il docente � responsabile didattico per la UD corrente
  }
]
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/moduli` - Retrieve information about modules

```java
/**
 * Retrieves the list of 'UDContestualizzata' objects. The optional parameters
 * start and limit allow paginating the results.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Study path description (using * applies a LIKE filter)
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<UDContestualizzata> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/moduli
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiaveUdContestualizzata": {
      "chiaveAdContestualizzata": {
        "cdsId": 1, // Academic activity delivery study course key (required)
        "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
        "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
        "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
        "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
        "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
        "pdsId": 1, // Academic activity delivery study path key (required)
        "pdsCod": "PDS_AD_1", // Academic activity delivery path code
        "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
        "aaOffId": 1, // Academic activity delivery offer year (required)
        "adId": 1, // Academic activity key (required)
        "adCod": "PDS_AD_1", // Academic activity code
        "adDes": "Esempio di PDS AD", // Academic activity description
        "afId": 1 // afId from U-Gov Didattica
      }, // ChiaveAdContestualizzata (required)
      "udId": 1, // Unit (module) key of the academic activity (required)
      "udCod": "CDS_AD_1", // Unit (module) code of the academic activity
      "udDes": "Esempio di CDS AD", // Unit (module) description of the academic activity
      "udDesEng": "Example of Description" // Unit (module) description of the academic activity in English
    }, // ChiaveUdContestualizzata (required)
    "udDesEng": "Esempio di CDS UD", // Unit (module) description of the academic activity in English
    "tipoUdCod": "M" // Didactic unit type, e.g., module, course, seminar. Mandatory if, in the choice rules, the AD requires the selection of modules of a particular type
  }
]
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/moduli/cancellabili` - Retrieve an integer indicating if the module is cancellable

```java
/**
 * Retrieves a 'UDOffertaCancellabile' object. The list of return codes
 * is visible on the endpoint
 * /offerte/{aaOffId}/{cdsOffId}/attivita/cancellabile
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param udId                 integer (query, optional)           - Didactic unit ID
 * @param udCod                string (query, optional)            - Didactic unit code
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<UDOffertaCancellabile> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/moduli/cancellabili
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "aaOffId": 1, // Academic Offer Year ID (required)
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code (required)
    "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
    "pdsCod": "PDS_AD_1", // Academic activity delivery path code (required)
    "adCod": "AD_1", // Generic academic activity code (required)
    "udCod": "UD_1", // Offered didactic unit code
    "cancellabile": 1 // Integer indicating the offered academic activity is cancellable
  }
]
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/segmenti` - Retrieve information about segments

```java
/**
 * Retrieves the list of SEGContestualizzato objects. The optional parameters
 * start and limit allow paginating the results.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Study path description (using * applies a LIKE filter)
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param udId                 integer (query, optional)           - Didactic unit ID
 * @param udCod                string (query, optional)            - Didactic unit code
 * @param udDes                string (query, optional)            - Didactic unit description (using * applies a LIKE filter)
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<SEGContestualizzato> on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/segmenti
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "chiaveSegContestualizzato": {
      "chiaveUdContestualizzata": {
        "chiaveAdContestualizzata": {
          "cdsId": 1, // Academic activity delivery study course key (required)
          "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
          "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
          "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
          "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
          "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
          "pdsId": 1, // Academic activity delivery study path key (required)
          "pdsCod": "PDS_AD_1", // Academic activity delivery path code
          "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
          "aaOffId": 1, // Academic activity delivery offer year (required)
          "adId": 1, // Academic activity key (required)
          "adCod": "PDS_AD_1", // Academic activity code
          "adDes": "Esempio di PDS AD", // Academic activity description
          "afId": 1 // afId from U-Gov Didattica
        }, // ChiaveAdContestualizzata (required)
        "udId": 1, // Unit (module) key of the academic activity (required)
        "udCod": "CDS_AD_1", // Unit (module) code of the academic activity
        "udDes": "Esempio di CDS AD", // Unit (module) description of the academic activity
        "udDesEng": "Example of Description" // Unit (module) description of the academic activity in English
      }, // ChiaveUdContestualizzata (required)
      "segId": 1 // Academic activity segment key (required)
    }, // ChiaveSegContestualizzato (required)
    "settCod": "E12X", // Identification code of the reference scientific disciplinary sector
    "settDes": "MICROBIOLOGIA GENERALE", // Description of the reference scientific disciplinary sector
    "discCod": "MAT01", // Identification code of the discipline (within the scientific disciplinary sector) of the current segment
    "tipoCreCod": "LEZ", // Identification code of the credit type of the current segment
    "tipoCreDes": "Lezione", // Identification description of the credit type of the current segment
    "tipoCreDesEng": "Lezione", // Identification description of the credit type in English
    "durUniVal": 30.0, // Hours of frontal activity of the segment
    "durStuInd": 10.0, // Hours of individual study of the segment.
    "nota": "bla bla", // Free notes.
    "tipoAfCod": "A", // Educational Activity Type
    "tipoAfDes": "Base", // Educational Activity Type description
    "tipoAfDesEng": "Basic compulsory subjects", // Educational Activity Type description in English
    "freqObbligFlg": 0, // Indicates if attendance is mandatory
    "oreMinFreq": 4.0, // Minimum value of required attendance hours, filled if attendance is mandatory (FREQ_OBBLIG_FLG set to YES)
    "peso": 4.0, // Number of credits associated with the segment
    "ambId": 4, // Disciplinary field ID related to the segment
    "ambDes": "Legge", // Disciplinary field description
    "ambDesEng": "Law", // Disciplinary field description in English
    "tipoAfReitCod": "A", // Educational activity type to assign to the activity during the implementation of career plans at the first iteration
    "tipoAfReitDes": "Base", // Educational Activity Type description at the first iteration
    "tipoAfReitDesEng": "Basic compulsory subjects", // Educational Activity Type description in English at the first iteration
    "aaRegIni": 2012, // Cohort year (rules) segment validity start; use AA_ORD_ID as default
    "aaRegFin": 2012, // Cohort year (rules) segment validity end; use 9999 as default
    "interclaTipoAfCod": "A", // Educational activity type (TAF) valued only in case of cds with interclass (DM 270)
    "interclaTipoAfDes": "Base", // Educational Activity Type description in case of cds with interclass
    "interclaTipoAfDesEng": "Basic compulsory subjects", // Educational Activity Type description in English at the first iteration
    "interclaAmbId": 4, // Disciplinary field ID valued only in case of cds with interclass (DM 270)
    "interclaAmbDes": "Legge", // Disciplinary field description valued only in case of cds with interclass (DM 270)
    "interclaAmbDesEng": "Law", // Disciplinary field description in English valued only in case of cds with interclass (DM 270)
    "liberaOdFlg": 1 // Indicates if an academic activity is free
  }
]
```

<br>

---

<br>

### `GET /offerte/{aaOffId}/{cdsOffId}/{aaOrdOffId}/{pdsOffId}/{adOffId}` - Retrieve information of a contextualized academic activity

```java
/**
 * Retrieves the ADContestualizzata object.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdOffId           integer (path, required)            - Sort year ID of the study course
 * @param pdsOffId             integer (path, required)            - Study path ID
 * @param adOffId              integer (path, required)            - Academic activity ID
 * @return ADContestualizzata on success
 */
GET /offerte/{aaOffId}/{cdsOffId}/{aaOrdOffId}/{pdsOffId}/{adOffId}
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
{
  "chiaveAdContestualizzata": {
    "cdsId": 1, // Academic activity delivery study course key (required)
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "aaOrdId": 2016, // Sorting year of the academic activity delivery study course (required)
    "aaOrdCod": "CDS_AD_1", // Academic activity delivery sorting code
    "aaOrdDes": "Esempio di CDS AD", // Academic activity delivery sorting description
    "pdsId": 1, // Academic activity delivery study path key (required)
    "pdsCod": "PDS_AD_1", // Academic activity delivery path code
    "pdsDes": "Esempio di PDS AD", // Academic activity delivery path description
    "aaOffId": 1, // Academic activity delivery offer year (required)
    "adId": 1, // Academic activity key (required)
    "adCod": "PDS_AD_1", // Academic activity code
    "adDes": "Esempio di PDS AD", // Academic activity description
    "afId": 1 // afId from U-Gov Didattica
  }, // ChiaveAdContestualizzata (required)
  "adDesEng": "Esempio di PDS AD", // Academic activity description in English
  "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
  "aaOrdDesEng": "Esempio di CDS AD", // Academic activity delivery sorting description in English
  "pdsDesEng": "Esempio di PDS AD", // Academic activity delivery path description
  "linguaInsDes": "inglese", // Teaching language of the academic activity used for ECTS
  "linguaInsDesEng": "inglese", // Teaching language of the academic activity in English
  "nonErogabileOdFlg": 0, // Non-deliverable activity. If = 1 indicates that for this activity the partitions (classes) of p09_ad_log should NOT be considered in career processes, i.e., in the student booklet the partition (class) is never assigned for this activity
  "tipoEsaCod": "S", // Exam type code
  "tipoEsaDes": "scritto", // Exam type description
  "tipoEsaDesEng": "wrote", // Exam type description in English
  "tipoValCod": "V", // Evaluation type code
  "tipoValDes": "voto", // Evaluation type description
  "tipoValDesEng": "grade", // Evaluation type description in English
  "tipoInsCod": "FON, CAR", // Teaching type code. Valid only for pre-reform CDS
  "tipoInsDes": "fondamentale", // Teaching type description. Valid only for pre-reform CDS
  "gruppoGiudCod": "1", // Where the evaluation type is judgment, i.e., TIPO_VAL_COD = G, indicates the judgment group used
  "gruppoGiudDes": "Esempio di descrizione del gruppo di giudizio", // Judgment group description
  "reiterabile": 1, // Indicates if an activity can be repeated more than once within the student's career (e.g., literature courses). Contains the maximum number of possible repetitions.
  "urlSitoWeb": "Esempio di url", // URL of the structure's website
  "urlCorsoMoodle": "www.moodle.com", // URL of the MOODLE course linked to the academic activity
  "adCapogruppo": {
    "cdsId": 1, // Academic activity delivery study course key
    "cdsCod": "CDS_AD_1", // Academic activity delivery study course code
    "cdsDes": "Esempio di CDS AD", // Academic activity delivery course description
    "cdsDesEng": "Esempio di CDS AD", // Academic activity delivery course description in English
    "adId": 1, // Academic activity key
    "adCod": "AD_1", // Academic activity code
    "adDes": "Esempio di AD", // Academic activity description
    "adDesEng": "Esempio di AD", // Academic activity description in English
    "tipoRagCod": "TIPO_RAG_1", // Grouping type code
    "tipoRagDes": "Esempio di tipo di raggruppamento", // Grouping type description
    "tipoRagDesEng": "Esempio di tipo di raggruppamento in inglese", // Grouping type description in English
    "annoCoorte": 2020 // Cohort year
  }, // AdCapogruppo
  "capoGruppoFlg": 0, // Indicates if it is a head AD.
  "adWebViewFlg": 0 // Indicates if the current AD is visible on the web
}
```

<br>

---

<br>

### `GET /offerteEliminate` - Retrieve information about deleted offers

```java
/**
 * Retrieves the 'OffertaEliminata' objects with the deletion date
 *
 * @param dataModOd            string (query, optional)            - Last modification date of the academic offer
 * @param start                integer (query, optional)           - Used with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @return List<OffertaEliminata> on success
 */
GET /offerteEliminate
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** configuration

#### Response

**`200 OK`**

```json
[
  {
    "cdsId": 1, // Study course ID (required)
    "aaId": 1, // Academic Offer Year ID (required)
    "dataModOd": "15/10/2015" // Last modification date of the offer and details. Format: DD/MM/YYYY
  }
]
```

<br>

---

<br>

### `GET /offerteFull/{aaOffId}/{cdsOffId}/` - Retrieve full academic offer information

```java
/**
 * Retrieves the OffertaConDettagli object.
 *
 * @param aaOffId              integer (path, required)            - Offer year ID
 * @param cdsOffId             integer (path, required)            - Study course ID
 * @param aaOrdId              integer (query, optional)           - Sorting ID
 * @param pdsId                integer (query, optional)           - Study path ID
 * @param pdsCod               string (query, optional)            - Study path code
 * @param pdsDes               string (query, optional)            - Study path description (using * applies a LIKE filter)
 * @param adId                 integer (query, optional)           - Academic activity ID
 * @param adCod                string (query, optional)            - Academic activity code
 * @param adDes                string (query, optional)            - Academic activity description (using * applies a LIKE filter)
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. Syntax is * +/- : s...
 * @param fields               string (query, optional)            - Specifies the list of optional fields (not retrieved by default)
 * @return OffertaConDettagli on success
 */
GET /offerteFull/{aaOffId}/{cdsOffId}/
```

**Auth:** `UserTecnicoMassivo` required · Supported: `Basic`, `JWT`

**Cache:** none

#### Response

**`200 OK`**

```json
{}
```

<br>

---

<br>

---

## References

- **Swagger UI:** [Offerta Api V1 - ESSE3 REST Docs](<https://unimol.esse3.cineca.it/e3rest/docs/?urls.primaryName=Offerta%20Api%20V1%20(https%3A%2F%2Funimol.esse3.cineca.it%2Fe3rest%2Fapi%2Fofferta-service-v1)>)
- **Spec YAML:** [offertaApiV1.yaml](https://unimol.esse3.cineca.it/e3rest/api/swagger-service-v1/swagger/specs/p09-offertaApiV1.yaml)
- **ESSE3 REST API General Documentation:** [wiki.u-gov.it](https://wiki.u-gov.it/confluence/display/ESSE3/Servizi+REST+su+ESSE3)
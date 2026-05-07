---
title: Nazioni API V1 | OhMyUniversity!
description: REST API documentation for the Nazioni service (nazioni-service-v1) - CINECA ESSE3.
head:
  - - meta
    - property: og:title
      content: Nazioni API V1 | OhMyUniversity!
  - - meta
    - property: og:description
      content: REST API documentation for the Nazioni service (nazioni-service-v1) - CINECA ESSE3.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/api/cineca/api-unimol/nazioni-api-v1
  - - meta
    - name: keywords
      content: nazioni v1 api, esse3 rest api, cineca api, ohmyuniversity api, nazioni-service-v1
  - - meta
    - name: twitter:title
      content: Nazioni API V1 | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: REST API documentation for the Nazioni service (nazioni-service-v1) - CINECA ESSE3.
---

# OhMyUniversity! - Unimol: Nazioni API V1

**ENG:** `Countries`

**Version:** `1.0.0` · **Base URL:** `/nazioni-service-v1`

ESSE3 REST API to access information about countries, regions, municipalities, provinces, and postal codes.

---

## Endpoints - Nations (Nazioni)

### `GET /nazioni` - Retrieve nations

```java
/**
 * Allows retrieving nations. If the iso6392Cod parameter is null,
 * it will be set to 'ita' by default.
 *
 * @param iso6392Cod           string (query, optional)            - ISO language code
 * @return List<Nazione> on success,
 *         DettaglioErrore on failure
 */
GET /nazioni
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** lowRefreshRateUserIndependent

#### Response

**`200 OK`**

```json
[
  {
    "nazioneId": 245, // Unique numeric ID of the nation
    "codFisc": "Z704", // 4-character code (Letter + 3 numbers): Znnn - the letter is always a Z. In the case of Italy this attribute is not instantiated.
    "cod": "X1", // 3-character code - 3 numbers, used to describe the nation. Italy 200 Albania 201 ...
    "des": "ISOLE VITI", // Description
    "dataInizioVal": "31-DIC-75", // If the start year of validity is not null, the Codifica_Precedente attribute must also be instantiated, which contains the indication of the state from which the coded nation originated. For example, SLOVENIA, CROATIA, SERBIA-MONTENEGRO originated from Yugoslavia. The new codings will present an indication of the coding of the nation from which they originated. If the nation of origin is no longer valid, the end year of validity is also indicated.
    "dataFineVal": "31-DIC-75", // Nation validity end date.
    "codificaPrec": 200, // Previous nation coding, in the case of a nation replacing a no longer valid nation.
    "comuCodifFlg": 1, // Indicates whether the nation is managed at the level of coded municipalities and provinces.
    "nazioneCod": "CX", // MIUR code ID
    "csaCod": "CX", // CSA code, used to map nations during teacher alignment from the CSA system.
    "territorioCedutoFlg": 0, // Indicates that it is a ceded territory.
    "strIban": "DOkkBBBBBCCCCCCCCCCCCCCCCCCCC", // IBAN code structure.
    "prefixInternazionale": "39", // International prefix.
    "equipCorsoBancaItaFlg": 1, // Bank details management is equivalent to the Italian one.
    "codIso31661": "304" // 3-digit numeric ISO 3166 code
  }
]
```

**`422 Unprocessable Entity` - Invalid parameters**

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

## Endpoints - Postal Codes (Cap)

### `GET /nazioni/{nazioneId}/cap` - Retrieve postal codes

```java
/**
 * Allows retrieving postal codes.
 *
 * @param nazioneId            integer (path, required)            - Identifier of the nation
 * @param nazioneCodFisc       string (query, optional)            - Fiscal code of the nation
 * @param regioneCod           string (query, optional)            - Region code
 * @param sigla                string (query, optional)            - Province abbreviation
 * @param comuneId             integer (query, optional)           - Unique municipality ID
 * @param comuneCod            string (query, optional)            - 4-character code (Letter + 3 numbers) used in the fiscal code to indicate the municipality...
 * @return List<CAP> on success,
 *         DettaglioErrore on failure
 */
GET /nazioni/{nazioneId}/cap
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** lowRefreshRateUserIndependent

#### Response

**`200 OK`**

```json
[
  {
    "nazioneId": 1, // Unique numeric ID of the nation
    "nazioneCodFisc": "Z704", // 4-character code (Letter + 3 numbers): Znnn - the letter is always a Z. In the case of Italy this attribute is not instantiated.
    "regioneId": 1, // Numeric ID of the region
    "regioneCod": "1", // Unique numeric ID of the nation
    "regioneDes": "Piemonte", // Region description
    "sigla": "TO", // Automobile abbreviation of the province
    "provinciaCod": "provinciaCod", // 3-digit ISTAT code used to indicate the province. Example 084 Agrigento
    "provinciaDes": "Torino", // Province description
    "comuneId": 7839, // Unique numeric ID of the municipality
    "comuneCod": "L219", // 4-character code (Letter + 3 numbers) used in the fiscal code to indicate the municipality of birth.
    "comuneDes": "Torino", // Municipality description
    "comuCap": "10024", // 5-digit code - OBSOLETE.
    "capId": 10024, // Postal code identifier
    "cap": "10024", // Postal code of the municipality/location
    "capDes": "10024" // Description of the postal code area
  }
]
```

**`422 Unprocessable Entity` - Invalid parameters**

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

## Endpoints - Municipalities (Comuni)

### `GET /nazioni/{nazioneId}/comuni` - Retrieve municipalities

```java
/**
 * Allows retrieving municipalities.
 *
 * @param nazioneId            integer (path, required)            - Identifier of the nation
 * @param iso6392Cod           string (query, optional)            - ISO language code
 * @param nazioneCodFisc       string (query, optional)            - Fiscal code of the nation
 * @param regioneCod           string (query, optional)            - Region code
 * @param sigla                string (query, optional)            - Province abbreviation
 * @param comuneCod            string (query, optional)            - 4-character code (Letter + 3 numbers) used in the fiscal code to indicate the municipality...
 * @param start                integer (query, optional)           - Used together with `limit` for record pagination
 * @param limit                integer (query, optional)           - Used together with `start` for record pagination, `limit` indicates the number of...
 * @param order                string (query, optional)            - Allows specifying an order for retrieving records. The syntax is as follows * +/- : s...
 * @return List<Comune> on success,
 *         DettaglioErrore on failure
 */
GET /nazioni/{nazioneId}/comuni
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** lowRefreshRateUserIndependent

#### Response

**`200 OK`**

```json
[
  {
    "nazioneId": 1, // Unique numeric ID of the nation
    "nazioneCodFisc": "Z704", // 4-character code (Letter + 3 numbers): Znnn - the letter is always a Z. In the case of Italy this attribute is not instantiated.
    "regioneId": 1, // Numeric ID of the region
    "regioneCod": "1", // Unique numeric ID of the nation
    "regioneDes": "Piemonte", // Region description
    "sigla": "TO", // Automobile abbreviation of the province
    "provinciaCod": "1", // 3-digit ISTAT code used to indicate the province. Example 084 Agrigento
    "provinciaDes": "Torino", // Province description
    "comuneId": 7839, // Unique numeric ID of the municipality
    "idComune": 6395, // Municipality ID for MIUR tables
    "comuneCod": "L219", // 4-character code (Letter + 3 numbers) used in the fiscal code to indicate the municipality of birth.
    "codIstat": "698", // 6-digit code used by ISTAT to indicate the municipality
    "cap": "10024", // 5-digit code - OBSOLETE.
    "comuneDes": "Torino", // Municipality description
    "codIstatMiur": "698", // Mapping towards the ministerial table of Municipalities
    "distanzaKm": 10, // Distance in Km of the municipality from the university. Used for calculating fees
    "var": "ORA", // Variations linked to the municipality (ORA: Name change, AGG: aggregated, AGP: partially aggregated, AGT: temporarily aggregated, VED: Subject to further changes)
    "varCod": "L219", // Code variation.
    "varSigla": "TO", // Abbreviation variation.
    "varDes": "Torino", // Description variation.
    "dataCostit": "01/01/1920", // Municipality establishment date
    "newComuneId": 1, // Unique numeric ID of the municipality
    "attivoId": 1 // Indicates whether the municipality is active or not. 0) active; 1) obsolete, e.g. for the creation of a new province; 2) merged into another municipality.
  }
]
```

**`422 Unprocessable Entity` - Invalid parameters**

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

## Endpoints - Provinces (Province)

### `GET /nazioni/{nazioneId}/province` - Retrieve provinces

```java
/**
 * Allows retrieving provinces.
 *
 * @param nazioneId            integer (path, required)            - Identifier of the nation
 * @param nazioneCodFisc       string (query, optional)            - Fiscal code of the nation
 * @return List<Provincia> on success,
 *         DettaglioErrore on failure
 */
GET /nazioni/{nazioneId}/province
```

**Auth:** `ALL` required · Supported: `Basic`, `JWT`

**Cache:** lowRefreshRateUserIndependent

#### Response

**`200 OK`**

```json
[
  {
    "nazioneId": 1, // Unique numeric ID of the nation
    "nazioneCodFisc": "Z704", // 4-character code (Letter + 3 numbers): Znnn - the letter is always a Z. In the case of Italy this attribute is not instantiated.
    "regioneId": 1, // Numeric ID of the region
    "regioneCod": "1", // 2-digit code used by ISTAT to indicate the region: 19 Sicily 01 Piedmont
    "regioneDes": "Piemonte", // Description
    "codRegioVulc": "1", // Unique numeric ID of the nation
    "sigla": "TO", // Automobile abbreviation of the province
    "provinciaCod": "1", // 3-digit ISTAT code used to indicate the province. Example 084 Agrigento
    "provinciaDes": "Torino", // Province description
    "annoDefinizione": 1960, // Year of definition of the province.
    "provinciaPrecedente": "VC", // Populated in case of province separations, e.g., Prato and Florence.
    "annoFineValidita": 2000, // End year of province validity.
    "attivoId": 0 // Indicates whether the province is active: 0) active; 1) obsolete; 2) no longer exists because it was merged into another province.
  }
]
```

**`422 Unprocessable Entity` - Invalid parameters**

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

---

## References

- **Swagger UI:** [Nazioni Api V1 - ESSE3 REST Docs](<https://unimol.esse3.cineca.it/e3rest/docs/?urls.primaryName=Nazioni%20Api%20V1%20(https%3A%2F%2Funimol.esse3.cineca.it%2Fe3rest%2Fapi%2Fnazioni-service-v1)>)
- **Spec YAML:** [anag-nazioniApiV1.yaml](https://unimol.esse3.cineca.it/e3rest/api/swagger-service-v1/swagger/specs/p01-nazioniApiV1.yaml)
- **ESSE3 REST API General Documentation:** [wiki.u-gov.it](https://wiki.u-gov.it/confluence/display/ESSE3/Servizi+REST+su+ESSE3)
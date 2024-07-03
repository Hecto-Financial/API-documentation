# Pay Out_KRW Remittance (w.Balance Inquiry) README

## Overview

### Anylink Front Domain
- Production: gw.settlebank.co.kr  
  (IP: Provided separately by Hecto Financial)

### Character set
- UTF-8

### Integration Method
- Request: POST  
  Response: JSON

### URL Encoding
- Request: URL encoding required  
  Response: No URL encoding required

### Timeout (Seconds)
- 30 seconds

### IP Access Control
- Register merchant's server IP at HF

### Transaction Success
- Success: outStatCd=0021 + outRsltCd=0000  
  Fail: Refer to the result code sheet

### Parameter Requirement
- ●: Required  
  ◐: Required depending on the situation

### Encryption Method
- AES256-ECB + BASE64 Encoding

### Plain Text/Encryption Key Sample
- 1111 / 12345678901234567890123456789012

### Encryption Result Sample
- Gzv1ziVXlhyFS0EYMbHvqA==

**Note:** Encryption sample is not for integration test use.

### Classification

#### Transaction Type Code

| Code | Description               |
|------|---------------------------|
| PYA1 | /pyag/v1/fxRate           |
| PYB1 | /pyag/v1/fxTrans          |
| PYV1 | /pyag/v1/fxResult         |
| PYV2 | /pyag/v1/fxBalance        |
| PYC1 | /pyag/v1/fxCancel         |
| PYA2 | /pyag/v1/fxVaccount       |

#### Notification

- Transaction Type Code: -  
  URL: Merchant's designated URL

---

## History of Change

### Version History

| Version | Date       | Description              | Author |
|---------|------------|--------------------------|--------|
| 1.0     | 2023-01-01 | Initial Document         | Hecto  |
| 1.1     | 2023-02-01 | Added new transaction types | Hecto  |

---

## Address - /pyag/v1/fxRate

### Request (Merchant -> Hecto Financial)

| PRMTR_NM  | PRMTR_EXPL         | Max. Len | Mandatory | Desc. |
|-----------|---------------------|----------|-----------|-------|
| mchtId    | Merchant ID         | 12       | ●         |       |
| trdDt     | Transaction Date    | 8        | ●         | yyyymmdd |
| trdTm     | Transaction Time    | 6        | ●         | hhmmss |

#### Request Sample


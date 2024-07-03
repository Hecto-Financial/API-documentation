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

```
https://[Address]/pyag/v1/fxRate
PostData: mchtId=mid_test&trdDt=20230101&trdTm=120000
```

### Response (Hecto Financial -> Merchant)

| PRMTR_NM   | PRMTR_EXPL           | Max. Len | Mandatory | Desc. |
|------------|----------------------|----------|-----------|-------|
| mchtId     | Merchant ID          | 12       | ●         |       |
| trdDt      | Transaction Date     | 8        | ●         | yyyymmdd |
| trdTm      | Transaction Time     | 6        | ●         | hhmmss |
| rate       | Exchange Rate        | 12       | ●         |       |
| rateQuoteId| Exchange Rate Quote ID | 32     | ●         |       |
| status     | Status               | 4        | ●         | 0021: Success, 0031: Failure |

#### Response Sample (Success)

```json
{
    "mchtId": "mid_test",
    "trdDt": "20230101",
    "trdTm": "120000",
    "rate": "1100.50",
    "rateQuoteId": "RATE20230101120000",
    "status": "0021"
}
```

---

## Address - /pyag/v1/fxTrans

### Request (Merchant -> Hecto Financial)

| PRMTR_NM   | PRMTR_EXPL           | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Desc. |
|------------|----------------------|----------|--------------|---------------|-----------------|-------|
| mchtId     | Merchant ID          | 12       | ●            | ●             | ●               |       |
| mchtTrdNo  | Merchant Order Number| 100      | ●            | ●             | ●               | *At least unique per month |
| encCd      | Encryption Code      | 2        | ●            | ●             | ●               | 23: AES256ECB+BASE64 |
| trdDt      | Transaction Date     | 8        | ○            | ○             | ○               | For response, changed to Hecto Financial processed date |
| trdTm      | Transaction Time     | 6        | ○            | ○             | ○               | For response, changed to Hecto Financial processed time |
| svcDivCd   | Transaction Classification | 12 | ●            | ●             | ●               | FX:Exchange, RMT:Remittance, FXRMT:Exchange+Remittance |
| rateQuoteId| Exchange Rate ID     | 32       | ●            |               | ●               |       |
| sellCrcCd  | Selling Currency     | 12       | ●            |               | ●               |       |
| sellAmt    | Selling Amount       | 32       | ○            |               | ○               | Amount requested for exchange(Selling currency standard), just one between selling/buying. *Url encoding after encryption is required. |
| buyCrcCd   | Buying Currency      | 12       | ●            |               | ●               | Required for foreign exchange |
| buyAmt     | Buying Amount        | 32       | ○            |               | ○               | Amount requested for exchange(Buying currency standard) *Url encoding after encryption is required. |
| remitCat   | Remittance Classification | 12  |              | ●             | ●               | 1:Overseas, 2:Domestic, 3:Same Bank |
| remitAmt   | Remittance Amount    | 32       |              | ●             | ●               | In the case of USD, up to two decimal places allowed *Url encoding after encryption is required. |
| remitCrcCd | Remittance Currency  | 12       |              | ●             | ●               | USD, KRW *For KRW remittance without foreign exchange, real-time remittance is executed |
| rcvrNm     | Receiver's English Name | 35    |              | ●             | ●               |       |
| rcvrNmKr   | Receiver's Korean name | 35     |              | ◐             | ◐               | Url encoding required. Required if the nationality of the receiver is 'KR' |
| rcvrBirthDt| Receiver's Date of Birth | 32   |              | ●             | ●               | *Url encoding after encryption is required. |
| rcvrLiveNtnCd | Receiver's Country of Residence | 2 |        | ●             | ●               | 2-digit ISO country code |
| rcvrNtnCd  | Receiver's Nationality | 2      |              | ●             | ●               | 2-digit ISO country code |
| rcvrAddr   | Receiver's Address   | 135      |              | ◐             | ◐               | Omit for domestic KRW remittance |
| rcvrBankCd | Receiving Bank's Code | 11      |              | ●             | ●               | SWIFT BIC code For domestic KRW remittance, 3-digit domestic bank code |
| rcvrBankNm | Receiving Bank's Name | 128     |              | ◐             | ◐               | Omit for domestic KRW remittance |
| rcvrBankAddr | Receiving Bank's Address | 135 |              | ◐             | ◐               | Omit for domestic KRW remittance |
| rcvrAcntNo | Receiving Account Number | 64   |              | ●             | ●               | *Url encoding after encryption is required |
| remitRsnCd | Remittance Reason Code | 5      |              | ◐             | ◐               | Omit for domestic KRW remittance *Attached |
| rcvrAcntSumry | Receiving Account Remark | 32 |              | ◐             | ◐               | Valid only for KRW domestic remittance. Valid until maximum 7th letter only (Url encoding) |
| invFileNm  | Invoice File Name    | 32       |              | ◐             | ◐               | File name. (Korean letter and special character not allowed) Allowed extensions: jpg, png, jpeg * Required for domestic -> overseas remittance (excluding remittance to one's own account) |
| invFileData | Invoice File Data   | -        |              | ◐             | ◐               | BASE64 encoding then sent *For size, only below 100KB is allowed. |

* Excluding 'Receiver's Korean Name' and 'Receiving Account Remark', only English capital letters are allowed.

#### Request Sample

```
https://[Address]/pyag/v1/fxTrans
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&svcDivCd=FXRMT&sellCrcCd=KRW&buyC


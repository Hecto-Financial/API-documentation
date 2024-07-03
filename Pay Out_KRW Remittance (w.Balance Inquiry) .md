# Pay Out_KRW Remittance (w.Balance Inquiry) API

# History of Change

| Version | Date of Revision | Revision                                                  |
|---------|------------------|-----------------------------------------------------------|
| 1.0     | 2023-09-13       | * Draft                                                   |
| 1.1     | 2023-11-22       | Additional explanation and Url encoding explanation added |
| 1.6     | 2023-12-21       | Deposit Balance Inquiry API added                         |
| 1.7     | 2024-04-24       | Added Deposit Notification                                |


# Overview

## Anylink Front Domain

```
Production: gw.settlebank.co.kr
       ( IP: 61.252.169.53, 14.34.14.21 )
TEST: tbgw.settlebank.co.kr
       ( IP: 61.252.169.42 )
```

## Character set
```
UTF-8
```

## Integration Method
```
Request: POST,      Response: Json
```

## URL Encoding
```
Request: URL Encoding is required
Response: No URL Encoding
```

## Timeout (Seconds)
```
30 seconds
```

## IP Access Control
```
Merchant's server IP registration is required
```

## Encryption Method
```
AES256-ECB + BASE64 encoding
```

## Plain Text / Encryption Key Sample
```
1111 / 12345678901234567890123456789012
```

## Encryption Result Sample
```
Gzv1ziVXlhyFS0EYMbHvqA==
```

> ※ Encryption sample is not for integration test. It is a sample to check the encryption/decryption method. For test, test encryption key sent by Hecto Financial should be used.

## Parameter Classification
| Classification | Name                | Transaction Type Code | URL                         |
|----------------|---------------------|-----------------------|-----------------------------|
| Parameter      | KRW Remittance      | PYB9                  | /pyag/v1/fxTransKrw         |
|                | Result Inquiry      | PYV1                  | /pyag/v1/fxResult           |
|                | Balance Inquiry     | PYV2                  | /pyag/v1/fxBalance          |
|                | Cancellation Request| PYC1                  | /pyag/v1/fxCancel           |


# Remittance (B9)

# Detailed Address

## /pyag/v1/fxTransKrw

### Request (Merchant → Hecto Financial)

| PRMTR_NM     | PRMTR_EXPL               | Max. Len | Desc.                                                                                       |
|--------------|--------------------------|----------|--------------------------------------------------------------------------------------------|
| mchtId       | Merchant ID              | 12       |                                                                                            |
| mchtTrdNo    | Merchant Order Number    | 100      | *At least monthly unique                                                                   |
| encCd        | Encryption Code          | 2        | 23: AES256ECB+BASE64                                                                       |
| trdDt        | Transaction Date         | 8        | For response, changed to Hecto Financial process date                                      |
| trdTm        | Transaction Time         | 6        | For response, changed to Hecto Financial process time                                      |
| bankCd       | Deposit Bank Code        | 3        | 3-digit bank code                                                                          |
| custAcntNo   | Deposit Account Number   | 64       | *URL encoding after encryption. Numbers without character "-"                              |
| custAcntSumry| Deposit Account Remark   | 32       | Valid up to maximum of 7 Korean letters (URL Encoding)                                     |
| amt          | Deposit Amount           | 32       | *URL encoding after encryption. Integer value                                              |

### Request Sample

```
https://[Address]/pyag/v1/fxTransKrw
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&trdDt=20230919&trdTm=191919&bankCd=011&custAcntNo=r8tZfMLqcIzxano31w57V=&custAcntSumry=헥토&amt=zfaskAWEVZwewef=
```

* The content above is just randomly generated content for reference. custAcntNo and amt may vary based on the encryption key provided to merchants.

### Response (Merchant ← Hecto Financial)

| PRMTR_NM     | PRMTR_EXPL               | Max. Len | Desc.                                                                                       |
|--------------|--------------------------|----------|--------------------------------------------------------------------------------------------|
| mchtId       | Merchant ID              | 12       |                                                                                            |
| mchtTrdNo    | Merchant Order Number    | 100      | *At least monthly unique                                                                   |
| encCd        | Encryption Code          | 2        | 23: AES256ECB+BASE64                                                                       |
| trdNo        | Transaction Number       | 40       | Transaction number generated by Hecto Financial, valid for response only                   |
| trdDt        | Transaction Date         | 8        |                                                                                            |
| trdTm        | Transaction Time         | 6        |                                                                                            |
| outStatCd    | Transaction Status       | 4        |                                                                                            |
| outRsltCd    | Response Result          | 8        |                                                                                            |
| outRsltMsg   | Response Message         | 200      |                                                                                            |
| bankCd       | Deposit Bank Code        | 3        | 3-digit bank code                                                                          |
| custAcntNo   | Deposit Account Number   | 64       | *Encrypted                                                                                 |
| custAcntSumry| Deposit Account Remark   | 32       | Valid up to maximum of 7 Korean letters                                                    |
| amt          | Deposit Amount           | 32       | *Encrypted                                                                                 |
| balance      | Balance After Transaction| 12       | Responds when the transaction is successful                                                |

* If the response result is VTIM, please check whether it is success / failure after result inquiry.
If the provider(bank)'s error(or maintenance) gets extended, success/failure may not be determined during that time.

### Response Sample

```json
{
    "mchtId": "mid_test",
    "mchtTrdNo": "1234567890",
    "encCd": "23",
    "trdNo": "20230920HF1234",
    "trdDt": "20230919",
    "trdTm": "192000",
    "outStatCd": "0021",
    "outRsltCd": "0000",
    "outRsltMsg": "Normal Processing",
    "bankCd": "011",
    "custAcntNo": "r8tZfMLqcIzxano31w57V=",
    "custAcntSumry": "헥토",
    "amt": "zfaskAWEVZwewef=",
    "balance": "1300000"
}
```
# Result Inquiry (V1)

# Request (Merchant → Hecto Financial)

| PRMTR_NM  | PRMTR_EXPL             | Max. Len | Desc.                                                                 |
|-----------|------------------------|----------|----------------------------------------------------------------------|
| mchtId    | Merchant ID            | 12       |                                                                      |
| mchtTrdNo | Merchant Order Number  | 100      | Original Transaction Merchant Transaction Number                     |
| trdNo     | Transaction Number     | 40       | Original Transaction Hecto Financial Generated Transaction Number *Required to choose one between mchtTrdNo or trdNo |
| orgTrdDt  | Original Transaction Date | 8    |                                                                      |

## Request Sample

```
https://[Address]/pyag/v1/fxResult
PostData: mchtId=mid_test&mchtTrdNo=1234567890&trdNo=20230920HF1234&orgTrdDt=20230920
```

# Response (Merchant ← Hecto Financial)

| PRMTR_NM   | PRMTR_EXPL              | Max. Len | Desc.                                                   |
|------------|-------------------------|----------|--------------------------------------------------------|
| mchtId     | Merchant ID             | 12       |                                                        |
| mchtTrdNo  | Merchant Order Number   | 100      | Original Transaction Merchant Transaction Number        |
| trdNo      | Transaction Number      | 40       | Original Transaction Transaction Number                 |
| trdDt      | Transaction Date        | 8        | Inquiry Date                                           |
| trdTm      | Transaction Time        | 6        | Inquiry Time                                           |
| outStatCd  | Transaction Status      | 4        |                                                        |
| outRsltCd  | Response Result         | 8        |                                                        |
| outRsltMsg | Response Message        | 200      |                                                        |
| status     | Status                  | 12       |                                                        |

## Response Sample

```json
{
  "mchtId": "mid_test",
  "mchtTrdNo": "1234567890",
  "trdNo": "20230920HF1234",
  "trdDt": "20230920",
  "trdTm": "120000",
  "outStatCd": "0021",
  "outRsltCd": "0000",
  "outRsltMsg": "Normal Processing",
  "status": "0"
}
```

# Status Description

- 00: (Request Data Error) Remittance cannot be processed
- 99: (Undefined error status)
- 29: (Processing remittance) Did not receive the final result due to bank timeout
- 21: (Remittance success)
- 22: (Remittance failure) Final remittance failure status
- 23: (Remittance cancellation) Remittance is canceled according to merchant or Hecto's internal policy



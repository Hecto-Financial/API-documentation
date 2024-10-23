![image](https://github.com/user-attachments/assets/9d3a8184-6055-4794-b55b-82356a04f249)# Integrated API

![image](https://github.com/Hecto-Financial/API-documentation/assets/73467915/939d9b90-6f12-4f0c-96dd-93bfba443ca2)

# History of Change

| Version | Revision Date         | Revision                                        |
|---------|-----------------------|-------------------------------------------------|
| 1.0     | 2023-09-13            | *Draft                                          |
| 1.1     | 2023-10-05            | Status code and result code added               |
| 1.2     | 2023-10-17            | Result inquiry status code description added    |
| 1.3     | 2023-11-22            | Url encoding description added                  |
| 1.4     |	2023-12-05	          | Overseas remittance failure status code added (classify domestic and overseas) |
|1.5      |	2023-12-20	          | "Deposit notification added                      
|         |                       |  For overseas remittance, added receiver's address parameter" |
|1.6      |	2023-12-21	          |"Deposit balance inquiry API added               |
|         |                       |B1 parameter modification: Domestic KRW remittance receiving bank remark parameter added" |
|1.7      |	2024-01-16	          |"Added Virtual account issuance API for deposits,|
|         |                       |Changed invoice requirement: required only for domestic -> international transfers (but not for transfers to own account).|
|         |                       |Added error codes for virtual accounts"          |
|1.7.1	  |2024-03-28	            |Added information about required field           |
|1.7.2    |2024-04.24	            |treadBankCd field added in Deposit Notification  |
|1.7.3	  |2024-07-09             |Status code 19(Processing currency exchange) added|
|1.8      |2024-08-29             |Added response parameters for KRW remittance over 1 Billion KRW and new status code(19) on Result Inquiry(V1). 
|         |                       |Added New result code (ST13)                      |
|         |                       |Added Bank Institution code                       |


# Overview

## Anylink Front Domain


**Production** 

gw.settlebank.co.kr  
(IP: 61.252.169.53, 14.34.14.21)

**Test** 

tbgw.settlebank.co.kr  
(IP: 61.252.169.42)


## Character set

UTF-8

## Integration Method

Request: POST  
Response: JSON

## URL Encoding

Request: URL encoding required  
Response: No URL encoding required

## Timeout (Seconds)

30 seconds

## IP Access Control

Register merchant's server IP at HF

## Transaction Success

```

Success: **outStatCd=0021 + outRsltCd=0000  **
Fail: Others(Refer to the result code sheet)

```

## Parameter Requirement

●: Required  
◐: Required depending on the situation
○: Unmarked: Can Be Omitted

## Encryption Method

AES256-ECB + BASE64 Encoding

## Plain Text/Encryption Key Sample

1111 / 12345678901234567890123456789012

## Encryption Result Sample

Gzv1ziVXlhyFS0EYMbHvqA==

**Note:**  Encryption sample is not for integration test. It is a sample for checking encryption / decryption method.
Test encryption key sent by Hecto Financial should be used for test.

| **Classification** | **Name**                           | **Transaction Type Code** | **URL**                        |
|--------------------|------------------------------------|----------------------------|--------------------------------|
| API                | Exchange Rate Inquiry              | PYA1                       | /pyag/v1/fxRate                |
|                    | Refund, Remittance (Merchant)      | PYB1                       | /pyag/v1/fxTrans               |
|                    | Result Inquiry                     | PYV1                       | /pyag/v1/fxResult              |
|                    | Balance Inquiry                    | PYV2                       | /pyag/v1/fxBalance             |
|                    | Cancellation Request               | PYC1                       | /pyag/v1/fxCancel              |
|                    | Virtual account Issuance Request   | PYA2                       | /pyag/v1/fxVaccount            |

| **Classification** | **Name**                | **Transaction Type Code** | **URL**                           |
|--------------------|-------------------------|----------------------------|-----------------------------------|
| Notification       | Deposit Notification     | -                          | Merchant's designated URL         |



## Exchange Rate Inquiry (A1)

### Address
 /pyag/v1/fxRate		

### Request (Merchant -> Hecto Financial)

| **PRMTR_NM**   | **PRMTR_EXPL**            | **Max. Len** | **Mandatory** | **Desc.**                                                           |
|----------------|---------------------------|--------------|---------------|---------------------------------------------------------------------|
| mchtId         | Merchant ID                | 12           | ●             |                                                                     |
| mchtTrdNo      | Merchant Order Number      | 100          | ●             | *At least unique per month                                           |
| rateCategoryId | Exchange Rate Classification| 12          | ●             | 1M, 25H                                                             |
| sellCrcCd      | Selling Currency           | 12           | ●             | KRW, USD                                                            |
| buyCrcCd       | Buying Currency            | 12           | ●             | KRW, USD  *Buying currency and selling currency cannot be the same   |

**Request Sample**

```
https://[Address]/pyag/v1/fxRate
PostData: mchtId=mid_test&mchtTrdNo=1234567890&rateCategoryId=25H&sellCrcCd=KRW&buyCrcCd=USD
```

### Response (Hecto Financial -> Merchant)

| **PRMTR_NM**   | **PRMTR_EXPL**             | **Max. Len** | **Mandatory** | **Desc.**                                                            |
|----------------|----------------------------|--------------|---------------|----------------------------------------------------------------------|
| mchtId         | Merchant ID                 | 12           | ●             |                                                                     |
| mchtTrdNo      | Merchant Order Number       | 100          | ●             |    *At least unique per month                                       |
| trdNo          | Transaction Number          | 40           | ●             | Transaction number generated by Hecto Financial, valid for response only |
| trdDt          | Transaction Date            | 8            | ●             |                                                                      |
| trdTm          | Transaction Time            | 6            | ●             |                                                                      |
| outStatCd      | Transaction Status          | 4            | ●             | 0021: Success, 0031: Failure                                          |
| outRsltCd      | Response Result             | 8            | ●             | 0000: Success, Others: Refer to Result Code sheet                     |
| outRsltMsg     | Response Message            | 200          | ●             |                                                                      |
| rateCategoryId | Exchange Rate Classification| 12           | ●             | 1M, 25H                                                              |
| sellCrcCd      | Selling Currency            | 12           | ●             | KRW, USD                                                             |
| buyCrcCd       | Buying Currency             | 12           | ●             | KRW, USD  *Buying currency and selling currency cannot be the same    |
| rateQuoteId    | Exchange Rate ID            | 32           | ◐             |                                                                      |
| rate           | Exchange Rate               | 32           | ◐             | Decimals marked                                                      |
| validFrom      | Start of Exchange Rate Valid Period | 14     | ◐             | YYYYMMDDhhmmss                                                       |
| validTo        | End of Exchange Rate Valid Period   | 14     | ◐             | YYYYMMDDhhmmss                                                       |
| mchtDcRt       | Preferential Exchange Rate  | 10           | ◐             | Marked with %                                                        |

**Response Sample**

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
    "rateCategoryId": "25H",
    "sellCrcCd": "KRW",
    "buyCrcCd": "USD",
    "rateQuoteId": "HCTO1MT4WWQJETEST",
    "rate": "1230.00",
    "validFrom": "20230920100000",
    "validTo": "20230921110000",
    "mchtDcRt": "80"
}
```

## Virtual Account Issuance ((A2)

### Address
 /pyag/v1/fxVaccount		

### Request (Merchant -> Hecto Financial)

| PRMTR_NM       | PRMTR_EXPL                         | Max. Len      | Mandatory| Desc.                                         |
|----------------|------------------------------------|---------------|----------|--------------------------------------------|
| mchtId         | Merchant ID                        | AN(12)        | ●        | "midtest"                                   |
| trdType        | Transaction Type                   | AN(14)        | ●        | 1:Issuance,  2:Termination, 3:Change of account holder name |
| validTo        | Validity End Date & Time           | AN(14)        | ○        | "YYYYMMDDhhmmss If it is not set, it would be set to indefinite period. Valid only for issuance" |
| accountNm      | Account Owner Name                 | AN(32)        | ◐        | "When inquiring the account, the name that will be displayed as account holder. Maximum 10 characters. *Requires URL encoding. Valid only for issuance. *trdType: 3 Required." |
| bankCd         | Bank Code                          | AN(3)         | ◐        | *trdType: 2, 3 Required.                   |
| vaccountNo     | Number of Virtual Account Being Terminated | AN(32)        | ◐        | *trdType: 2, 3 Required.                   |

#### Request Samples

**Issuance**  
```
https://[Address]/pyag/v1/fxVaccount
PostData: mchtId=mid_test&trdType=1&validTo=20501231235959&accountNm=HECTO
```

**Terminate**  
```
https://[Address]/pyag/v1/fxVaccount
PostData: mchtId=mid_test&trdType=2&bankCd=023&vaccountNo=1234567890
```

**Change**  
```
https://[Address]/pyag/v1/fxVaccount
PostData: mchtId=mid_test&trdType=2&bankCd=023&vaccountNo=1234567890&accountNm=NEW_HECTO
```

### Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows:

| PRMTR_NM     | PRMTR_EXPL                           | Max. Len      | Mandatory| Desc.                                      |
|----------------|------------------------------------|---------------|----------|--------------------------------------------|
| mchtId         | Merchant ID                        | AN(12)        | ●        | "midtest"                                   |
| trdNo          | Transaction Number                 | AN(40)        | ●        | Transaction number generated by Hecto Financial, valid for response only |
| trdDt          | Transaction Date                   | AN(8)         | ●        |                                            |
| trdTm          | Transaction Time                   | AN(6)         | ●        |                                            |
| outStatCd      | Transaction Status                 | AN(4)         | ●        | 0021: Success, 0031: Failure               |
| outRsltCd      | Response Result                    | AN(8)         | ●        | 0000: Success, Others: Refer to result code sheet. |
| outRsltMsg     | Response Message                   | AN(200)       | ●        |                                            |
| bankCd         | Bank Code                          | AN(3)         | ●        |                                            |
| vaccountNo     | Virtual Account Number             | AN(32)        | ●        |                                            |

### Response Sample 
```
"{
 ""mchtId"":""mid_test"",  
 ""trdNo"":""20240116HF1234"",  ""trdDt"":""20240116"",
 ""trdTm"":""120000"",   ""outStatCd"":""0021"",   ""outRsltCd"":""0000"",
 ""outRsltMsg"":""정상처리"", ""bankCd"":""023"",
 ""vaccountNo"":""9876543210123""
}"				
```



# Exchange, Remittance (B1)

## /pyag/v1/fxTrans

### Request (Merchant -> Hecto Financial)

| PRMT_NM        | PRMTR_EXPL                         | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Desc. |
|----------------|------------------------------------|----------|--------------|---------------|-----------------|-------|
| mchtId         | Merchant ID                        | 12       | ●            | ●             | ●               |       |
| mchtTrdNo      | Merchant Order Number              | 100      | ●            | ●             | ●               | *At least unique per month |
| encCd          | Encryption Code                    | 2        | ●            | ●             | ●               | 23: AES256ECB+BASE64 |
| trdDt          | Transaction Date                   | 8        | ○            | ○             | ○               | For response, changed to Hecto Financial processed date |
| trdTm          | Transaction Time                   | 6        | ○            | ○             | ○               | For response, changed to Hecto Financial processed time |
| svcDivCd       | Transaction Classification         | 12       | ●            | ●             | ●               | FX:Exchange, RMT:Remittance, FXRMT:Exchange+Remittance |
| rateQuoteId    | Exchange Rate ID                   | 32       | ●            |               | ●               |       |
| sellCrcCd      | Selling Currency                   | 12       | ●            |               | ●               |       |
| sellAmt        | Selling Amount                     | 32       | ○            |               | ○               | Amount requested for exchange (Selling currency standard), just one between selling/buying. *Url encoding after encryption is required. |
| buyCrcCd       | Buying Currency                    | 12       | ●            |               | ●               | Required for foreign exchange |
| buyAmt         | Buying Amount                      | 32       | ○            |               | ○               | Amount requested for exchange (Buying currency standard) *Url encoding after encryption is required. |
| remitCat       | Remittance Classification          | 12       |              | ●             | ●               | 1:Overseas, 2:Domestic, 3:Same Bank |
| remitAmt       | Remittance Amount                  | 32       |              | ●             | ●               | In the case of USD, up to two decimal places allowed *Url encoding after encryption is required. |
| remitCrcCd     | Remittance Currency                | 12       |              | ●             | ●               | USD, KRW *For KRW remittance without foreign exchange, real-time remittance is executed |
| rcvrNm         | Receiver's English Name            | 35       |              | ●             | ●               | Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrNmKr       | Receiver's Korean Name             | 35       |              | ◐             | ◐               | Url encoding required. Required if the nationality of the receiver is 'KR'. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrBirthDt    | Receiver's Date of Birth           | 32       |              | ●             | ●               | *Url encoding after encryption is required. |
| rcvrLiveNtnCd  | Receiver's Country of Residence    | 2        |              | ●             | ●               | 2-digit ISO country code |
| rcvrNtnCd      | Receiver's Nationality             | 2        |              | ●             | ●               | 2-digit ISO country code |
| rcvrAddr       | Receiver's Address                 | 135      |              | ◐             | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrBankCd     | Receiving Bank's Code              | 11       |              | ●             | ●               | SWIFT BIC code. For domestic KRW remittance, 3-digit domestic bank code |
| rcvrBankNm     | Receiving Bank's Name              | 128      |              | ◐             | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrBankAddr   | Receiving Bank's Address           | 135      |              | ◐             | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrAcntNo     | Receiving Account Number           | 64       |              | ●             | ●               | *Url encoding after encryption is required |
| remitRsnCd     | Remittance Reason Code             | 5        |              | ◐             | ◐               | Omit for domestic KRW remittance *Attached |
| rcvrAcntSumry  | Receiving Account Remark           | 32       |              | ◐             | ◐               | Valid only for KRW domestic remittance. Valid until maximum 7th letter only (Url encoding) |
| invFileNm      | Invoice File Name                  | 32       |              | ◐             | ◐               | File name. (Korean letter and special character not allowed) Allowed extensions: jpg, png, jpeg * Required for domestic -> overseas remittance (excluding remittance to one's own account) |
| invFileData    | Invoice File Data                  | -        |              | ◐             | ◐               | BASE64 encoding then sent *For size, only below 100KB is allowed. |

* Excluding 'Receiver's Korean Name' and 'Receiving Account Remark', only English capital letters are allowed.

### Request Sample

```
" https://[Address]/pyag/v1/fxTrans
 PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&svcDivCd=FXRMT&sellCrcCd=KRW&buyCrcCd=USD&buyAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCat=1&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCrcCd=USD&rcvrNm=JACKSON&rcvrNmKr=JACKSON&rcvrBirthDt=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrLiveNtnCd=US&rcvrNtnCd=US&rcvrBankCd=CITIUS33&rcvrBankAddr=24STREET NEWYORK CITY US&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==&remitRsnCd=10101&invFileNm=INVOID_2023.JPG"				


```


### Response (Hecto Financial -> Merchant)

| PRMT_NM        | PRMTR_EXPL                         | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Desc. |
|----------------|------------------------------------|-----------|-------------|-------------|-------------|-------------|
| mchtId         | Merchant ID                        | 12        | ●           | ●           | ●           |             |
| mchtTrdNo      | Merchant Order Number              | 100       | ●           | ●           | ●           | *At least unique per month |
| encCd          | Encryption Code                    | 2         | ●           | ●           | ●           | 23: AES256ECB+BASE64 |
| trdNo          | Transaction Number                 | 40        | ●           | ●           | ●           | Hecto Financial generated transaction number, valid for response only |
| trdDt          | Transaction Date                   | 8         | ●           | ●           | ●           |             |
| trdTm          | Transaction Time                   | 6         | ●           | ●           | ●           |             |
| outStatCd      | Transaction Status                 | 4         | ●           | ●           | ●           | 0021: Success, 0031: Failure |
| outRsltCd      | Response Result                    | 8         | ●           | ●           | ●           | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg     | Response Message                   | 200       | ●           | ●           | ●           |             |
| svcDivCd       | Transaction Classification         | 12        | ●           | ●           | ●           | FX:Foreign Exchange, RMT:Remittance, FXRMT:Foreign Exchange+Remittance |
| rateQuoteId    | Exchange Rate ID                   | 32        | ●           |             | ●           |             |
| sellCrcCd      | Selling Currency                   | 12        | ●           |             | ●           |             |
| sellAmt        | Selling Amount                     | 32        | ●           |             | ●           | Amount requested for exchange(Selling currency standard), just one between selling/buying. *Encryption |
| buyCrcCd       | Buying Currency                    | 12        | ●           |             | ●           |             |
| buyAmt         | Buying Amount                      | 32        | ●           |             | ●           | Amount requested for exchange(Buying currency standard) *Encryption |
| remitCat       | Remittance Classification          | 12        |             | ●           | ●           | 1:Overseas, 2:Domestic, 3:Same Bank |
| remitAmt       | Remittance Amount                  | 32        |             | ●           | ●           | In the case of USD, upto two decimal places allowed *Encryption. |
| remitCrcCd     | Remittance Currency                | 12        |             | ●           | ●           | USD, KRW |
| rcvrNm         | Receiver's English Name            | 35        |             | ●

### Response Sample

```
" {
 "mchtId":"mid_test",   "mchtTrdNo":"1234567890",   "encCd":"23",
 "trdNo":"20230920HF1234",  "trdDt":"20230920",
 "trdTm":"120000",   "outStatCd":"0021",   "outRsltCd":"0000",    "outRsltMsg":"Normal Processing", "svcDivCd":"FXRMT",
 "sellCrcCd":"KRW",  "sellAmt":"Gzv1ziVXlhyFS0EYMbHvqA==",    "buyCrcCd":"USD",   "buyAmt":"Gzv1ziVXlhyFS0EYMbHvqA=="
 "remitCat":"1",  "remitAmt":"Gzv1ziVXlhyFS0EYMbHvqA==",      "remitCrcCd":"USD",  "rcvrNm":"JACKSON",
 "rcvrNmKr":"JACKSON",  "rcvrBirthDt":"Gzv1ziVXlhyFS0EYMbHvqA==",     "rcvrLiveNtnCd":"US",  "rcvrNtnCd":"US",
 "rcvrBankCd":"CITIUS33",  "rcvrBankAddr":"24STREET NEWYORK CITY US",
 "rcvrAcntNo":"Gzv1ziVXlhyFS0EYMbHvqA==",  "remitRsnCd":"10101",     "rate":"1230.00",  "balance":"120000"
}				

```
# Result Inquiry (V1)

### Address
**/pyag/v1/fxResult**

### Request (Merchant -> Hecto Financial)

# API Documentation: /pyag/v1/fxResult

## Request (Merchant -> Hecto Financial)

| PRMTR_NM     | PRMTR_EXPL                                   | Max. Len      | Mandatory     | Desc.                                            |
|--------------|----------------------------------------------|--------------|---------------|---------------------------------------------------|
| mchtId       | Merchant ID                                  | 12           | ●             |                                                  |
| mchtTrdNo    | Merchant Order Number                        | 100          | ◑             |Original Transaction Merchant Transaction Number |
| trdNo        | Transaction Number                           | 40           | ◑             |Original Transaction Hecto Financial Generated Transaction Number|
|              |                                              |              |                |*One between mchtTrdNo or trdNo is required        | 
| orgTrdDt     | Original Transaction Date                    | 8            | ◯             |                                                   |

### Request Sample
```
 https://[Address]/pyag/v1/fxResult
 PostData: mchtId=mid_test&mchtTrdNo=1234567890&trdNo=20230920HF1234&orgTrdDt=20230920
```

### Response (Hecto Financial -> Merchant)

| PRMTR_NM     | PRMTR_EXPL                                   | Max. Len     |   Mandatory   | Desc.                                             |
|--------------|----------------------------------------------|--------------|---------------|---------------------------------------------------|
| mchtId       | Merchant ID                                  | 12           | ●             |                                                   |
| mchtTrdNo    | Merchant Order Number                        | 100          | ●             | Original Transaction Merchant Transaction Number  |
| trdNo        | Transaction Number                           | 40           | ◑             | Original Transaction Transaction Number          |
| trdDt        | Transaction Date                             | 8            | ●             | Inquiry Date                                      |
| trdTm        | Transaction Time                             | 6            | ●             | Inquiry Time                                      |
| outStatCd    | Transaction Status                           | 4            | ●             | 0021: Success, 0031: Failure *                     |
| outRsltCd    | Response Result                              | 8            | ●             | 0000: Success, Others: Refer to Result code sheet |
| outRsltMsg   | Response Message                             | 200          | ●              |                                                   |
| status       | Status                                       | 12           | ●             |  00:Request Data Error,   99: Undefined Status,    |
|              |                                              |              |               | 10:Exchange Pending, 11:Exchange Success, 12:Exchange Failure,|
|              |                                              |              |               |13:Exchange Cancellation,20:Pending Remittance, 21:Remittance Success,| 
|              |                                              |              |               |22:Remittance Failure,23:Remittance Cancellation 29: Processing Remittance|                                                 

### Response Sample (Success)

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
  "status": "11"
}
```
## Status Description

- **00: (Request Data Error)** Exchange and remittance cannot be processed
- **99: (Undefined)** Internal error
- **10: (Exchange Pending)** Merchant has made the exchange request normally, it is in the status of before processing the exchange
- **19: (Processing Currency Exchange)** Bank is processing currency exchange. Exchanged funds are not transmitted to bank account yet.
- **11: (Exchange Success)** Exchange process success.
- **12: (Exchange Failure)** Exchange process failure, can retry automatically to change to Exchange Success
- **13: (Exchange Cancellation)** Merchant cancelled the exchange
- **20: (Pending Remittance)** Pending remittance status. If requested with exchange, after exchange success, pending remittance status
- **29: (Processing Remittance)** Hecto Financial → Domestic Bank remittance request completed. Before processing Domestic Bank → Overseas Bank.
- **21: (Remittance Success)** Domestic Bank → Overseas Bank remittance request completed, in the status in which remittance failure can occur later on in the Overseas Bank.
- **22: (Remittance Failure)** Hecto Financial → Domestic Bank failure
- **24: (Remittance Failure)** After Hecto Financial → Domestic Bank success, Domestic Bank→Overseas Bank failure
- **23: (Remittance Cancellation)** Remittance cancelled due to merchant or Hecto's internal policy


# Balance Inquiry (V2)

## Request (Merchant -> Hecto Financial)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |

## Request Sample

```
 https://[Address]/pyag/v1/fxBalance
 PostData: mchtId=mid_test
```


## Response (Hecto Financial -> Merchant)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |
| trdNo     | Transaction Number | 40  | ●         | Hecto Financial generated transaction number, valid for response only|
| trdDt     | Transaction Date | 8     | ●         | Inquiry Date      |
| trdTm     | Transaction Time | 6     | ●         | Inquiry Time      |
| outStatCd | Transaction Status | 4   | ●         | 0021: Success, 0031: Failure |
| outRsltCd | Response Result | 8      | ●         | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg | Response Message | 200  | ●         |       |
|blcKrw     | KRW Balance  | 20        |●          |       |
| blcUsd   | USD Balance      | 20       | ●         | Mark until second decimal place only. |

## Response Sample (Success)

```json
{
 "mchtId":"mid_test",
 "trdNo":"20230920HF1234",  "trdDt":"20230920",  "trdTm":"120000",
 "outStatCd":"0021",   "outRsltCd":"0000", "outRsltMsg":"Normal Processing",
 "blcKrw":"150000",  "blcUsd":"1234.56"
}
```


### Cancellation (C1)

# Cancellation (C1)

## Request (Merchant -> Hecto Financial)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |
| mchtTrdNo | Merchant Order Number | 100 | ● | Original Transaction Merchant Transaction Number |
| trdNo     | Transaction Number | 40  | ●         | Original Transaction Hecto Financial Generated Transaction Number |
| trdDt     | Original Transaction Date | 8 | ● |       |

## Request Sample

```
https://[Address]/pyag/v1/fxCancel
PostData: mchtId=mid_test&mchtTrdNo=1234567890&trdNo=20230920HF1234&trdDt=20230920
```

## Response (Hecto Financial -> Merchant)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |
| mchtTrdNo | Merchant Order Number | 100 | ● | Original Transaction Merchant Transaction Number |
| trdNo     | Transaction Number | 40  | ●         | Original Transaction Transaction Number |
| trdDt     | Transaction Date | 8     | ●         | Cancellation Date |
| trdTm     | Transaction Time | 6     | ●         | Cancellation Time |
| outStatCd | Transaction Status | 4   | ●         | 0021: Success, 0031: Failure |
| outRsltCd | Response Result | 8      | ●         | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg | Response Message | 200  | ●         |       |

## Response Sample (Success)

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
}
```

# Deposit Notification

## Request (Hecto Financial -> Merchant)

### Start IP (Hecto Financial Notification Server IP)
Testbed: 61.252.169.22  
Production Environment:  61.252.169.24,   14.34.14.23

### End Address (Merchant Notification API URL)
Provided by the merchant side, register on merchant's server beforehand.

### Note
 Only deposit notifications inputted through the above-mentioned Hecto Financial's start IP should be processed.

Here is the GitHub README markdown format for the content you provided:

### Request (Hecto Financial → Merchant)

| **PRMTR_NM**  | **PRMTR_EXPL**             | **Max. Len** | **Mandatory** | **Desc.**                                       |
|---------------|-----------------------------|--------------|---------------|-------------------------------------------------|
| notiType      | Notification Type           | 12           | ●             | Fixed Value: DEPOSIT                            |
| mchtId        | Merchant ID                 | 12           | ●             |                                                 |
| dpDt          | Deposit Date                | 8            | ●             | YYYYMMDD                                        |
| dpTm          | Deposit Time                | 6            | ●             | hhmmss                                          |
| dpTrdNo       | Deposit Transaction Number  | 32           | ●             |                                                 |
| outStatCd     | Transaction Status Code     | 4            | ●             | Fixed Value: 0021                               |
| dpCrcCd       | Deposit Currency Code       | 20           | ●             | Fixed Value: KRW                                |
| dpAmt         | Deposit Amount              | 20           | ●             |                                                 |
| blc           | Deposit Balance             | 20           | ●             |                                                 |
| bankCd        | Deposit Virtual Account Bank Code | 3        | ●             | Virtual account's bank code                     |
| vtlAcntNo     | Deposit Virtual Account Number | 32        | ●             | *URL encoding after encryption                  |
| treatBankCd   | Treated Bank Codes          | 3            | ●             | Code of the bank used by the client for deposit |
| dpstrNm       | Depositor Name              | 30           | ●             | UTF-8 + URL encoding                            |

**Request Sample**

```plaintext
[Notification URL]?
PostData: notiType=DEPOSIT&mchtId=mid_test&dpDt=20231220&dpTm=120000&dpTrdNo=0A52B81FBNHIAFEACEHNDDKF008106FC&outStatCd=0021&dpCrcCd=KRW&dpAmt=100000&blc=150000&bankCd=023&vtlAcntNo=Gzv1ziVXlhyFS0EYMbHvqA%3d%3d&treatBankCd=011&dpStrNm=%ed%99%8d%ea%b8%b8%eb%8f%99
```

### Response (Hecto Financial ← Merchant)

| **PRMTR_NM** | **PRMTR_EXPL** | **Max. Len** | **Mandatory** | **Desc.**                                                                            |
|--------------|----------------|--------------|---------------|--------------------------------------------------------------------------------------|
| Normal Processing | OK  |               | ●             | If "OK" is in the response data, processed as success. Notification not resent        |
| Failure        | FAIL          |               | ●             | Notification resent                                                                  |
| Others         | Failure       |               | ●             | Notification resent                                                                  |
| Resent according to the set interval/number of times.                                                                |

**Response Sample: Success**

```plaintext
OK
```

**Response Sample: Failure**

```plaintext
FAIL
```

**Response Sample: Success**

```html
<HTML><BODY>OK : Normal Processing </BODY></HTML>
```
※ If there is "OK" in the response, it is processed as success.


# Result Code

### Result Code Table

| **Result Code** | **Message**                                   | **Revision Date** |
|-----------------|-----------------------------------------------|-------------------|
| 0000            | Normal processing                             | 2023.09.18        |
| PY11            | Bank maintenance time                         | 2023.09.18        |
| PY12            | Insufficient deposit balance                  | 2023.09.18        |
| PY13            | Requested outside of KRW 1B+ transfer service hours | 2024.08.07        |
| PY21            | No receiving account                          | 2023.09.18        |
| PY31            | Exchange rate information error               | 2023.09.18        |
| PY41            | Exceeded service capacity                     | 2023.09.18        |
| PY42            | Transaction cannot be done with this bank     | 2023.09.18        |
| PY43            | Blocked according to WLF                      | 2023.09.18        |
| PY44            | Target for WLF, needs separate approval       | 2023.09.18        |
| PY50            | Duplicate parameter request                   | 2023.09.18        |
| PY51            | Already cancelled transaction                 | 2023.09.18        |
| PY52            | Original transaction does not exist           | 2023.10.05        |
| PY53            | Cancellation not allowed                      | 2023.10.05        |
| PY61            | Invalid virtual account numbers               | 2024.01.16        |
| PY97            | Merchant is not registered (Check message)    | 2023.09.18        |
| PY98            | Invalid request parameter (Check message)     | 2023.09.18        |
| PY99            | Internal system error                         | 2023.09.18        |
| VTIM            | Provider timeout                              | 2023.09.18        |



# Remittance Reason Code Table 

### Remittance Reason Code Table on or After January 1, 2015 (Use remittance reason code when remitting foreign currency overseas)

#### Codename: New code values (for financial institution reporting)

| Code values (for financial institution reporting)   | **Codename**                                               |
|----------|-----------------------------------------------------------------------------------------------------------|
| 10101    | Advance payment for customs clearance receipts                                                             |
| 10103    | Post-Transfer Customs Payments                                                                             |
| 10201    | Advance payment of customs-free imports (brokered trade imports)                                            |
| 10202    | Payments for customs-free imports (brokered trade imports) by postal transfer                               |
| 10203    | Advance remittance payment for customs-free imports (foreign receipts)                                      |
| 10204    | Payments for customs-free imports (foreign receipts) by post remittance method                              |
| 10205    | Advance remittance method customs-free import (domestic import) payment                                     |
| 10206    | Payments for customs-free imports (domestic receipts) by post remittance method                             |
| 10299    | Other income-related payments                                                                               |
| 19901    | Trade-related payments (small transactions of $1,000 or less)                                               |
| 20101    | Advance payment of customs clearance export proceeds (other than new ships and offshore equipment)          |
| 20102    | Advance payment of customs clearance export payment (new ships and offshore equipment)                      |
| 20103    | Payment of the return portion of the customs clearance export payment (other than new ships and offshore equipment) |
| 20104    | Payment of the return portion of the customs clearance export payment (new ships and offshore equipment)    |
| 20201    | Pre- and post-customs duty-free export (brokered trade export) return payments                              |
| 20202    | Pre- and post-clearance export (foreign export) return payments                                             |
| 20203    | Pre- and post-clearance export (domestic) return payments                                                   |
| 20299    | Other export-related payment returns                                                                        |
| 30101    | Study and training (as specified by the correspondent bank)                                                 |
| 30201    | Business travel                                                                                             |
| 30202    | Therapeutic travel                                                                                          |
| 30203    | Sightseeing, familiarization, and other tours                                                               |
| 30206    | Travel expenses (credit cards)                                                                              |
| 30207    | General expatriate travel expenses (excluding study abroad and trainees)                                    |
| 30208    | Expatriate travel expenses related to study and training                                                    |
| 30301    | Travel arrangement fees                                                                                     |
| 30401    | Nonresident-related reconversion                                                                            |
| 30402    | Reconversion for currency exchange agents                                                                   |
| 30901    | Travel (microtransactions under $1000)                                                                      |
| 31101    | Air freight rates                                                                                           |
| 31102    | Ocean freight rates                                                                                         |
| 31201    | Airline passenger fares                                                                                     |
| 31202    | Ocean Passenger Fares                                                                                       |
| 31301    | Ocean Freight Goods                                                                                         |
| 31302    | Air Transportation Goods                                                                                    |
| 31303    | Maritime operating expenses                                                                                 |
| 31304    | Airline operating expenses                                                                                  |
| 31401    | Repayment of principal on a vessel-related wholly-owned finance lease                                        |
| 31402    | Principal repayment of an aircraft-related wholly-owned finance lease                                        |
| 31403    | Vessel-related wholly-owned finance lease interest payments                                                  |
| 31404    | Interest payments on aircraft-related wholly-owned finance leases                                           |
| 31501    | Ship charter rates                                                                                          |
| 31502    | Aircraft charter rates                                                                                      |
| 31503    | Ship Rent                                                                                                   |
| 31504    | Aircraft lease rates                                                                                        |

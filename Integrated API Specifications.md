

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
|1.9        |2024-10-31                  |Changed the description for encCd to no encryption. |
|           |                            |Added remark for Receiver bank institution code for KEB-Hana Bank (1064)|

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
|                    | Exchange, Remittance (Merchant)    | PYB1                       | /pyag/v1/fxTrans               |
|                    | KRW Remittance(Over 10B KRW)       | PYB4                       | /pyag/v1/fxTransOnlyKrw        |
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
```
/pyag/v1/fxVaccount		
```

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

### Address
/pyag/v1/fxTrans

### Request (Merchant -> Hecto Financial)

| Parameter Name     | Description                     | Max Length | Mandatory FX | Mandatory RMT (USD) | Mandatory RMT (KRW) | Mandatory FXRMT | Desc.                                                                                     |
|---------------------|---------------------------------|------------|--------------|----------------------|---------------------|-----------------|-------------------------------------------------------------------------------------------|
| mchtId           | Merchant ID                    | 12         | ●            | ●                    | ●                   | ●               |                                                                                           |
| mchtTrdNo        | Merchant Order Number          | 100        | ●            | ●                    | ●                   | ●               | At least unique per month                                                                 |
| encCd            | Encryption Code                | 2          | ●            | ●                    | ●                   | ●               | Encryption Code and meaning:<br>**23 (Fixed Value)**: AES256ECB + BASE64                  |
| trdDt            | Transaction Date               | 8          | ○            | ○                    | ○                   | ○               | For response, changed to Hecto Financial processed date                                   |
| trdTm            | Transaction Time               | 6          | ○            | ○                    | ○                   | ○               | For response, changed to Hecto Financial processed time                                   |
| svcDivCd         | Transaction Classification     | 12         | ●            | ●                    | ●                   | ●               | FX: Foreign Exchange<br>RMT: Remittance<br>FXRMT: Exchange + Remittance                  |
| rateQuoteId      | Exchange Rate ID               | 32         | ●            |                      |                     | ●               |                                                                                           |
| sellCrcCd        | Selling Currency               | 12         | ●            |                      |                     | ●               |                                                                                           |
| sellAmt          | Selling Amount                 | 32         | ○            |                      |                     | ○               | Amount requested for exchange (Selling currency standard).<br>*URL encoding required.*    |
| buyCrcCd         | Buying Currency                | 12         | ●            |                      |                     | ●               | Required for foreign exchange.                                                            |
| buyAmt           | Buying Amount                  | 32         | ○            |                      |                     | ○               | Amount requested for exchange (Buying currency standard).<br>*URL encoding required.*     |
| remitCat         | Remittance Classification      | 12         |              | ●                    | ●                   | ●               | 1: Overseas 2: Domestic 3: Same Bank                                               |
| remitAmt         | Remittance Amount              | 32         |              | ●                    | ●                   | ●               | In case of USD, up to two decimal places allowed.<br>*URL encoding required.*             |
| remitCrcCd       | Remittance Currency            | 12         |              | ●                    | ●                   | ●               | USD, KRW<br>*For KRW remittance without foreign exchange, real-time remittance executed.* |
| rcvrNm           | Receiver's English Name        | 35         |              | ●                    |                     | ◐               |                                                                                           |
| rcvrNmKr         | Receiver's Korean Name         | 35         |              |                      | ○                   | ○               | URL encoding required.                                                                    |
| rcvrBirthDt      | Receiver's Date of Birth       | 32         |              | ●                    |                     | ◐               | *Url encoding after encryption is required.                                                                    |
| rcvrLiveNtnCd  | Receiver's Country of Residence    | 2          |              | ●                    |                     | ●               | 2-digit ISO country code |
| rcvrNtnCd      | Receiver's Nationality             | 2          |              | ●                    |                     | ●               | 2-digit ISO country code |
| rcvrAddr       | Receiver's Address                 | 135        |              | ◐                   |                     | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrBankCd     | Receiving Bank's Code              | 11         |              | ●                    |                     | ●               | SWIFT BIC code. For domestic KRW remittance, 3-digit domestic bank code |
| rcvrBankNm     | Receiving Bank's Name              | 128        |              | ◐                   |                     | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrBankAddr   | Receiving Bank's Address           | 135        |              | ◐                   |                     | ◐               | Omit for domestic KRW remittance. Allowed special characters: [ ,, -, ., /, @, (, ) ] |
| rcvrAcntNo     | Receiving Account Number           | 64         |              | ●                    |                     | ●               | *Url encoding after encryption is required |
| remitRsnCd     | Remittance Reason Code             | 5          |              | ◐                   |                     | ◐               | Omit for domestic KRW remittance *Attached |
| rcvrAcntSumry  | Receiving Account Remark           | 32         |              | ◐                   |                     | ◐               | Valid only for KRW domestic remittance. Valid until maximum 7th letter only (Url encoding) |
| invFileNm      | Invoice File Name                  | 32         |              | ◐                   |                     | ◐               | File name. (Korean letter and special character not allowed)<br> Allowed extensions: jpg, png, jpeg<br> * Required for domestic> overseas remittance (excluding remittance to one's own account) |
| invFileData    | Invoice File Data                  | -          |              | ◐                   |                     | ◐               | BASE64 encoding then sent *For size, only below 100KB is allowed. |
* Excluding 'Receiver's Korean Name' and 'Receiving Account Remark', only English capital letters are allowed.

### Request Sample
```plaintext
POST https://[Address]/pyag/v1/fxTrans
PostData:
mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&svcDivCd=FXRMT&sellCrcCd=KRW&buyCrcCd=USD&buyAmt=Gzv1ziVXlhyFS0EYMbHvqA==
&remitCat=1&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCrcCd=USD&rcvrNm=JACKSON&rcvrNmKr=JACKSON&rcvrBirthDt=Gzv1ziVXlhyFS0EYMbHvqA==
&rcvrLiveNtnCd=US&rcvrNtnCd=US&rcvrBankCd=CITIUS33&rcvrBankAddr=24STREET NEWYORK CITY US&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==
&remitRsnCd=10101&invFileNm=INVOID_2023.JPG
```
### Response (Hecto Financial → Merchant)

| Parameter Name      | Description                        | Max Length | Mandatory FX | Mandatory RMT (USD) | Mandatory RMT (KRW) | Mandatory FXRMT | Desc                                                                                     |
|----------------------|------------------------------------|------------|--------------|----------------------|---------------------|-----------------|-------------------------------------------------------------------------------------------|
| mchtId            | Merchant ID                       | 12         | ●            | ●                    | ●                   | ●               |                                                                                           |
| mchtTrdNo         | Merchant Order Number             | 100        | ●            | ●                    | ●                   | ●               | At least unique per month                                                                 |
| encCd             | Encryption Code                   | 2          | ●            | ●                    | ●                   | ●               | 23                                                                                        |
| trdNo             | Transaction Number                | 40         | ●            | ●                    | ●                   | ●               | Hecto Financial generated transaction number, valid for response only                     |
| trdDt             | Transaction Date                  | 8          | ●            | ●                    | ●                   | ●               |                                                                                           |
| trdTm             | Transaction Time                  | 6          | ●            | ●                    | ●                   | ●               |                                                                                           |
| outStatCd         | Transaction Status                | 4          | ●            | ●                    | ●                   | ●               | 0021: Success, 0031: Failure                                                              |
| outRsltCd         | Response Result                   | 8          | ●            | ●                    | ●                   | ●               | 0000: Success, Others: Refer to Result Code sheet                                         |
| outRsltMsg        | Response Message                  | 200        | ●            | ●                    | ●                   | ●               |                                                                                           |
| svcDivCd          | Transaction Classification        | 12         | ●            | ●                    | ●                   | ●               | FX: Foreign Exchange, RMT: Remittance, FXRMT: Foreign Exchange + Remittance              |
| rateQuoteId       | Exchange Rate ID                  | 32         | ●            |                      |                     | ●               |                                                                                           |
| sellCrcCd         | Selling Currency                  | 12         | ●            |                      |                     | ●               |                                                                                           |
| sellAmt           | Selling Amount                    | 32         | ●            |                      |                     | ●               | Amount requested for exchange (Selling currency standard), just one between selling/buying. <br>*Encryption required.* |
| buyCrcCd          | Buying Currency                   | 12         | ●            |                      |                     | ●               |                                                                                           |
| buyAmt            | Buying Amount                     | 32         | ●            |                      |                     | ●               | Amount requested for exchange (Buying currency standard). <br>*Encryption required.*      |
| remitCat          | Remittance Classification         | 12         |              | ●                    | ●                   | ●               | 1: Overseas, 2: Domestic, 3: Same Bank                                                   |
| remitAmt          | Remittance Amount                 | 32         |              | ●                    | ●                   | ●               | In the case of USD, up to two decimal places allowed. <br>*Encryption required.*          |
| remitCrcCd        | Remittance Currency               | 12         |              | ●                    | ●                   | ●               | USD, KRW                                                                                 |
| rcvrNm            | Receiver's English Name           | 35         |              | ●                    |                     | ◐               |                                                                                           |
| rcvrNmKr          | Receiver's Korean Name            | 35         |              |                      | ●                   | ◐               |                                                                                           |
| rcvrBirthDt       | Receiver's Date of Birth          | 32         |              | ●                    |                     | ◐               | *Encryption required.*                                                                    |
| rcvrLiveNtnCd     | Receiver's Country of Residence   | 2          |              | ●                    |                     | ◐               |                                                                                           |
| rcvrNtnCd         | Receiver's Nationality            | 2          |              | ●                    | ●                   | ●               |                                                                                           |
| rcvrAddr          | Receiver's Address                | 135        |              | ●                    |                     | ◐               |                                                                                           |
| rcvrBankCd        | Receiving Bank's Code             | 11         |              | ●                    | ●                   | ●               |                                                                                           |
| rcvrBankNm        | Receiving Bank's Name             | 128        |              | ●                    |                     | ◐               |                                                                                           |
| rcvrBankAddr      | Receiving Bank's Address          | 135        |              | ●                    |                     | ◐               |                                                                                           |
| rcvrAcntNo        | Receiving Account Number          | 64         |              | ●                    | ●                   | ●               | *Encryption required.*                                                                    |
| remitRsnCd        | Remittance Reason Code            | 5          |              | ●                    |                     | ◐               |                                                                                           |
| rcvrAcntSumry     | Receiving Account Remark          | 32         |              | ●                    | ●                   | ◐               | Request value given as response                                                          |
| rate              | Exchange Rate                    | 32         | ◐            |                      |                     | ◐               |                                                                                           |
| balance           | Balance                           | 32         | ◐            | ◐                    | ◐                   | ◐               | Balance after remittance. KRW                                                            |


### Response Sample
```json
{
  "mchtId": "mid_test",
  "mchtTrdNo": "1234567890",
  "encCd": "23",
  "trdNo": "20230920HF1234",
  "trdDt": "20230920",
  "trdTm": "120000",
  "outStatCd": "0021",
  "outRsltCd": "0000",
  "outRsltMsg": "Normal Processing",
  "svcDivCd": "FXRMT",
  "sellCrcCd": "KRW",
  "sellAmt": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "buyCrcCd": "USD",
  "buyAmt": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "remitCat": "1",
  "remitAmt": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "remitCrcCd": "USD",
  "rcvrNm": "JACKSON",
  "rcvrNmKr": "JACKSON",
  "rcvrBirthDt": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "rcvrLiveNtnCd": "US",
  "rcvrNtnCd": "US",
  "rcvrBankCd": "CITIUS33",
  "rcvrBankAddr": "24STREET NEWYORK CITY US",
  "rcvrAcntNo": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "remitRsnCd": "10101",
  "rate": "1230.00",
  "balance": "120000"
}
```
# Domestic KRW Remittance (B4)

### Address
```
/pyag/v1/fxTransOnlyKrw
```
**※ If you are transferring more than 1 Billion KRW to a domestic bank account, it will not be processed in real time. You must wait 10 minutes after the request to check the final result. Transfers of 1 Billion KRW or less are processed in real-time.**

### Request (Merchant → Hecto Financial)

| Parameter Name       | Description                          | Max Length | Mandatory (≤1B KRW) | Mandatory (>1B KRW) | Desc.                                                                                     |
|-----------------------|--------------------------------------|------------|----------------------|---------------------|-------------------------------------------------------------------------------------------|
| mchtId               | Merchant ID                         | 12         | ●                    | ●                   |                                                                                           |
| mchtTrdNo            | Merchant Order Number               | 100        | ●                    | ●                   | *At least unique per month                                                                  |
| encCd                | Encryption Code                     | 2          | ●                    | ●                   | Encryption Code and meaning:<br>**23 (Fixed Value)**: AES256ECB + BASE64                                           
| trdDt                | Transaction Date                    | 8          | ○                    | ○                   | For response, changed to Hecto Financial processed date                                |
| trdTm                | Transaction Time                    | 6          | ○                    | ○                   |For response, changed to Hecto Financial processed date                                |
| remitAmt             | Remittance Amount                   | 32         | ●                    | ●                   | **URL encoding after encryption is required**                                             |
| rcvrBankCd           | Receiver Bank Code                  | 3          | ●                    | ●                   | 3-digit domestic bank code for KRW remittance                                             |
| rcvrAcntNo           | Receiver Bank Account Number        | 64         | ●                    | ●                   | **URL encoding after encryption is required**                                             |
| rcvrBankOrgCd        | Receiver Bank Institution Code      | 4          | ○                    | ●                   | Refer to the provided [Institution code table](#Receiver-Bank-Institution-Code)  
# Remittance Reason Code Table
| rcvrBankBranchNm     | Receiver Bank Branch Name           | 35         | ○                    | ●                   | Max 9 Korean characters, **URL encoding required**                                       |
| rcvrNmKr             | Receiver's Korean Name              | 35         | ○                    | ●                   | Max 10 Korean or English characters, **URL encoding required**                            |
| rcvrNtnCd            | Receiver's Nationality              | 2          | ●                    | ●                   | 2-digit ISO country code (Korea = KR)                                                    |
| rcvrAcntSumry        | Receiving Account Remark            | 35         | ●                    | ●                   | Max 10 Korean characters, **URL encoding required**                                       |


### Request Sample: 1 Billion KRW or Less
```
https://[address]/pyag/v1/fxTransOnlyKrw
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrBankCd=011&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrNmKr=%ed%99%8d%ea%b8%b8%eb%8f%99&rcvrNtnCd=KR&rcvrAcntSumry=%ec%a0%95%ec%82%b0%eb%8c%80%ea%b8%88
```

### Request Sample: More than 1 Billion KRW
```
https://[address]/pyag/v1/fxTransOnlyKrw
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrBankCd=011&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrBankOrgCd=1033&rcvrBankBranchNm=%ec%97%ad%ec%82%bc%ec%84%bc%ed%84%b0%ec%a0%90&rcvrNmKr=%ed%99%8d%ea%b8%b8%eb%8f%99&rcvrNtnCd=KR&rcvrAcntSumry=%ec%a0%95%ec%82%b0%eb%8c%80%ea%b8%88
```

---

## Response Parameters (Merchant ← Hecto Financial)

| Parameter Name       | Description                          | Max Length | Mandatory (≤1B KRW) | Mandatory (>1B KRW) | Notes                                                                                     |
|-----------------------|--------------------------------------|------------|----------------------|---------------------|-------------------------------------------------------------------------------------------|
| mchtId               | Merchant ID                         | 12         | ●                    | ●                   |                                                                                           |
| mchtTrdNo            | Merchant Order Number               | 100        | ●                    | ●                   | Must be unique per month                                                                  |
| encCd                | Encryption Code                     | 2          | ●                    | ●                   | Value: 23                                                                                 |
| trdNo                | Transaction Number                  | 40         | ●                    | ●                   | Hecto Financial generated transaction number, valid for response only                     |
| trdDt                | Transaction Date                    | 8          | ●                    | ●                   |                                                                                           |
| trdTm                | Transaction Time                    | 6          | ●                    | ●                   |                                                                                           |
| outStatCd            | Transaction Status                  | 4          | ●                    | ●                   | 0021: Success, 0031: Failure                                                              |
| outRsltCd            | Response Result                     | 8          | ●                    | ●                   | 0000: Success, others: Refer to result code sheet                                         |
| outRsltMsg           | Response Message                    | 200        | ●                    | ●                   |                                                                                           |
| remitAmt             | Remittance Amount                   | 32         | ●                    | ●                   |                                                                                           |
| rcvrBankCd           | Receiver Bank Code                  | 3          | ●                    | ●                   |                                                                                           |
| rcvrAcntNo           | Receiver Bank Account Number        | 64         | ●                    | ●                   |                                                                                           |
| rcvrBankOrgCd        | Receiver Bank Institution Code      | 4          | ○                    | ●                   |                                                                                           |
| rcvrBankBranchNm     | Receiver Bank Branch Name           | 35         | ○                    | ●                   |                                                                                           |
| rcvrNmKr             | Receiver's Korean Name              | 35         | ○                    | ●                   |                                                                                           |
| rcvrNtnCd            | Receiver's Nationality              | 2          | ●                    | ●                   | 2-digit ISO country code                                                                  |
| rcvrAcntSumry        | Receiving Account Remark            | 35         | ●                    | ●                   |                                                                                           |
| balance              | Balance                             | 32         | ◐                    | ◐                   | Balance after remittance (KRW)                                                           |

---

### Response Sample
```json
{
  "mchtId": "mid_test",
  "mchtTrdNo": "1234567890",
  "encCd": "23",
  "trdNo": "20240820HF1234",
  "trdDt": "20240820",
  "trdTm": "1200000000",
  "outStatCd": "0021",
  "outRsltCd": "0000",
  "outRsltMsg": "정상처리",
  "svcDivCd": "FXRMT",
  "remitAmt": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "rcvrBankCd": "011",
  "rcvrAcntNo": "Gzv1ziVXlhyFS0EYMbHvqA==",
  "rcvrBankOrgCd": "1033",
  "rcvrBankBranchNm": "%ec%97%ad%ec%82%bc%ec%84%bc%ed%84%b0%ec%a0%90",
  "rcvrNmKr": "%ed%99%8d%ea%b8%b8%eb%8f%99",
  "rcvrNtnCd": "KR",
  "rcvrAcntSumry": "%ec%a0%95%ec%82%b0%eb%8c%80%ea%b8%88",
  "balance": "23000000000"
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

# Receiver Bank Institution Code 

| Code  | Name of Institution (English)                       | Name of Institution (Korean)       | Remark|
|-------|-----------------------------------------------------|------------------------------------|        |
| 1016  | Bank of Korea                                       | 한국은행                             | 
| 1017  | Kwollirak Securities                                | 권리락증권                           |
| 1020  | KDB                                                 | 한국산업은행                         |
| 1021  | Industrial Bank - ADB Loan                          | 산업은행-ADB전대차관                 |
| 1033  | NongHyup Bank                                       | 농협은행                             |
| 1047  | IBK                                                 | 중소기업은행                         |
| 1051  | Kookmin Bank                                        | 국민은행                             |
| 1064  | Hana Bank (KEB Hana Bank)                           | 한국외환은행                         | Only use in case for old Hana Bank accounts named KEB-Hana and use 1261 for Hana Bank accounts|
| 1081  | Suhyup Bank                                         | 수협중앙회                           |
| 1095  | Export-Import Bank of Korea                         | 수출입은행                           |
| 1171  | Woori Bank                                          | 우리은행                             |
| 1184  | SC Bank                                             | SC제일은행                           |
| 1211  | Shinhan Bank                                        | 신한은행                             |
| 1225  | Citi Bank                                           | 한국씨티은행                         |
| 1260  | Hana Bank                                           | 하나은행                             |
| 1314  | Daegu Bank (IM Bank)                                | 대구은행                             |
| 1328  | Busan Bank                                          | 부산은행                             |
| 1345  | Kwangju Bank                                        | 광주은행                             |
| 1359  | Jeju Bank                                           | 제주은행                             |
| 1376  | Jeonbuk Bank                                        | 전북은행                             |
| 1393  | Kyongnam Bank                                       | 경남은행                             |
| 1510  | JP Morgan Bank                                      | JP모간은행                           |
| 1517  | Goldman Sachs International Bank                    | 골드만삭스인터내셔널은행               |
| 1537  | BOA (Bank of America)                               | BOA                                  |
| 1541  | Mitsubishi Tokyo UFJ Bank                           | 미쓰비시도쿄유에프제이은행            |
| 1554  | Tokyo Mitsubishi                                    | 도쿄미쓰비시                         |
| 1571  | Mizuho Corporate Bank                               | 미즈호코퍼레이트                     |
| 1585  | FUJI                                                | FUJI                                 |
| 1599  | Sumitomo Mitsui Banking Corporation                 | 미쓰이스미토모                       |
| 1612 | ＢＡＮＫＯＮ                                        | ＢＡＮＫＯＮ                              |
| 1626 | Crédit Lyonnais                                    | 크레디리오네은행                          |
| 1630 | ＡＲＡＢ                                           | ＡＲＡＢ                                  |
| 1643 | ＮＡ－ＣＡＮＡＤＡ                                  | ＮＡ－ＣＡＮＡＤＡ                        |
| 1657 | ＡＭＥＸ                                           | ＡＭＥＸ                                  |
| 1674 | ＬＢＰ                                             | ＬＢＰ                                    |
| 1688 | BNP Paribas                                       | 비엔피파리바                              |
| 1691 | ＰＡＲＩＢＡＳ                                     | ＰＡＲＩＢＡＳ                            |
| 1701 | ＢＢＰ                                             | ＢＢＰ                                    |
| 1715 | ＢＣＣＩ                                           | ＢＣＣＩ                                  |
| 1729 | ＩＯＢ                                             | ＩＯＢ                                    |
| 1732 | ＮＡ－ＡＵＳＴ                                     | ＮＡ－ＡＵＳＴ                            |
| 1746 | ＨＡＷＡＩＩ                                       | ＨＡＷＡＩＩ                              |
| 1763 | ＩＮＧ                                             | ＩＮＧ                                    |
| 1777 | Hualian Bank                                       | 화련                                      |
| 1780 | ＮＯＶＡＳ                                         | ＮＯＶＡＳ                                |
| 1794 | Bank of Montreal                                   | 몬트리올                                  |
| 1835 | ＤＥＵＴＳＣＨＥ                                   | ＤＥＵＴＳＣＨＥ                          |
| 1849 | ＣＨＥＭＩ－ＯＬＤ                                 | ＣＨＥＭＩ－ＯＬＤ                        |
| 1852 | Crédit Agricole CIB                               | 크레디아그리콜ＣＩＢ                     |
| 1866 | Chemical Bank                                      | 케미컬                                    |
| 1870 | ＢＴＣ                                             | ＢＴＣ                                    |
| 1883 | RBS Bank                                           | 알비에스은행                              |
| 1897 | ＵＢＡＦ                                           | ＵＢＡＦ                                  |
| 1907 | ＦＩＣＡＬ                                         | ＦＩＣＡＬ                                |
| 1911 | ＭＴＢＣ                                           | ＭＴＢＣ                                  |
| 1938 | DBS Bank                                           | 디비에스은행                              |
| 1941 | ＵＦＪ                                             | ＵＦＪ                                    |
| 1955 | ＳＥＰＡＣ                                         | ＳＥＰＡＣ                                |
| 1969 | Fleet National Bank                                | 플릿내셔널                                |
| 1986 | ＴＯＫＡＩ                                         | ＴＯＫＡＩ                                |
| 1990 | ＳＧＢ                                             | ＳＧＢ                                    |
| 2004 | ＷＦＢ－ＳＬ                                       | ＷＦＢ－ＳＬ                              |
| 2018 | ＵＯＢ                                             | ＵＯＢ                                    |
| 2052 | Westpac Bank                                       | 웨스트팩                                  |
| 2066 | Yamaguchi Bank                                     | 야마구찌                                  |
| 2070 | ＣＡＬＩＦＯＲＮＩＡ                              | ＣＡＬＩＦＯＲＮＩＡ                     |
| 2083 | ＡＮＺ                                             | ＡＮＺ                                    |
| 2097 | ＮＢＰ                                             | ＮＢＰ                                    |
| 2107 | 뉴욕                                             | 뉴욕                                      |
| 2124 | ＳＡＫＵＲＡ                                       | ＳＡＫＵＲＡ                              |
| 2138 | ＡＳＡＨＩ                                         | ＡＳＡＨＩ                                |
| 2155 | ＤＡＩＷＡ                                         | ＤＡＩＷＡ                                |
| 2165 | Gyeonggi Commit Savings Bank Co., Ltd.           | 경기코미트신용금고（주）                 |
| 2258 | ＯＣＢＣ                                           | ＯＣＢＣ                                  |
| 2351 | ＵＢＳ                                             | ＵＢＳ                                    |
| 2508 | ＨＳＢＣ                                           | ＨＳＢＣ                                  |
| 2525 | National Westminster Bank                          | 내쇼날웨스터민스터                       |
| 2539 | ＢＯＣ                                             | ＢＯＣ                                    |
| 2556 | ＭＥＴＲＯ                                        | ＭＥＴＲＯ                                |
| 2560 | Wachovia Bank                                      | 와코비아                                  |
| 2573 | Credit Suisse Bank                                 | 크레디트스위스                            |
| 2587 | Industrial and Commercial Bank of China (ICBC)   | 중국공상                                  |
| 2590 | Industrial and Commercial Bank of China (ICBC)   | 모간                                      |
| 2600 | Industrial and Commercial Bank of China (ICBC)   | 바클레이즈                                |
| 2601 | Industrial and Commercial Bank of China (ICBC)   | 중국건설                                  |
| 2602 | Bank of Communications                             | 교통은행                                  |
| 2603 | Morgan Stanley Bank                                | 모간스탠리은행                            |
| 2604 | Macquarie Bank                                     | 맥쿼리은행                                |
| 2610 | Merrill Lynch International Bank                   | 메릴린치인터내셔널은행                   |
| 2620 | Lehman Brothers Bank                               | 리먼브러더스뱅크하우스                   |
| 2630 | State Bank of Baden-Württemberg (LBBW)            | 바덴뷔르템베르크주립은행                 |
| 2640 | Banco Bilbao Vizcaya Argentaria (BBVA)            | 빌바오비스까야아르헨따리아은행          |
| 2651 | Agricultural Bank of China                         | 중국농업은행                              |
| 2663 | Royal Bank of Scotland PLC (RBS)                  | 알비에스피엘씨은행                        |
| 5116 | ＮＥＧＡ－ＭＡＬ                                   | ＮＥＧＡ－ＭＡＬ                          |
| 5337 | ＮＯＲＩＮ－ＳＰ                                   | ＮＯＲＩＮ－ＳＰ                          |
| 5461 | ＳＡＮＰＡ－ＳＰ                                     | ＳＡＮＰＡ－ＳＰ                      |
| 5462 | ＢＯＪ－ＴＫ                                       | ＢＯＪ－ＴＫ                        |
| 7041 | ＡＤＢ－ＴＡＳＦ（기금）                            | ＡＤＢ－ＴＡＳＦ（기금）             |
| 7051 | ＡＤＢ－Ｂ                                         | ＡＤＢ－Ｂ                          |
| 7065 | ＡＤＢ－Ｃ                                         | ＡＤＢ－Ｃ                          |
| 7066 | ＡＤＢ－ＴＡＳＦ                                    | ＡＤＢ－ＴＡＳＦ                     |
| 7067 | ＡＤＢ－ＫＰＦ                                      | ＡＤＢ－ＫＰＦ                       |
| 7068 | ＩＭＦ－ＰＦＴＡＣ                                  | ＩＭＦ－ＰＦＴＡＣ                   |
| 7071 | ＩＢＲＤ－ＣＴＦ（기금）                            | ＩＢＲＤ－ＣＴＦ（기금）             |
| 7096 | ＩＢＲＤ－Ｎ                                       | ＩＢＲＤ－Ｎ                        |
| 7137 | ＩＦＣ                                             | ＩＦＣ                               |
| 7168 | ＩＤＡ－Ｇ                                         | ＩＤＡ－Ｇ                          |
| 7171 | ＡＦＤＢ                                           | ＡＦＤＢ                            |
| 7172 | ＡＦＤＢ－ＫＣＴ（기금）                           | ＡＦＤＢ－ＫＣＴ（기금）             |
| 7185 | ＭＩＧＡ－Ｇ                                       | ＭＩＧＡ－Ｇ                        |
| 7211 | ＥＢＲＤ－ＴＣＦ（기금）                           | ＥＢＲＤ－ＴＣＦ（기금）             |
| 7226 | ＡＤＢ－Ａ                                         | ＡＤＢ－Ａ                          |
| 7274 | ＡＤＦ                                             | ＡＤＦ                               |
| 7291 | ＢＯＪ　ＡＳ　ＡＧＥＮＴ                          | ＢＯＪ　ＡＳ　ＡＧＥＮＴ            |
| 7301 | ＩＢＲＤ－Ｔ                                       | ＩＢＲＤ－Ｔ                        |
| 7315 | ＩＤＢ                                             | ＩＤＢ                               |
| 7316 | ＩＩＣ                                             | ＩＩＣ                               |
| 7317 | ＭＩＦ                                             | ＭＩＦ                               |
| 7318 | ＭＩＦ－ＩＩ                                      | ＭＩＦ－ＩＩ                       |
| 7319 | ＩＤＢ－ＺＢＡ                                     | ＩＤＢ－ＺＢＡ                       |
| 7320 | ＢＯＪＢＳＡ－１                                   | ＢＯＪＢＳＡ－１                     |
| 7321 | ＢＯＪＢＳＡ－２                                   | ＢＯＪＢＳＡ－２                     |
| 8019 | Hans Capital                                       | 한스종금                            |
| 8022 | Korea Capital                                      | 한국종금                            |
| 8036 | Hyundai Capital                                    | 현대종금                            |
| 8040 | Saehan Capital                                     | 새한종금                            |
| 8053 | Meritz Capital                                     | 메리츠종금                          |
| 8067 | Samyang Capital                                    | 삼양종금                            |
| 8070 | Korea Securities Finance                            | 한국증권금융                        |
| 8084 | Cheongsol Capital                                  | 청솔종금                            |
| 8098 | Hanareum Capital                                   | 한아름종금                          |
| 8201 | Hi Investment & Securities                          | 하이투자증권                        |
| 8214 | Samsung Investment Trust Securities                 | 삼성투자신탁증권                    |
| 8228 | Hannam Investment & Securities                      | 한남투자증권                        |
| 8231 | Joongang Investment Trust                           | 중앙투자신탁                        |
| 8245 | Hana Daetoo Securities                             | 하나대투증권                        |
| 8259 | Korea Investment & Securities                       | 한국투자증권                        |
| 8262 | Kookmin Investment Trust                           | 국민투자신탁                        |
| 8276 | Hanil Investment Trust                             | 한일투자신탁                        |
| 8509 | Daewoo Securities                                   | 대우증권                            |
| 8510 | Hanmac Investment & Securities                      | 한맥투자증권                        |
| 8511 | Nomura Financial Investment                         | 노무라금융투자                      |
| 8512 | Shinhan Financial Investment                        | 신한금융투자                       |
| 8526 | Woori Investment & Securities                       | 우리투자증권                        |
| 8543 | Hyundai Securities                                   | 현대증권                            |
| 8557 | Daishin Securities                                  | 대신증권                            |
| 8560 | Goryeo Securities                                   | 고려증권                            |
| 8574 | SK Securities                                       | 에스케이증권                        |
| 8588 | Yuanta Securities(Dongyang Securities)             | 동양증권                            |
| 8591 | Hanwha Investment & Securities                      | 한화투자증권                        |
| 8598 | Ahnkuk Fire Insurance                               | 안국화재보험                        |
| 8601 | Dongwon Securities                                   | 동원증권                            |
| 8615 | YAMA-TK                                           | ＹＡＭＡ－ＴＫ                       |
| 8629 | DAIWA-TK                                          | ＤＡＩＷＡ－ＴＫ                     |
| 8632 | NWJAP-TK                                          | ＮＷＪＡＰ－ＴＫ                     |
| 8646 | NOMUR-TK                                          | ＮＯＭＵＲ－ＴＫ                     |
| 8650 | DAIW-SP                                           | ＤＡＩＷ－ＳＰ                       |
| 8663 | Hana IB Securities                                   | 하나ＩＢ증권                        |
| 8677 | Shinyoung Securities                                | 신영증권                            |
| 8680 | Yuhwa Securities                                    | 유화증권                            |
| 8694 | Hanyang Securities                                  | 한양증권                            |
| 8704 | Eugene Investment & Securities                      | 유진투자증권                        |
| 8718 | Golden Bridge Investment & Securities               | 골든브릿지투자증권                  |
| 8735 | Bookook Securities                                  | 부국증권                            |
| 8749 | Meritz Capital Securities                           | 메리츠종금증권                      |
| 8766 | Korea Industrial Securities                          | 한국산업증권                        |
| 8770 | Korea Securities Depository                         | 한국예탁결제원                      |
| 8783 | Kyobo Securities                                    | 교보증권                            |
| 8797 | HMC Investment & Securities                         | ＨＭＣ투자증권                      |
| 8807 | Jang Securities                                     | 장은증권                            |
| 8808 | (Former) JP Morgan Securities                       | （구）제이피모간증권                 |
| 8810 | Dongbang Peregrine Securities                      | 동방페레그린증권                    |
| 8824 | BTI Securities                                      | ＢＴＩ증권                          |
| 8838 | KB Investment & Securities                          | ＫＢ투자증권                        |
| 8841 | Construction Securities                             | 건설증권                            |
| 8855 | UBS                                               | 유비에스                            |
| 8869 | Citi Global Markets Securities                      | 씨티글로벌마켓증권                  |
| 8872 | Daiwa Securities                                    | 다이와증권                          |
| 8880 | Korea Securities Depository (Government Bond)      | 한국예탁결제원（국채）               |
| 8888 | Other Institutions                                  | 기타기관                            |
| 8891 | Citi Securities                                     | 시티증권                            |
| 8892 | UBS Securities                                      | 유비에스증권                        |
| 8893 | Kiwoom Securities                                   | 키움증권                            |
| 8894 | Nomura International Securities                     | 노무라인터내셔날증권                |
| 8895 | UBS Securities Limited                              | 유비에스증권리미티드                |
| 8900 | Kyobo Life Insurance                                | 교보생명보험                        |
| 8913 | Dong-A Life Insurance                               | 동아생명보험                        |
| 8927 | Jeil Life Insurance                                 | 제일생명보험                        |
| 8930 | Daehan Fire Insurance                               | 대한화재보험                        |
| 8944 | Reinsurance                                         | 재보험                              |
| 8961 | Hanwha Life Insurance                               | 한화생명보험                        |
| 8975 | Samsung Life Insurance                              | 삼성생명보험                        |
| 8989 | Heungkuk Life Insurance                           | 흥국생명보험                        |
| 8990 | Mirae Asset Life Insurance                        | 미래에셋생명보험                    |
| 8991 | Dongbu Life Insurance                             | 동부생명보험                        |
| 8992 | Dongyang Fire Insurance                           | 동양화재보험                       |
| 8993 | International Fire Insurance                      | 국제화재보험                       |
| 8995 | Jeil Fire Insurance                              | 제일화재보험                       |
| 8996 | Hyundai Fire Insurance                            | 현대화재보험                       |
| 9007 | Samsung Fire Insurance                            | 삼성화재보험                       |
| 9008 | Ssangyong Fire Insurance                          | 쌍용화재보험                       |
| 9009 | LIG Insurance                                    | 엘아이지손해보험                   |
| 9011 | Dongbu Fire Insurance                             | 동부화재보험                       |
| 9012 | Prudential Life Insurance                         | 푸르덴셜생명보험                   |
| 9013 | KDB Life Insurance                               | 케이디비생명보험                   |
| 9014 | NongHyup Life Insurance                           | 농협생명보험                       |
| 9192 | Kyungnam Comprehensive Finance                    | 경남종합금융                       |
| 9202 | Regent Capital                                   | 리젠트종금                         |
| 9216 | LG Capital                                       | 엘지종금                           |
| 9220 | Hansol Comprehensive Finance                      | 한솔종합금융                       |
| 9233 | Daegu Comprehensive Finance                       | 대구종합금융                       |
| 9247 | Hangil Comprehensive Finance                      | 한길종합금융                       |
| 9250 | Hangdo Comprehensive Finance                      | 항도종합금융                       |
| 9264 | Ssangyong Comprehensive Finance                  | 쌍용종합금융                       |
| 9278 | Hyundai Ulsan Capital                            | 현대울산종금                       |
| 9295 | Dongyang Hyundai Capital                          | 동양현대종금                       |
| 9305 | Dongbu Securities                                 | 동부증권                           |
| 9322 | Samsung Securities                                 | 삼성증권                           |
| 9336 | Joongang Capital                                  | 중앙종금                           |
| 9340 | SamSam Comprehensive Finance                      | 삼삼종합금융                       |
| 9353 | Daehan Capital                                   | 대한종금                           |
| 9367 | IlEun Securities                                 | 일은증권                           |
| 9370 | Jeil Comprehensive Finance                        | 제일종합금융                       |
| 9384 | Shinhan Comprehensive Finance                     | 신한종합금융                       |
| 9408 | NH Investment & Securities                        | ＮＨ농협증권                      |
| 9411 | Nara Capital                                     | 나라종금                           |
| 9439 | IM Investment & Securities                        | 아이엠투자증권                     |
| 9442 | Yeongnam Capital                                  | 영남종금                           |
| 9456 | Kumho Capital                                     | 금호종금                           |
| 9460 | Shinsegae Comprehensive Finance                   | 신세계종합금융                     |
| 9473 | Goryeo Comprehensive Finance                      | 고려종합금융                       |
| 9487 | Kyungil Comprehensive Finance                     | 경일종합금융                       |
| 9500 | Korea Minting and Security Printing Corporation    | 한국조폐공사                       |
| 9514 | MOD                                              | ＭＯＤ                             |
| 9528 | ARRMNT                                           | ＡＲＲＭＮＴ                        |
| 9531 | Foreign Exchange Equalization Fund                | 외국환평형기금                    |
| 9532 | NDF                                              | ＮＤＦ                             |
| 9545 | KKBC                                             | ＫＫＢＣ                            |
| 9559 | SMBC                                             | ＳＭＢＣ                            |
| 9562 | ABC                                              | ＡＢＣ                             |
| 9576 | KMBC                                             | ＫＭＢＣ                            |
| 9580 | KFBC                                             | ＫＦＢＣ                            |
| 9593 | KIBC                                             | ＫＩＢＣ                            |
| 9594 | Resolution and Finance Corporation                | 정리금융공사                      |
| 9601 | CLS                                              | ＣＬＳ                             |
| 9603 | Deposit Insurance Corporation                      | 예금보험공사                      |
| 9604 | Korea Development Bank Financial Group            | 한국정책금융공사                  |
| 9616 | National Forestry Cooperative Federation (NFCF)  | 산림조합중앙회                    |
| 9617 | National Credit Union Federation of Korea (NACUFOK) | 신협중앙회                    |
| 9620 | Korean Federation of Community Credit Cooperatives (KFCC) | 새마을금고연합회           |
| 9621 | Korea Federation of Savings Bank( KFSB)          | 상호저축은행중앙회               |
| 9622 | Korea Exchange                                   | 한국거래소                        |
| 9623 | National Agricultural Cooperative Federation (NACF) | 농협중앙회                     |
| 9666 | Credit Management Fund                            | 신용관리기금                      |
| 9670 | National Pension Service                           | 국민연금                          |
| 9706 | Foreign Exchange Equalization Fund (Regular)      | 외국환평형기금（정기）          |
| 9710 | Newedge Financial Securities                       | 뉴엣지파이낸셜증권              |
| 9711 | Barclays Securities                               | 바클레이즈증권                   |
| 9712 | BS Investment Securities                           | ＢＳ투자증권                     |
| 9721 | Seoul Foreign Exchange Brokerage                   | 서울외국환중개                   |
| 9722 | JP Morgan Securities                              | ＪＰ모간증권                     |
| 9723 | (Former) Deutsche Securities                      | （구）도이치증권                |
| 9724 | Mirae Asset Securities                             | 미래에셋증권                     |
| 9725 | Korea RB Securities Brokerage                      | 코리아ＲＢ증권중개               |
| 9726 | E*TRADE Securities                               | 이트레이드증권                   |
| 9727 | Leading Investment Securities                      | 리딩투자증권                     |
| 9728 | ABN AMRO Securities                               | 에이비엔암로증권                 |
| 9729 | MOA Securities Brokerage                           | 모아증권중개                     |
| 9730 | Get More Securities Brokerage                      | 겟모어증권중개                   |
| 9731 | Credit Lyonnais Securities                         | 크레디리요네증권                |
| 9732 | BNG Securities                                     | 비엔지증권                       |
| 9733 | KIDB Bond Brokerage                                | ＫＩＤＢ채권중개                 |
| 9734 | Credit Suisse Securities                           | 크레디트스위스증권              |
| 9735 | Shinhan Life Insurance                             | 신한생명보험                     |
| 9736 | State Street Bank                                 | 스테이트스트리트은행            |
| 9737 | Merrill Lynch Securities                           | 메릴린치증권                     |
| 9738 | Mellat Bank                                       | 멜라트은행                       |
| 9739 | Dongyang Orion Investment Securities               | 동양오리온투자증권              |
| 9740 | Lehman Brothers Securities                         | 리먼브러더스증권                |
| 9741 | Nomura Securities                                  | 노무라증권                       |
| 9742 | SG Securities                                      | 에스지증권                       |
| 9743 | LIG Investment Securities                          | 엘아이지투자증권                |
| 9744 | IBK Investment Securities                          | 아이비케이투자증권              |
| 9745 | KTB Investment Securities                          | 케이티비투자증권                |
| 8989 | Heungkuk Life Insurance                                                       | 흥국생명보험                           |
| 8990 | Mirae Asset Life Insurance                                                    | 미래에셋생명보험                       |
| 8991 | Dongbu Life Insurance                                                         | 동부생명보험                           |
| 8992 | Dongyang Fire Insurance                                                       | 동양화재보험                           |
| 8993 | International Fire Insurance                                                  | 국제화재보험                           |
| 8995 | Jeil Fire Insurance                                                           | 제일화재보험                           |
| 8996 | Hyundai Fire Insurance                                                         | 현대화재보험                           |
| 9007 | Samsung Fire Insurance                                                         | 삼성화재보험                           |
| 9008 | Ssangyong Fire Insurance                                                      | 쌍용화재보험                           |
| 9009 | LIG Insurance                                                                 | 엘아이지손해보험                       |
| 9011 | Dongbu Fire Insurance                                                         | 동부화재보험                           |
| 9012 | Prudential Life Insurance                                                      | 푸르덴셜생명보험                       |
| 9013 | KDB Life Insurance                                                             | 케이디비생명보험                       |
| 9014 | NongHyup Life Insurance                                                       | 농협생명보험                           |
| 9192 | Kyungnam Comprehensive Finance                                                | 경남종합금융                           |
| 9202 | Regent Capital                                                                | 리젠트종금                             |
| 9216 | LG Capital                                                                    | 엘지종금                               |
| 9220 | Hansol Comprehensive Finance                                                  | 한솔종합금융                           |
| 9233 | Daegu Comprehensive Finance                                                   | 대구종합금융                           |
| 9247 | Hangil Comprehensive Finance                                                  | 한길종합금융                           |
| 9250 | Hangdo Comprehensive Finance                                                  | 항도종합금융                           |
| 9264 | Ssangyong Comprehensive Finance                                               | 쌍용종합금융                           |
| 9278 | Hyundai Ulsan Capital                                                         | 현대울산종금                           |
| 9295 | Dongyang Hyundai Capital                                                      | 동양현대종금                           |
| 9305 | Dongbu Securities                                                              | 동부증권                               |
| 9322 | Samsung Securities                                                             | 삼성증권                               |
| 9336 | Joongang Capital                                                              | 중앙종금                               |
| 9340 | SamSam Comprehensive Finance                                                   | 삼삼종합금융                           |
| 9353 | Daehan Capital                                                                | 대한종금                               |
| 9367 | IlEun Securities                                                              | 일은증권                               |
| 9370 | Jeil Comprehensive Finance                                                    | 제일종합금융                           |
| 9384 | Shinhan Comprehensive Finance                                                 | 신한종합금융                           |
| 9408 | NH Investment & Securities                                                    | ＮＨ농협증권                           |
| 9411 | Nara Capital                                                                  | 나라종금                               |
| 9439 | IM Investment & Securities                                                    | 아이엠투자증권                         |
| 9442 | Yeongnam Capital                                                              | 영남종금                               |
| 9456 | Kumho Capital                                                                 | 금호종금                               |
| 9460 | Shinsegae Comprehensive Finance                                               | 신세계종합금융                         |
| 9473 | Goryeo Comprehensive Finance                                                  | 고려종합금융                           |
| 9487 | Kyungil Comprehensive Finance                                                 | 경일종합금융                           |
| 9500 | Korea Minting and Security Printing Corporation                                | 한국조폐공사                           |
| 9514 | MOD                                                                           | ＭＯＤ                                 |
| 9528 | ARRMNT                                                                       | ＡＲＲＭＮＴ                            |
| 9531 | Foreign Exchange Equalization Fund                                            | 외국환평형기금                         |
| 9532 | NDF                                                                           | ＮＤＦ                                 |
| 9545 | KKBC                                                                          | ＫＫＢＣ                                 |
| 9559 | SMBC                                                                          | ＳＭＢＣ                                 |
| 9562 | ABC                                                                           | ＡＢＣ                                 |
| 9576 | KMBC                                                                          | ＫＭＢＣ                                 |
| 9580 | KFBC                                                                          | ＫＦＢＣ                                 |
| 9593 | KIBC                                                                          | ＫＩＢＣ                                 |
| 9594 | Resolution and Finance Corporation                                            | 정리금융공사                           |
| 9601 | CLS                                                                           | ＣＬＳ                                 |
| 9603 | Deposit Insurance Corporation                                                  | 예금보험공사                           |
| 9604 | Korea Development Bank Financial Group                                         | 한국정책금융공사                       |
| 9616 | National Forestry Cooperative Federation (NFCF)                               | 산림조합중앙회                         |
| 9617 | National Credit Union Federation of Korea (NACUFOK)                          | 신협중앙회                             |
| 9620 | Korean Federation of Community Credit Cooperatives (KFCC)                    | 새마을금고연합회                       |
| 9621 | Korea Federation of Savings Bank( KFSB)                                      | 상호저축은행중앙회                     |
| 9622 | Korea Exchange                                                                | 한국거래소                             |
| 9623 | National Agricultural Cooperative Federation (NACF)                          | 농협중앙회                             |
| 9666 | Credit Management Fund                                                         | 신용관리기금                           |
| 9670 | National Pension Service                                                       | 국민연금                               |
| 9706 | Foreign Exchange Equalization Fund (Regular)                                  | 외국환평형기금（정기）                 |
| 9710 | Newedge Financial Securities                                                   | 뉴엣지파이낸셜증권                     |
| 9711 | Barclays Securities                                                            | 바클레이즈증권                         |
| 9712 | BS Investment Securities                                                       | ＢＳ투자증권                          |
| 9721 | Seoul Foreign Exchange Brokerage                                               | 서울외국환중개                         |
| 9722 | JP Morgan Securities                                                           | ＪＰ모간증권                           |
| 9723 | (Former) Deutsche Securities                                                  | （구）도이치증권                       |
| 9724 | Mirae Asset Securities                                                         | 미래에셋증권                           |
| 9725 | Korea RB Securities Brokerage                                                  | 코리아ＲＢ증권중개                     |
| 9726 | E*TRADE Securities                                                             | 이트레이드증권                         |
| 9727 | Leading Investment Securities                                                  | 리딩투자증권                           |
| 9728 | ABN AMRO Securities                                                           | 에이비엔암로증권                       |
| 9729 | MOA Securities Brokerage                                                       | 모아증권중개                           |
| 9730 | Get More Securities Brokerage                                                  | 겟모어증권중개                         |
| 9731 | Credit Lyonnais Securities                                                    | 크레디리요네증권                       |
| 9732 | BNG Securities                                                                 | 비엔지증권                             |
| 9733 | KIDB Bond Brokerage                                                            | ＫＩＤＢ채권중개                       |
| 9734 | Credit Suisse Securities                                                       | 크레디트스위스증권                     |
| 9735 | Shinhan Life Insurance                                                         | 신한생명보험                           |
| 9736 | State Street Bank                                                             | 스테이트스트리트은행                   |
| 9737 | Merrill Lynch Securities                                                       | 메릴린치증권                           |
| 9738 | Mellat Bank                                                                   | 멜라트은행                             |
| 9739 | Dongyang Orion Investment Securities                                           | 동양오리온투자증권                     |
| 9740 | Lehman Brothers Securities                                                     | 리먼브러더스증권                       |
| 9741 | Nomura Securities                                                              | 노무라증권                             |
| 9742 | SG Securities                                                                  | 에스지증권                             |
| 9743 | LIG Investment Securities                                                      | 엘아이지투자증권                       |
| 9744 | IBK Investment Securities                                                      | 아이비케이투자증권                     |
| 9745 | KTB Investment Securities                                                      | 케이티비투자증권                       |
| 9746 | Taurus Investment Securities                                                    | 토러스투자증권                         |
| 9747 | Korea Standard Chartered Securities                                            | 한국스탠다드차타드증권                 |
| 9748 | Apple Investment Securities Brokerage                                           | 애플투자증권중개                       |
| 9749 | Baro Investment Securities                                                     | 바로투자증권                           |
| 9758 | Daiwa Securities Korea                                                         | 다이와증권코리아                       |
| 9801 | Heungkuk Securities Brokerage Co.                                             | 흥국증권중개（주）                     |
| 9809 | Korea Money Brokerage                                                          | 한국자금중개                           |
| 9811 | BGC Capital Markets Foreign Exchange Brokerage (BGC Capital Markets)          | 비지시캐피탈마켓외국환중개(비지시캐피탈마켓) |
| 9906 | Samsung Life Asset Management                                                  | 삼성생명투신운용                       |
| 9907 | Kyobo Asset Management                                                         | 교보투신운용                           |
| 9908 | Daishin Asset Management                                                       | 대신투신운용                           |
| 9909 | Dongbu Asset Management                                                        | 동부투신운용                           |
| 9910 | Dongwon Asset Management                                                       | 동원투신운용                           |
| 9911 | Samsung Asset Management                                                        | 삼성투신운용                           |
| 9912 | Woori Asset Management                                                         | 우리투신운용                           |
| 9913 | KDB Asset Management                                                           | 산은자산운용                           |
| 9914 | Shinyoung Asset Management                                                     | 신영투신운용                           |
| 9915 | Korea Post                                                                     | 우체국                                 |
| 9916 | Shinhan BNP Paribas Asset Management                                          | 신한ＢＮＰ파리바투신운용                |
| 9917 | Franklin Templeton Asset Management                                            | 프랭클린템플턴투신운용                  |
| 9918 | Foreign Exchange Commerz Asset Management                                      | 외환코메르쯔투신운용                   |
| 9919 | Choheung Asset Management                                                      | 조흥투신운용                           |
| 9920 | Chohung Asset Management                                                       | 주은투신운용                           |
| 9921 | Hanil Asset Management                                                          | 한일투신운용                           |
| 9922 | Hanwha Asset Management                                                        | 한화투신운용                           |
| 9923 | LG Asset Management                                                             | ＬＧ투신운용                           |
| 9924 | SK Asset Management                                                             | ＳＫ투신운용                           |
| 9925 | NongHyup CA Asset Management                                                   | 농협ＣＡ투신운용                       |
| 9926 | Hana UBS Asset Management                                                       | 하나ＵＢＳ자산운용                     |
| 9927 | Deutsche Asset Management                                                       | 도이치투신운용                         |
| 9928 | Dongyang Asset Management                                                      | 동양투신운용                           |
| 9929 | Landmark Asset Management                                                       | 랜드마크투신운용                       |
| 9930 | Mirae Asset Asset Management                                                   | 미래에셋투신운용                       |
| 9931 | Sejong Asset Management                                                         | 세종투신운용                           |
| 9932 | Schroders Asset Management                                                      | 슈로더투신운용                         |
| 9933 | Ai Asset Management                                                             | 아이투신운용                           |
| 9934 | Jeil Asset Management                                                           | 제일투신운용                           |
| 9936 | Taekwang Asset Management                                                      | 태광투신운용                           |
| 9937 | Hana Allianz Asset Management                                                   | 하나알리안츠투신운용                   |
| 9938 | Korea Investment Asset Management                                               | 한국투신운용                           |
| 9939 | Hyundai Asset Management                                                        | 현대투신운용                           |
| 9940 | PCA Asset Management                                                            | ＰＣＡ투신운용                         |
| 9941 | Kansas Asset Management                                                         | 칸서스자산운용                         |
| 9942 | Macquarie IMM Asset Management                                                  | 맥쿼리ＩＭＭ자산운용（주）               |
| 9943 | IBK SG Asset Management                                                         | 기은에스지자산운용（주）                |
| 9944 | Plus Asset Management                                                           | 플러스자산운용（주）                   |
| 9945 | Wise Asset Management                                                           | 와이즈에셋자산운용（주）                |
| 9946 | Phoenix Asset Management                                                        | 피닉스자산운용                         |
| 9947 | Alpha Asset Management                                                          | 알파에셋자산운용（주）                  |
| 9948 | Midas Asset Management                                                          | 마이다스에셋자산운용（주）              |
| 9949 | Golden Bridge Asset Management                                                  | 골든브릿지자산운용（주）                |
| 9950 | Sey Asset Management                                                            | 세이에셋코리아자산운용（주）            |
| 9951 | Korea Value Asset Management                                                    | 한국밸류자산운용（주）                  |
| 9952 | Daol Real Estate Asset Management                                              | 다올부동산자산운용（주）                |
| 9953 | Good & Rich Asset Management                                                   | 굿앤리치자산운용（주）                  |
| 9954 | KTB Asset Management                                                            | ＫＴＢ자산운용（주）                    |
| 9955 | Yurie Asset Management                                                          | 유리자산운용（주）                      |
| 9956 | KB Asset Management                                                             | 케이비자산운용（주）                    |
| 9957 | ING Asset Management                                                            | ＩＮＧ자산운용（주）                    |
| 9958 | Kyobo AXA Asset Management                                                     | 교보악사자산운용                        |
| 9959 | GS Asset Management                                                             | ＧＳ자산운용                            |
| 9969 | Kookmin Asset Management                                                       | 국민투신운용                            |
| 9970 | Korea Housing Finance Corporation                                               | 한국주택금융공사                       |
| 9971 | Hyundai Investment & Securities                                                 | 현대인베스트먼트자산운용                |
| 9972 | Asset Plus Asset Management                                                    | 에셋플러스자산운용                      |
| 9973 | Meritz Asset Management                                                         | 메리츠자산운용                         |
| A001 | Korea Financial Telecommunications & Clearings Institute (KFTC)                | 금융결제원                            |


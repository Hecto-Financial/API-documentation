## Exchange Rate Inquiry (A1)

### Address

Testbed: 252.169.22  
Production Environment: gw.settlebank.co.kr  
(IP: 61.252.169.23)

### Request (Merchant -> Hecto Financial)

#### Columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter Name | Description                        | Type (Length) | Required | Example     |
|----------------|------------------------------------|---------------|----------|-------------|
| mchtId         | Merchant ID                        | AN(8)         | ●        | "midtest"   |
| outStatCd      | Transaction status code            | AN(4)         | ●        | Success: 0021 Fail: 0031 |
| outRsltCd      | Result code                        | AN(4)         | ●        | ST08 : If it is an account that is already registered, send the information of the registered account |
| outRsltMsg     | Result message                     | AN(100)       | ●        | “Processed normally.” |
| bankCd         | Bank code                          | N(3)          | ○        | “011”       |
| bankNm         | Bank name                          | AN(45)        | ○        | “Kookmin Bank” |
| custAcntNo     | Account number                     | AN(15)        | ○        | “234*********123” |
| useStDt        | Recurring payment start date       | AN(8)         | ○        | "20210830"  |
| useEdDt        | Recurring payment end date         | AN(8)         | ○        | “20991231”  |
| rglPmtAmt      | Amount of recurring payment        | N(13)         | ○        | “1000”      |
| pmtAcmAmt      | Accumulated payment amount         | N(13)         | ○        | “5000”      |
| pmtAcmOrd      | Accumulated payment round          | N(13)         | ○        | “5”         |
| unregDt        | Cancellation date                  | AN(14)        | ○        | "20210830080000" |
| unregStatCd    | Cancellation code                  | AN(10)        | ○        | EXPIRE: End of Period FAIL: Payment failed CANCEL: Merchant cancellation. ADMIN: Administrator cancellation |

### Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter       | Name                                | Description                                            | Type (Length) | Required | Example |
|-----------------|-------------------------------------|--------------------------------------------------------|---------------|----------|---------|
| outStatCd       | Transaction status                  | Transaction status code (success/fail)                 | AN(4)         | ●        | Success: 0021 Fail: 0031 |
| outRsltCd       | Result code                         | Refer to reject code table                             | AN(4)         | ●        | ST08 : If it is an account that is already registered, send the information of the registered account |
| outRsltMsg      | Result message                      | When an error occurs, a message on error is sent       | AN(100)       | ●        | “Processed normally.” |
| trdNo           | Transaction number                  | Hecto Financial transaction number                     | AN(40)        | ○        | “SFP_FIRM12345678901234567890” |
| rglPmtProcStatCd | Recurring payment processing status | Response of recurring payment processing status        | A(1)          | ○        | R : Recurring payment reprocessing is required C : Recurring payment cancellation is required |
| trdAmt          | Transaction amount                  | Withdrawn amount (transaction amount)                  | N(13)         | ○        | “1000” |
| svcDivCd        | Services Used at Payment            | Service paid (withdraw transfer)                       | N(4)          | ○        | 1 : Firm banking |

## Virtual Account Management API (/pyag/v1/fxVaccount)

### Address
**Testbed:** https://[Address]/pyag/v1/fxVaccount  
**Production:** https://[Address]/pyag/v1/fxVaccount

### Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter Name | Description                        | Type (Length) | Required | Example                                    |
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
https://[Address]/pyag/v1/fxVaccount
PostData: mchtId=mid_test&trdType=1&validTo=20501231235959&accountNm=HECTO


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

| Parameter Name | Description                        | Type (Length) | Required | Example                                    |
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




# Exchange, Remittance (B1)

## /pyag/v1/fxTrans

### Request (Merchant -> Hecto Financial)

| Parameter Name | Description                        | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Description |
|----------------|------------------------------------|-----------|-------------|-------------|-------------|-------------|
| mchtId         | Merchant ID                        | 12        | ●           | ●           | ●           |             |
| mchtTrdNo      | Merchant Order Number              | 100       | ●           | ●           | ●           | *At least unique per month |
| encCd          | Encryption Code                    | 2         | ●           | ●           | ●           | 23: AES256ECB+BASE64 |
| trdDt          | Transaction Date                   | 8         | ○           | ○           | ○           | For response, changed to Hecto Financial processed date |
| trdTm          | Transaction Time                   | 6         | ○           | ○           | ○           | For response, changed to Hecto Financial processed time |
| svcDivCd       | Transaction Classification         | 12        | ●           | ●           | ●           | FX:Exchange, RMT:Remittance, FXRMT:Exchange+Remittance |
| rateQuoteId    | Exchange Rate ID                   | 32        | ●           |             | ●           |             |
| sellCrcCd      | Selling Currency                   | 12        | ●           |             | ●           |             |
| sellAmt        | Selling Amount                     | 32        | ○           |             | ○           | Amount requested for exchange(Selling currency standard), just one between selling/buying. *Url encoding after encryption is required. |
| buyCrcCd       | Buying Currency                    | 12        | ●           |             | ●           | Required for foreign exchange |
| buyAmt         | Buying Amount                      | 32        | ○           |             | ○           | Amount requested for exchange(Buying currency standard) *Url encoding after encryption is required. |
| remitCat       | Remittance Classification          | 12        |             | ●           | ●           | 1:Overseas, 2:Domestic, 3:Same Bank |
| remitAmt       | Remittance Amount                  | 32        |             | ●           | ●           | In the case of USD, upto two decimal places allowed *Url encoding after encryption is required. |
| remitCrcCd     | Remittance Currency                | 12        |             | ●           | ●           | USD, KRW *For KRW remittance without foreign exchange, real-time remittance is executed |
| rcvrNm         | Receiver's English Name            | 35        |             | ●           | ●           |             |
| rcvrNmKr       | Receiver's Korean Name             | 35        |             | ◐           | ◐           | Url encoding required. Required if the nationality of the receiver is 'KR' |
| rcvrBirthDt    | Receiver's Date of Birth           | 32        |             | ●           | ●           | *Url encoding after encryption is required. |
| rcvrLiveNtnCd  | Receiver's Country of Residence    | 2         |             | ●           | ●           | 2-digit ISO country code |
| rcvrNtnCd      | Receiver's Nationality             | 2         |             | ●           | ●           | 2-digit ISO country code |
| rcvrAddr       | Receiver's Address                 | 135       |             | ◐           | ◐           | Omit for domestic KRW remittance |
| rcvrBankCd     | Receiving Bank's Code              | 11        |             | ●           | ●           | SWIFT BIC code For domestic KRW remittance, 3-digit domestic bank code |
| rcvrBankNm     | Receiving Bank's Name              | 128       |             | ◐           | ◐           | Omit for domestic KRW remittance |
| rcvrBankAddr   | Receiving Bank's Address           | 135       |             | ◐           | ◐           | Omit for domestic KRW remittance |
| rcvrAcntNo     | Receiving Account Number           | 64        |             | ●           | ●           | *Url encoding after encryption is required |
| remitRsnCd     | Remittance Reason Code             | 5         |             | ◐           | ◐           | Omit for domestic KRW remittance *Attached |
| rcvrAcntSumry  | Receiving Account Remark           | 32        |             | ◐           | ◐           | Valid only for KRW domestic remittance. Valid until maximum 7th letter only (Url encoding) |
| invFileNm      | Invoice File Name                  | 32        |             | ◐           | ◐           | File name. (Korean letter and special character not allowed) Allowed extensions: jpg, png, jpeg * Required for domestic -> overseas remittance (excluding remittance to one's own account) |
| invFileData    | Invoice File Data                  | -         |             | ◐           | ◐           | BASE64 encoding then sent *For size, only below 100KB is allowed. |

* Excluding 'Receiver's Korean Name' and 'Receiving Account Remark', only English capital letters are allowed.

### Request Sample

**Issuance**

```
https://[Address]/pyag/v1/fxTrans
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&svcDivCd=FXRMT&sellCrcCd=KRW&buyCrcCd=USD&buyAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCat=1&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCrcCd=USD&rcvrNm=JACKSON&rcvrNmKr=JACKSON&rcvrBirthDt=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrLiveNtnCd=US&rcvrNtnCd=US&rcvrBankCd=CITIUS33&rcvrBankAddr=24STREET NEWYORK CITY US&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==&remitRsnCd=10101&invFileNm=INVOID_2023.JPG
```


### Response (Hecto Financial -> Merchant)

| Parameter Name | Description                        | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Description |
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


# Result Inquiry (V1)

## /pyag/v1/fxTrans

### Request (Merchant -> Hecto Financial)

| Parameter Name  | Description                     | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Description                                                                                         |
|-----------------|---------------------------------|----------|--------------|---------------|-----------------|-----------------------------------------------------------------------------------------------------|
| mchtId          | Merchant ID                     | 12       | ●            | ●             | ●               |                                                                                                     |
| mchtTrdNo       | Merchant Order Number           | 100      | ●            | ●             | ●               | *At least unique per month                                                                          |
| encCd           | Encryption Code                 | 2        | ●            | ●             | ●               | 23: AES256ECB+BASE64                                                                                |
| trdDt           | Transaction Date                | 8        | ○            | ○             | ○               | For response, changed to Hecto Financial processed date                                             |
| trdTm           | Transaction Time                | 6        | ○            | ○             | ○               | For response, changed to Hecto Financial processed time                                             |
| svcDivCd        | Transaction Classification      | 12       | ●            | ●             | ●               | FX:Exchange, RMT:Remittance, FXRMT:Exchange+Remittance                                              |
| rateQuoteId     | Exchange Rate ID                | 32       | ●            |               | ●               |                                                                                                     |
| sellCrcCd       | Selling Currency                | 12       | ●            |               | ●               |                                                                                                     |
| sellAmt         | Selling Amount                  | 32       | ○            |               | ○               | Amount requested for exchange(Selling currency standard), just one between selling/buying. *Url encoding after encryption is required. |
| buyCrcCd        | Buying Currency                 | 12       | ●            |               | ●               | Required for foreign exchange                                                                       |
| buyAmt          | Buying Amount                   | 32       | ○            |               | ○               | Amount requested for exchange(Buying currency standard) *Url encoding after encryption is required. |
| remitCat        | Remittance Classification       | 12       |              | ●             | ●               | 1:Overseas, 2:Domestic, 3:Same Bank                                                                 |
| remitAmt        | Remittance Amount               | 32       |              | ●             | ●               | In the case of USD, upto two decimal places allowed *Url encoding after encryption is required.     |
| remitCrcCd      | Remittance Currency             | 12       |              | ●             | ●               | USD, KRW *For KRW remittance without foreign exchange, real-time remittance is executed             |
| rcvrNm          | Receiver's English Name         | 35       |              | ●             | ●               |                                                                                                     |
| rcvrNmKr        | Receiver's Korean Name          | 35       |              | ◐             | ◐               | Url encoding required. Required if the nationality of the receiver is 'KR'                          |
| rcvrBirthDt     | Receiver's Date of Birth        | 32       |              | ●             | ●               | *Url encoding after encryption is required.                                                         |
| rcvrLiveNtnCd   | Receiver's Country of Residence | 2        |              | ●             | ●               | 2-digit ISO country code                                                                            |
| rcvrNtnCd       | Receiver's Nationality          | 2        |              | ●             | ●               | 2-digit ISO country code                                                                            |
| rcvrAddr        | Receiver's Address              | 135      |              | ◐             | ◐               | Omit for domestic KRW remittance                                                                    |
| rcvrBankCd      | Receiving Bank's Code           | 11       |              | ●             | ●               | SWIFT BIC code For domestic KRW remittance, 3-digit domestic bank code                              |
| rcvrBankNm      | Receiving Bank's Name           | 128      |              | ◐             | ◐               | Omit for domestic KRW remittance                                                                    |
| rcvrBankAddr    | Receiving Bank's Address        | 135      |              | ◐             | ◐               | Omit for domestic KRW remittance                                                                    |
| rcvrAcntNo      | Receiving Account Number        | 64       |              | ●             | ●               | *Url encoding after encryption is required                                                          |
| remitRsnCd      | Remittance Reason Code          | 5        |              | ◐             | ◐               | Omit for domestic KRW remittance *Attached                                                          |
| rcvrAcntSumry   | Receiving Account Remark        | 32       |              | ◐             | ◐               | Valid only for KRW domestic remittance. Valid until maximum 7th letter only (Url encoding)          |
| invFileNm       | Invoice File Name               | 32       |              | ◐             | ◐               | File name. (Korean letter and special character not allowed) Allowed extensions: jpg, png, jpeg * Required for domestic -> overseas remittance (excluding remittance to one's own account) |
| invFileData     | Invoice File Data               | -        |              | ◐             | ◐               | BASE64 encoding then sent *For size, only below 100KB is allowed.                                   |

* Excluding 'Receiver's Korean Name' and 'Receiving Account Remark', only English capital letters are allowed.

### Request Sample

**Issuance**  
```

https://[Address]/pyag/v1/fxTrans
PostData: mchtId=mid_test&mchtTrdNo=1234567890&encCd=23&svcDivCd=FXRMT&sellCrcCd=KRW&buyCrcCd=USD&buyAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCat=1&remitAmt=Gzv1ziVXlhyFS0EYMbHvqA==&remitCrcCd=USD&rcvrNm=JACKSON&rcvrNmKr=JACKSON&rcvrBirthDt=Gzv1ziVXlhyFS0EYMbHvqA==&rcvrLiveNtnCd=US&rcvrNtnCd=US&rcvrBankCd=CITIUS33&rcvrBankAddr=24STREET NEWYORK CITY US&rcvrAcntNo=Gzv1ziVXlhyFS0EYMbHvqA==&remitRsnCd=10101&invFileNm=INVOID_2023.JPG

```


### Response (Hecto Financial -> Merchant)

| Parameter Name  | Description                     | Max. Len | Mandatory FX | Mandatory RMT | Mandatory FXRMT | Description                                                                                         |
|-----------------|---------------------------------|----------|--------------|---------------|-----------------|-----------------------------------------------------------------------------------------------------|
| mchtId          | Merchant ID                     | 12       | ●            | ●             | ●               |                                                                                                     |
| mchtTrdNo       | Merchant Order Number           | 100      | ●            | ●             | ●               | *At least unique per month                                                                          |
| encCd           | Encryption Code                 | 2        | ●            | ●             | ●               | 23: AES256ECB+BASE64                                                                                |
| trdNo           | Transaction Number              | 40       | ●            | ●             | ●               | Hecto Financial generated transaction number, valid for response only                               |
| trdDt           | Transaction Date                | 8        | ●            | ●             | ●               |                                                                                                     |
| trdTm           | Transaction Time                | 6        | ●            | ●             | ●               |                                                                                                     |
| outStatCd       | Transaction Status              | 4        | ●            | ●             | ●               | 0021: Success, 0031: Failure                                                                        |
| outRsltCd       | Response Result                 | 8        | ●            | ●             | ●               | 0000: Success, Others: Refer to Result Code sheet                                                   |
| outRsltMsg      | Response Message                | 200      | ●            | ●             | ●               |                                                                                                     |
| svcDivCd        | Transaction Classification      | 12       | ●            | ●             | ●               | FX:Foreign Exchange, RMT:Remittance, FXRMT:Foreign Exchange+Remittance                              |
| rateQuoteId     | Exchange Rate ID                | 32       | ●            |               | ●               |                                                                                                     |
| sellCrcCd       | Selling Currency                | 12       | ●            |               | ●               |                                                                                                     |
| sellAmt         | Selling Amount                  | 32       | ●            |               | ●               | Amount requested for exchange(Selling currency standard), just one between selling/buying. *Encryption |
| buyCrcCd        | Buying Currency                 | 12       | ●            |               | ●               |                                                                                                     |
| buyAmt          | Buying Amount                   | 32       | ●            |               | ●               | Amount requested for exchange(Buying currency standard) *Encryption                                  |
| remitCat        | Remittance Classification       | 12       |              | ●             | ●               | 1:Overseas, 2:Domestic, 3:Same Bank                                                                 |
| remitAmt        | Remittance Amount               | 32       |              | ●             | ●               | In the case of USD, upto two decimal places allowed *Encryption                                     |


# Balance Inquiry (V2)

## Request (Merchant -> Hecto Financial)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |
| trdNo     | Transaction Number | 40  | ●         |       |
| trdDt     | Original Transaction Date | 8 | ● |       |

## Request Sample

```
https://[Address]/pyag/v1/fxBalance
PostData: mchtId=mid_test&trdNo=1234567890&trdDt=20230920
```


## Response (Hecto Financial -> Merchant)

| PRMTR_NM  | PRMTR_EXPL   | Max. Len | Mandatory | Desc. |
|-----------|--------------|-----------|-----------|-------|
| mchtId    | Merchant ID  | 12        | ●         |       |
| trdNo     | Transaction Number | 40  | ●         |       |
| trdDt     | Transaction Date | 8     | ●         |       |
| trdTm     | Transaction Time | 6     | ●         |       |
| outStatCd | Transaction Status | 4   | ●         | 0021: Success, 0031: Failure |
| outRsltCd | Response Result | 8      | ●         | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg | Response Message | 200  | ●         |       |
| balance   | Balance      | 32        | ●         | Balance after inquiry. KRW |

## Response Sample (Success)

```json
{
  "mchtId": "mid_test",
  "trdNo": "20230920HF1234",
  "trdDt": "20230920",
  "trdTm": "120000",
  "outStatCd": "0021",
  "outRsltCd": "0000",
  "outRsltMsg": "Normal Processing",
  "balance": "120000"
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
| trdDt     | Transaction Date | 8     | ●         | Inquiry Date |
| trdTm     | Transaction Time | 6     | ●         | Inquiry Time |
| outStatCd | Transaction Status | 4   | ●         | 0021: Success, 0031: Failure |
| outRsltCd | Response Result | 8      | ●         | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg | Response Message | 200  | ●         |       |
| status    | Status       | 12        | ●         | 00:Request Data Error, 99: Undefined Status, 10:Exchange Pending, 11:Exchange Success, 12:Exchange Failure, 13:Exchange Cancellation, 20:Pending Remittance, 21:Remittance Success, 22:Remittance Failure, 23:Remittance Cancellation, 29: Processing Remittance |

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
  "status": "11"
}
```
## Status Description
00: (Request Data Error) Exchange and remittance cannot be processed
99: (Undefined) Internal error
10: (Exchange Pending) Merchant has made the exchange request normally, it is in the status of before processing the exchange
11: (Exchange Success) Exchange process success.
12: (Exchange Failure) Exchange process failure, can retry automatically to change to Exchange Success
13: (Exchange Cancellation) Merchant cancelled the exchange
20: (Pending Remittance) Pending remittance status. If requested with exchange, after exchange success, pending remittance status
29: (Processing Remittance) Hecto Financial → Domestic Bank remittance request completed. Before processing Domestic Bank → Overseas Bank.
21: (Remittance Success) Domestic Bank → Overseas Bank remittance request completed, in the status in which remittance failure can occur later on in the Overseas Bank.
22: (Remittance Failure) Hecto Financial → Domestic Bank failure
24: (Remittance Failure) After Hecto Financial → Domestic Bank success, Domestic Bank → Overseas Bank failure
23: (Remittance Cancellation) Remittance cancelled due to merchant or Hecto's internal policy



# Deposit Notification

## Request (Hecto Financial -> Merchant)

### Start IP (Hecto Financial Notification Server IP)
Testbed: 61.252.169.22  
Production Environment: Provided by the merchant side, register on merchant's server beforehand.

### End Address (Merchant Notification API URL)
Provided by the merchant side, register on merchant's server beforehand.

### Note
Only deposit notifications inputted through Hecto Financial's server will be notified.

## Request Sample

### Request (Hecto Financial -> Merchant)

| PRMTR_NM | PRMTR_EXPL | Max. Len | Mandatory | Desc. |
|----------|------------|----------|-----------|-------|
| mchtId   | Merchant ID | 12       | ●         |       |
| trdNo    | Transaction Number | 40 | ●         |       |
| trdDt    | Transaction Date | 8   | ●         |       |
| trdTm    | Transaction Time | 6   | ●         |       |
| outStatCd | Transaction Status | 4 | ●         | 0021: Success, 0031: Failure |
| outRsltCd | Response Result | 8  | ●         | 0000: Success, Others: Refer to Result Code sheet |
| outRsltMsg | Response Message | 200 | ●         |       |
| balance  | Balance      | 32      | ●         | Balance after notification. KRW |

## Response Sample (Success)

```json
{
  "mchtId": "mid_test",
  "trdNo": "20230920HF1234",
  "trdDt": "20230920",
  "trdTm": "120000",
  "outStatCd": "0021",
  "outRsltCd": "0000",
  "outRsltMsg": "Normal Processing",
  "balance": "120000"
}
```

# Result Code

| Result Code | Message                        | Revision Date |
|-------------|--------------------------------|---------------|
| 0000        | Normal processing              | 2023.09.18    |
| PY11        | Bank maintenance time          | 2023.09.18    |
| PY12        | Insufficient deposit balance   | 2023.09.18    |
| PY21        | No receiving account           | 2023.09.18    |
| PY22        | Receiver's bank maintenance time | 2023.09.18    |
| PY23        | Withdrawal limit exceeded      | 2023.09.18    |
| PY24        | Sender account does not exist  | 2023.09.18    |
| PY25        | Incorrect password             | 2023.09.18    |
| PY26        | Insufficient balance in sender's account | 2023.09.18 |
| PY31        | Remittance request rejected    | 2023.09.18    |
| PY32        | Remittance limit exceeded      | 2023.09.18    |
| PY33        | Exceed remittance amount limit | 2023.09.18    |
| PY34        | Sender &#8203;:citation[oaicite:0]{index=0}&#8203;




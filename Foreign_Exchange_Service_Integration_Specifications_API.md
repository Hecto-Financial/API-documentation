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
|https://[Address]/pyag/v1/fxVaccount|
|PostData: mchtId=mid_test&trdType=1&validTo=20501231235959&accountNm=HECTO|


**Terminate**  

|https://[Address]/pyag/v1/fxVaccount|
|PostData: mchtId=mid_test&trdType=2&bankCd=023&vaccountNo=1234567890|


**Change**  

|https://[Address]/pyag/v1/fxVaccount|
|PostData: mchtId=mid_test&trdType=2&bankCd=023&vaccountNo=1234567890&accountNm=NEW_HECTO|


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






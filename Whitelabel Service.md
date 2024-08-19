# White Label Service WEB API

## Contents

1. [Outline](#outline)  
   1.1 [Purpose](#purpose)  
   1.2 [Target](#target)  
   1.3 [Document Specification](#document-specification)  
   1.4 [Integration Process](#integration-process)
   
2. [Standard Payment Window (UI) Integration](#standard-payment-window-ui-integration)  
   2.1 [Summary](#summary)  
   2.2 [Caution](#caution)  
   2.3 [Payment Window URI](#payment-window-uri)  
   2.4 [Request and Response Headers](#request-and-response-headers)  
   2.5 [Request Parameter Verification](#request-parameter-verification)
   
3. [API Service Integration (Non-UI)](#api-service-integration-non-ui)  
   3.1 [Summary](#summary-1)  
   3.2 [API URI](#api-uri)  
   3.3 [Request and Response Headers](#request-and-response-headers-1)  
   3.4 [JSON Request Data Example](#json-request-data-example)
   
4. [Integration Server](#integration-server)  
   4.1 [Server IP Address](#server-ip-address)
   
5. [Crucial Information Security](#crucial-information-security)  
   5.1 [Personal Information and Crucial Information Encryption / Decryption](#personal-information-and-crucial-information-encryption--decryption)  
   5.2 [Personal Information Encryption Key](#personal-information-encryption-key)  
   5.3 [Forgery Prevention Algorithm](#forgery-prevention-algorithm)  
   5.4 [Hash Generation Authentication Key](#hash-generation-authentication-key)
   
6. [Payment Window Invoke (UI)](#payment-window-invoke-ui)  
   6.1 [Caution](#caution-1)  
   6.2 [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial)  
   6.3 [Request Parameter Hash Code](#request-parameter-hash-code)  
   6.4 [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant)
   
7. [Payment API (Non-UI)](#payment-api-non-ui)  
   7.1 [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-1)  
   7.2 [Request Parameter Hash Code](#request-parameter-hash-code-1)  
   7.3 [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-1)  
   7.4 [Response Parameter Hash Code](#response-parameter-hash-code)
   
8. [Cancellation API (Non-UI)](#cancellation-api-non-ui)  
   8.1 [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-2)  
   8.2 [Request Parameter Hash Code](#request-parameter-hash-code-2)  
   8.3 [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-2)  
   8.4 [Request Parameter Hash Code](#request-parameter-hash-code-3)
   
9. [Others](#others)  
   9.1 [Table of Reject Codes](#table-of-reject-codes)  
   9.2 [Financial Institution Identifiers](#financial-institution-identifiers)  
   9.3 [Bank Regular Maintenance Time](#bank-regular-maintenance-time)

---

## 1.Outline

### 1.1 Purpose

This document is intended to provide technical understanding and define detailed specifications for White Label service integration development service provided by Hecto Financial.

### 1.2 Target

The target for this document is the developer of the client that will execute payment through Hecto Financial’s White Label service.

### 1.3 Document Specification

The following is a general description of the integration referred to in this document.

- Required fields in the request/response parameters use symbol '●' and selected fields use symbol '○'.
- Request / response parameters’ data type are as follows:
  - N: Number type string
  - A: Alphabet type string
  - H: Korean letter type string
- The length of the request / response parameters is based on the plain text UTF-8 encoded value (Byte).

### 1.4 Integration Process

Refer to "6. Invoke Payment Window (UI)" and do number 1 authentication request UI integration invoke. According to the responded result, refer to “7. Payment API (Non-UI)” and do number 2 payment request API (NON-UI) integration invoke to proceed with payment.

## 2. Standard Payment Window (UI) Integration

### Summary

- Check request URI per function. (Refer to [2.3 API URI](#payment-window-uri))
- Check request parameter per function set the request parameter and request in POST method.
- Personal/sensitive information-related parameter should be encrypted. (Refer to [5. Crucial Information Security](#crucial-information-security)).
- Should open Hecto Financial White Label payment window and then proceed with the payment.
- When the payment is completed, the result (response parameter) is returned to the URL set in the request parameter `nextUrl`.
- If the user forcibly exits the payment window during payment (payment window ‘X’ button), the result (response parameter) is returned to the URL set in the `cancUrl` parameter.

### Caution

- It only supports browsers Chrome and Edge. It may not function properly on other browsers.
- Merchants are responsible for any costs incurred when testing with merchant ID (`mchtId`) in production environment.
- Only POST method is used for requests.
- For request parameters, please use only those specified in the integration specification. Otherwise, an error might occur.
- Please refrain from using special characters like `:`, `&`, `?`, `'`, new line `<`, `>` in the request parameter.
- Please refrain from using emoji in the request parameter. If an emoji is included, it will be removed regardless of the intent of the user.
- Request and response parameters may change without notice, this should be considered while developing.
- There are cases in which it does not function normally on certain browsers or devices if Iframe is used for payment window integration, so please refrain from using Iframe.

#### nextUrl parameter

- It is the URL invoked when Hecto Financial payment is completed. The request parameter should be delivered in POST method. If the customer forcibly exits the payment window (browser ‘X’ button) it won’t be invoked. `nextUrl` delivers the authentication result and the service should be provided only when the payment request result is success.

#### nextUrl cancUrl protocol

- If HTTP is used it may be against the browser policy (cross-origin etc.) so the payment window might not function normally. Thus, use of HTTPS is recommended.

### 2.3 Payment Window URI

Hecto Financial White Label payment window (UI) server domain names are as follows:

| Classification          | Domain Name                 |
|-------------------------|-----------------------------|
| Testbed                 | tbwl.settlebank.co.kr        |
| Production Environment   | wl.settlebank.co.kr         |

Hecto Financial White Label payment window (UI) API URI are as follows:

| Function Classification  | Payment Method | URI                                           | HTTP Method |
|--------------------------|----------------|-----------------------------------------------|-------------|
| Standard Payment Window (UI) | Invoke Payment Window | https://{domain}/whitelabel/main.do           | POST        |

### 2.4 Request and Response Headers

| Classification | Content                                                         |
|----------------|-----------------------------------------------------------------|
| Request        | `Content-type=application/x-www-form-urlencoded; charset=UTF-8` |
| Response       | `Content-type=text/html; charset=UTF-8`                         |

### 2.5 Request Parameter Verification

Upon parameter verification, if there is an error like missing required value, HASH data mismatch, and length check, the response code below is returned.

```json
{
   "outRsltMsg" : "결제 요청 정보 누락 (상품명)",
   "mchtTrdNo" : "PG_whitelabel_20231218081443",
   "outRsltCd" : "1011",
   "outStatCd" : "0031",
   "mchtId" : "pg_test"
}
```
("결제 요청 정보 누락 (상품명)" = “Payment Request Information Missing (Product Name)”)
```

## 3. API Service Integration (Non-UI)

### 3.1 Summary

- After checking the request parameter, set the applicable request parameter.
- Do HTTP Connection through Server to Server and request/respond through JSON data.
- Personal/sensitive information-related parameter should be encrypted. (Refer to [5. Crucial Information Security](#crucial-information-security)).

### 3.2 API URI

Hecto Financial White Label API server domain names are as follows:

| Classification         | Domain Name                  |
|------------------------|------------------------------|
| Testbed                | tbapi.settlebank.co.kr        |
| Production Environment | api.settlebank.co.kr          |

API server URI are as follows:

| Function Classification | Service          | URI                                           | HTTP Method |
|--------------------------|------------------|-----------------------------------------------|-------------|
| Payment API (Non-UI)     | Payment Request  | https://{domain}/whitelabel/v1/pay_confirm.do | POST        |
| Cancellation API (Non-UI)| Cancellation Request | https://{domain}/whitelabel/v1/pay_cancel.do | POST        |

### 3.3 Request and Response Headers

| Classification | Content                                       |
|----------------|-----------------------------------------------|
| Request        | `Content-type=application/json; charset=UTF-8`|
| Response       | `Content-type=application/json; charset=UTF-8`|

### 3.4 JSON Request Data Example

The following is the payment request parameter JSON example.

```json
{
    "params": {
        "mchtId": "pg_test",
        "ver": "0A18",
        "method": "WL",
        "bizType": "C4",
        "encCd": "23",
        "mchtTrdNo": "PG_API20231218085315",
        "trdDt": "20231218",
        "trdTm": "085315",
        "mobileYn": "N",
        "osType": "W"
    },
    "data": {
        "pktHash": "197884af9f63bcd2d9792cd5cd39f7b1e7525e88888e2cee28f64fa23bf53312",
        "mchtCustId": "hecto01",
        "orgTrdNo": "STFP_PGCApg_test0000231218090723M1717578",
        "trdAmt": "vqIWIiimsJ5efjSJpfnnTw=="
    }
}
```

## 4. Integration Server

### 4.1 Server IP Address

The following is the Hecto Financial server’s IP address.

| Classification                  | Domain Name          | IP Address                                |
|---------------------------------|----------------------|--------------------------------------------|
| Payment Window (UI)             | Testbed              | tbwl.settlebank.co.kr                      | 
|                                 |                      | 61.252.169.78                              |
|                                 | Production           | wl.settlebank.co.kr                        |
|                                 |                      | 61.252.169.80 (primary)                    |
|                                 |                      | 14.34.14.38 (secondary)                    |
| Cancellation API Service (Non-UI)| Testbed              | tbapi.settlebank.co.kr                     |
|                                 |                      | HTTPS(TCP/443)                             |
|                                 |                      | 61.252.169.42                              |
|                                 | Production           | api.settlebank.co.kr                       |
|                                 |                      | HTTPS(TCP/443)                             |
|                                 |                      | 61.252.169.53 (primary)                    |
|                                 |                      | 14.34.14.21 (secondary)                    |

Hecto Financial PG Next Gen System is configured with IDC center geo-redundancy (Production environment). Therefore, the main and secondary centers may be switched without notice due to GLB system operation. It is recommended to connect by DNS Lookup.

Therefore, we request that you allow access of two public IP addresses (production) in the firewall, and we do not recommend configuring the hosts file.

If you configure the domain address as a fixed setting in the hosts file due to your company's internal policy, when switching to Hecto Financial IDC Center, you must manually change the hosts file of all servers to `#(comment) IP address` as shown below to use the payment service normally.

All communication uses HTTPS (TCP/443) protocol, and it is strongly recommended to connect using TLS 1.2 or higher. TLS 1.1 or lower may be discontinued without notice due to security advisories.

## 5. Crucial Information Security

### 5.1 Personal Information and Crucial Information Encryption / Decryption

When sending or receiving data for personal information / crucial information fields, the following encryption/decryption should be done.

| Classification       | Entry                          | Application                        | Encoding         |
|----------------------|--------------------------------|------------------------------------|------------------|
| Personal Information | Algorithm                      | AES-256/ECB/PKCS5Padding           | Base64 Encoding  |
|                      | Target Field                   | Transaction Amount, Customer Name, Mobile Number, Email, etc. (Fields that should be encrypted are specified in the remark section of individual API’s request field specifications.) |                  |

### 5.2 Personal Information Encryption Key

Personal information and crucial information encryption/decryption key information differ depending on the environment and are as follows. For a simple integration test, use the first entry in the table. Once the integration is done successfully, use the unique key issued by Hecto Financial.

| Classification         | Encryption / Decryption Key            |
|------------------------|----------------------------------------|
| Testbed Key (Common Key for Test) | `pgSettle30y739r82jtd709yOfZ2yK5K` |
| Production Environment Key (Merchant Unique Key) | Separately notified when the service is implemented |

### 5.3 Forgery Prevention Algorithm

Hash key verification is additionally executed to verify that the request data has not been forged and the hash key generation algorithm is as follows.

| Classification | Entry                          | Application            | Encoding         |
|----------------|--------------------------------|------------------------|------------------|
| Forgery        | Algorithm                      | SHA-256                | Hex Encoding     |

### 5.4 Hash Generation Authentication Key

For a simple integration test, use the first entry in the table. Once the integration is done successfully, use the unique key issued by Hecto Financial.

| Classification         | Authentication Key                     |
|------------------------|-----------------------------------------|
| Testbed Key (Common Key for Test) | `ST1009281328226982205`           |
| Production Environment Key (Merchant Unique Key) | Separately notified when the service is implemented |

## 6. Payment Window Invoke (UI)

### 6.1 Caution

Please refer to the issuance amount parameter processing standard.
- Ex) If the taxable merchant sends a transaction amount of 1000 won as follows:
  1. Only the transaction amount sent: Processed as taxable 901, VAT 99
  2. Taxable amount 900, VAT amount 100 sent: Processed as taxable 900, VAT 100

### 6.2 Request Parameter (Merchant -> Hecto Financial)

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(10)         | ●        | "pg_test"                                       |
| `method`       | Payment Method               | White label payment classification code                                 | AN(20)         | ●        | "whitelabel"※ Fixed value                        |
| `trdDt`        | Request Date                 | yyyyMMdd                                                                | N(8)           | ●        | "20231218"                                      |
| `trdTm`        | Request Time                 | HH24MISS                                                                | N(6)           | ●        | "100000"                                        |
| `mchtTrdNo`    | Merchant Order Number        | Unique order number generated by the merchant, excluding Korean letters | AN(100)        | ●        | "ORDER20211231100000"                           |
| `pmtPrdtNm`    | Product Name                 | Payment Product Name                                                    | AHN(300)       | ●        | "Test Product"                                  |
| `trdAmt`       | Transaction Amount           | Transaction Amount, AES Encryption                                      | N(12)          | ●        | "1000"                                          |
| `nextUrl`      | Payment Screen URL           | Result delivery and moved page URL                                      | AN(250)        | ●        | "https://example.com/nextUrl"                   |
| `cancUrl`      | Payment Cancellation URL     | Result delivery and moved page URL when customer forcibly exits         | AN(250)        | ●        | "https://example.com/cancUrl"                   |
| `mchtParam`    | Merchant Reserved Field      | Merchant reserved field to input other order information                | AHN(4000)      | ○        | "name=HongGilDong&age=25"                       |
| `email`        | Email                        | Email address, AES encryption                                           | AN(60)         | ○        | "HongGilDong@example.com"                       |
| `mchtCustId`   | Merchant Customer            | Unique customer ID or unique key sent by merchant, AES encryption       | AN(100)        | ●        | "HongGilDong"                                   |
| `taxTypeCd`    | Tax-Free Status              | N: Taxable, Y: Tax-free, G: Compound tax. If blank, follows merchant setting | A(1)       | ○        | "N"                                             |
| `taxAmt`       | Taxable Amount               | Taxable amount (Required if it is compound tax), AES encryption        

 | N(12)          | ○        | "909"                                           |
| `vatAmt`       | VAT Amount                   | VAT amount (Required if it is compound tax), AES encryption             | N(12)          | ○        | "91"                                            |
| `taxFreeAmt`   | Non-Taxable Amount           | Tax-free amount (Required if it is compound tax), AES encryption        | N(12)          | ○        | "0"                                             |
| `svcAmt`       | Service Charge               | Credit card service charge, AES encryption                              | N(12)          | ○        | "10"                                            |
| `themeColorCd` | Theme Color                  | Theme color                                                             | AN(10)         | ○        | "#630511"                                       |
| `custAcntSumry`| Customer Account Remark      | Remark on the customer’s account                                        | AHN(30)        | ○        | "Remark"                                        |
| `cupDepositAmt`| Container Deposit            | Cup deposit                                                             | N(12)          | ○        | "0"                                             |
| `addDdtTypeCd` | Cash Receipt Additional Deduction Classification | Cash receipt additional deduction classification        | A(1)   | ○        | "Y": Public Transportation, "C": Book Concert fee |
| `ciChkYn`      | CI Verification Status       | Customer CI verification status. If used (Y), input CI in customer ID   | A(1)           | ○        | "N"                                             |
| `pktHash`      | Hash Data                    | Hash value generated with SHA256 method. Refer to [Request Parameter Hash Code](#request-parameter-hash-code) | AN(200) | ● | "f395b6725a9a18f2563ce34f8bc76698051d27c05e5ba815f463f00429061c0c" |

### 6.3 Request Parameter Hash Code

| Entry    | Combination Field                                                                            |
|----------|----------------------------------------------------------------------------------------------|
| `pktHash` | Merchant ID + Payment Method + Merchant Order Number + Request Date + Request Time + Transaction Amount (Plain Text) + Hash Key |

### 6.4 Response Parameter (Hecto Financial -> Merchant)

Columns that respond to the merchant side from the White Label payment window are defined as follows.

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(10)         | ●        | "pg_test"                                       |
| `outStatCd`    | Transaction Status           | Transaction Status Code (Success / Failure). 0021: Success, 0031: Failure | AN(4)       | ●        | "0021"                                          |
| `outRsltCd`    | Reject Code                  | If the transaction status is “0031”, deliver detailed code. Refer to [Table of Reject Code](#table-of-reject-codes) | AN(4) | ● | "0000"                                          |
| `outRsltMsg`   | Result Message               | Deliver result message, URL Encoding UTF-8                               | AHN(200)       | ●        | "Success"                                       |
| `mchtTrdNo`    | Merchant Order Number        | Unique order number generated by the merchant, excluding Korean letters | AN(100)        | ●        | "ORDER20211231100000"                           |
| `mchtCustId`   | Merchant Customer ID         | Unique customer ID or unique key sent, AES encryption                   | AN(100)        | ○        | "HongGilDong"                                   |
| `trdNo`        | Transaction Number           | Hecto Financial transaction number                                       | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `trdAmt`       | Transaction Amount           | Transaction amount, AES encryption                                      | N(12)          | ●        | "1000"                                          |
| `mchtParam`    | Merchant Reserved Field      | Field value received as request is Bypassed as response                 | AHN(4000)      | ○        | "name=HongGilDong&age=25"                       |

## 7. Payment API (Non-UI)

### 7.1 Request Parameter (Merchant -> Hecto Financial)

API URI:
- Test Environment: `https://tbapi.settlebank.co.kr/whitelabel/v1/pay_confirm.do`
- Production Environment: `https://api.settlebank.co.kr/whitelabel/v1/pay_confirm.do`

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `params`       |                              |                                                                         |                |          |                                                 |
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(12)         | ●        | "pg_test"                                       |
| `ver`          | Version                      | Version of the parameter                                                | AN(4)          | ●        | "0A18"※ Fixed value                              |
| `method`       | Payment Method               | Payment method                                                          | A(2)           | ●        | "WL"※ Fixed value                                |
| `bizType`      | Work Classification          | Work classification code                                                | AN(2)          | ●        | "B9"※ Fixed value                                |
| `encCd`        | Encryption Classification    | Encryption classification code                                          | N(2)           | ●        | "23"※ Fixed value                                |
| `mchtTrdNo`    | Merchant Order Number        | Unique order number generated by merchant                               | AN(100)        | ●        | "ORDER20211231100000"                           |
| `trdDt`        | Request Date                 | Date of sending the current parameter (YYYYMMDD)                        | N(8)           | ●        | "20231218"                                      |
| `trdTm`        | Request Time                 | Time of sending the current parameter (HHMMSS)                          | N(6)           | ●        | "100000"                                        |
| `mobileYn`     | Mobile Status                | Y: Mobile web/app, N: PC or others                                      | A(1)           | ○        | "N"                                             |
| `osType`       | OS Classification            | A: Android, I: IOS, W: Windows, M: Mac, E: Others, Blank: Cannot be checked | A(1)       | ○        | "W"                                             |
| `data`         |                              |                                                                         |                |          |                                                 |
| `pktHash`      | Hash Data                    | Hash value generated with SHA256 method. Refer to [Request Parameter Hash Code](#request-parameter-hash-code) | AN(200) | ● | "f395b6725a9a18f2563ce34f8bc76698051d27c05e5ba815f463f00429061c0c" |
| `mchtCustId`   | Merchant Customer ID         | Merchant Customer ID                                                    | AN(100)        | ●        | "HongGilDong"                                   |
| `authTrdNo`    | Authentication Transaction Number | Hecto Financial Transaction Number                                    | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `trdAmt`       | Transaction Amount           | Transaction amount, AES encryption                                      | N(12)          | ●        | "1000"                                          |

### 7.2 Request Parameter Hash Code

| Entry    | Combination Field                                                                            |
|----------|----------------------------------------------------------------------------------------------|
| `pktHash` | Request Date + Request Time + Merchant ID + Merchant Order Number + Transaction Amount (Plain Text) + Hash Key |

### 7.3 Response Parameter (Hecto Financial -> Merchant)

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `params`       |                              |                                                                         |                |          |                                                 |
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(12)         | ●        | "pg_test"                                       |
| `ver`          | Version                      | Parameter’s version                                                     | AN(4)          | ●        | "0A18"※ Fixed value                              |
| `method`       | Payment Method               | Payment method                                                          | A(2)           | ●        | "WL"※ Fixed value                                |
| `bizType`      | Work classification          | Work classification code                                                | AN(2)          | ●        | "B9"※ Fixed value                                |
| `encCd`        | Encryption classification    | Encryption classification code                                          | N(2)           | ●        | "23"※ Fixed value                                |
| `mchtTrdNo`    | Merchant Order Number        | Order number generated by the merchant                                  | AN(100)        | ●        | "ORDER20211231100000"                           |
| `trdNo`        | Hecto Financial Transaction Number | Unique transaction number issued by Hecto Financial                    | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `trdDt`        | Request Date                 | Date of sending the current parameter (YYYYMMDD)                        | N(8)           | ●        | "20231218

"                                      |
| `trdTm`        | Request Time                 | Time of sending the current parameter (HHMMSS)                          | N(6)           | ●        | "100000"                                        |
| `outStatCd`    | Transaction Status           | Transaction Status Code (Success / Failure). 0021: Success, 0031: Failure | AN(4)       | ●        | "0021"                                          |
| `outRsltCd`    | Reject Code                  | If the transaction status is “0031”, deliver detailed code. Refer to [Table of Reject Code](#table-of-reject-codes) | AN(4) | ● | "0000"                                          |
| `outRsltMsg`   | Result Message               | Deliver result message, URL Encoding UTF-8                               | AHN(200)       | ●        | "Normally processed."                           |
| `data`         |                              |                                                                         |                |          |                                                 |
| `pktHash`      | Hash Value                   | When requested, hash value itself returned                               | AN(64)         | ●        |                                                 |
| `mchtCustId`   | Merchant Customer ID         | Merchant Customer ID                                                    | AN(100)        | ●        | "HongGilDong"                                   |
| `authTrdNo`    | Authentication Transaction Number | Hecto Financial Transaction Number                                    | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `trdAmt`       | Transaction Amount           | Transaction Amount, AES Encryption                                      | N(12)          | ●        | "1000"                                          |
| `payMethod`    | Work Classification Code     | CA: Credit Card, RP: Easy Cash Payment                                  | AN(2)          | ●        | "CA"                                            |
| `fnNm`         | Card Company Name            | Name of the card company used for payment                               | HA(20)         | ○        | (For Credit Card) “Kookmin”                     |
| `fnCd`         | Card Company Code            | Code of the card company used for payment                               | AN(4)          | ○        | (For Credit Card) "KBC"                         |
| `svcDivCd`     | Easy Cash Integration Classification | 1: Firm Banking, 2: Open Banking                                   | N(1)           | ○        | (For Easy Cash) "1"                             |

### 7.4 Response Parameter Hash Code

| Entry    | Combination Field                                                                            |
|----------|----------------------------------------------------------------------------------------------|
| `pktHash` | Transaction Status Code + Request Date + Request Time + Merchant ID + Merchant Order Number + Transaction Amount + Hash Key |

## 8. Cancellation API (Non-UI)

### 8.1 Request Parameter (Merchant -> Hecto Financial)

API URI:
- Test Environment: `https://tbapi.settlebank.co.kr/whitelabel/v1/pay_cancel.do`
- Production Environment: `https://api.settlebank.co.kr/whitelabel/v1/pay_cancel.do`

If there is Easy Cash Payment cancellation, the refund (deposit) is done to the bank account in which the money was withdrawn from.

Columns requested from the merchant server to Hecto Financial are defined as follows.

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `params`       |                              |                                                                         |                |          |                                                 |
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(12)         | ●        | "pg_test"                                       |
| `ver`          | Version                      | Parameter’s version                                                     | AN(4)          | ●        | "0A18"※ Fixed value                              |
| `method`       | Payment Method               | Payment method                                                          | A(2)           | ●        | "WL"※ Fixed value                                |
| `bizType`      | Work Classification          | Work Classification Code                                                | AN(2)          | ●        | "C4"※ Fixed value                                |
| `encCd`        | Encryption Classification    | Encryption classification code                                          | N(2)           | ●        | "23"※ Fixed value                                |
| `mchtTrdNo`    | Merchant Order Number        | Unique order number generated by the merchant                           | AN(100)        | ●        | "ORDER20211231100000"                           |
| `trdDt`        | Cancellation Request Date    | Date of sending the current parameter (YYYYMMDD)                        | N(8)           | ●        | "20231218"                                      |
| `trdTm`        | Cancellation Request Time    | Time of sending the current parameter (HHMMSS)                          | N(6)           | ●        | "100000"                                        |
| `mobileYn`     | Mobile Status                | Y: Mobile Web/App, N: PC or others                                      | A(1)           | ○        | "N"                                             |
| `osType`       | OS Type                      | A: Android, I: IOS, W: Windows, M: Mac, E: Others, Blank: Cannot be checked | A(1)       | ○        | "W"                                             |
| `data`         |                              |                                                                         |                |          |                                                 |
| `pktHash`      | Hash Data                    | Hash value generated with SHA256 method. Refer to [Request Parameter Hash Code](#request-parameter-hash-code) | AN(200) | ● | "f395b6725a9a18f2563ce34f8bc76698051d27c05e5ba815f463f00429061c0c" |
| `mchtCustId`   | Merchant Customer ID         | Merchant Customer ID                                                    | AN(100)        | ●        | "HongGilDong"                                   |
| `orgTrdNo`     | Original Transaction Number  | Transaction number issued by Hecto Financial when payment is made       | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `taxTypeCd`    | Tax-Free Status              | Y: Tax-free, N: Taxable, G: Compound tax. If blank, follows merchant setting | A(1)       | ○        | "N"                                             |
| `trdAmt`       | Transaction Amount           | Transaction Amount, AES encryption                                      | N(12)          | ○        | "1000"                                          |
| `taxAmt`       | Taxable Amount               | Taxable amount among canceled amount (Required if compound tax), AES encryption | N(12)     | ○        | "909"                                           |
| `vatAmt`       | VAT Amount                   | VAT amount among canceled amount (Required if compound tax), AES encryption | N(12)       | ○        | "91"                                            |
| `taxFreeAmt`   | Non-Taxable Amount           | Non-taxable amount among canceled amount (Required if compound tax), AES encryption | N(12)    | ○        | "0"                                             |
| `svcAmt`       | Service Charge               | Service charge among canceled amount, AES encryption                    | N(12)          | ○        | "0"                                             |
| `crcCd`        | Currency Classification      | Currency classification value                                           | A(3)           | ○        | (Exclusive for credit card) Required - "KRW"    |
| `cnclOrd`      | Number of Cancellation       | Start from 001. For second partial cancellation 002                     | N(3)           | ○        | (Exclusive for credit card) Required - "001"    |

### 8.2 Request Parameter Hash Code

| Entry    | Combination Field                                                                            |
|----------|----------------------------------------------------------------------------------------------|
| `pktHash` | Cancellation Request Date + Cancellation Request Time + Merchant ID + Merchant Order Number + Cancellation Amount (Plain Text) + Hash Key |

### 8.3 Response Parameter (Hecto Financial -> Merchant)

Columns that respond to the merchant from Hecto Financial are defined as follows.

| Parameter      | Name                         | Description                                                             | Type (Length)  | Required | Remark                                          |
|----------------|------------------------------|-------------------------------------------------------------------------|----------------|----------|-------------------------------------------------|
| `params`       |                              |                                                                         |                |          |                                                 |
| `mchtId`       | Merchant ID                  | Unique merchant ID given by Hecto Financial                             | AN(12)         | ●        | "pg_test"                                       |
| `ver`          | Version                      | Parameter’s version                                                     | AN(4)          | ●        | "0A18"※ Fixed value                              |
| `method`       | Payment method               | Payment method                                                          | A(2)           | ●        | "WL"※ Fixed value                                |
| `bizType`      | Work classification          | Work classification code                                                | AN(2)          | ●        | "C4"※ Fixed value                                |
| `encCd`        | Encryption classification    | Encryption classification code                                          | N(2)           | ●        | "23"※ Fixed value                                |
| `mchtTrdNo`    | Merchant Order Number        | Unique order number generated by the merchant                           | AN(100)        | ●        | "ORDER20211231100000"                           |
| `trdNo`        | Hecto Financial Transaction Number | Unique transaction number generated by Hecto Financial                | AN(40)         | ●        | "STFP_PGCApg_test0000231218092215M1103678"      |
| `trdDt`        | Cancellation Response Date   | Date of sending the current response parameter

 (YYYYMMDD)               | N(8)           | ●        | "20231218"                                      |
| `trdTm`        | Cancellation Response Time   | Time of sending the current response parameter (HHMMSS)                 | N(6)           | ●        | "100000"                                        |
| `outStatCd`    | Transaction Status           | Transaction status code (Success / Failure). 0021: Success, 0031: Failure | AN(4)       | ●        | "0021"                                          |
| `outRsltCd`    | Rejection Code               | If the transaction status is “0031”, deliver detailed code. Refer to [Table of Reject Code](#table-of-reject-codes) | AN(4) | ● | "0000"                                          |
| `outRsltMsg`   | Result Message               | Deliver result message, URL Encoding UTF-8                               | AHN(200)       | ●        | "It was normally processed."                    |
| `data`         |                              |                                                                         |                |          |                                                 |
| `pktHash`      | Hash Value                   | When requested, hash value itself returned                               | AN(64)         | ●        |                                                 |
| `mchtCustId`   | Merchant Customer ID         | Merchant customer ID                                                    | AN(100)        | ●        | "HongGilDong"                                   |
| `payMethod`    | Work Classification Code     | Work classification code. "RT" : Easy Payment Cancellation, "CA" : Credit Card Cancellation | A(2)       | ●        |                                                 |
| `orgTrdNo`     | Original Transaction Number  | Transaction number issued by Hecto Financial when payment is made       | AN(40)         | ●        | "STFP_PGCApg_test0000231218090723M1717578"      |
| `taxTypeCd`    | Tax-Free Status              | Y: Tax-free, N: Taxable, G: Compound tax. If blank, follows merchant setting | A(1)       | ○        | "N"                                             |
| `trdAmt`       | Transaction amount           | Transaction amount, AES encryption                                      | N(12)          | ○        | "1000"                                          |
| `taxAmt`       | Taxable amount               | Taxable amount among canceled amount (Required if compound tax), AES encryption | N(12)     | ○        | "909"                                           |
| `vatAmt`       | VAT Amount                   | VAT amount among canceled amount (Required if compound tax), AES encryption | N(12)       | ○        | "91"                                            |
| `taxFreeAmt`   | Non-Taxable Amount           | Non-taxable amount among canceled amount (Required if compound tax), AES encryption | N(12)    | ○        | "0"                                             |
| `svcAmt`       | Service Charge               | Service charge among canceled amount, AES encryption                    | N(12)          | ○        | "0"                                             |
| `crcCd`        | Currency Classification      | Currency classification value                                           | A(3)           | ○        | (Exclusive for credit card)                     |
| `cnclOrd`      | Number of Cancellation       | Start from 001. For second partial cancellation 002                     | N(3)           | ○        | (Exclusive for credit card)                     |
| `cardCnclAmt`  | Credit Card Canceled Amount  | Credit card transaction amount, AES encryption                          | N(12)          | ○        | (Exclusive for credit card)                     |
| `blcAmt`       | Cancellable Balance          | Cancellable balance, AES encryption                                     | N(12)          | ○        | "0"                                             |
| `pktNo`        | Parameter Number             | Parameter number                                                        | AN(20)         | ○        | (Exclusive for Easy Cash Payment)               |

### 8.4 Request Parameter Hash Code

| Entry    | Combination Field                                                                            |
|----------|----------------------------------------------------------------------------------------------|
| `pktHash` | Transaction Status Code + Cancellation Response Date + Cancellation Response Time + Merchant ID + Merchant Order Number + Transaction Amount (Plain Text) + Hash Key |

## 9. Others

### 9.1 Table of Reject Codes

**White Label Exclusive Response Code**

* Depending on the payment method (Easy Cash Payment PG) used, the response message for the same response code may vary. Please refer to all response codes below.

| Code  | Content                                      | Code  | Content                                      |
|-------|----------------------------------------------|-------|----------------------------------------------|
| 0000  | Normal Processing                            | 0009  | User cancellation                            |
| 1001  | Payment request info omitted (Merchant ID)   | 1002  | Payment request info omitted (Request Date)  |
| 1003  | Payment request info omitted (Request Time)  | 1004  | Payment request info omitted (Merchant Order Number) |
| 1005  | Payment request info omitted (Transaction Amount) | 1006  | Payment request info omitted (Result Screen URL) |
| 1007  | Payment request info omitted (Payment Cancellation URL) | 1008  | Payment request info omitted (Merchant Customer ID) |
| 1009  | Payment request info omitted (Email)         | 1010  | Payment request info omitted (hash Data)     |
| 1011  | Payment request info omitted (Product Name)  | 1101  | Payment request info length error (Merchant ID) |
| 1102  | Payment request info length error (Request Date) | 1103  | Payment request info length error (Request Time) |
| 1104  | Payment request info length error (Merchant Order Number) | 1105  | Payment request info length error (Transaction Amount) |
| 1106  | Payment request info length error (Result Screen URL) | 1107  | Payment request info length error (Payment Cancellation URL) |
| 1108  | Payment request info length error (Merchant Customer ID) | 1109  | Payment request info length error (hash Data) |
| 1110  | Payment request info length error (Merchant Name) | 1111  | Payment request info length error (Merchant Reservation Field) |
| 1112  | Payment request info length error (Email)    | 1113  | Payment request info length error (Tax-Free Status) |
| 1114  | Payment request info length error (Taxable Amount) | 1115  | Payment request info length error (VAT Amount) |
| 1116  | Payment request info length error (Non-Taxable Amount) | 1117  | Payment request info length error (Service Charge Amount) |
| 1118  | Payment request info length error (Product Name) | 1119  | Payment request info length error (Bank Remark Content) |
| 1120  | Payment request info length error (Container Deposit) | 1121  | Payment request info length error (Cash Receipt Additional Deduction Classification) |
| 1122  | Payment request info length error (CI Verification Status) | 1901  | Hash value mismatch error                    |
| 1902  | Encryption entry unprocessed error           | 2005  | Password verification failure                |
| 2010  | Customer service termination                 | 5001  | token null                                   |
| 5002  | token expired                                | 5003  | token encoding error                         |
| 5004  | token parse error                            | 5005  | token mapping error                          |
| 5006  | token io error                               | 6001  | Invalid request parameter                    |
| 9002  | Payment window option inquiry error          | 9003  | Payment window banner inquiry error          |
| 9004  | Terms list inquiry error                     | 9005  | Notice inquiry error                         |
| 9006  | Affiliate discount inquiry error             | 9007  | Payment information inquiry error            |
| 9901  | Gateway request parameter error              | 9902  | Gateway response parameter error             |
| 9903  | Gateway request error                        | 9904  | Gateway response error                       |
| 9905  | Gateway system error                         | 9999  | Internal system error                        |
| WL01  | There were 10 or more authentication attempts with the same IP within a certain period of them, so the authentication is restricted for 15 minutes. Please try again later. | WL02 | Customer information is not valid. Please check the self-authentication personal information. |

### Easy Cash Payment Response Code

* Recent common reject codes can be downloaded from Common Reject Codes

| Code | Content                                               | Code | Content                                                         |
|------|-------------------------------------------------------|------|-----------------------------------------------------------------|
| 0021 | Success                                               | 0031 | Failure                                                         |
| ST01 | Nonexistent account                                   | ST02 | Invalid request                                                 |
| ST03 | Duplicate withdrawal                                  | ST04 | System error occurred during VAN                                |
| ST05 | No VAN response information                           | ST06 | No transaction number information                               |
| ST07 | Communication Error                                   | ST08 | Already registered account                                      |
| ST09 | Invalid request parameter                             | ST10 | Internal system error                                           |
| ST11 | Bank maintenance time                                 | ST12 | Insufficient balance in the withdrawal account                  |
| ST13 | ARS authentication result does not exist              | ST14 | ARS authentication result value differs                         |
| ST15 | Recurring transfer terminated account                 | ST16 | Withdrawal account transaction restricted                       |
| ST17 | Resident number business number error                 | ST18 | Account error (easy account registration not allowed)           |
| ST19 | Transaction not allowed for other reasons             | ST20 | Account error                                                   |
| ST21 | No receiving account                                   | ST22 | Legally restricted account                                      |
| ST23 | Non-real name account                                 | ST24 | Account holder mismatch                                         |
| ST25 | Already canceled transaction                          | ST26 | Cancellation amount error                                       |
| ST27 | ARS authentication failed                             | ST28 | ARS reception not allowed                                       |
| ST29 | Account registration ongoing                          | ST30 | Refund in process                                               |
| ST31 | Duplicate remittance occurred                         | ST32 | Payer name inquiry failure                                      |
| ST33 | Limit per transaction is exceeded                     | ST34 | Daily limit exceeded                                            |
| ST35 | Fraudulent account                                    | ST36 | Disconnection after a certain period of time                    |
| ST37 | Easy payment cancellation                             | ST38 | Request in progress                                             |
| ST39 | Refund duplicate request                              | ST40 | There is a request being processed                              |
| ST41 | Service capacity exceeded                             | ST42 | System BUSY                                                     |
| ST43 | Already registered account                            | ST44 | Transaction with the bank not allowed                           |
| ST50 | Duplicate parameter request                           | ST51 | Already registered cash receipt user                            |
| ST52 | Unregistered cash receipt user                        | ST53 | Already terminated account                                      |
| ST60 | Transaction failure                                   | ST61 | Amount limit per transaction is exceeded                        |
| ST62 | Daily limit amount exceeded                           | ST63 | Monthly limit amount exceeded                                   |
| ST64 | Daily number of transaction limit exceeded            | ST65 | Monthly number of transaction limit exceeded                    |
| ST66 | Password registration failure                         | ST67 | Password mismatch                                               |
| ST68 | Service use suspension                                | ST69 | According to policy, the payment service cannot be used. Please try using another payment method. |
| ST70 | According to the policy, this payment service cannot be used. Please contact Hecto Financial’s customer service. (1600-5220) | ST72 | ARS second authentication needed for payment |
| ST86 | Authentication failure. (Mobile self-verification)    | ST87 | User is not registered to easy self-verification. (Mobile self-verification) |
| VTIM | Relay institution TIMEOUT                             | ST99 | Easy payment system maintenance                                 |
| SE01 | Authentication valid time has expired.                | SE02 | Authentication number mismatch                                  |
| SE03 | Number of allowed attempts for authentication check has been exceeded |         |                                                                  |

### PG Common Response Code

| Code | Content                                                        | Code | Content                                                                  |
|------|----------------------------------------------------------------|------|--------------------------------------------------------------------------|
| ST01 | No customer information                                        | STR1~STR8 | Internal policy                                                      |
| ST06 | No transaction information                                     | ST03 | Already paid transaction                                                  |
| ST08 | Already registered account                                     | ST07 | Invalid parameter                                                         |
| ST09 | Invalid request parameter                                      | ST10 | Internal system error                                                     |
| ST11 | Provider maintenance time                                      | ST13 | No authentication detail                                                  |
| ST19 | Transaction not allowed for other reasons                      | ST24 | Account holder mismatch                                                   |
| ST25 | Duplicate cancellation request                                 | ST26 | Cancellation information error                                            |
| ST30 | Authentication time expired                                    | ST32 | Account holder name error                                                 |
| ST38 | Processing                                                     | ST39 | Refund duplicate request                                                  |
| ST41 | Traffic overload                                               | ST44 | Provider information error                                                |
| ST45 | Cancellation failure                                           | ST46 | Cancellation period elapsed                                               |
| ST47 | Unregistered merchant                                          | ST48 | Unregistered merchant                                                     |
| ST50 | Duplicate request                                              | ST54 | Partial cancellation not allowed                                          |
| ST55 | Compound tax amount mismatch                                   | ST56 | Number of cancellation error                                              |
| ST57 | Cancellation amount (refund amount) exceeds the original transaction amount | ST58 | Cancellation request amount and original transaction amount do not match. |
| ST60 | Abnormal processing                                            | ST68 | Service suspension                                                        |
| ST69 | Service not allowed                                            | ST70 | Policy block                                                              |
| ST79 | System error                                                   | ST90~ST97 | Individual definition per payment method                                  |
| ST99 | Error for other reasons                                        | VTIM | Provider timeout                                                          |
| 3007 | Cancellation limit exceeded                                    |

### Credit Card Response Code

| Code | Content                                                        | Code | Content                                                                  |
|------|----------------------------------------------------------------|------|--------------------------------------------------------------------------|
| CA01 | Provider maintenance                                           | CA02 | Provider error                                                            |
| CA03 | Parameter validity check                                       | CA04 | Payment relay institution error                                           |
| CA10 | Transaction information does not exist                         | CA11 | Already acquired transaction                                              |
| CA12 | Already canceled transaction                                   | CA13~CA19 | Request information error                                                |
| CA20 | Card validity is not valid                                     | CA21~CA23 | Installment-related error                                                |
| CA26 | Number of uses exceeded                                        | CA27~CA29 | Password-related error                                                   |
| CA30 | Identification number error                                    | CA31~CA37 | Limit-related error                                                      |
| CA38 | Transaction not allowed as point limit is not reached          | CA40~CA41 | Transaction amount error                                                 |
| CA42 | Transaction not allowed for amounts below 1000 won or amounts over 99,999,999 won | CA50~CA57 | Card status error                                         |
| CA58 | Unregistered card. Please contact the card company.            | CA60~CA64 | Merchant status error                                                    |
| CA59 | Transaction with corporate card is not allowed.                | J999 | Please call the card company                                              |
| CA65 | Authentication transaction non-committed merchant              | CA70 | Identification number (date of birth / business number) + password are not valid |
| CA75 | Joint certification authentication information is not valid.   | CA80 | Service not allowed for the transaction                                  |
| CA81 | Error occurred while processing transaction                    | 0905 | Requesting self-verification                                              |
| CA82 | Please call the card company                                    | CA83 | This transaction cannot be canceled.                                      |
| CA84 | Error for other reasons has occurred                           | CA85 | An error has occurred while processing cancellation transaction.          |
| CA86 | Duplicate transaction                                           | CA87 | Merchant number is not valid                                              |
| CA88 | Partial cancellation is not allowed for the pre-paid card (gift card). | 8373 | Please call the card company                                 |
| 8375 | Other error processing                                          |

### 9.2 Financial Institution Identifiers

**Credit card unique identification codes are as follows:**

| Credit Card Company Code | Credit Card Company Name | Credit Card Company Code | Credit Card Company Name |
|--------------------------|-------------------------|--------------------------|--------------------------|
| BCC                      | BC                      | KBC                      | Kookmin                  |
| HNC                      | Hana(Korea Exchange)    | SSC                      | Samsung                  |
| SHN                      | Shinhan                 | WRI                      | Woori                    |
| HDC                      | Hyundai                 | LTC                      | Lotte                    |
| 007                      | Suhyup                  | NHC                      | NH Nonghyup              |
| 035                      | Jeju                    | 034                      | Kwangju                  |
| 037                      | Jeonbuk                 | 027                      | Citi                     |
| 218                      | KB Securities           | 050                      | Savings Bank             |
| 071                      | Korea Post              | 048                      | NACUFOK                  |
| 002                      | KDB                     | 090                      | Kakao Bank               |
| 089                      | Kbank                   | KBP                      | KBPay                    |
| SSP                      | Samsung Pay             |                          |                          |

**Financial institution unique identification codes are as follows:**

| Financial Institution Code | Financial Institution Name                   | Financial Institution Code | Financial Institution Name                       |
|----------------------------|----------------------------------------------|----------------------------|--------------------------------------------------|
| 002                        | KDB                                           | 088                        | Shinhan Bank                                      |
| 003                        | IBK                                           | 089                        | Kbank                                             |
| 092                        | Toss Bank                                     | 271                        | Toss Securities                                   |
| 004                        | Kookmin Bank                                  | 090                        | Kakao Bank                                        |
| 005                        | Exchange Bank                                 | 209                        | Yuanta Securities                                 |
| 007                        | Suhyup Bank                                   | 218                        | KB Securities                                     |
| 011                        | NH Nonghyup                                   | 238                        | Mirae Asset Securities                            |
| 012                        | NACF                                          | 240                        | Samsung Securities                                |
| 020                        | Woori Bank                                    | 243                        | Korea Investment Securities                       |
| 023                        | SC Bank                                       | 247                        | NH Investment Securities                          |
| 027                        | Korea Citi Bank                               | 261                        | Kyobo Securities                                  |
| 031                        | Daegu Bank                                    | 262                        | HI Investment Securities                          |
| 032                        | Busan Bank                                    | 263                        | Hyundai Motor Investment Securities               |
| 034                        | Kwangju Bank                                  | 264                        | Kiwoom Securities                                 |
| 035                        | Jeju Bank                                     | 265                        | eBEST Investment & Securities                     |
| 037                        | Jeonbuk Bank                                  | 266                        | SK Securities                                     |
| 039                        | Kyongnam Bank                                 | 267                        | Daishin Securities                                |
| 045                        | Korean Federation of Community Credit Cooperatives (KFCC) | 269 | Hanwha Investment Securities            |
| 048                        | National Credit Union Federation of Korea (NACUFOK) | 270                | Hana Financial Investment                        |
| 050                        | Mutual Savings Bank                           | 278                        | Shinhan Financial Investment                      |
| 054                        | HSBC Bank                                     | 279                        | DB Securities                                     |
| 055                        | Deutsche Bank                                 | 280                        | Eugene Investment Securities                      |
| 057                        | JP Morgan Chase Bank                          | 287                        | Meritz Securities                                 |
| 060                        | BOA Bank                                      | 290                        | Bookook Securities                                |
| 062                        | Industrial and Commercial Bank of China       | 291                        | Shinyoung Securities                              |
| 064                        | National Forestry Cooperative Federation (NFCF) | 292                      | Cape Investment Securities                        |
| 071                        | Korea Post                                    | 103                        | SBI Savings Bank                                  |
| 081                        | KEB Hana Bank                                 |                            |                                                  |

### 9.3 Bank Regular Maintenance Time

| Financial Institution Code | Financial Institution Name                                  | Bank Maintenance Time     | Hecto Financial Working Time | Regular Maintenance                                   |
|----------------------------|-------------------------------------------------------------|---------------------------|-----------------------------|------------------------------------------------------|
| 002                        | KDB                                                         | 23:30~00:30               | 23:50                       | 00:15 Every second Sunday of the month 00:00~04:00    |
| 003                        | IBK                                                         | 24:00~00:30               | 23:50                       | 00:12 Every Sunday 00:00~00:30                       |
| 004                        | Kookmin Bank                                                | 24:00~00:30               | 23:50                       | 00:12 Every third Sunday of the month 00:00~00:30, 05:00~05:30 |
| 007                        | Suhyup Bank                                                 | 23:50~00:30               | 23:30                       | 00:30 None                                            |
| 011                        | Nonghyup Bank                                               | 24:00~00:30               | 23:50                       | 00:12 Every third Monday of the month 00:00~04:00     |
| 020                        | Woori Bank                                                  | 23:50~00:30               | 23:50                       | 00:10 Every second Sunday of the month 02:00~06:00    |
| 023                        | SC Bank                                                     | 23:30~00:30               | 23:50                       | 00:12 None                                            |
| 027                        | Korea Citi Bank                                             | 23:40~00:30               | 23:50                       | 00:30 Daily 00:30~04:30                               |
| 031                        | Daegu Bank                                                  | 23:40~00:30               | 23:50                       | 00:05 None                                            |
| 032                        | Busan Bank                                                  | 23:30~00:30               | 23:50                       | 00:05 None                                            |
| 034                        | Kwangju Bank                                                | 23:40~00:30               | 23:50                       | 00:05 Every second Sunday of the month 02:00~06:00    |
| 035                        | Jeju Bank                                                   | 23:40~00:30               | 23:50                       | 00:12 Every Sunday 04:30~05:00                        |
| 037                        | Jeonbuk Bank                                                | 24:00~00:30               | 23:50                       | 00:05 Every second Saturday of the month 00:00~04:00  |
| 039                        | Kyongnam Bank                                               | 23:40~00:30               | 23:50                       | 00:05 Every second Sunday of the month 00:00~07:00    |
| 045                        | KFCC                                                        | 23:50~00:30               | 23:30                       | 00:30 None                                            |
| 048                        | NACUFOK                                                     | 23:40~00:30               | 23:50                       | 00:05 None                                            |
| 050                        | Mutual Savings Bank                                         | 23:50~00:10               | 23:50                       | 00:35 None                                            |
| 064                        | NFCF                                                        | 23:30~00:30               | 23:30                       | 01:00 None                                            |
| 071                        | Korea Post                                                  | 23:40~00:30               | 23:50                       | 00:05 None                                            |
| 081                        | Hana Bank                                                   | 23:40~00:30               | 23:50                       | 00:15 Every second Sunday of the month 00:00~08:00    |
| 088                        | Shinhan Bank                                                | 23:40~00:30               | 23:50                       | 00:05 None                                            |
| 089                        | Kbank                                                       | 23:40~00:30               | 23:35                       | 00:35 None                                            |
| 090                        | Kakao Bank                                                  | 23:50~00:10               | 23:50                       | 00:05 None                                            |
| 092                        | Toss Bank                                                   | 23:55~00:05               | 23:55                       | 00:05 None                                            |
| 103                        | SBI Savings Bank                                            | 23:55~00:10               | 23:50                       | 00:05 None                                            |
| 209                        | Yuanta Securities                                           | 23:50~00:10               | 23:50                       | 00:10 None                                            |
| 238                        | Mirae Asset Securities                                      | 23:30~00:20               | 23:30                       | 00:20 None                                            |
| 240                        | Samsung Securities                                          | 23:30~00:20               | 23:50                       | 00:10 None                                            |
| 243                        | Korea Investment & Securities                               | 23:40~00:10               | 23:40                       | 00:10 None                                            |
| 247                        | NH Investment & Securities                                  | 23:50~00:15               | 23:50                       | 00:05 None                                            |
| 266                        | SK Securities                                               | 23:50~00:30               | 23:30                       | 00:30 None                                            |
| 267                        | Daishin Securities                                          | 23:55~00:25               | 23:55                       | 00:25 None                                            |
| 278                        | Shinhan Financial Investment                                | 23:25~00:15               | 23:30                       | 00:15 Daily 23:30~00:10, 03:00~03:10                  |
| 280                        | Eugene Investment & Securities                              | 23:30~00:30               | 23:50                       | 00:35 None                                            |

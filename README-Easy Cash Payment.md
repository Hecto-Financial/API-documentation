# Easy Cash Payment Standard WEB API

## Version
2.7

## Initial Release Date
2019.11.29

## Last Revised Date
2023.04.26

## Made by
Hecto Financial IT Division

## History

| Date       | Version | Description                                                                                   | Made by       |
|------------|---------|-----------------------------------------------------------------------------------------------|---------------|
| 2019.11.25 | 1.0     | Initial version release                                                                       | Hyunwoo Kim   |
| 2019.12.19 | 1.1     | Form changed                                                                                  | Hyunwoo Kim   |
| 2020.03.04 | 1.2     | Added ARS identity confirmation code error and changed fields for payment issuing and refund  | Hyunwoo Kim   |
| 2020.05.07 | 1.3     | Changed custAcntKey option for payment request                                                | Hongsik Jeon  |
| 2020.05.21 | 1.4     | API for recurring payment added                                                               | Hongsik Jeon  |
| 2020.06.29 | 1.5     | Added identity confirmation for bank account registration                                     | Hongsik Jeon  |
| 2020.08.26 | 1.6     | Changed UI size for account holder check                                                      | Hongsik Jeon  |
| 2020.09.11 | 1.7     | Modified time for bank regular maintenance                                                    | Hongsik Jeon  |
| 2020.10.19 | 1.8     | Modified issuing information                                                                  | Joowan Ryu    |
| 2020.12.23 | 1.9     | Added merchant registration process                                                           | Joowan Ryu    |
| 2021.03.04 | 2.0     | Added parameter for ARS authentication (receiving the record file)                            | Byeongwoon Jang|
| 2021.03.12 | 2.1     | Added service division delete account key for response                                        | Joowan Ryu    |
| 2021.03.15 | 2.2     | Added inquiry of account holder name (included amount)                                        | Joowan Ryu    |
| 2021.03.25 | 2.3     | Added account occupancy certification service                                                 | Joowan Ryu    |
| 2021.06.25 | 2.4     | Added test call telegraphic                                                                   | Byeongwoon Jang|

## Contents
1. [Outline](#outline)
    1. [Purpose](#purpose)
    2. [Target](#target)
    3. [API URI](#api-uri)
    4. [Caution](#caution)
2. [Easy Cash Payment API General](#easy-cash-payment-api-general)
    1. [Easy Cash Payment Process](#easy-cash-payment-process)
    2. [General](#general)
3. [API Invoke](#api-invoke)
    1. [API Connection Information](#api-connection-information)
    2. [Request and Response Headers](#request-and-response-headers)
    3. [Timeout](#timeout)
    4. [Others](#others)
4. [Security of Important Information](#security-of-important-information)
    1. [Encryption & Decryption of Personal Information and Important Information](#encryption--decryption-of-personal-information-and-important-information)
    2. [Encryption Key for Personal Information](#encryption-key-for-personal-information)
    3. [Anti-forgery Algorithm](#anti-forgery-algorithm)
    4. [Hash Generation Authentication Key](#hash-generation-authentication-key)
5. [Mobile Phone Verification Request](#mobile-phone-verification-request)
    1. [API Summary](#api-summary)
    2. [API URL](#api-url)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial)
    4. [Request Hash Code](#request-hash-code)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant)
6. [Mobile Phone Verification Confirmation](#mobile-phone-verification-confirmation)
    1. [API Summary](#api-summary-1)
    2. [API URL](#api-url-1)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-1)
    4. [Request Hash Code](#request-hash-code-1)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-1)
7. [Account Holder Name Inquiry (Date of Birth Excluded)](#account-holder-name-inquiry-date-of-birth-excluded)
    1. [API Summary](#api-summary-2)
    2. [API URL](#api-url-2)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-2)
    4. [Request Hash Code](#request-hash-code-2)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-2)
8. [Account Holder Name Confirmation (Date of Birth Included)](#account-holder-name-confirmation-date-of-birth-included)
    1. [API Summary](#api-summary-3)
    2. [API URL](#api-url-3)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-3)
    4. [Request Hash Code](#request-hash-code-3)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-3)
9. [Account Holder Name Inquiry (Including Amount)](#account-holder-name-inquiry-including-amount)
    1. [API Summary](#api-summary-4)
    2. [API URL](#api-url-4)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-4)
    4. [Request Hash Code](#request-hash-code-4)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-4)
10. [Account Ownership Verification](#account-ownership-verification)
    1. [API Summary](#api-summary-5)
    2. [API URL](#api-url-5)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-5)
    4. [Request Hash Code](#request-hash-code-5)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-5)
11. [Request for Account Ownership Verification](#request-for-account-ownership-verification)
    1. [API Summary](#api-summary-6)
    2. [API URL](#api-url-6)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-6)
    4. [Request Hash Code](#request-hash-code-6)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-6)
12. [Confirmation of Account Ownership Verification](#confirmation-of-account-ownership-verification)
    1. [API Summary](#api-summary-7)
    2. [API URL](#api-url-7)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-7)
    4. [Request Hash Code](#request-hash-code-7)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-7)
13. [ARS Authentication Request](#ars-authentication-request)
    1. [API Summary](#api-summary-8)
    2. [API URL](#api-url-8)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-8)
    4. [Request Hash Code](#request-hash-code-8)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-8)
14. [Confirmation of ARS Authentication](#confirmation-of-ars-authentication)
    1. [API Summary](#api-summary-9)
    2. [API URL](#api-url-9)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-9)
    4. [Request Hash Code](#request-hash-code-9)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-9)
15. [Account Registration](#account-registration)
    1. [API Summary](#api-summary-10)
    2. [API URL](#api-url-10)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-10)
    4. [Request Hash Code](#request-hash-code-10)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-10)
16. [Account Cancellation](#account-cancellation)
    1. [API Summary](#api-summary-11)
    2. [API URL](#api-url-11)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-11)
    4. [Request Hash Code](#request-hash-code-11)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-11)
17. [Merchant Account Registration](#merchant-account-registration)
    1. [API Summary](#api-summary-12)
    2. [API URL](#api-url-12)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-12)
    4. [Request Hash Code](#request-hash-code-12)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-12)
18. [Payment](#payment)
    1. [API Summary](#api-summary-13)
    2. [API URL](#api-url-13)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-13)
    4. [Request Hash Code](#request-hash-code-13)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-13)
19. [Payment Cancellation/Refund](#payment-cancellationrefund)
    1. [API Summary](#api-summary-14)
    2. [API URL](#api-url-14)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-14)
    4. [Request Hash Code](#request-hash-code-14)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-14)
20. [Approval of Recurring Payment](#approval-of-recurring-payment)
    1. [API Summary](#api-summary-15)
    2. [API URL](#api-url-15)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-15)
    4. [Request Hash Code](#request-hash-code-15)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-15)
21. [Cancellation of Recurring Payment](#cancellation-of-recurring-payment)
    1. [API Summary](#api-summary-16)
    2. [API URL](#api-url-16)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-16)
    4. [Request Hash Code](#request-hash-code-16)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-16)
22. [Recurring Payment Information Inquiry](#recurring-payment-information-inquiry)
    1. [API Summary](#api-summary-17)
    2. [API URL](#api-url-17)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-17)
    4. [Request Hash Code](#request-hash-code-17)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-17)
23. [Recurring Payment Transaction History Inquiry](#recurring-payment-transaction-history-inquiry)
    1. [API Summary](#api-summary-18)
    2. [API URL](#api-url-18)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-18)
    4. [Request Hash Code](#request-hash-code-18)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-18)
24. [Remittance](#remittance)
    1. [API Summary](#api-summary-19)
    2. [API URL](#api-url-19)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-19)
    4. [Request Hash Code](#request-hash-code-19)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-19)
25. [Remittance Account Balance Inquiry](#remittance-account-balance-inquiry)
    1. [API Summary](#api-summary-20)
    2. [API URL](#api-url-20)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-20)
    4. [Request Hash Code](#request-hash-code-20)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-20)
26. [Transaction Result Inquiry](#transaction-result-inquiry)
    1. [API Summary](#api-summary-21)
    2. [API URL](#api-url-21)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-21)
    4. [Request Hash Code](#request-hash-code-21)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-21)
27. [Transaction History Inquiry](#transaction-history-inquiry)
    1. [API Summary](#api-summary-22)
    2. [API URL](#api-url-22)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-22)
    4. [Request Hash Code](#request-hash-code-22)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-22)
28. [Account List Inquiry](#account-list-inquiry)
    1. [API Summary](#api-summary-23)
    2. [API URL](#api-url-23)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-23)
    4. [Request Hash Code](#request-hash-code-23)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-23)
29. [Bank List Inquiry](#bank-list-inquiry)
    1. [API Summary](#api-summary-24)
    2. [API URL](#api-url-24)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-24)
    4. [Request Hash Code](#request-hash-code-24)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-24)
30. [Bank Maintenance Inquiry](#bank-maintenance-inquiry)
    1. [API Summary](#api-summary-25)
    2. [API URL](#api-url-25)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-25)
    4. [Request Hash Code](#request-hash-code-25)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-25)
31. [Account Ownership Verification History Inquiry](#account-ownership-verification-history-inquiry)
    1. [API Summary](#api-summary-26)
    2. [API URL](#api-url-26)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-26)
    4. [Request Hash Code](#request-hash-code-26)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-26)
32. [Account Ownership Verification Bank Maintenance Inquiry](#account-ownership-verification-bank-maintenance-inquiry)
    1. [API Summary](#api-summary-27)
    2. [API URL](#api-url-27)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-27)
    4. [Request Hash Code](#request-hash-code-27)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-27)
33. [KFTC Withdrawal Transfer Information Cancellation Inquiry](#kftc-withdrawal-transfer-information-cancellation-inquiry)
    1. [API Summary](#api-summary-28)
    2. [API URL](#api-url-28)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-28)
    4. [Request Hash Code](#request-hash-code-28)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-28)
34. [Test Call](#test-call)
    1. [API Summary](#api-summary-29)
    2. [API URL](#api-url-29)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-29)
    4. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-29)
35. [Payment Password Confirmation](#payment-password-confirmation)
    1. [API Summary](#api-summary-30)
    2. [API URL](#api-url-30)
    3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-30)
    4. [Request Hash Code](#request-hash-code-30)
    5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-30)
36. [Others](#others)
    1. [Table of Reject Codes](#table-of-reject-codes)
    2. [Financial Institution Identifier](#financial-institution-identifier)
    3. [Bank Regular Maintenance Time](#bank-regular-maintenance-time)


## 1.Outline

### 1.1 Purpose
This document is created for understanding the technical requirements for integration of Easy Cash Payment Non-UI API provided by Hecto Financial and to define detailed specifications.

### 1.2 Target
This document is intended for clients’ developers to integrate Easy Cash Payment through easy payment system of Hecto Financial.

### 1.3 API URI
Easy Cash Payment service provides the following APIs; each API is mapped to a web service URL.

| Function                                   | API                                      | URI                                              | HTTP Method |
|--------------------------------------------|------------------------------------------|--------------------------------------------------|-------------|
| Authentication service                     | Request for self-authentication          | `https://npay.settlebank.co.kr/v1/api/auth/mobile/req` | POST        |
| Confirmation for self-authentication       |                                          | `https://npay.settlebank.co.kr/v1/api/auth/mobile/check` | POST        |
| Name check of the account holder 1         |                                          | `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck1` | POST        |
| Name check of the account holder 2         |                                          | `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck2` | POST        |
| Name check of the account holder (including amount) |                                          | `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheckWithAmount` | POST        |
| Account ownership verification             |                                          | `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownership` | POST        |
| Request for account ownership verification |                                          | `https://npay.settlebank.co.kr/v1/api/auth/ownership/req` | POST        |
| Confirmation for account ownership verification |                                          | `https://npay.settlebank.co.kr/v1/api/auth/ownership/check` | POST        |
| ARS authentication request                 |                                          | `https://npay.settlebank.co.kr/v1/api/auth/ars`   | POST        |
| ARS authentication confirmation            |                                          | `https://npay.settlebank.co.kr/v1/api/auth/arscheck` | POST        |
| ARS authentication (record file is transmitted) |                                          | `http://npay.settlebank.co.kr/v1/api/auth/arsrec` | POST        |
| Account management                         | Account registration                     | `https://npay.settlebank.co.kr/v1/api/acnt/reg`   | POST        |
| Account cancellation                       |                                          | `https://npay.settlebank.co.kr/v1/api/acnt/unreg` | POST        |
| Merchant account registration (*Open Banking account registration) |                                          | `https://npay.settlebank.co.kr/v1/api/acnt/reg/self` | POST        |
| Transfer service                           | Payment                                  | `https://npay.settlebank.co.kr/v1/api/pay/confirm` | POST        |
| Payment cancellation / Refund              |                                          | `https://npay.settlebank.co.kr/v1/api/pay/cancel` | POST        |
| Remittance                                 |                                          | `https://npay.settlebank.co.kr/v1/api/pay/rmt`    | POST        |
| Remittance account confirmation            |                                          | `https://npay.settlebank.co.kr/v2/api/pay/rmt/blc` | POST        |
| Recurring payment                          | Recurring payment approval               | `https://npay.settlebank.co.kr/v1/api/regular/confirm` | POST        |
| Unregister recurring payment               |                                          | `https://npay.settlebank.co.kr/v1/api/regular/unreg` | POST        |
| Recurring payment information check        |                                          | `https://npay.settlebank.co.kr/v1/api/regular/info` | POST        |
| Recurring payment transaction history check|                                          | `https://npay.settlebank.co.kr/v1/api/regular/translist` | POST        |
| Inquiry service                            | Transaction result inquiry               | `https://npay.settlebank.co.kr/v1/api/pay/morw`   | POST        |
| Transaction history inquiry                |                                          | `https://npay.settlebank.co.kr/v1/api/pay/translist` | POST        |
| Account list inquiry                       |                                          | `https://npay.settlebank.co.kr/v1/api/acnt/list`  | POST        |
| Bank list inquiry                          |                                          | `https://npay.settlebank.co.kr/v1/api/bank/list`  | POST        |
| Bank maintenance inquiry                   |                                          | `https://npay.settlebank.co.kr/v1/api/bank/timecheck` | POST        |
| Account ownership verification history inquiry |                                          | `https://npay.settlebank.co.kr/v1/api/auth/ownership/translist` | POST        |
| Account ownership verification bank maintenance inquiry |                                          | `https://npay.settlebank.co.kr/v1/api/bank/timecheck/detail` | POST        |
| KFTC withdrawal transfer information cancellation inquiry |                                          | `https://npay.settlebank.co.kr/v1/api/acnt/isttunreg/list` | POST        |
| Test call                                  |                                          | `http://npay.settlebank.co.kr/v1/api/test/testcall` | POST        |
| Check payment password                     |                                          | `http://npay.settlebank.co.kr/v1/api/auth/pwdcnf`  | POST        |

### 1.4 Caution
- If a test in the operating environment is needed, there should be a consultation with the Hecto Financial representative. The cost caused by operating test transactions done without prior discussion should be paid by the merchant.
- Response parameters may be changed without prior notice. When developing, please develop so that there would be no impact caused by the change in parameters.
- **Merchants that use Easy Cash Payment UI standard payment window:** When using the Easy Payment UI Standard Payment window, if the standard payment window is invoked through an iframe tag, there are cases in which it doesn’t work normally on some browsers or devices. If you are using the Easy Payment UI standard payment window, please refrain from using the iframe tag.

## 2 Easy Cash Payment API General

### 2.1 Easy Cash Payment Process
Easy Cash Payment process is as follows:

![image](https://github.com/Hecto-Financial/API-documentation/assets/73467915/e4612d54-1aac-42e9-b738-7e4bb8a528bb)

(1) Self-authentication with customer’s mobile information 
(3) Self-authentication result inquiry
(5) After phone self-authentication, use the name and account details to execute account holder name inquiry.
(7) Withdrawal transfer registration evidence is created through ARS authentication
(12) Account registration (withdrawal transfer registration)
(14) Pay with the registered account 

### 2.2 General
- Required fields in the request/response parameters use the ‘●’ symbol, and selected fields use the ‘○’ symbol.

#### Data Type
- N: Numeric Characters
- A: Alphabetic Characters
- AN: Both alphabetic and numeric

- 
- The fields for encryption are specified in the descriptions of the request field specification of each API.
- If it is possible to type in Korean in the field, refer to the descriptions for the byte size of Korean.

### Request parameter validation
If there is an error with parameter verification such as missing required values, inconsistency of hash data to determine forgery, and requested variable length check, the following response code is returned:

```json
{
  "outStatCd": "0031",
  "outRsltCd": "ST09",
  "outRsltMsg": "invalid request parameter"
}

```
## 3. API Invoke

### 3.1 API Connection Information
The following is API server connection information:

| Environment | URL                                   |
|-------------|---------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr       |
| Production  | https://npay.settlebank.co.kr         |

### 3.2 Request and Response Headers
API request and response header format are as below:

| Request Header   | Value                             |
|------------------|-----------------------------------|
| Content-type     | application/json;charset=UTF-8    |

| Response Header  | Value                             |
|------------------|-----------------------------------|
| Content-type     | application/json;charset=UTF-8    |

### 3.3 Timeout
The timeout process of API’s response is 30 seconds.

### 3.4 Others
The following describes the general requirement for HTTP specification:
- Provided API is REST-oriented but does not meet the entire specification of REST (Most transaction requests use POST method only).
- For ‘Request’ use POST method only.
- Commonly variable values `&`, `?`, `new line`, `<`, `>` are not allowed.

## 4. Security of Important Information

### 4.1 Encryption & Decryption of Personal Information and Important Information
When sending and receiving data, the following encryption & decryption should be performed for the personal/important information fields:

| Section             | Entry                          | Application                | Encoding          |
|---------------------|--------------------------------|----------------------------|-------------------|
| Personal information| Algorithm                      | AES-256/ECB/PKCS5Padding   | Base64 Encoding   |
| Field               | The name of the person in charge, phone number, mobile phone number, e-mail, account holder’s name, account number, etc. |                        |


### 4.2 Encryption Key for Personal Information
For encryption and decryption of personal information and important information, key information differs depending on the operation environment and is as follows:

| Section           | Encryption Key                  |
|-------------------|---------------------------------|
| Testbed key       | SETTLEBANKISGOODSETTLEBANKISGOOD (32 byte) |
| Production key    | Will be provided by separate notice when the service is carried out |



### 4.3 Anti-forgery Algorithm
To verify whether the request data is forged or falsified, a hash algorithm is used. The hash value generation algorithm is as follows:

| Section   | Entry                     | Application   | Encoding     |
|-----------|---------------------------|---------------|--------------|
| Forgery   | Algorithm                 | SHA-256       | Hex Encoding |

### 4.4 Hash Generation Authentication Key

| Section   | Authentication key        |
|-----------|---------------------------|
| Testbed key | ST190808090913247723 (20 byte) |
| Production key | Will be provided by separate notice when the service is carried out |

## 5. Mobile Phone Verification Request

### 5.1 API Summary
Request a mobile phone verification from the merchant’s server to Hecto Financial’s server. To get the name of the customer (account holder) needed for the account holder verification and one’s mobile phone number which is needed for ARS authentication, mobile phone verification is used. The APP and SMS methods are supported. When there is an APP verification request, the customer’s mobile app is invoked, and when there is an SMS verification request, a 6-digit verification number will be sent to the customer’s device through SMS.

*Mobile phone verification service can only be used for Hecto Financial Easy Payment registration.

### 5.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/mobile/req` |
| Production| `http://npay.settlebank.co.kr/v1/api/auth/mobile/req` |

### 5.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| mchtCustId   | Customer ID              | Unique Customer ID provided by Merchant or Unique Key AES Encryption | AN(100)    | ●        | “honggildong”           |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Uii          | Date of birth/Business registration number | yymmdd+ gender identification, Business registration number (hyphen excluded) AES Encryption | AN(10) | ● | “8001012”               |
| telecomCd    | Operator                 | SKT: 1 KT: 2 LGT: 3 SKT MVNO :4 KT MVNO:5 LGT MVNO:6             | N(1)          | ●        | “1”                     |
| cphoneNo     | Mobile phone number      | 010xxxxyyyy (exclude hyphen) AES Encryption                      | AN(11)        | ●        | “01000001234”           |
| mchtCustNm   | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong” (in Korean) |
| authMthdCd   | Certification Classification | SMS : 1 APP : 2                                                 | N(1)          | ●        | “1”                     |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | Hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 5.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Customer ID (plain text) + Request date + Request time + Date of birth (plain text) + Mobile phone (plain text) + Certification Classification + Authentication key |

### 5.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Parameter         | Description                                                      | Type (Length) | Required | Example                    |
|--------------|-------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Transaction status| Transaction status code (Success/Fail)                           | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code       | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message    | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| Tid          | Authentication key| Authentication key that matches the verification number sent as message | AN(50)     | ●        |                            |
| trdNo        | Transaction number| Hecto Financial transaction number for mobile phone verification | AN(40)        | ●        | “SFP_FIRM12345678901234567890” |

## 6. Mobile Phone Verification Confirmation

### 6.1 API Summary
API that checks the status for the request on mobile phone verification is provided. Through ‘Mobile Phone Verification Request’ API, SMS is sent or APP verification request is made through the customer’s device. The customer runs this and through the API checks the completion status of the verification. In the case of SMS verification method, the request to check is done with the verification number sent. In the case of APP verification, the request to check the completion status is done in the APP.

*Mobile phone verification service can only be used for Hecto Financial Easy Payment registration.

### 6.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/mobile/check` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/mobile/check`   |

### 6.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M200_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| authNo       | Authentication number    | Authentication number sent by SMS (Required if the authentication classification of the mobile phone verification request is SMS) | N(6) | ○ | “123456” |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number       | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 6.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Self Authentication key + Authentication key |

### 6.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(

4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| mchtCustId   | Customer ID         | Customer ID                                                      | AN(100)       | ●        |                            |
| mchtCustNm   | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| cphoneNo     | Mobile phone number | Customer’s mobile phone number AES Decryption                    | AN(11)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |

## 7. Account Holder Name Inquiry (Date of Birth Excluded)

### 7.1 API Summary
Provides an API to check the consistency of the bank, account number, and account holder’s name.
Verifies if the account was opened with customer’s name using customer’s (account holder) name obtained through mobile phone verification, and inputted bank and account number. 
The name of the account holder is inputted. The name of the account holder inputted during request and the name of the account holder given by the bank are compared by Hecto Financial to see if they match and then the success/fail code is given. If the name of the account holder is not inputted and only the bank code and the account number are given, the name of the account holder given by the bank is given as the result value. 
Generally, banks give account holder name with maximum of 12 bytes, but some banks (SC Bank, Nonghyup, Woori, IBK, Jeju. Kwangju, Kakao Bank) give 20 bytes. The account holder names provided by SC Bank use full-width forms. When Hecto Financial provides them to the clients, everything is converted to half-width forms and provided with a maximum of 10 digits.


### 7.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/acnt/ownercheck1` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck1`   |

### 7.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 7.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Authentication key |

### 7.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |

## 8. Account Holder Name Confirmation (Date of Birth Included)

### 8.1 API Summary
Provides an API to check the consistency of the bank, account number, account holder’s name, and date of birth.
Verifies if the account was opened with customer’s name using customer’s (account holder) name, date of birth obtained through mobile phone verification, and inputted bank and account number. 
The name of the account holder is inputted. The name of the account holder inputted during request and the name of the account holder given by the bank are compared by Hecto Financial to see if they match and then the success/fail code is given. If the name of the account holder is not inputted and only the bank code and the account number are given, the name of the account holder given by the bank is given as the result value. 
Generally, banks give account holder name with maximum of 12 bytes, but some banks (SC Bank, Nonghyup, Woori, IBK, Jeju. Kwangju, Kakao Bank) give 20 bytes. The account holder names provided by SC Bank use full-width forms. When Hecto Financial provides them to the clients, everything is converted to half-width forms and provided with a maximum of 10 digits.


### 8.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/acnt/ownercheck2` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck2`   |

### 8.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Uii          | Date of birth/Business registration number | yymmdd+ gender identification, Business registration number (hyphen excluded) AES Encryption | AN(10) | ● | “8001012”               |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 8.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Date of birth (plain text) + Authentication key |

### 8.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |

## 9. Account Holder Name Inquiry (Including Amount)

### 9.1 API Summary
Provides an API to check the consistency of the bank, account number, account holder’s name, and amount.
Verifies if the account was opened with customer’s name using customer’s (account holder) name obtained through mobile phone verification, and inputted bank, account number, and amount. 
The name of the account holder is inputted. The name of the account holder inputted during request and the name of the account holder given by the bank are compared by Hecto Financial to see if they match and then the success/fail code is given. If the name of the account holder is not inputted and only the bank code and the account number are given, the name of the account holder given by the bank is given as the result value. 
Generally, banks give account holder name with maximum of 12 bytes, but some banks (SC Bank, Nonghyup, Woori, IBK, Jeju. Kwangju, Kakao Bank) give 20 bytes. The account holder names provided by SC Bank use full-width forms. When Hecto Financial provides them to the clients, everything is converted to half-width forms and provided with a maximum of 10 digits.


### 9.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed  

 | `https://tbnpay.settlebank.co.kr/v1/api/auth/acnt/ownercheckWithAmount` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheckWithAmount`   |

### 9.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| trnsAmnt     | Transaction amount       | Amount for transaction (Excludes comma)                          | N(13)         | ●        | “5000”                  |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 9.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Amount + Authentication key |

### 9.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |

## 10. Account Ownership Verification

### 10.1 API Summary
Provides the function of sending 1 KRW to the account to check whether the customer actually owns the account. With 1 KRW, the account transaction code that was set when the request was made is sent. The customer can check one’s account and compare the code on the account transaction to verify whether one owns the account.

※ To prevent fraudulent use, the account number check is limited to 5 times a day when operating service starts.
The number of use can be changed upon request.


### 10.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/acnt/ownership` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/acnt/ownership`   |

### 10.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Uii          | Date of birth/Business registration number | yymmdd+ gender identification, Business registration number (hyphen excluded) AES Encryption | AN(10) | ● | “8001012”               |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 10.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Date of birth (plain text) + Authentication key |

### 10.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |

## 11. Request for Account Ownership Verification

### 11.1 API Summary
Provides the function of sending 1 KRW to the account to check whether the customer actually owns the account. With 1 KRW, the account transaction code that was set when the request was made is sent. The customer can check one’s account and compare the code on the account transaction to verify whether one owns the account.
If there are repeated requests over a certain period of time, 1 KRW will not be deposited. Instead, it will respond with processing. A separate agreement is needed to decide the period of time.


### 11.2 API URL

| Section    | URL                                                         |
|------------|-------------------------------------------------------------|
| Testbed    | `https://tbnpay.settlebank.co.kr/v1/api/auth/ownership/req` |
| Production | `https://npay.settlebank.co.kr/v1/api/auth/ownership/req`   |

### 11.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Parameter name          | Description                                                   | Type (Bytes) | Required | Example                     |
|-------------|-------------------------|---------------------------------------------------------------|--------------|----------|-----------------------------|
| hdInfo      | Information             | Parameter Information code                                     | AN(50)       | ●        | “SPAY_AA10_1.0“             |
|             |                         |                                                               |              |          | ※ Fixed value              |
| mchtId      | Merchant ID             | Unique merchant ID provided by Hecto Financial                 | AN(8)        | ●        | “midtest”                   |
| mchtTrdNo   |                         | Merchant transaction number                                     | AN(100)      | ●        | “OID201902210001”           |
|             | Merchant transaction number | * exclude Korean                                           |              |          |                             |
| mchtCustId  | Customer ID             | Unique customer ID sent by Merchant or Unique Key (AES Encryption) | AN(100)  | ●        | “honggildong”               |
| reqDt       | Request date            | yyyyMMdd                                                      | AN(8)        | ●        | “20191231”                  |
| reqTm       | Request time            | HH24MISS                                                      | AN(6)        | ●        | “120000”                    |
| mchtCustNm  | Account holder name     | Account holder name (Korean 3 byte) (AES Encryption)           | AN(20)       | ○        | “Hong Gildong” (in Korean)  |
| bankCd      | Bank code               | Refer to Table of Financial Institution Codes                  | AN(3)        | ●        | “004”                       |
| custAcntNo  | Account number          | Account number exclude hyphen (AES Encryption)                 | AN(15)       | ●        | “1234567890”                |
| authType    | Authentication number type | Type of generated authentication number                      | AN(1)        | ●        | “1” : Three-digit number    |
|             |                         |                                                               |              |          | “2” : English capital letter + two-digit number |
|             |                         |                                                               |              |          | “3” : Combination of 4 letter Korean word |
|             |                         |                                                               |              |          | “4” : 4 digit number        |
| remitterNm  | Sender information (Remitter name) | Fixed string of letters for the front or back of the code content that will be on the customer’s bank account (English capital letters or Korean letters) (AES Encryption) | AN(8)  | ○        | “Ebay” or “ㅣ” (Maximum 4 letters) |
|             |                         |                                                               |              |          | ※ Three random digits (numbers, Korean letters) are combined and put on the front or the back of the requested letters on the account transaction of the customer |
|             |                         | Customer Code Content                                         |              |          | e.g) EbayA12 or A12Ebay     |
|             |                         | ※ If the authentication number type is “3” and the authentication number generator is “automatically generated”, the item could be ignored. |              |          |                             |
|             |                         | ※ If the authentication number type is “4”, it supports maximum of 3 letters. |              |          |                             |
| textPos     | Position of sender information (Remitter name) | Sender information (Remitter name) Position of letters | AN(1)        | ○        | “F” : Prefix or “R” : Postfix Default: “R” |
| authVldTm   | Authentication expiry time | Per second                                                  | N(5)         | ○        | “600”                       |
|             |                         | ※ After requesting account ownership verification and if there was a success response, based on transaction number generation, the verification time is set. |              |          | Default : 600 seconds       |
| apintTm     | Repeat request prevention time | Per second                                                 | N(5)         | ○        | “600”                       |
|             |                         | ※ When there are repeated requests within the designated time, 1 KRW will not be deposited. Instead, it will respond with processing |              |          | Default: 600 seconds        |
| custIp      | Customer’s IP address   | Customer’s device’s IP address, Not the Merchant server’s IP   | AN(15)       | ○        | “127.0.0.1”                 |
|             |                         | Needed when setting IP blocking policy                        |              |          |                             |
| pktHash     | Hash data               | Hash data generated by sha256 method                           | AN(200)      | ●        |                             |

### 11.4 Request Hash Code

| Section      | Combination field                                                                 |
|--------------|-----------------------------------------------------------------------------------|
| pktHash value| Merchant ID + Request date + Request time + Bank code + Account number (Plain text) + Authentication key |

### 11.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Parameter name          | Description                                                   | Type (Bytes) | Required | Example                     |
|-------------|-------------------------|---------------------------------------------------------------|--------------|----------|-----------------------------|
| outStatCd   | Transaction status      | Transaction status code (Success/Fail)                         | AN(4)        | ●        | Success: 0021               |
|             |                         |                                                               |              |          | Fail: 0031                  |
| outRsltCd   | Reject code             | Refer to reject code table                                     | AN(4)        | ●        |                             |
| outRsltMsg  | Result message          | When an error occurs, a message on error is sent               | AN(300)      | ●        | “Success”                   |
| mchtTrdNo   |                         | Merchant transaction number                                     | AN(100)      | ●        | “OID201902210001”           |
|             | Merchant transaction number | * Excluded Korean                                           |              |          |                             |
| trdNo       | Transaction number      | Hecto Financial Transaction number                             | AN(40)       | ●        | “SFP_FIRM12345678901234567890” |
| mchtCustId  | Customer ID             | Unique customer ID sent by Merchant or Unique Key (AES Encryption) | AN(100)  | ●        | “honggildong”               |
| bankChkYn   | Bank maintenance check  | Bank maintenance check based on request time                   | AN(1)        | ●        | “Y” or “N”                  |
|             |                         | ※ In case of regular/irregular maintenance time of bank, if the bank shares about the fail situation or if delayed transaction can be checked, the response will be bank maintenance. |              |          |                             |
| stDtm       | Time and date of maintenance starting | yyyyMMddhhmmss                                      | AN(14)       | ○        | “20200729000900”            |
| edDtm       | Time and date of maintenance ending | yyyyMMddhhmmss                                        | AN(14)       | ○        | “20200729001000”            |


## 12. Confirmation of Account Ownership Verification

### 12.1 API Summary
Confirmation of account ownership verification.

### 12.2 API URL

| Section    | URL                                                            |
|------------|----------------------------------------------------------------|
| Testbed    | `https://tbnpay.settlebank.co.kr/v1/api/auth/ownership/check`  |
| Production | `https://npay.settlebank.co.kr/v1/api/auth/ownership/check`    |

### 12.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                  |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|--------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“          |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”                |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”               |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                 |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”              |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                          |

### 12.4 Request Hash Code

| Section      | Combination Field                                                            |
|--------------|------------------------------------------------------------------------------|
| pktHash value| Merchant ID + Request date + Request time + Authentication key + Authentication key |

### 12.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custIp       | Customer IP Address | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”                |

## 13. ARS Authentication Request

### 13.1 API Summary
Request for ARS authentication.

### 13.2 API URL

| Section    | URL                                                    |
|------------|--------------------------------------------------------|
| Testbed    | `https://tbnpay.settlebank.co.kr/v1/api/auth/ars`      |
| Production | `https://npay.settlebank.co.kr/v1/api/auth/ars`        |

### 13.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| cphoneNo     | Mobile phone number      | Customer’s mobile phone number AES Encryption                    | AN(11)        | ●        | “01000001234”           |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption

## 14. Confirmation of ARS Authentication

### 14.1 API Summary
Confirmation of ARS authentication.

### 14.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/auth/arscheck` |
| Production| `https://npay.settlebank.co.kr/v1/api/auth/arscheck` |

### 14.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number       | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 14.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Authentication key + Authentication key |

### 14.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |

## 15. Account Registration

### 15.1 API Summary
Request for account registration.

### API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/acnt/reg`    |
| Production| `https://npay.settlebank.co.kr/v1/api/acnt/reg`      |

### 15.2 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
|

 custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 15.3 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Authentication key + Authentication key |

### 15.4 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |

## 16. Account Cancellation

### 16.1 API Summary
Request for account cancellation.

### 16.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/acnt/unreg`  |
| Production| `https://npay.settlebank.co.kr/v1/api/acnt/unreg`    |

### 16.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 16.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Authentication key + Authentication key |

### 16.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |

## 17. Merchant Account Registration

### 17.1 API Summary
Request for merchant account registration.

### 17.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/acnt/reg/self` |
| Production| `https://npay.settlebank.co.kr/v1/api/acnt/reg/self` |

### 17.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| custIp       | Customer IP Address      | Customer’s device’s IP address, Not the Merchant server’s IP     | AN(15)        | ○        | “127.0.0.1”             |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 17.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Authentication key + Authentication key |

### 17.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        |

 “Success”                  |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |

## 18. Payment

### 18.1 API Summary
Request a payment from the Merchant server to the Hecto Financial server.
With the account registered through account registration (withdrawal transfer registration), payment (withdrawal transfer from customer’s account) can be made. As a result of executing this API, cash is transferred from the customer's account to the main account of the Merchant.


### 18.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/pay/confirm` |
| Production| `https://npay.settlebank.co.kr/v1/api/pay/confirm`   |

### 18.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trnsAmnt     | Transaction amount       | Amount for transaction (Excludes comma)                          | N(13)         | ●        | “5000”                  |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 18.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Transaction amount + Authentication key |

### 18.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |

## 19. Payment Cancellation/Refund

### 19.1 API Summary
Request cancellation/refund of payment from merchant server to Hecto Financial server.
Refund (payment cancellation) is done for the transaction that was paid (withdrawn) for. The refund of the transaction can be done with the transaction number of the paid transaction.


### 19.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/pay/cancel`  |
| Production| `https://npay.settlebank.co.kr/v1/api/pay/cancel`    |

### 19.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| trdNo        | Transaction number       | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount       | Amount for transaction (Excludes comma)                          | N(13)         | ●        | “5000”                  |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 19.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Transaction number + Transaction amount + Authentication key |

### 19.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |

## 20. Approval of Recurring Payment

### 20.1API Summary
Invoke the WEB API for approval request of recurring payment transactions.
If the first payment was made with the recurring payment status parameter set to ‘Y’ at the time of the request,
Recurring payment is registered to the account (recurring payment key response). After that, the recurring payment key is used to execute recurring payment through the API.


### 20.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/regular/confirm` |
| Production| `https://npay.settlebank.co.kr/v1/api/regular/confirm`   |

### 20.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trnsAmnt     | Transaction amount       | Amount for transaction (Excludes comma)                          | N(13)         | ●        | “5000”                  |
| pktHash

      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 20.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Transaction amount + Authentication key |

### 20.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |

## 21. Cancellation of Recurring Payment

### 21.1 API Summary
Invoke the WEB API to cancel a pre-registered recurring payment.

### 21.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/regular/unreg` |
| Production| `https://npay.settlebank.co.kr/v1/api/regular/unreg`   |

### 21.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 21.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Authentication key + Authentication key |

### 21.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |

## 22. Recurring Payment Information Inquiry

### 22.1 API Summary
Request customer`s recurring payment account information.

### 22.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/regular/info` |
| Production| `https://npay.settlebank.co.kr/v1/api/regular/info`   |

### 22.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 22.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Authentication key + Authentication key |

### 22.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| custNm       | Customer name       | Customer name AES Decryption                                     | AN(60)        | ●        |                            |
| custAcntNo   | Account number      | Customer account number AES Decryption                           | AN(20)        | ●        |                            |
| custGndrCd   | Customer gender code| 1: Male, 2: Female                                               | N(1)          | ●        | “1”                        |
| tid          | Authentication key  | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |

## 23. Recurring Payment Transaction History Inquiry

### 23.1 API Summary
Request for inquiry of recurring payment transaction history.

### 23.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/regular/translist` |
| Production| `https://npay.settlebank.co.kr/v1/api/regular/translist`   |

### 23.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
|

 reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 23.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Authentication key + Authentication key |

### 23.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |
| histList     | Transaction history | List of transaction history                                      | List          | ●        |                            |

## 24. Remittance

### 24.1 API Summary
Request a remittance from the merchant server to the Hecto Financial server.

### 24.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/pay/rmt`     |
| Production| `https://npay.settlebank.co.kr/v1/api/pay/rmt`       |

### 24.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| custAcntNo   | Account number           | Customer account number AES Encryption                           | AN(20)        | ●        | “11012341234”           |
| custAcntBank | Bank code                | Financial institution identifier                                 | AN(3)         | ●        | “004”                   |
| custNm       | Customer name            | Customer name (Korean 3 byte) AES Encryption                     | AN(60)        | ●        | “Hong Gildong”          |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trnsAmnt     | Transaction amount       | Amount for transaction (Excludes comma)                          | N(13)         | ●        | “5000”                  |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 24.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Account number (plain text) + Bank code + Customer name (plain text) + Transaction amount + Authentication key |

### 24.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |

## 25. Remittance Account Balance Inquiry

### 25.1 API Summary
Inquire the balance of the remittance account from the merchant server to the Hecto Financial server.

### 25.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v2/api/pay/rmt/blc` |
| Production| `https://npay.settlebank.co.kr/v2/api/pay/rmt/blc`   |

### 25.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 25.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Authentication key + Authentication key |

### 25.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |
| blcAmnt      | Balance amount      | Balance amount in the remittance account                         | N(13)         | ●        | “10000”                  |

## 26. Transaction Result Inquiry

### 26.1 API Summary
For payment approval result and remit request result, if the result code is ‘in progress’ or if there is a timeout, until there is ‘success’ or ‘fail’ codes, the API should be invoked repeatedly.

### 26.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/pay/morw`    |
| Production| `https://npay.settlebank.co.kr/v1/api/pay/morw`      |

### 26.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd

                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| Tid          | Authentication key       | Authentication key value that is sent as the response value when requested | AN(50)       | ●        | “settlebank20190430034655251226” |
| trdNo        | Transaction number       | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 26.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Transaction number + Request date + Request time + Authentication key |

### 26.5 Response (Hecto Financial -> Merchant)
The columns that respond from Hecto Financial server to the Merchant are as follows:

| Parameter    | Name                | Description                                                      | Type (Length) | Required | Example                    |
|--------------|---------------------|------------------------------------------------------------------|---------------|----------|----------------------------|
| outStatCd    | Status              | Result code (Success /Fail)                                      | AN(4)         | ●        | Success: 0021, Fail: 0031  |
| outRsltCd    | Reject code         | Refer to reject code table                                       | AN(4)         | ●        |                            |
| outRsltMsg   | Result message      | When an error occurs, a message on error is sent                 | AN(300)       | ●        | “Success”                  |
| trdNo        | Transaction number  | Hecto Financial transaction number                               | AN(50)        | ●        | “SFP_FIRM12345678901234567890” |
| trnsAmnt     | Transaction amount  | Amount for transaction                                           | N(13)         | ●        | “5000”                  |
| trnsRslt     | Transaction result  | Transaction result code                                          | AN(4)         | ●        | “0000”                  |

## 27. Transaction History Inquiry

### 27.1 API Summary
Request for inquiry of transaction history.

### 27.2 API URL

| Environment | URL                                                        |
|-------------|------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/pay/translist       |
| Production  | https://npay.settlebank.co.kr/v1/api/pay/translist         |

### 27.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                      | Description                                  | Type (Length) | Required | Example                |
|-------------|---------------------------|----------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information               | Parameter Information code                   | AN(50)        | ●        | “SPAY_LT00_1.0“        |
| mchtId      | Merchant ID               | Unique Merchant ID provided by Hecto Financial | AN(8)         | ●        | “midtest"              |
| pktDivCd    | Task classification       | Payment: RP, Cancellation: RT                | AN(2)         | ●        | “RP”                   |
| mchtTrdNo   | Order number              | Merchant order number * Exclude Korean       | AN(100)       | ●        | "OID201902210001"      |
| mchtTrdDt   | Order date                | yyyyMMdd                                     | AN(8)         | ●        | “20191231”             |
| reqDt       | Request date              | yyyyMMdd                                     | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time              | HH24MISS                                     | AN(6)         | ●        | “120000”               |
| pktHash     | hash data                 | Hash value generated by sha256 method        | AN(200)       | ●        |                        |

#### 27.4 Request Hash Code

| Parameter name | Combination field                                                     |
|----------------|-----------------------------------------------------------------------|
| pktHash value  | Merchant ID + Order number + Order date + Request date + Request time + Authentication key |

#### 27.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter       | Name                         | Description                                | Type (Length) | Description         | Example               |
|-----------------|------------------------------|--------------------------------------------|---------------|----------------------|-----------------------|
| outStatCd       | Transaction status           | Transaction status code (success/fail)     | AN(4)         | ●                    | Success: 0021         |
|                 |                              |                                            |               |                      | Fail: 0031            |
| outRsltCd       | Result code                  | Refer to reject code table                 | AN(4)         | ●                    |                       |
| outRsltMsg      | Result message               | When an error occurs, a message on error is sent | AN(300)  | ●                    | “Processed normally.” |
| dataCnt         | Number of transactions       | When transaction history exists            | N(4)          | ●                    | “10”                  |
| trRecords       | Transaction history          |                                            | <object>      | ●                    |                       |
| --recordOutRsltCd | Transaction result code     | Refer to reject code table                 | AN(4)         | ●                    | “ST00”                |
| --recordOutRsltMsg | Transaction result message | Result message                             | AN(200)       | ●                    | “Success”             |
| --recordOutStatCd | Transaction status code     | Status code                                | AN(4)         | ●                    | Success: 0021         |
|                 |                              |                                            |               |                      | Fail: 0031            |
| --recordPktDivCd  | Transaction classification  | Payment: RP, Cancellation: RT              | AN(2)         | ●                    | “RP”                  |
| --recordMchtTrdNo | Order number                | Merchant Order Number * Exclude Korean letters | AN(100)   | ●                    | "OID203739450"        |
| --recordTrdNo     | Transaction number          | Hecto Financial transaction number         | AN(40)        | ○                    | "SFP_FIRM123454667789"|
| --recordTrdDt     | Transaction date            | yyyymmdd                                   | AN(8)         | ●                    | "20210820"            |
| --recordTrdAmt    | Transaction amount          | Amount to be withdrawn (transaction amount)| N(13)         | ●                    | "1000"                |
| --recordBankCd    | Withdrawal bank code        | Withdrawal bank code                       | AN(3)         | ●                    | “011”                 |
| --recordCustAcntNo | Withdrawal bank account    | Masked account number                      | AN(15)        | ●                    | “831*******11”        |
| --recordSvcDivCd  | Service used during transaction | Service used during transaction       | N(1)          | ○                    | 1 : Firm banking      |

### 28. Account List Inquiry
##### API Summary
Request customer's account list information. Can inquire the list of accounts registered (withdraw transfer registration) in customer’s ID.

#### 28.1 API URL

| Environment | URL                                                        |
|-------------|------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/acnt/list           |
| Production  | https://npay.settlebank.co.kr/v1/api/acnt/list             |

#### 28.2 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                      | Description                                  | Type (Length) | Required | Example                |
|-------------|---------------------------|----------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information               | Parameter Information code                   | AN(50)        | ●        | “SPAY_LA00_1.0“        |
| mchtId      | Merchant ID               | Unique Merchant ID provided by Hecto Financial | AN(8)         | ●        | “midtest"              |
| mchtCustId  | Customer ID               | Unique Customer ID or Unique Key from merchant | AN(100)       | ●        | “honggildong”          |
| bankCd      | Bank code                 | Three-digit bank code                        | AN(3)         | ○        | “004”                  |
| reqDt       | Request date              | yyyyMMdd                                     | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time              | HH24MISS                                     | AN(6)         | ●        | “120000”               |
| pktHash     | hash data                 | Hash value generated by sha256 method        | AN(200)       | ●        | “1000”                 |

#### 28.3 Request Hash Code

| Parameter name | Combination field                                                    |
|----------------|----------------------------------------------------------------------|
| pktHash value  | Merchant ID + Customer ID (plain text) + Request Date + Request Time + Authentication Key |

#### 28.4 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter       | Name                         | Description                                | Type (Length) | Required | Example                |
|-----------------|------------------------------|--------------------------------------------|---------------|----------|------------------------|
| outStatCd       | Transaction status           | Transaction status code (success/fail)     | AN(4)         | ●        | Success: 0021          |
|                 |                              |                                            |               |          | Fail: 0031             |
| outRsltCd       | Reject code                  | Refer to reject code table                 | AN(4)         | ●        |                        |
| outRsltMsg      | Result message               | When an error occurs, a message on error is sent | AN(300)  | ●        | “Processed normally.”  |
| dataCnt         | Number of accounts           | Number of transactions if transaction history exists | N(4)   | ●        | “10”                   |
| acntRecords     | Account history              |                                            | <object>      | ●        |                        |
| --recordTrdDt   | Registration date and time   | yyyyyMMddhhmmss                            | AN(14)        | ●        | "20210820000000"       |
| --recordBankCd  | Bank code                    | Bank code                                  | AN(3)         | ●        | "011"                  |
| --recordCustAcntNoMask | Account number        | Masked account number                      | AN(15)        | ●        | "831*******11"         |
| --recordCustAcntKey | Account number key       | Serial number of account registered for withdrawal transfer | AN(50) | ●  | "d5409fdsl3558"       |
| --recordSvcDivCd | Account registration service classification | Withdrawal transfer registered service | AN(4) | ● | 1 : Firm Banking        |
|                 |                              |                                            |               |          | 2 : Open Banking       |
|                 |                              |                                            |               |          | 3 : Firm & Open Banking|
| --recordUnregDt | Date of cancellation         | yyyyMMddhhmmss                             | AN(14)        | ○        | "20210820000000"       |

## 29. Bank List Inquiry

### 29.1 API Summary
Respond to the list of available banks on merchant side. 
Usually used to invoke the API in the merchant's payment window and configure the UI.


### 29.2 API URL

| Section   | URL                                                  |
|-----------|------------------------------------------------------|
| Testbed   | `https://tbnpay.settlebank.co.kr/v1/api/bank/list`   |
| Production| `https://npay.settlebank.co.kr/v1/api/bank/list`     |

### 29.3 Request (Merchant -> Hecto Financial)
The columns requested by Merchant server from Hecto Financial are defined as follows:

| Parameter    | Parameter Name           | Description                                                      | Type (Length) | Required | Example                 |
|--------------|--------------------------|------------------------------------------------------------------|---------------|----------|-------------------------|
| hdInfo       | Information              | Parameter Information code                                       | AN(50)        | ●        | “SPAY_M100_1.0“         |
| mchtId       | Merchant ID              | Unique Merchant ID provided by Hecto Financial                   | AN(8)         | ●        | “midtest”               |
| reqDt        | Request date             | yyyyMMdd                                                         | AN(8)         | ●        | “20191231”              |
| reqTm        | Request time             | HH24MISS                                                         | AN(6)         | ●        | “120000”                |
| pktHash      | hash data                | Hash value generated by sha256 method                            | AN(200)       | ●        |                         |

### 29.4 Request Hash Code

| Section   | Combination Field                                        |
|-----------|----------------------------------------------------------|
| pktHash value | Merchant ID + Request date + Request time + Authentication key + Authentication key |

#### 29.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter        | Name                      | Description                                | Type (Length) | Description         | Example                |
|------------------|---------------------------|--------------------------------------------|---------------|----------------------|------------------------|
| outStatCd        | Transaction status        | Transaction status code (success/fail)     | AN(4)         | ●                    | Success: 0021          |
|                  |                           |                                            |               |                      | Fail: 0031             |
| outRsltCd        | Reject code               | Refer to reject code table                 | AN(4)         | ●                    |                        |
| outRsltMsg       | Result message            | When an error occurs, a message on error is sent | AN(300)  | ●                    | “Processed normally.”  |
| dataCnt          | Number of banks           | Number of transactions if transaction history exists | N(4)   | ●                    | “10”                   |
| bankRecords      | Bank records              |                                            | <object>      | ●                    |                        |
| --recordBankCd   | Bank code                 | Bank code                                  | AN(3)         | ●                    | “004”                  |
| --recordBankNm   | Bank name                 | Bank name (Korean 3 byte)                  | AN(45)        | ●                    | “Kookmin bank”         |
| --recordBankOrd  | Order                     | Displayed order in UI                      | N(2)          | ●                    |                        |
| --recordUseYn    | Availability              | Bank opening and availability              | A(1)          | ●                    | “Y”                    |

### 30. Bank Maintenance Inquiry

#### 30.1 API Summary
Check the maintenance status of each bank.

#### 30.2 API URL

| Environment | URL                                                        |
|-------------|------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/bank/timecheck      |
| Production  | https://npay.settlebank.co.kr/v1/api/bank/timecheck        |

#### 30.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                | Description                               | Type (Length) | Required | Example                |
|-------------|---------------------|-------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information         | Parameter Information code                | AN(50)        | ●        | “SPAY_TP00_1.0“        |
| pktDivCd    | Task classification | RE, DE, RP, RT, NA, NN, AA, AR            | AN(2)         | ●        | RE: Account registration, DE: Account termination, RP: Payment, RT: Payment cancellation (refund), NA: Account holder name inquiry (not including date of birth), NN : Account holder name inquiry (including date of birth), AA: Account ownership verification, AR: Remittance |
| mchtId      | Merchant ID         | Unique Merchant ID provided by Hecto Financial | AN(8)     | ●        | “midtest"              |
| reqDt       | Request date        | yyyyMMdd                                  | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time        | HH24MISS                                  | AN(6)         | ●        | “120000”               |
| bankCd      | Bank code           | Three-digit bank code                     | AN(3)         | ●        | “004”                  |
| pktHash     | hash data           | Hash value generated by sha256 method     | AN(200)       | ●        |                        |

#### 30.4 Request Hash Code

| Parameter name | Combination field                                                   |
|----------------|----------------------------------------------------------------------|
| pktHash value  | Merchant ID + Request date + Request time + Bank code + Authentication Key |

#### 30.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Name                        | Description                                    | Type (Length) | Required | Example                |
|-------------|-----------------------------|------------------------------------------------|---------------|----------|------------------------|
| outStatCd   | Transaction status          | Transaction status code (success/fail)          | AN(4)         | ●        | Success: 0021          |
|             |                             |                                                |               |          | Fail: 0031             |
| outRsltCd   | Reject code                 | Refer to reject code table                      | AN(4)         | ●        |                        |
| outRsltMsg  | Result message              | When an error occurs, a message on error is sent | AN(300)       | ●        | “Processed normally.”  |
| bankCd      | Bank code                   | Three-digit bank code                           | AN(3)         | ●        | “004”                  |

### 31. Account Ownership Verification History Inquiry

#### 31.1 API Summary
Can inquire account ownership verification history

#### 31.2 API URL

| Environment | URL                                                               |
|-------------|-------------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/auth/ownership/translist   |
| Production  | http://npay.settlebank.co.kr/v1/api/auth/ownership/translist      |

#### 31.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                         | Description                                                 | Type (Length) | Required | Example                       |
|-------------|------------------------------|-------------------------------------------------------------|---------------|----------|-------------------------------|
| hdInfo      | Information                  | Parameter Information code                                   | AN(50)        | ●        | “SPAY_LT00_1.0”               |
| mchtId      | Merchant ID                  | Unique Merchant ID provided by Hecto Financial               | AN(8)         | ●        | “midtest”                     |
| orgTrdDt    | Original transaction date    | Original transaction date (yyyyMMdd)                         | AN(6)         | ●        |                               |
| mchtTrdNo   | Order number                 | Merchant order number                                        | AN(100)       | ○        | "OID201902210001"             |
| trdNo       | Transaction number           | Transaction number given as response when Hecto Financial transaction number verification is requested | AN(50) | ●        | "SFP_FIRM123344568901234567890" |
| mchtCustId  | Customer ID                  | Unique Customer ID or Unique Key from merchant (AES encryption) | AN(100)   | ●        | "honggildong"                 |
| reqDt       | Request date                 | yyyyMMdd                                                    | AN(8)         | ●        | “20191231”                    |
| reqTm       | Request time                 | HH24MISS                                                    | AN(6)         | ●        | “120000”                      |
| custIp      | Customer ID address          | Customer’s device’s IP address, Not the Merchant server’s IP | AN(15)        | ○        | “127.0.0.1”                   |
| pktHash     | Hash data                    | Hash value generated by sha256 method                        | AN(200)       | ●        |                               |

#### 31.4 Request Hash Code

| Parameter name | Combination field                                                               |
|----------------|---------------------------------------------------------------------------------|
| pktHash value  | Merchant ID + Request date + Request time + Merchant transaction number + Authentication Key |

#### 31.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Name                        | Description                                                 | Type (Length) | Required | Example                       |
|-------------|-----------------------------|-------------------------------------------------------------|---------------|----------|-------------------------------|
| outStatCd   | Transaction status          | Transaction status code (success/fail)                       | AN(4)         | ●        | Success: 0021                 |
|             |                             |                                                             |               |          | Fail: 0031                    |
| outRsltCd   | Reject code                 | Refer to reject code table                                   | AN(4)         | ●        |                               |
| outRsltMsg  | Result message              | When an error occurs, a message on error is sent             | AN(300)       | ●        | “Success”                     |
| mchtTrdNo   | Merchant transaction number | Merchant transaction number (Exclude Korean)                 | AN(100)       | ●        | "OID20210830000000"           |
| mchtCustId  | Customer ID                 | Unique Customer ID or Unique Key from merchant (AES encryption) | AN(100)   | ○        | "honggildong"                 |
| trdNo       | Transaction number          | Hecto Financial transaction number                           | AN(50)        | ●        | "SFP_FIRM123454657890"        |
| bankCd      | Bank code                   | Refer to Table of Financial Institution Codes                | AN(3)         | ●        | "004"                         |
| custAcntNo  | Account number (masked)     | Account number excluding hyphen                              | AN(15)        | ●        | "12345********890"            |
| custAcntSumry | Account code content      | Code content on the customer’s account (AES encryption)      | AN(21)        | ●        | "EbayA12"                     |
| reqTrdDtm   | Date and time of verification request | yyyyMMddhhmmss                                     | AN(14)        | ●        | "20210830000000"              |
| chkTrdDtm   | Date and time of checking verification | yyyyMMddhhmmss                                     | AN(14)        | ●        | "20210830000000"              |
| trdStatCd   | Verification transaction status | Verification success / fail                              | AN(4)         | ●        | Success: 0021                 |
|             |                             |                                                             |               |          | Fail: 0031                    |
| trdRsltCd   | Verification fail code      | Refer to reject code table                                   | AN(4)         | ●        |                               |
| trdRsltMsg  | Verification result message | When an error occurs, a message on error is sent             | AN(600)       | ●        | “Success”                     |

### 32. Account Ownership Verification Bank Maintenance Inquiry 

#### 32.1 API Summary
Can check the status of bank maintenance

#### 32.2 API URL

| Environment | URL                                                               |
|-------------|-------------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/auth/ownership/translist   |
| Production  | http://npay.settlebank.co.kr/v1/api/auth/ownership/translist      |

#### 32.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                | Description                             | Type (Length) | Required | Example               |
|-------------|---------------------|-----------------------------------------|---------------|----------|-----------------------|
| hdInfo      | Information         | Parameter Information code              | AN(50)        | ●        | “SPAY_TP1W_1.0”       |
| pktDivCd    | Task classification | AA                                      | AN(2)         | ●        | AA: Account ownership verification |
| mchtId      | Merchant ID         | Unique Merchant ID provided by Hecto Financial | AN(8)  | ●        | “midtest”             |
| reqDt       | Request date        | yyyyMMdd                                | AN(8)         | ●        | “20191231”            |
| reqTm       | Request time        | HH24MISS                                | AN(6)         | ●        | “120000”              |
| bankCd      | Bank code           | Three-digit bank code                   | AN(3)         | ●        | "004"                 |
| pktHash     | Hash data           | Hash value generated by sha256 method   | AN(200)       | ●        |                       |

#### 32.4 Request Hash Code

| Parameter name | Combination field                                                   |
|----------------|----------------------------------------------------------------------|
| pktHash value  | Merchant ID + Request date + Request time + Bank code + Authentication Key |

#### 32.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Name                        | Description                                    | Type (Length) | Required | Example                |
|-------------|-----------------------------|------------------------------------------------|---------------|----------|------------------------|
| outStatCd   | Transaction status          | Transaction status code (success/fail)          | AN(4)         | ●        | Success: 0021          |
|             |                             |                                                |               |          | Fail: 0031             |
| outRsltCd   | Reject code                 | Refer to reject code table                      | AN(4)         | ●        |                        |
| outRsltMsg  | Result message              | When an error occurs, a message on error is sent | AN(300)       | ●        | “Success”              |
| bankCd      | Bank code                   | Three-digit bank code                           | AN(3)         | ●        | "004"                  |
| bankChkYn   | Bank maintenance check      | Bank maintenance check based on request time    | AN(1)         | ●        | "Y" or "N"             |
| stDtm       | Starting time and date of maintenance | yyyyMMddhhmmss                          | AN(14)        | ○        | "20210830000000"       |
| edDtm       | Ending time and date of maintenance | yyyyMMddhhmmss                            | AN(14)        | ○        | "20210830000000"       |

### 33. KFTC Withdrawal Transfer Information Cancellation Inquiry

#### 33.1 API Summary
If a customer applies for account cancellation from a financial institution (bank, etc.) or the KFTC (PAYINFO), the customer account’s withdrawal and transfer will be canceled from the Hecto Financial Easy Payment System one business day after the day of application (before 11 AM). The API is provided for consistency with the merchant’s customer withdrawal transfer information. From 11 AM onwards, the list of information on withdrawal transfer accounts canceled on the day can be checked.

#### 33.2 API URL

| Environment | URL                                                        |
|-------------|------------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/acnt/isttunreg/list |
| Production  | https://npay.settlebank.co.kr/v1/api/acnt/isttunreg/list   |



#### 33.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                      | Description                                     | Type (Length) | Required | Example                |
|-------------|---------------------------|-------------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information               | Parameter Information code                      | AN(50)        | ●        | “SPAY_PI0C_1.0“        |
| mchtId      | Merchant ID               | Unique Merchant ID provided by Hecto Financial  | AN(8)         | ●        | “midtest"              |
| unregDt     | Cancellation date         | Date of cancellation of customer account’s withdrawal transfer | AN(8) | ●        | “20191230”             |
| reqDt       | Request date              | yyyyMMdd                                        | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time              | HH24MISS                                        | AN(6)         | ●        | “120000”               |
| pktHash     | hash data                 | Hash value generated by sha256 method           | AN(200)       | ●        |                        |

#### 33.4 Request Hash Code

| Parameter name | Combination field                                                    |
|----------------|----------------------------------------------------------------------|
| pktHash value  | Merchant ID + Cancellation date + Request date + Request time + Authentication key |

#### 33.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter       | Name                         | Description                                | Type (Length) | Required | Example                |
|-----------------|------------------------------|--------------------------------------------|---------------|----------|------------------------|
| outStatCd       | Transaction status           | Transaction status code (success/fail)     | AN(4)         | ●        | Success: 0021          |
|                 |                              |                                            |               |          | Fail: 0031             |
| outRsltCd       | Reject code                  | Refer to reject code table                 | AN(4)         | ●        |                        |
| outRsltMsg      | Result message               | When an error occurs, a message on error is sent | AN(300)  | ●        | “Processed normally.”  |
| unregDt         | Cancellation date            | yyyyMMdd                                    | AN(14)        | ●        | "20210830"             |
| dataCnt         | Number of canceled accounts  | Number of accounts with withdrawal transfer canceled through KFTC | N(4) | ●        | “10”                   |
| unregAcntRecords | List of canceled accounts   |                                            | <object>      | ●        |                        |
| --recordMchtCustId | Customer ID               | Unique Customer ID or Unique Key from merchant (AES encryption) | AN(100) | ● | “honggildong”        |
| --recordBankCd   | Bank code                   | Bank code                                  | AN(3)         | ●        | “004”                  |
| --recordCustAcntNo | Account number            | Masked account number (ex: 234*********123) | AN(15)        | ●        | “234*********123”      |
| --recordCustAcntKey | Account number key       | Serial number of account registered for withdrawal transfer | AN(50) | ● | “d894kjd93kd9” |
| --recordUnregDtm | Cancellation date and time  | yyyyMMddhhmmss                             | AN(14)        | ●        | "20210831080000"       |
| --recordUnregOcuIstt | Cancellation generating institution | Financial company/integrated management system | N(1) | ● | 1: Financial company, 4: Integrated management system |
| --recordUnregDivCd | Cancellation classification | 0/1/2/9/N                                | AN(1)         | ●        | 0 : Customer request, 1: Financial company arbitrary cancellation (automatic transfer, etc.), 2: Financial company account change, 9: Others, N : Absence of consent data |

### 35. Test Call

#### 35.1 API Summary
Request a test call from the merchant server to the Hecto Financial server. The Alive status of the service is checked with it.

#### 35.2 API URL

| Environment | URL                                                       |
|-------------|-----------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/test/testcall      |
| Production  | http://npay.settlebank.co.kr/v1/api/test/testcall         |

#### 35.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                      | Description                               | Type (Length) | Required | Example                |
|-------------|---------------------------|-------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information               | Parameter Information code                | AN(50)        | ●        | “SPAY_TC00_1.0“        |
| mchtId      | Merchant ID               | Unique Merchant ID provided by Hecto Financial | AN(8)    | ●        | “midtest"              |
| procDiv     | Task classification       | A0 : Withdrawal registration, XX : ARS Scenario | AN(2)   | ○        | “TC” : Test call       |
| mchtTrdNo   | Transaction number        | Merchant transaction number (Exclude Korean) | AN(100)   | ●        | “OID201902210001”      |
| reqDt       | Request date              | yyyyMMdd                                  | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time              | HH24MISS                                  | AN(6)         | ●        | “120000”               |

#### 35.4 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Name                      | Description                               | Type (Length) | Required | Example                |
|-------------|---------------------------|-------------------------------------------|---------------|----------|------------------------|
| outStatCd   | Transaction status        | Transaction status code (success/fail)    | AN(4)         | ●        | Success: 0021          |
|             |                           |                                           |               |          | Fail: 0031             |
| outRsltCd   | Reject code               | Refer to reject code table                | AN(4)         | ●        |                        |
| outRsltMsg  | Result message            | When an error occurs, a message on error is sent | AN(300)  | ●        | “Processed normally.”  |
| mchtTrdNo   | Order number              | Merchant order number (Exclude Korean)    | AN(100)       | ●        | “OID201902210001”      |
| trdNo       | Transaction number        | Hecto Financial transaction number        | AN(40)        | ●        | “SFP_FIRM12345678901234567890” |

### 36. Payment Password Confirmation

#### 36.1 API Summary
From the Merchant server to Hecto Financial server, payment password confirmation is requested. Whether the inputted payment password for the customer’s ID matches the registered payment password is checked.

#### 36.2 API URL

| Environment | URL                                                       |
|-------------|-----------------------------------------------------------|
| Testbed     | https://tbnpay.settlebank.co.kr/v1/api/auth/pwdcnf        |
| Production  | http://npay.settlebank.co.kr/v1/api/auth/pwdcnf           |

#### 36.3 Request (Merchant -> Hecto Financial)

The columns requested by Merchant server from Hecto Financial are defined as follows.

| Parameter   | Name                      | Description                               | Type (Length) | Required | Example                |
|-------------|---------------------------|-------------------------------------------|---------------|----------|------------------------|
| hdInfo      | Information               | Parameter Information code                | AN(50)        | ●        | “SPAY_PW00_1.0”        |
| mchtId      | Merchant ID               | Unique Merchant ID provided by Hecto Financial | AN(8)    | ●        | “midtest"              |
| mchtTrdNo   | Order number              | Merchant order number (Exclude Korean)    | AN(100)       | ●        | “OID201902210001”      |
| mchtCustId  | Customer ID               | Unique Customer ID provided by Merchant or Unique Key (AES Encryption) | AN(100) | ● | "honggildong"        |
| reqDt       | Request date              | yyyyMMdd                                  | AN(8)         | ●        | “20191231”             |
| reqTm       | Request time              | HH24MISS                                  | AN(6)         | ●        | “120000”               |
| pmtPwd      | Payment password          | The encrypted value of the six numbers set by the customer (AES encryption) | AN(24) | ● | "314257" |
| custIp      | Customer IP Address       | Customer’s device’s IP address, Not the Merchant server’s IP | AN(15)   | ○        | “127.0.0.1”            |
| pktHash     | hash data                 | Hash value generated by sha256 method     | AN(200)       | ●        |                        |

#### 36.4 Request Hash Code

| Parameter name | Combination field                                                               |
|----------------|---------------------------------------------------------------------------------|
| pktHash value  | Merchant ID + Customer ID (Plain text) + Request date + Request time + Payment password (plain text) + Authentication key |

#### 36.5 Response (Hecto Financial -> Merchant)

The columns that respond from Hecto Financial server to the Merchant are as follows.

| Parameter   | Name                      | Description                                                 | Type (Length) | Required | Example                       |
|-------------|---------------------------|-------------------------------------------------------------|---------------|----------|-------------------------------|
| outStatCd   | Transaction status        | Transaction status code (success/fail)                       | AN(4)         | ●        | Success: 0021                 |
|             |                           |                                                             |               |          | Fail: 0031                    |
| outRsltCd   | Reject code               | Refer to reject code table                                   | AN(4)         | ●        | ST01 : If the payment password does not exist, this response code is returned |
| outRsltMsg  | Result message            | When an error occurs, a message on error is sent             | AN(300)       | ●        | “Processed normally.”         |
| mchtCustld  | Customer ID               | Unique Customer ID provided by Merchant or Unique Key (AES Encryption) | AN(100)   | ●        | "honggildong"                 |
| mchtTrdNo   | Order number              | Merchant order number (Exclude Korean)                       | AN(100)       | ○        | "OLD20190221001"              |
| trdNo       | Transaction number        | Hecto Financial transaction number                           | AN(40)        | ●        | “SFP_FIRM12345678901234567890” |


### 37. Others

#### 37.1 Table of Reject Codes
The server-side response rejection code field details are as follows.
*The recent common reject codes can be downloaded from common reject codes(https://develop.sbsvc.online/22/bbsList.do?tx=R&articleSeq=522). 

| Code | Description                          | Code | Description                                      |
|------|--------------------------------------|------|--------------------------------------------------|
| 0021 | Success                              | 0031 | Fail                                             |
| ST01 | Non-Existent Account                 | ST02 | Invalid Account                                  |
| ST03 | Double withdrawal has occurred       | ST04 | System Error During VAN Request                  |
| ST05 | No VAN Response Information          | ST06 | No transaction number information                |
| ST07 | Communication disruption             | ST08 | Already registered account                       |
| ST09 | Invalid Request                      | ST10 | Internal System Error                            |
| ST11 | Bank maintenance time                | ST12 | Insufficient balance in withdrawal account       |
| ST13 | No ARS authentication result         | ST14 | ARS authentication request values differ         |
| ST15 | Automatic transfer canceled account  | ST16 | Withdrawal account transaction restriction       |
| ST17 | Resident Number Business Number Error| ST18 | Account error (Easy Account Registration not possible) |
| ST19 | Other transactions impossible        | ST20 | Account error                                    |
| ST21 | No recipient account                 | ST22 | Legally Restricted Accounts                      |
| ST23 | A non-real-name account              | ST24 | Account holder mismatch                          |
| ST25 | Already canceled transaction         | ST26 | Cancellation amount error                        |
| ST27 | ARS authentication failed            | ST28 | Unable to receive ARS                            |
| ST29 | Account registration in progress     | ST30 | Refund in progress                               |
| ST31 | Double remittance occurred           | ST32 | Failed to check payer's name                     |
| ST33 | Exceeded the limit per transaction   | ST34 | Exceeded the daily limit                         |
| ST35 | Fraudulent account                   | ST36 | The connection was lost after a certain period of time. |
| ST37 | Easy Payment cancellation            | ST38 | Request in progress                              |
| ST39 | Duplicate refund request             | ST40 | There is a request in progress                   |
| ST41 | Service capacity exceeded            | ST42 | System BUSY                                      |
| ST43 | The account is already registered.   | ST44 | A non-tradeable bank                             |
| ST50 | Duplicate request                    | ST51 | Cash receipt user already registered             |
| ST52 | Unregistered cash receipt user       | ST53 | Already canceled account                         |
| ST60 | Transaction failed                   | ST61 | Exceeded the limit in amount per transaction     |
| ST62 | Exceeded the daily limit in amount   | ST63 | Exceeded the monthly limit in amount             |
| ST64 | Exceeded the daily limit in number of transactions | ST65 | Exceeded the monthly limit in number of transactions |
| ST66 | Password registration failed         | ST67 | Password mismatch                                |
| ST68 | Service suspension                   | ST69 | The selected payment service is not available due to policy. Please use another payment method. |
| ST70 | The selected payment service is not available due to policy. Please contact Hecto Financial Customer Center (1600-5220) | ST72 | ARS 2nd Authentication Required for Payment      |
| ST75 | Multiple attempts were made with wrong information. The account cannot be used. Please try again on the next day. | ST76 | Multiple attempts were made with wrong information. The account cannot be used. Please try again on the next day. |
| ST86 | Authentication failed (Mobile phone verification) | ST87 | Not registered for Easy Self-Authentication. (Mobile Phone Verification) |
| VTIM | A relay institution TIMEOUT          | ST99 | Easy Payment System is under maintenance         |
| SE01 | The authentication validity time has expired. | SE02 | Verification number mismatch.                    |
| SE03 | Exceeded the number of authentications allowed. | SE04 | Open Banking canceled account. Need to be cancelled. |
| SE05 | Open Banking is not valid for the account. Need to be cancelled. |      |                                                  |

> **Note:** 
> - ST75: Account cannot be used due to account holder inquiry abusing attempt (several account numbers).
> - ST76: Account cannot be used due to account holder inquiry abusing attempt (exceeded number of failures).

#### 37.2 Financial Institution Identifier
Financial institution unique identification codes provided by Hecto Financial are as follows:

| Financial Institution Code | Name of Financial Institution                    | Financial Institution Code | Name of Financial Institution                  |
|----------------------------|--------------------------------------------------|----------------------------|------------------------------------------------|
| 002                        | KDB                                              | 071                        | Korea Post                                     |
| 003                        | IBK                                              | 081                        | Hana Bank                                      |
| 004                        | Kookmin Bank                                     | 088                        | Shinhan Bank                                   |
| 007                        | Suhyup Bank                                      | 089                        | Kbank                                          |
| 011                        | Nonghyup Bank                                    | 090                        | Kakao Bank                                     |
| 012                        | National Agricultural Cooperative Federation (NACF) | 092                        | Toss Bank                                      |
| 020                        | Woori Bank                                       | 103                        | SBI Savings Bank                               |
| 023                        | SC Bank                                          | 209                        | Yuanta Securities                              |
| 027                        | Citi Bank                                        | 238                        | Mirae Asset Securities                         |
| 031                        | Daegu Bank                                       | 240                        | Samsung Securities                             |
| 032                        | Busan Bank                                       | 243                        | Korea Investment & Securities                  |
| 034                        | Kwangju Bank                                     | 247                        | NH Investment & Securities                     |
| 035                        | Jeju Bank                                        | 266                        | SK Securities                                  |
| 037                        | Jeonbuk Bank                                     | 267                        | Daishin Securities                             |
| 039                        | Kyongnam Bank                                    | 278                        | Shinhan Financial Investment                   |
| 045                        | Korean Federation of Community Credit Cooperatives (KFCC) | 280                        | Eugene Investment & Securities                 |
| 048                        | National Credit Union Federation of Korea (NACUFOK) | 287                        | Meritz Securities                              |
| 050                        | Korea Federation of Savings Banks (KFSB)         | 099                        | KFTC                                           |
| 064                        | National Forestry Cooperative Federation (NFCF)  |                            |                                                |

### 37.3 Bank Regular Maintenance Time

Due to the specifics of bank operating hours, Hecto Financial recommends transactions between 01:00 and 23:30. Maintenance time could be extended by bank.

| Code | Name                          | Bank Maintenance Time | Hecto Financial Maintenance Time | Regular Maintenance                                      |
|------|-------------------------------|-----------------------|----------------------------------|----------------------------------------------------------|
| 002  | KDB                           | 23:30~00:30           | 23:50~00:15                      | Every 2nd Sunday of the month 00:00~04:00                |
| 003  | IBK                           | 24:00~00:30           | 23:50~00:12                      | Every Sunday 00:00~00:30                                 |
| 004  | Kookmin Bank                  | 24:00:~00:30          | 23:50~00:12                      | Every 3rd Sunday of the month 00:00~00:30, 05:00~05:30   |
|      |                               |                       |                                  | External work 01:00~06:00 (Intermittent transaction error) |
| 007  | Suhyup Bank                   | 23:50~00:30           | 23:30~00:30                      | None                                                     |
| 011  | Nonghyup Bank                 | 24:00~00:30           | 23:50~00:12                      | Every 3rd Monday of the month 00:00 to 04:00             |
|      |                               |                       |                                  | (Next business day if the date is a public holiday)      |
| 020  | Woori Bank                    | 23:50~00:30           | 23:50~00:10                      | Every 2nd Sunday of the month 02:00 to 06:00             |
|      |                               |                       |                                  | (Work date is notified by bank in advance)               |
| 023  | SC Bank                       | 23:30~00:30           | 23:50~00:12                      | None                                                     |
| 027  | Citi Bank                     | 23:40~00:30           | 23:50~00:30                      | Every day 00:30~04:30                                    |
| 031  | Daegu Bank                    | 23:40~00:30           | 23:50~00:05                      | None                                                     |
| 032  | Busan Bank                    | 23:30~00:30           | 23:50~00:05                      | None                                                     |
| 034  | Kwangju Bank                  | 23:40~00:30           | 23:50~00:05                      | Every 2nd Sunday of the month 02:00~06:00                |
| 035  | Jeju Bank                     | 23:40~00:30           | 23:50~00:12                      | Every Sunday 04:30~05:00                                 |
| 037  | Jeonbuk Bank                  | 24:00~00:30           | 23:50~00:05                      | Every 2nd Saturday of the month 00:00~04:00              |
| 039  | Kyongnam Bank                 | 23:40~00:30           | 23:50~00:05                      | Every 2nd Sunday of the month 00:00~07:00                |
| 045  | KFCC                          | 23:50~00:30           | 23:30~00:30                      | None                                                     |
| 048  | NACUFOK                       | 23:40~00:30           | 23:50~00:05                      | None                                                     |
| 050  | KFSB                          | 23:50~00:10           | 23:50~00:35                      | None                                                     |
| 064  | NFCF                          | 23:30~00:30           | 23:30~01:00                      | None                                                     |
| 071  | Korea Post                    | 23:40~00:30           | 23:50~00:05                      | Daily 04:00~05:00                                        |
| 081  | Hana Bank                     | 23:40~00:30           | 23:50~00:15                      | Every 2nd Sunday of the month 00:00~08:00                |
| 088  | Shinhan Bank                  | 23:40~00:30           | 23:50~00:05                      | None                                                     |
| 089  | Kbank                         | 23:40~00:30           | 23:35~00:35                      | None                                                     |
| 090  | Kakao Bank                    | 23:50~00:10           | 23:50~00:05                      | None                                                     |
| 092  | Toss Bank                     | 23:55~00:05           | 23:55~00:05                      | None                                                     |
| 103  | SBI Savings Bank              | 23:55~00:10           | 23:50~00:05                      | None                                                     |
| 209  | Yuanta Securities             | 23:50~00:10           | 23:50~00:10                      | None                                                     |
| 238  | Mirae Asset Securities        | 23:30~00:20           | 23:30~00:20                      | None                                                     |
| 240  | Samsung Securities            | 23:30~00:20           | 23:50~00:10                      | None                                                     |
| 243  | Korea Investment & Securities | 23:40~00:10           | 23:40~00:10                      | None                                                     |
| 247  | NH Investment & Securities    | 23:50~00:15           | 23:50~00:05                      | None                                                     |
| 266  | SK Securities                 | 23:50~00:30           | 23:30~00:30                      | None                                                     |
| 267  | Daishin Securities            | 23:55~00:25           | 23:55~00:25                      | None                                                     |
| 278  | Shinhan Financial Investment  | 23:25~00:15           | 23:30~00:15                      | Every day 23:30~00:10, 03:00~03:10                       |
| 280  | Eugene Investment & Securities| 23:30~00:30           | 23:50~00:35                      | None                                                     |
| 287  | Meritz Securities             | 23:50~00:20           | 23:50~00:20                      | None                                                     |


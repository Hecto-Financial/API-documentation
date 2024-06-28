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
1.1. [Purpose](#purpose)
1.2. [Target](#target)
1.3. [API URI](#api-uri)
1.4. [Caution](#caution)
2. [Easy Cash Payment API General](#easy-cash-payment-api-general)
2.1. [Easy Cash Payment Process](#easy-cash-payment-process)
2.2. [General](#general)
3. [API Invoke](#api-invoke)
3.1. [API Connection Information](#api-connection-information)
3.2. [Request and Response Headers](#request-and-response-headers)
3.3. [Timeout](#timeout)
3.4. [Others](#others)
4. [Security of Important Information](#security-of-important-information)
4.1. [Encryption & Decryption of Personal Information and Important Information](#encryption--decryption-of-personal-information-and-important-information)
4.2. [Encryption Key for Personal Information](#encryption-key-for-personal-information)
4.3. [Anti-forgery Algorithm](#anti-forgery-algorithm)
4.4. [Hash Generation Authentication Key](#hash-generation-authentication-key)
5. [Mobile Phone Verification Request](#mobile-phone-verification-request)
5.1. [API Summary](#api-summary)
5.2. [API URL](#api-url)
5.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial)
5.4. [Request Hash Code](#request-hash-code)
5.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant)
6. [Mobile Phone Verification Confirmation](#mobile-phone-verification-confirmation)
6.1. [API Summary](#api-summary-1)
6.2. [API URL](#api-url-1)
6.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-1)
6.4. [Request Hash Code](#request-hash-code-1)
6.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-1)
7. [Account Holder Name Inquiry (Date of Birth Excluded)](#account-holder-name-inquiry-date-of-birth-excluded)
7.1. [API Summary](#api-summary-2)
7.2. [API URL](#api-url-2)
7.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-2)
7.4. [Request Hash Code](#request-hash-code-2)
7.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-2)
8. [Account Holder Name Confirmation (Date of Birth Included)](#account-holder-name-confirmation-date-of-birth-included)
8.1. [API Summary](#api-summary-3)
8.2. [API URL](#api-url-3)
8.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-3)
8.4. [Request Hash Code](#request-hash-code-3)
8.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-3)
9. [Account Holder Name Inquiry (Including Amount)](#account-holder-name-inquiry-including-amount)
9.1. [API Summary](#api-summary-4)
9.2. [API URL](#api-url-4)
9.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-4)
9.4. [Request Hash Code](#request-hash-code-4)
9.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-4)
10. [Account Ownership Verification](#account-ownership-verification)
10.1. [API Summary](#api-summary-5)
10.2. [API URL](#api-url-5)
10.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-5)
10.4. [Request Hash Code](#request-hash-code-5)
10.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-5)
11. [Request for Account Ownership Verification](#request-for-account-ownership-verification)
11.1. [API Summary](#api-summary-6)
11.2. [API URL](#api-url-6)
11.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-6)
11.4. [Request Hash Code](#request-hash-code-6)
11.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-6)
12. [Confirmation of Account Ownership Verification](#confirmation-of-account-ownership-verification)
12.1. [API Summary](#api-summary-7)
12.2. [API URL](#api-url-7)
12.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-7)
12.4. [Request Hash Code](#request-hash-code-7)
12.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-7)
13. [ARS Authentication Request](#ars-authentication-request)
13.1. [API Summary](#api-summary-8)
13.2. [API URL](#api-url-8)
13.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-8)
13.4. [Request Hash Code](#request-hash-code-8)
13.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-8)
14. [Confirmation of ARS Authentication](#confirmation-of-ars-authentication)
14.1. [API Summary](#api-summary-9)
14.2. [API URL](#api-url-9)
14.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-9)
14.4. [Request Hash Code](#request-hash-code-9)
14.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-9)
15. [Account Registration](#account-registration)
15.1. [API Summary](#api-summary-10)
15.2. [API URL](#api-url-10)
15.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-10)
15.4. [Request Hash Code](#request-hash-code-10)
15.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-10)
16. [Account Cancellation](#account-cancellation)
16.1. [API Summary](#api-summary-11)
16.2. [API URL](#api-url-11)
16.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-11)
16.4. [Request Hash Code](#request-hash-code-11)
16.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-11)
17. [Merchant Account Registration](#merchant-account-registration)
17.1. [API Summary](#api-summary-12)
17.2. [API URL](#api-url-12)
17.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-12)
17.4. [Request Hash Code](#request-hash-code-12)
17.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-12)
18. [Payment](#payment)
18.1. [API Summary](#api-summary-13)
18.2. [API URL](#api-url-13)
18.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-13)
18.4. [Request Hash Code](#request-hash-code-13)
18.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-13)
19. [Payment Cancellation/Refund](#payment-cancellationrefund)
19.1. [API Summary](#api-summary-14)
19.2. [API URL](#api-url-14)
19.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-14)
19.4. [Request Hash Code](#request-hash-code-14)
19.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-14)
20. [Approval of Recurring Payment](#approval-of-recurring-payment)
20.1. [API Summary](#api-summary-15)
20.2. [API URL](#api-url-15)
20.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-15)
20.4. [Request Hash Code](#request-hash-code-15)
20.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-15)
21. [Cancellation of Recurring Payment](#cancellation-of-recurring-payment)
21.1. [API Summary](#api-summary-16)
21.2. [API URL](#api-url-16)
21.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-16)
21.4. [Request Hash Code](#request-hash-code-16)
21.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-16)
22. [Recurring Payment Information Inquiry](#recurring-payment-information-inquiry)
22.1. [API Summary](#api-summary-17)
22.2. [API URL](#api-url-17)
22.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-17)
22.4. [Request Hash Code](#request-hash-code-17)
22.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-17)
23. [Recurring Payment Transaction History Inquiry](#recurring-payment-transaction-history-inquiry)
23.1. [API Summary](#api-summary-18)
23.2. [API URL](#api-url-18)
23.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-18)
23.4. [Request Hash Code](#request-hash-code-18)
23.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-18)
24. [Remittance](#remittance)
24.1. [API Summary](#api-summary-19)
24.2. [API URL](#api-url-19)
24.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-19)
24.4. [Request Hash Code](#request-hash-code-19)
24.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-19)
25. [Remittance Account Balance Inquiry](#remittance-account-balance-inquiry)
25.1. [API Summary](#api-summary-20)
25.2. [API URL](#api-url-20)
25.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-20)
25.4. [Request Hash Code](#request-hash-code-20)
25.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-20)
26. [Transaction Result Inquiry](#transaction-result-inquiry)
26.1. [API Summary](#api-summary-21)
26.2. [API URL](#api-url-21)
26.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-21)
26.4. [Request Hash Code](#request-hash-code-21)
26.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-21)
27. [Transaction History Inquiry](#transaction-history-inquiry)
27.1. [API Summary](#api-summary-22)
27.2. [API URL](#api-url-22)
27.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-22)
27.4. [Request Hash Code](#request-hash-code-22)
27.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-22)
28. [Account List Inquiry](#account-list-inquiry)
28.1. [API Summary](#api-summary-23)
28.2. [API URL](#api-url-23)
28.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-23)
28.4. [Request Hash Code](#request-hash-code-23)
28.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-23)
29. [Bank List Inquiry](#bank-list-inquiry)
29.1. [API Summary](#api-summary-24)
29.2. [API URL](#api-url-24)
29.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-24)
29.4. [Request Hash Code](#request-hash-code-24)
29.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-24)
30. [Bank Maintenance Inquiry](#bank-maintenance-inquiry)
30.1. [API Summary](#api-summary-25)
30.2. [API URL](#api-url-25)
30.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-25)
30.4. [Request Hash Code](#request-hash-code-25)
30.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-25)
31. [Account Ownership Verification History Inquiry](#account-ownership-verification-history-inquiry)
31.1. [API Summary](#api-summary-26)
31.2. [API URL](#api-url-26)
31.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-26)
31.4. [Request Hash Code](#request-hash-code-26)
31.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-26)
32. [Account Ownership Verification Bank Maintenance Inquiry](#account-ownership-verification-bank-maintenance-inquiry)
32.1. [API Summary](#api-summary-27)
32.2. [API URL](#api-url-27)
32.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-27)
32.4. [Request Hash Code](#request-hash-code-27)
32.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-27)
33. [KFTC Withdrawal Transfer Information Cancellation Inquiry](#kftc-withdrawal-transfer-information-cancellation-inquiry)
33.1. [API Summary](#api-summary-28)
33.2. [API URL](#api-url-28)
33.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-28)
33.4. [Request Hash Code](#request-hash-code-28)
33.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-28)
34. [Test Call](#test-call)
34.1. [API Summary](#api-summary-29)
34.2. [API URL](#api-url-29)
34.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-29)
34.4. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-29)
35. [Payment Password Confirmation](#payment-password-confirmation)
35.1. [API Summary](#api-summary-30)
35.2. [API URL](#api-url-30)
35.3. [Request (Merchant -> Hecto Financial)](#request-merchant---hecto-financial-30)
35.4. [Request Hash Code](#request-hash-code-30)
35.5. [Response (Hecto Financial -> Merchant)](#response-hecto-financial---merchant-30)
36. [Others](#others)
36.1. [Table of Reject Codes](#table-of-reject-codes)
36.2. [Financial Institution Identifier](#financial-institution-identifier)
36.3. [Bank Regular Maintenance Time](#bank-regular-maintenance-time)

## Outline

### Purpose
This document is created for understanding the technical requirements for integration of Easy Cash Payment Non-UI API provided by Hecto Financial and to define detailed specifications.

### Target
This document is intended for clients’ developers to integrate Easy Cash Payment through easy payment system of Hecto Financial.

### API URI
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

### Caution
- If a test in the operating environment is needed, there should be a consultation with the Hecto Financial representative. The cost caused by operating test transactions done without prior discussion should be paid by the merchant.
- Response parameters may be changed without prior notice. When developing, please develop so that there would be no impact caused by the change in parameters.
- **Merchants that use Easy Cash Payment UI standard payment window:** When using the Easy Payment UI Standard Payment window, if the standard payment window is invoked through an iframe tag, there are cases in which it doesn’t work normally on some browsers or devices. If you are using the Easy Payment UI standard payment window, please refrain from using the iframe tag.

## Easy Cash Payment API General

### Easy Cash Payment Process
Easy Cash Payment process is as follows:

1. Request mobile verification
2. Mobile verification request response
3. Request mobile verification check
4. Mobile verification check response
5. Request account holder name inquiry
6. Account holder name inquiry request response
7. ARS authentication request
8. ARS authentication w/customer’s mobile
9. ARS authentication request response
10. ARS authentication check request
11. ARS authentication check request response
12. Account registration request
13. Account registration request response
14. Payment request to registered account
15. Payment request response

### General
- Required fields in the request/response parameters use the ‘●’ symbol, and selected fields use the ‘○’ symbol.

#### Data Type
- N: Numeric Characters
- A: Alphabetic Characters
- AN: Both alphabetic and numeric

### Request parameter validation
If there is an error with parameter verification such as missing required values, inconsistency of hash data to determine forgery, and requested variable length check, the following response code is returned:

```json
{
  "outStatCd": "0031",
  "outRsltCd": "ST09",
  "outRsltMsg": "invalid request parameter"
}

API Invoke
API Connection Information
The following is API server connection information:

Environment	URL
Testbed	https://tbnpay.settlebank.co.kr
Production	https://npay.settlebank.co.kr
Request and Response Headers
API request and response header format are as below:

Request Header	Value
Content-type	application/json;charset=UTF-8
Response Header	Value
Content-type	application/json;charset=UTF-8
Timeout
The timeout process of API’s response is 30 seconds.

Others
The following describes the general requirement for HTTP specification:

Provided API is REST-oriented but does not meet the entire specification of REST (Most transaction requests use POST method only).
For ‘Request’ use POST method only.
Commonly variable values &, ?, new line, <, > are not allowed.
Security of Important Information
Encryption & Decryption of Personal Information and Important Information
When sending and receiving data, the following encryption & decryption should be performed for the personal/important information fields:

Section	Entry	Application	Encoding
Personal information	Algorithm	AES-256/ECB/PKCS5Padding	Base64 Encoding
Field	The name of the person in charge, phone number, mobile phone number, e-mail, account holder’s name, account number, etc.		
The fields for encryption are specified in the descriptions of the request field specification of each API.
If it is possible to type in Korean in the field, refer to the descriptions for the byte size of Korean.
Encryption Key for Personal Information
For encryption and decryption of personal information and important information, key information differs depending on the operation environment and is as follows:

Section	Encryption Key
Testbed key	SETTLEBANKISGOODSETTLEBANKISGOOD (32 byte)
Production key	Will be provided by separate notice when the service is carried out
Anti-forgery Algorithm
To verify whether the request data is forged or falsified, a hash algorithm is used. The hash value generation algorithm is as follows:

Section	Entry	Application	Encoding
Forgery	Algorithm	SHA-256	Hex Encoding
Hash Generation Authentication Key
Section	Authentication key
Testbed key	ST190808090913247723 (20 byte)
Production key	Will be provided by separate notice when the service is carried out
Mobile Phone Verification Request
API Summary
Request a mobile phone verification from the merchant’s server to Hecto Financial’s server. To get the name of the customer (account holder) needed for the account holder verification and one’s mobile phone number which is needed for ARS authentication, mobile phone verification is used. The APP and SMS methods are supported. When there is an APP verification request, the customer’s mobile app is invoked, and when there is an SMS verification request, a 6-digit verification number will be sent to the customer’s device through SMS.

*Mobile phone verification service can only be used for Hecto Financial Easy Payment registration.

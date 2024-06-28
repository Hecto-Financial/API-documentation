To create a README file for GitHub from the provided "Easy Cash Payment" document, I will follow the same format as the provided README file. Here is the converted content:

```markdown
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
2. [Easy Cash Payment API General](#easy-cash-payment-api-general)
3. [API Invoke](#api-invoke)
4. [Security of Important Information](#security-of-important-information)
5. [Mobile Phone Verification Request](#mobile-phone-verification-request)
6. [Mobile Phone Verification Confirmation](#mobile-phone-verification-confirmation)
7. [Account Holder Name Inquiry (Date of Birth Excluded)](#account-holder-name-inquiry-date-of-birth-excluded)
8. [Account Holder Name Confirmation (Date of Birth Included)](#account-holder-name-confirmation-date-of-birth-included)
9. [Account Holder Name Inquiry (Including Amount)](#account-holder-name-inquiry-including-amount)
10. [Account Ownership Verification](#account-ownership-verification)
11. [Request for Account Ownership Verification](#request-for-account-ownership-verification)
12. [Confirmation of Account Ownership Verification](#confirmation-of-account-ownership-verification)
13. [ARS Authentication Request](#ars-authentication-request)
14. [Confirmation of ARS Authentication](#confirmation-of-ars-authentication)
15. [Account Registration](#account-registration)
16. [Account Cancellation](#account-cancellation)
17. [Merchant Account Registration](#merchant-account-registration)
18. [Payment](#payment)
19. [Payment Cancellation/Refund](#payment-cancellationrefund)
20. [Approval of Recurring Payment](#approval-of-recurring-payment)
21. [Cancellation of Recurring Payment](#cancellation-of-recurring-payment)
22. [Recurring Payment Information Inquiry](#recurring-payment-information-inquiry)
23. [Recurring Payment Transaction History Inquiry](#recurring-payment-transaction-history-inquiry)
24. [Remittance](#remittance)
25. [Remittance Account Balance Inquiry](#remittance-account-balance-inquiry)
26. [Transaction Result Inquiry](#transaction-result-inquiry)
27. [Transaction History Inquiry](#transaction-history-inquiry)
28. [Account List Inquiry](#account-list-inquiry)
29. [Bank List Inquiry](#bank-list-inquiry)
30. [Bank Maintenance Inquiry](#bank-maintenance-inquiry)
31. [Account Ownership Verification History Inquiry](#account-ownership-verification-history-inquiry)
32. [Account Ownership Verification Bank Maintenance Inquiry](#account-ownership-verification-bank-maintenance-inquiry)
33. [KFTC Withdrawal Transfer Information Cancellation Inquiry](#kftc-withdrawal-transfer-information-cancellation-inquiry)
34. [Test Call](#test-call)
35. [Payment Password Confirmation](#payment-password-confirmation)
36. [Others](#others)

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
| Bank list inquiry                          |                                          | `https

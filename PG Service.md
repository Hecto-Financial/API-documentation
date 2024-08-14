# PG Service Standard WEB API

## Content

1. [Outline](#outline)
    - [Purpose](#purpose)
    - [Target](#target)
    - [Specification](#specification)
    - [Others](#others)
2. [Standard Payment Window (UI) Integration](#standard-payment-window-ui-integration)
    - [Description](#description)
    - [Cautions](#cautions)
    - [API URI](#api-uri)
    - [Request and Response Headers](#request-and-response-headers)
    - [Request Parameter Verification](#request-parameter-verification)
    - [Integration Script Provision](#integration-script-provision)
    - [Sample Source Provision](#sample-source-provision)
3. [API Server Integration (Non-UI)](#api-server-integration-non-ui)
    - [Description](#description-1)
    - [API URI](#api-uri-1)
    - [Request and Response Headers](#request-and-response-headers-1)
    - [JSON Request Data Example](#json-request-data-example)
4. [Integration Server](#integration-server)
    - [Server IP Address](#server-ip-address)
    - [Notification Server](#notification-server)
5. [Crucial Information Security](#crucial-information-security)
    - [Encryption/Decryption of Personal Information and Crucial Information](#encryptiondecryption-of-personal-information-and-crucial-information)
    - [Personal Information Encryption Key](#personal-information-encryption-key)
    - [Forgery Prevention Algorithm](#forgery-prevention-algorithm)
    - [Hash Generation Authentication Key](#hash-generation-authentication-key)
6. [Credit Card Payment (UI)](#credit-card-payment-ui)
    - [Cautions](#cautions-1)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial)
    - [Request Parameter Hash Code](#request-parameter-hash-code)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant)
7. [Credit Card WebView](#credit-card-webview)
    - [APP SCHEME Setting](#app-scheme-setting)
    - [Android](#android)
    - [IOS](#ios)
8. [Credit Card Payment API (Non-UI)](#credit-card-payment-api-non-ui)
    - [Payment API Request Parameter (Billkey Issuing Included)](#payment-api-request-parameter-billkey-issuing-included)
    - [Request Parameter Hash Code](#request-parameter-hash-code-1)
    - [Payment API Response Parameter (Billkey Issuing Included)](#payment-api-response-parameter-billkey-issuing-included)
    - [Response Parameter Hash Code](#response-parameter-hash-code)
    - [Billkey Issuing API Request Parameter](#billkey-issuing-api-request-parameter)
    - [Request Parameter Hash Code](#request-parameter-hash-code-2)
    - [Billkey Issuing API Response Parameter](#billkey-issuing-api-response-parameter)
    - [Response Parameter Hash Code](#response-parameter-hash-code-1)
9. [Credit Card Billkey Payment API (Non-UI)](#credit-card-billkey-payment-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-1)
    - [Request Parameter Hash Code](#request-parameter-hash-code-3)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-1)
    - [Response Parameter Hash Code](#response-parameter-hash-code-2)
10. [Credit Card Billkey Deleting API (Non-UI)](#credit-card-billkey-deleting-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-2)
    - [Request Parameter Hash Code](#request-parameter-hash-code-4)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-2)
    - [Response Parameter Hash Code](#response-parameter-hash-code-3)
11. [Credit Card Cancellation (Non-UI)](#credit-card-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-3)
    - [Request Parameter Hash Code](#request-parameter-hash-code-5)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-3)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-1)
12. [Account Transfer Payment (UI)](#account-transfer-payment-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-4)
    - [Request Parameter Hash Code](#request-parameter-hash-code-6)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-4)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-2)
13. [Account Transfer Cancellation (Non-UI)](#account-transfer-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-5)
    - [Request Parameter Hash Code](#request-parameter-hash-code-7)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-5)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-3)
14. [Virtual Account Payment (UI)](#virtual-account-payment-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-6)
    - [Request Parameter Hash Code](#request-parameter-hash-code-8)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-6)
    - [Test Environment Deposit Test API](#test-environment-deposit-test-api)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-4)
15. [Virtual Account Number Issuance (Non-UI)](#virtual-account-number-issuance-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-7)
    - [Request Parameter Hash Code](#request-parameter-hash-code-9)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-7)
    - [Response Parameter Hash Code](#response-parameter-hash-code-4)
    - [Test Environment Deposit Test API](#test-environment-deposit-test-api-1)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-5)
    - [Test Environment Static Virtual Account List](#test-environment-static-virtual-account-list)
    - [PG Virtual Account Bank Code](#pg-virtual-account-bank-code)
16. [Virtual Account Number Issuance Information Change (Non-UI)](#virtual-account-number-issuance-information-change-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-8)
    - [Request Parameter Hash Code](#request-parameter-hash-code-10)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-8)
17. [Virtual Account Number Issuance Cancellation (Non-UI)](#virtual-account-number-issuance-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-9)
    - [Request Parameter Hash Code](#request-parameter-hash-code-11)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-9)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-6)
18. [Virtual Account Refund (Non-UI)](#virtual-account-refund-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-10)
    - [Request Parameter Hash Code](#request-parameter-hash-code-12)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-10)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-7)
19. [010 Virtual Account Payment (UI)](#010-virtual-account-payment-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-11)
    - [Request Parameter Hash Code](#request-parameter-hash-code-13)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-11)
    - [Test Environment Deposit Test API](#test-environment-deposit-test-api-2)
    - [Notification Parameter (

Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-8)
20. [Direct Carrier Billing (DCB) (UI)](#direct-carrier-billing-dcb-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-12)
    - [Request Parameter Hash Code](#request-parameter-hash-code-14)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-12)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-9)
21. [Phone Approval API (Hybrid)](#phone-approval-api-hybrid)
    - [API URI](#api-uri-2)
    - [Request and Response Headers](#request-and-response-headers-2)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-13)
    - [Request Parameter Hash Code](#request-parameter-hash-code-15)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-13)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-10)
22. [Phone Recurring Payment (Non-UI)](#phone-recurring-payment-non-ui)
    - [Description](#description-2)
    - [API URI](#api-uri-3)
    - [Request and Response Headers](#request-and-response-headers-3)
    - [Monthly Recurring Request Parameter (Merchant -> Hecto Financial)](#monthly-recurring-request-parameter-merchant---hecto-financial)
    - [Request Parameter Hash Code](#request-parameter-hash-code-16)
    - [Monthly Recurring Response Parameter (Hecto Financial -> Merchant)](#monthly-recurring-response-parameter-hecto-financial---merchant)
    - [Key Issuance Recurring Payment Request Parameter (Merchant -> Hecto Financial)](#key-issuance-recurring-payment-request-parameter-merchant---hecto-financial)
    - [Key Issuance Recurring Payment Response Parameter (Hecto Financial -> Merchant)](#key-issuance-recurring-payment-response-parameter-hecto-financial---merchant)
23. [Direct Carrier Billing (DCB) Cancellation (Non-UI)](#direct-carrier-billing-dcb-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-14)
    - [Request Parameter Hash Code](#request-parameter-hash-code-17)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-14)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-11)
24. [Direct Carrier Billing (DCB) Refund (Non-UI)](#direct-carrier-billing-dcb-refund-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-15)
    - [Request Parameter Hash Code](#request-parameter-hash-code-18)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-15)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-12)
25. [Teen Cash/Happy Money/Culture Cash/Smart Gift Voucher/Book Voucher/Tmoney Payment (UI)](#teen-cashhappy-moneyculture-cashsmart-gift-voucherbook-vouchertmoney-payment-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-16)
    - [Request Parameter Hash Code](#request-parameter-hash-code-19)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-16)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-13)
26. [Teen Cash/Happy Money/Culture Cash/Smart Gift Voucher/Book Voucher/Tmoney Cancellation (Non-UI)](#teen-cashhappy-moneyculture-cashsmart-gift-voucherbook-vouchertmoney-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-17)
    - [Request Parameter Hash Code](#request-parameter-hash-code-20)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-17)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-14)
27. [POINTDAMOA Payment (UI)](#pointdamoa-payment-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-18)
    - [Request Parameter Hash Code](#request-parameter-hash-code-21)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-18)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-15)
28. [POINTDAMOA Cancellation (Non-UI)](#pointdamoa-cancellation-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-19)
    - [Request Parameter Hash Code](#request-parameter-hash-code-22)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-19)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-16)
29. [PAYCO Easy Payment (UI)](#payco-easy-payment-ui)
    - [API URI](#api-uri-4)
    - [Request and Response Headers](#request-and-response-headers-4)
    - [PAYCO (Development Environment) Sign Up](#payco-development-environment-sign-up)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-20)
    - [Request Parameter Hash Code](#request-parameter-hash-code-23)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-20)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-17)
30. [PAYCO Easy Payment Approval API (Non-UI)](#payco-easy-payment-approval-api-non-ui)
    - [API URI](#api-uri-5)
    - [Request and Response Headers](#request-and-response-headers-5)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-21)
    - [Request Parameter Hash Code](#request-parameter-hash-code-24)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-21)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-18)
31. [PAYCO Easy Payment Cancellation API (Non-UI)](#payco-easy-payment-cancellation-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-22)
    - [Request Parameter Hash Code](#request-parameter-hash-code-25)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-22)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-19)
32. [Kakao Pay Easy Payment (UI)](#kakao-pay-easy-payment-ui)
    - [API URI](#api-uri-6)
    - [Request and Response Headers](#request-and-response-headers-6)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-23)
    - [Request Parameter Hash Code](#request-parameter-hash-code-26)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-23)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-20)
33. [Kakao Pay Easy Payment Cancellation API (Non-UI)](#kakao-pay-easy-payment-cancellation-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-24)
    - [Request Parameter Hash Code](#request-parameter-hash-code-27)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-24)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-21)
34. [Naver Pay Easy Payment (UI)](#naver-pay-easy-payment-ui)
    - [API URI](#api-uri-7)
    - [Request and Response Headers](#request-and-response-headers-7)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-25)
    - [Request Parameter Hash Code](#request-parameter-hash-code-28)
    - [Response Parameter (

Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-25)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-22)
35. [Naver Pay Easy Payment Cancellation API (Non-UI)](#naver-pay-easy-payment-cancellation-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-26)
    - [Request Parameter Hash Code](#request-parameter-hash-code-29)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-26)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-23)
36. [Samsung Pay Easy Payment (UI)](#samsung-pay-easy-payment-ui)
    - [API URI](#api-uri-8)
    - [Request and Response Headers](#request-and-response-headers-8)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-27)
    - [Request Parameter Hash Code](#request-parameter-hash-code-30)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-27)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-24)
37. [Samsung Pay Easy Payment Cancellation API (Non-UI)](#samsung-pay-easy-payment-cancellation-api-non-ui)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-28)
    - [Request Parameter Hash Code](#request-parameter-hash-code-31)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-28)
    - [Notification Parameter (Hecto Financial -> Merchant)](#notification-parameter-hecto-financial---merchant-25)
38. [Real-Time Transaction Inquiry API (Non-UI)](#real-time-transaction-inquiry-api-non-ui)
    - [Integration Method](#integration-method)
    - [API URI](#api-uri-9)
    - [Request and Response Headers](#request-and-response-headers-9)
    - [JSON Request Data Example](#json-request-data-example-1)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-29)
    - [Request Parameter Hash Code](#request-parameter-hash-code-32)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-29)
    - [Payment Method Code](#payment-method-code)
39. [Transaction Collation API (Non-UI)](#transaction-collation-api-non-ui)
    - [API Description](#api-description)
    - [API URI](#api-uri-10)
    - [Request and Response Headers](#request-and-response-headers-10)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-30)
    - [Request Parameter Hash Code](#request-parameter-hash-code-33)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-30)
    - [Transaction Collation Response Value Example](#transaction-collation-response-value-example)
    - [Code Table](#code-table)
40. [Settlement Collation API (Non-UI)](#settlement-collation-api-non-ui)
    - [API Description](#api-description-1)
    - [API URI](#api-uri-11)
    - [Request and Response Headers](#request-and-response-headers-11)
    - [Request Parameter (Merchant -> Hecto Financial)](#request-parameter-merchant---hecto-financial-31)
    - [Request Parameter Hash Code](#request-parameter-hash-code-34)
    - [Response Parameter (Hecto Financial -> Merchant)](#response-parameter-hecto-financial---merchant-31)
    - [Settlement Inquiry Response Value Example](#settlement-inquiry-response-value-example)
41. [Others](#others)
    - [Reject Code Table](#reject-code-table)
    - [Credit Card Identifiers](#credit-card-identifiers)
    - [Financial Institution Identifiers](#financial-institution-identifiers)

## 1. Outline

### 1.1 Purpose

This document is for understanding technical requirements for integration development of the PG standard payment window provided by Hecto Financial and to define detailed specifications.

### 1.2 Target

This document is for the clients’ developers to execute payment through Hecto Financial’s PG system.

### 1.3 Specification

The following is a general description of the integration referred to in this document.

- Required fields in the request/response parameters use the ‘●’ symbol and selected fields use the ‘○’ symbol.
- Data types for request/response parameters are as follows.
  - N: Numeric characters
  - A: Alphabetic characters
  - H: Korean characters
- The standard request/response parameter’s length is the UTF-8 encoded value (Byte) of the plain text.

### 1.4 Others

- The offline document can be downloaded here. (Online document is always updated.)
- There is no need to read the entire document. It is divided into payment methods and services, so just refer to the parts needed.
- Link for FAQ

## 2. Standard Payment Window (UI) Integration

The integration method of the PG standard payment window provided by Hecto Financial is defined.

### 2.1 Description

- Check the request URI per payment method. (Refer to [2.3 API URI])
- Check the request parameter per payment method, set the request parameter, and request in the POST method.
- Parameters related to personal/sensitive information should be encrypted. (Refer to [5. Crucial Information Security])
- Open the Hecto Financial payment window then proceed with the payment.
- If you click the [Close] button on the payment completion window, the result (response parameter) is returned to the URL specified in the request parameter nextUrl.
- If the user forcefully quits the payment window during the payment (Settle payment window ‘X’ button), the result (response parameter) is returned to the URL specified in the cancUrl parameter.

### 2.2 Cautions

- The support for the IE browser ends starting June 15, 2022, please use the Edge browser.
- Refrain from using the Opera browser. Some functions may not work.
- Merchants are responsible for any costs incurred when running tests with Merchant ID (mchtId) in a production environment.
- Use only the POST method for the request.
- Use only those specified in the integration specification for the request parameter. Otherwise, an error may occur.
- Refrain from using special characters like :, &, ?, ', new line, <, > for the request parameter.
- Refrain from using emojis for the request parameter. If an emoji is included, it will get removed regardless of the intent of the user.
- The request parameter may change without prior notice.
- If Iframe is used while integrating the payment window, it may not work properly on some browsers or devices, so refrain from using Iframe.
- nextUrl, notiUrl parameters
  - nextUrl: It is the URL invoked when the customer clicks the ‘Close’ button on the Hecto Financial payment window completion screen. The response parameter is delivered in the POST method, and it is not invoked if the customer forcefully quits the payment window (browser ‘X’ button). Use nextUrl only for screen processing, and DB processing should be done in notiUrl.
  - notiUrl: If the payment was successfully processed in the Hecto Financial payment server, the server-to-server connection is made to the Merchant-side and the response parameter is delivered in the POST method. When the authorization succeeds, the response parameter is delivered regardless of whether the payment window was forcefully quit or not.
  - Therefore, on nextUrl, do screen processing to show the payment result with the delivered response parameter, and Merchant-side payment-related DB processing, you must do it on notiUrl.
- notiUrl hash check
  - To check data forgery, the process of verifying hash data received through notiUrl with the merchant DB must be done The service should be provided to the user only if it matches.
  - For the hash data check method and algorithm, refer to the Notification Parameter’s pktHash parameter.
- nextUrl, notiUrl, cancUrl protocol
  - When HTTP is used, the payment window may not work properly because of browser policy violations (such as cross-origin).
  - Therefore, the use of HTTPS is recommended.

### 2.3 API URI

Hecto Financial payment window (UI) server domain names are as follows.

| Type   | Domain Name                    |
| ------ | ------------------------------ |
| Testbed | tbnpg.settlebank.co.kr          |
| Production | npg.settlebank.co.kr          |

Hecto Financial Payment (UI) API URI are as follows.

| Function                        | Payment Method                       | URI                                         | HTTP Method |
| ------------------------------- | ------------------------------------- | ------------------------------------------- | ----------- |
| Standard Payment Window (UI)    | Credit Card                           | `https://{domain}/card/main.do`             | POST        |
|                                 | Credit Card - Direct                  | `https://{domain}/card/cardDirect.do`       |             |
|                                 | Account Transfer                      | `https://{domain}/bank/main.do`             |             |
|                                 | Virtual Account                       | `https://{domain}/vbank/main.do`            |             |
|                                 | DCB                                   | `https://{domain}/mobile

/main.do`           |             |
|                                 | Teen Cash Voucher                     | `https://{domain}/gift/teenCash/main.do`    |             |
|                                 | Happy Money Voucher                   | `https://{domain}/gift/happyMoney/main.do`  |             |
|                                 | Culture Land Voucher (Culture Cash)   | `https://{domain}/gift/cultureCash/main.do` |             |
|                                 | Smart Gift Voucher                    | `https://{domain}/gift/smartCash/main.do`   |             |
|                                 | Book Voucher                          | `https://{domain}/gift/booknlife/main.do`   |             |
|                                 | Tmoney                                | `https://{domain}/tmoney/main.do`           |             |
|                                 | POINTDAMOA                            | `https://{domain}/point/main.do`            |             |
|                                 | Easy Payment                          | `https://{domain}/corp/main.do`             |             |

### 2.4 Request and Response Headers

| Type     | Content                                                |
| -------- | ------------------------------------------------------ |
| Request  | `Content-type=application/x-www-form-urlencoded; charset=UTF-8` |
| Response | `Content-type=text/html; charset=UTF-8`                |

### 2.5 Request Parameter Verification

If there is an error after parameter verification, such as missing required values, inconsistency of hash data, and length check, the following response code is returned.

```json
{
  "outRsltMsg" : "Payment request information omitted (Product name)",
  "mchtTrdNo" : "ORDER20211231100000",
  "outRsltCd" : "1008",
  "outStatCd" : "0031",
  "mchtId" : "nxca_jt_il"
}
```

### 2.6 Integration Script Provision

For the convenience of the merchant when integrating, an integration script is provided.

| Type      | URL                                                 |
| --------- | --------------------------------------------------- |
| Testbed   | `https://tbnpg.settlebank.co.kr/resources/js/v1/SettlePG_v1.2.js` |
| Production| `https://npg.settlebank.co.kr/resources/js/v1/SettlePG_v1.2.js`  |

The following is an example of using the integration script.

```javascript
SETTLE_PG.pay({
  env: "https://npg.settlebank.co.kr",
  mchtId: "nxca_jt_il",
  method: "card",
  trdDt: "20211231",
  trdTm: "100000",
  mchtTrdNo: "ORDER20211231100000",
  mchtName: "Hecto Financial",
  mchtEName: "Hecto Financial",
  pmtPrdtNm: "Test Product",
  trdAmt: "AntV/eDpxIaKF0hJiePDKA==",
  mchtCustNm: "Hong Gil Dong",
  custAcntSumry: "Hecto Financial",
  notiUrl: "https://example.com/notiUrl",
  nextUrl: "https://example.com/nextUrl",
  cancUrl: "https://example.com/cancUrl",
  mchtParam: "name=HongGilDong&age=25",
  custIp: "127.0.0.1",
  pktHash: "f395b6725a9a18f2563ce34f8bc76698051d27c05e5ba815f463f00429061c0c",
  ui: {
    type: "popup",
    width: "430",
    height: "660"
  }
}, function(rsp){
  console.log(rsp);
});
```

- Close iframe

```javascript
parent.postMessage(JSON.stringify({action:"HECTO_IFRAME_CLOSE", params: _PAY_RESULT}), "*");
```

- Adjust iframe width

```javascript
parent.postMessage(JSON.stringify({action:"HECTO_IFRAME_RESIZE", params: {width:"500px"}}), "*");
```

- Iframe default size

```javascript
parent.postMessage(JSON.stringify({action:"HECTO_IFRAME_RETURNSIZE"}), "*");
```

### 2.7 Sample Source Provision

- To make Hecto Financial payment window integration easy, a sample source is provided.
- The sample source provided by Hecto Financial only includes the basics for integration, so please integrate it according to the Merchant’s environment when developing.
    1. Access the Hecto Financial Developer Support Site. [Click!](https://example.com)
    2. From the top menu [Developer Forum] > [SDK Download], download the sample source.
        - [Standard payment window (UI), click!](https://example.com)
        - [Credit card non/old verification API (Non-UI), click!](https://example.com)
        - [Virtual account API (Non-UI), click!](https://example.com)
    3. config.xxx setting change
        - It is the sample source setting file. Merchant ID, encryption key, log directory, and others can be set.
        - For the test environment, the default value can be used.
        - For a production environment, the Merchant ID and encryption key generated by Hecto Financial should be set.
    4. notiUrl, nextUrl, cancUrl request parameter value change
        - notiUrl: Put the URL that receives the response parameter delivered as Server To Server from Hecto Financial
        - nextUrl: Put the URL of the merchant-side screen that the user is directed to after completing payment on the Hecto Financial payment window,
        - cancUrl: Put the URL of the merchant-side screen that the user is directed to after forcefully quitting the Hecto Financial payment window
    5. Other cautions
        - JAVA (JSP): log4j setting file needs to be changed accordingly
        - PHP: Need to install curl and openssl package (Need to uncomment php.ini file)
        - Some functions may not work for PHP 5.4 or earlier.
        - For ASP Classic, there is a need to install a DLL file (encryption/decryption module) which was additionally distributed.
            - [ASP Classic DLL Guide, click!](https://example.com)

## 3. API Server Integration (Non-UI)

API service integration methods of cancellation per payment method, credit card billkey payment, mobile monthly recurring payment, virtual account issuance, etc. are defined.

### 3.1 Description

- Check the API URI of the service that you want to use.
- Check the request parameter of the service that you want to use, and set the appropriate request parameter.
- Do HTTP Connection through Server to Server and request/respond with JSON Data.
- Parameters related to personal/sensitive information should be encrypted. (Refer to [5. Crucial Information Security](#crucial-information-security))

### 3.2 API URI

Hecto Financial API server domain names are as follows.

| Type      | Domain Name                    |
| --------- | ------------------------------ |
| Testbed   | tbgw.settlebank.co.kr           |
| Production| gw.settlebank.co.kr             |

The API server URI is as follows.

| Function               | Service                                   | URI                                         | HTTP Method |
| ---------------------- | ----------------------------------------- | ------------------------------------------- | ----------- |
| Payment API (Non-UI)   | Credit card payment and billkey payment   | `https://{domain}/spay/APICardActionPay.do` | POST        |
|                        | Credit card billkey issuance              | `https://{domain}/spay/APICardAuth.do`      |             |
|                        | Phone monthly recurring payment          | `https://{domain}/spay/APIService.do`       |             |
|                        | PAYCO Easy Payment approval               | `https://{domain}/spay/APITrdPayco.do`      |             |
| Cancellation API (Non-UI) | Credit card cancellation               | `https://{domain}/spay/APICancel.do`        | POST        |
|                        | Account transfer cancellation             |                                             |             |
|                        | DCB cancellation                          |                                             |             |
|                        | Teen Cash cancellation                    |                                             |             |
|                        | Happy Money cancellation                  |                                             |             |
|                        | Culture Land voucher (Culture Cash) cancellation |                                      |             |
|                        | Smart Gift Voucher cancellation           |                                             |             |
|                        | Book Voucher cancellation                 |                                             |             |
|                        | Tmoney cancellation                       |                                             |             |
|                        | POINTDAMOA cancellation                   |                                             |             |
|                        | Easy Payment cancellation                 |                                             |             |
| Virtual Account Service API (Non-UI) | Virtual account number issuance | `https://{domain}/spay/APIVBank.do`         | POST        |
|                        | Virtual account number issuance cancellation | `https://{domain}/spay/APIVBank.do`        |             |
|                        | Virtual account refund                    | `https://{domain}/spay/APIRefund.do`        |             |
| Phone Refund API       | DCB refund                                | `https://{domain}/spay/APIRefund.do`        | POST        |
| Other Service API (Non-UI) | Credit card billkey delete            | `https://{domain}/spay/APICardActionDelkey.do` | POST        |
|                        | Real-time transaction inquiry             | `https://{domain}/spay/APITrdcheck.do`      |             |

### 3.3 Request and Response Headers

| Type     | Description                                         |
| -------- | --------------------------------------------------- |
| Request  | `Content-type=application/json; charset=UTF-8`      |
| Response | `Content-type=application/json; charset=UTF-8`      |

### 3.4 JSON Request Data Example

The following is the credit card cancellation request parameter JSON example.



```json
{
	"params" : {
		"mchtId" : "nxca_jt_il",
		"ver" : "0A19",
		"method" : "CA",
		"bizType" : "C0",
		"encCd" : "23",
		"mchtTrdNo" : "ORDER20211231100000",
		"trdDt" : "20211231",
		"trdTm" : "100000",
		"mobileYn" : "N",
		"osType" : "W"
	},
	"data" : {
		"pktHash" : "a2d6d597d55d7c9b689baa2e08c1ddf0ce71f4248c5b9b59fe61bfbf949543e1",
		"crcCd" : "KRW",
		"orgTrdNo" : "STFP_PGCAnxca_jt_il0211129135810M1494620",
		"cnclAmt" : "AntV/eDpxIaKF0hJiePDKA==",
		"cnclOrd" : "001",
		"cnclRsn" : "Not satisfied with the product"
	}
}
```

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
```

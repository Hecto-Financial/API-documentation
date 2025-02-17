# Hecto Financial

## Easy Cash Payment Standard WEB API

### Non-UI Integration

**Version:** 3.5  
**Initial:** 2019.11.29  
**Revised:** 2023.05.15  
**Made by:** Hecto Financial IT Division

---

## History

| Date       | Version | Description                                                                 | Made by        |
|------------|---------|-----------------------------------------------------------------------------|----------------|
| 2019.11.25 | 1.0     | Initial version release                                                     | Hyunwoo Kim    |
| 2019.12.19 | 1.1     | Form changed                                                                | Hyunwoo Kim    |
| 2020.03.04 | 1.2     | 11.5 ARS identity confirmation code error added 14.5 Field on division for use of payment issuing changed 15.3 Refund amount, VAT and service fee fields added | Hyunwoo Kim    |
| 2020.05.07 | 1.3     | 14.3 custAcntKey option for payment request changed                         | Hongsik Jeon   |
| 2020.05.21 | 1.4     | API for recurring payment added                                             | Hongsik Jeon   |
| 2020.06.29 | 1.5     | 12. Identity confirmation for bank account registration added               | Hongsik Jeon   |
| 2020.08.26 | 1.6     | 8. Account holder check (Included the date of birth) ui size changed        | Hongsik Jeon   |
| 2020.09.11 | 1.7     | 27.3 Time for bank regular maintenance modified                             | Hongsik Jeon   |
| 2020.10.19 | 1.8     | 16. Issuing information modified                                            | Joowan Ryu     |
| 2020.12.23 | 1.9     | 15. Merchant registration process was added                                 | Joowan Ryu     |
| 2021.03.04 | 2.0     | 30. ARS authentication (Receiving the record file) parameter added         | Byeongwoon Jang|
| 2021.03.12 | 2.1     | 15. Service division delete account key added for response                  | Joowan Ryu     |
| 2021.03.15 | 2.2     | 9. Inquiry of account holder name (Included amount) added                   | Joowan Ryu     |
| 2021.03.25 | 2.3     | 11. 12. 31. 32. Account occupancy certification service added               | Joowan Ryu     |
| 2021.06.25 | 2.4     | 35. Test call telegraphic added                                             | Byeongwoon Jang|
| 2021.11.23 | 2.5     | 36. Payment Password Check parameter added                                  | Byeongwoon Jang|
| 2022.04.08 | 2.6     | 13. ARS Authentication Request Payer Number item added                      | Jeongin Bae    |
| 2022.06.21 | 2.7     | 2. Easy cash payment API regular process table edited 7. 8. 9. Account holder name inquire API summary explanation edited 37. Others .2 Financial institution identifier code edited .3 Bank maintenance time added | Kyuri Kim      |
| 2022.07.13 | 2.9     | 40, 41, 42, 43 Kakao Pay authentication related API added Changed company name from SettleBank to Hecto Financial | Jeongin Bae    |
| 2022.09.06 | 3.0     | Payment (account number masking) API added                                  | Jeongin Bae    |
| 2022.11.17 | 3.0.2   | Open banking part removed from v 3.0                                        | Jinwook Kim    |
| 2022.11.30 | 3.1     | Container deposit item added to 18. Payment, 20. Recurring payment requests 19. Payment (account number masking) API removed 18. Account number masking item added to payment response parameter Reject code table updated (SE01, SE02, SE03) | Jaehyuk Lee    |
| 2022.12.21 | 3.2     | Parameter unit (length -> Byte) modified and edited Account holder name removed from required value for 11.3 Account ownership verification request 24.3 Unnecessary parameter (fintech use number) removed 28.5 Account list inquiry response parameter | Chanyoung Park |
| 2023.03.15 | 3.3     | 40.1 ST75, ST76, SE04, SE05 added to 40.1 Reject Code table                 | Jaehyuk Lee    |
| 2024.04.27 | 3.4     | Parameter added to check accounts that cannot be registered to 7, 8, 9 account holder name inquiry | Chanyoung Park |
| 2024.05.15 | 3.5     | 40.3 Bank maintenance time (Korea Post) edited                              | Jinwook Kim    |

---

## Contents

### 1. Outline
1.1 Purpose ...... 12  
1.2 Target ...... 12  
1.3 API URI ...... 12  
1.4 Caution ...... 13

### 2. Easy Cash Payment API General
2.1 Easy Cash Payment Process ...... 14  
2.2 General ...... 15  

### 3. API Invoke
3.1 API Connection Information ...... 16  
3.2 Request and Response Headers ...... 16  
3.3 Timeout ...... 16  
3.4 Others ...... 16  

### 4. Security of Important Information
4.1 Encryption & Decryption of Personal Information and Important Information ...... 17  
4.2 Encryption Key for Personal Information ...... 17  
4.3 Anti-forgery Algorithm ...... 17  
4.4 Hash Generation Authentication Key ...... 17  

### 5. Mobile Phone Verification Request
5.1 API Summary ...... 19  
5.2 API URL ...... 19  
5.3 Request (Merchant -> Hecto Financial) ...... 19  
5.4 Request Hash Code ...... 20  
5.5 Response (Hecto Financial -> Merchant) ...... 20  

### 6. Mobile Phone Verification Confirmation
6.1 API Summary ...... 21  
6.2 API URL ...... 21  
6.3 Request (Merchant -> Hecto Financial) ...... 21  
6.4 Request Hash Code ...... 22  
6.5 Response (Hecto Financial -> Merchant) ...... 22  

### 7. Account Holder Name Inquiry (Date of Birth Excluded)
7.1 API Summary ...... 23  
7.2 API URL ...... 23  
7.3 Request (Merchant -> Hecto Financial) ...... 23  
7.4 Request Hash Code ...... 24  
7.5 Response (Hecto Financial -> Merchant) ...... 24  

### 8. Account Holder Name Confirmation (Date of Birth Included)
8.1 API Summary ...... 26  
8.2 API URL ...... 26  
8.3 Request (Merchant -> Hecto Financial) ...... 26  
8.4 Request Hash Code ...... 27  
8.5 Response (Hecto Financial -> Merchant) ...... 27  

### 9. Account Holder Name Inquiry (Including Amount)
9.1 API Summary ...... 29  
9.2 API URL ...... 29  
9.3 Request (Merchant -> Hecto Financial) ...... 29  
9.4 Request Hash Code ...... 30  
9.5 Response (Hecto Financial -> Merchant) ...... 30  

### 10. Account Ownership Verification
10.1 API Summary ...... 32  
10.2 API URL ...... 32  
10.3 Request (Merchant -> Hecto Financial) ...... 32  
10.4 Request Hash Code ...... 33  
10.5 Response (Hecto Financial-> Merchant) ...... 33  

### 11. Request for Account Ownership Verification
11.1 API Summary ...... 34  
11.2 API URL ...... 34  
11.3 Request (Merchant -> Hecto Financial) ...... 34  
11.4 Request Hash Code ...... 36  
11.5 Response (Hecto Financial -> Merchant) ...... 36  

### 12. Confirmation of Account Ownership Verification
12.1 API Summary ...... 38  
12.2 API URL ...... 38  
12.3 Request (Merchant -> Hecto Financial) ...... 38  
12.4 Request Hash Code ...... 39  
12.5 Response (Hecto Financial -> Merchant) ...... 39  

### 13. ARS Authentication Request
13.1 API Summary ...... 40  
13.2 API URL ...... 40  
13.3 Request (Merchant -> Hecto Financial) ...... 40  
13.4 Request Hash Code ...... 41  
13.5 Response (Hecto Financial -> Merchant) ...... 41  

### 14. Confirmation of ARS Authentication
14.1 API Summary ...... 43  
14.2 API URL ...... 43  
14.3 Request (Merchant -> Hecto Financial) ...... 43  
14.4 Request Hash Code ...... 44  
14.5 Response (Hecto Financial -> Merchant) ...... 44  

### 15. Account Registration
15.1 API Summary ...... 46  
15.2 API URL ...... 46  
15.3 Request (Merchant -> Hecto Financial) ...... 46  
15.4 Request Hash Code ...... 47  
15.5 Response (Hecto Financial -> Merchant) ...... 47  

### 16. Account Cancellation
16.1 API Summary ...... 49  
16.2 API URL ...... 49  
16.3 Request (Merchant -> Hecto Financial) ...... 49  
16.4 Request Hash Code ...... 50  
16.5 Response (Hecto Financial -> Merchant) ...... 50  

### 17. Merchant Account Registration
17.1 API Summary ...... 51  
17.2 API URL ...... 51  
17.3 Request (Merchant -> Hecto Financial) ...... 51  
17.4 Request Hash Code ...... 52  
17.5 Response (Hecto Financial -> Merchant) ...... 52  

### 18. Payment
18.1 API Summary ...... 54  
18.2 API URL ...... 54  
18.3 Request (Merchant -> Hecto Financial) ...... 54  
18.4 Request Hash Code ...... 56  
18.5 Response (Hecto Financial -> Merchant) ...... 56  

### 19. Payment Cancellation/Refund
19.1 API Summary ...... 58  
19.2 API URL ...... 58  
19.3 Request (Merchant -> Hecto Financial) ...... 58  
19.4 Request Hash Code ...... 59  
19.5 Response (Hecto Financial -> Merchant) ...... 59  

### 20. Approval of Recurring Payment
20.1 API Summary ...... 61  
20.2 API URL ...... 61  
20.3 Request (Merchant -> Hecto Financial) ...... 61  
20.4 Request Hash Code ...... 63  
20.5 Response (Hecto Financial -> Merchant) ...... 63  

### 21. Cancellation of Recurring Payment
21.1 API Summary ...... 65  
21.2 API URL ...... 65  
21.3 Request (Merchant -> Hecto Financial) ...... 65  
21.4 Request Hash Code ...... 66  
21.5 Response (Hecto Financial -> Merchant) ...... 66  

### 22. Recurring Payment Information Inquiry
22.1 API Summary ...... 67  
22.2 API URL ...... 67  
22.3 Request (Merchant -> Hecto Financial) ...... 67  
22.4 Request Hash Code ...... 67  
22.5 Response (Hecto Financial -> Merchant) ...... 68  

### 23. Recurring Payment Transaction History Inquiry
23.1 API Summary ...... 70  
23.2 API URL ...... 70  
23.3 Request (Merchant -> Hecto Financial) ...... 70  
23.4 Request Hash Code ...... 71  
23.5 Response (Hecto Financial -> Merchant) ...... 71  

### 24. Remittance
24.1 API Summary ...... 73  
24.2 API URL ...... 73  
24.3 Request (Merchant -> Hecto Financial) ...... 73  
24.4 Request Hash Code ...... 75  
24.5 Response (Hecto Financial -> Merchant) ...... 75  

### 25. Remittance Account Balance Inquiry
25.1 API Summary ...... 76  
25.2 API URL ...... 76  
25.3 Request (Merchant -> Hecto Financial) ...... 76  
25.4 Request Hash Code ...... 76  
25.5 Response (Hecto Financial -> Merchant) ...... 76  

### 26. Transaction Result Inquiry
26.1 API Summary ...... 78  
26.2 API URL ...... 78  
26.3 Request (Merchant -> Hecto Financial) ...... 78  
26.4 Request Hash Code ...... 78  
26.5 Response (Hecto Financial -> Merchant) ...... 79  

### 27. Transaction History Inquiry
27.1 API Summary ...... 80  
27.2 API URL ...... 80  
27.3 Request (Merchant -> Hecto Financial) ...... 80  
27.4 Request Hash Code ...... 80  
27.5 Response (Hecto Financial -> Merchant) ...... 81  

### 28. Account List Inquiry
28.1 API Summary ...... 83  
28.2 API URL ...... 83  
28.3 Request (Merchant -> Hecto Financial) ...... 83  
28.4 Request Hash Code ...... 83  
28.5 Response (Hecto Financial -> Merchant) ...... 84  

### 29. Bank List Inquiry
29.1 API Summary ...... 85  
29.2 API URL ...... 85  
29.3 Request (Merchant -> Hecto Financial) ...... 85  
29.4 Request Hash Code ...... 85  
29.5 Response (Hecto Financial -> Merchant) ...... 85  

### 30. Bank Maintenance Inquiry
30.1 API Summary ...... 87  
30.2 API URL ...... 87  
30.3 Request (Merchant -> Hecto Financial) ...... 87  
30.4 Request Hash Code ...... 88  
30.5 Response (Hecto Financial -> Merchant) ...... 88  

### 31. Account Ownership Verification History Inquiry
31.1 API Summary ...... 89  
31.2 API URL ...... 89  
31.3 Request (Merchant -> Hecto Financial) ...... 89  
31.4 Request Hash Code ...... 90  
31.5 Response (Hecto Financial -> Merchant) ...... 90  

### 32. Account Ownership Verification Bank Maintenance Inquiry
32.1 API Summary ...... 92  
32.2 API URL ...... 92  
32.3 Request (Merchant -> Hecto Financial) ...... 92  
32.4 Request Hash Code ...... 92  
32.5 Response (Hecto Financial -> Merchant) ...... 93  

### 33. KFTC Withdrawal Transfer Information Cancellation Inquiry
33.1 API Summary ...... 94  
33.2 API URL ...... 94  
33.3 Request (Merchant -> Hecto Financial) ...... 94  
33.4 Request Hash Code ...... 94  
33.5 Response (Hecto Financial -> Merchant) ...... 95  

### 34. Test Call
34.1 API Summary ...... 96  
34.2 API URL ...... 96  
34.3 Request (Merchant -> Hecto Financial) ...... 96  
34.4 Response (Hecto Financial -> Merchant) ...... 96  

### 35. Payment Password Confirmation
35.1 API Summary ...... 98  
35.2 API URL ...... 98  
35.3 Request (Merchant -> Hecto Financial) ...... 98  
35.4 Request Hash Code ...... 99  
35.5 Response (Hecto Financial -> Merchant) ...... 99  

### 36. Others
36.1 Table of Reject Codes ...... 111  
36.2 Financial Institution Identifier ...... 112  
36.3 Bank Regular Maintenance Time ...... 113  

---

## 1. Outline

### 1.1 Purpose

This document is created for understanding the technical requirements for integration of Easy Cash Payment Non-UI API provided by Hecto Financial and to define detailed specification.

### 1.2 Target

This document is intended for clients’ developers to integrate Easy Cash Payment through easy payment system of Hecto Financial.

### 1.3 API URI

Easy Cash Payment service provides the following APIs; each API is mapped to a web service URL.

| Function               | API                                      | URI                                                                 | HTTP Method |
|------------------------|------------------------------------------|---------------------------------------------------------------------|-------------|
| Authentication service | Request for self-authentication          | https://npay.settlebank.co.kr/v1/api/auth/mobile/req                | POST        |
|                        | Confirmation for self-authentication     | https://npay.settlebank.co.kr/v1/api/auth/mobile/check              | POST        |
|                        | Name check of the account holder 1       | https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck1          | POST        |
|                        | Name check of the account holder 2       | https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheck2          | POST        |
|                        | Name check of the account holder (including amount) | https://npay.settlebank.co.kr/v1/api/auth/acnt/ownercheckWithAmount | POST        |
|                        | Account ownership verification           | https://npay.settlebank.co.kr/v1/api/auth/acnt/ownership            | POST        |
|                        | Request for account ownership verification | https://npay.settlebank.co.kr/v1/api/auth/ownership/req             | POST        |
|                        | Confirmation for account ownership verification | https://npay.settlebank.co.kr/v1/api/auth/ownership/check           | POST        |
|                        | ARS authentication request               | https://npay.settlebank.co.kr/v1/api/auth/ars                       | POST        |
|                        | ARS authentication confirmation          | https://npay.settlebank.co.kr/v1/api/auth/arscheck                  | POST        |
|                        | Kakao Pay Withdrawal Consent Authentication Request | https://npay.settlebank.co.kr/v1/api/auth/kakaopay/acttreg          | POST        |
|                        | Kakao Pay Electronic Signature Authentication Request | https://npay.settlebank.co.kr/v1/api/auth/kakaopay/sigureq          | POST        |
|                        | Kakao Pay Authentication Request         | https://npay.settlebank.co.kr/v1/api/auth/kakaop

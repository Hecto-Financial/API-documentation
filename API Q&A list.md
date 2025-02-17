#API Q&A List

# Easy Cash Payment API 

## Account Holder Name Inquire (Date of Birth Excluded)

### Account Holder Name Confirmation (Date of Birth Included)
1. **Q:** In the end-to-end tests for remittance, when I send this beneficiary name 헥토 but the response returns 홍길동  
   **A:** The test server basically operates with our simulator so for the account name verification the response is always fixed as success and 홍길동.
   
2. **Q:** I know we need to input Business registration number if we are a corporate user, but how do we collect the B user’s date found if it is not a date type value?  
   **A:** Either DOB or Business Registration number is fine. Users can choose whether to use DOB or their Business registration number based on whether they are submitting the application individually or as a corporate.

## Account Holder Name Inquiry (Including Amount)

## Account Ownership Verification

## Request for Account Ownership Verification

## Confirmation of Account Ownership Verification

## Payment

## Payment Cancellation/Refund

## Remittance
1. **Q:** Will the status of the transaction in the Recon File for Payout match all the status we received from API? Will there be any status change that will be notified in the recon file only?  
   **A:** For payment approval result and remit request result, if the result code is ‘in progress’ or if there is a timeout, until there are ‘success’ or ‘fail’ codes, the API should be invoked repeatedly. So when there is a timeout, the condition could be in progress on the API but if the timeout ends before SFTP file transfer, the status can be Success on the Recon File. So you should always be aware of timeouts when recording transactions.

## Remittance Account Balance Inquiry

## Foreign Exchange API

### Exchange Rate Inquiry (/pyag/v1/fxRate)
```json
I am getting the error '유효하지 않은 상점입니다'
Used link: https://tbgw.settlebank.co.kr/pyag/v1/fxRate

Request:
{
    "mchtId": "M2419529",
    "mchtTrdNo": "DCC2023082212332138974803",
    "rateCategoryId":"25H",
    "sellCrcCd":"KRW",
    "buyCrcCd":"USD"
}

Response:
{
    "mchtId": "",
    "mchtTrdNo": "",
    "encCd": "",
    "trdNo": "0A52B81FDFGBBKNJFLGCCOOD00324125",
    "trdDt": "20240123",
    "trdTm": "171252",
    "outStatCd": "0031",
    "outRsltCd": "PY97",
    "outRsltMsg": "유효하지않은 상점입니다.",
    "rateCategoryId": "",
    "sellCrcCd": "",
    "buyCrcCd": "",
    "rateQuoteId": "",
    "rate": "",
    "validFrom": "",
    "validTo": "",
    "mchtDcRt": ""
}
```
**A:** Foreign exchange service requests should be sent in the form of (POST) key=value, but throwing them in JSON format causes parsing errors.

## Virtual Account Issuance (/pyag/v1/fxVaccount)
1. **Q:** What is the time limit for Vaccount?  
   **A:** It returns immediately.

## Exchange, Remittance (B1) (/pyag/v1/fxTrans)
1. **Q:** Sending you request data

   - English Name: SENDER ENGLISH NAME TEST
   - Korean Name: 송신자 한글명 테스트
   - Date of Birth: 19800101
   - Country of Residence Code: US
   - Nationality Code: US
   - Address: 대한민국 테스트 시 테스트 동
   - Bank SWIFT Code: CJNBKRSE
   - Bank Name: CJNBKRSE
   - Bank Address: TEST FL. TEST STREET, TEST CITY, TEST STATE, US
   - Account Number: 86085028224
   - Remittance Reason Code: 20100
   - Receiving Account Remark: KEEP MY MONEY

   When I test RMT with the parameters above, I get the error "판매 통화 코드가 유효하지 않습니다".
   
   **A:** RMT is for local remittance. For overseas remittance, you need two remitCat (1: Overseas, 2: Local, 3: Bank).

2. **Q:** What is the reason for the following error {“outRsltMsg”: ”유효하지 않은 요청전문”, ”outRsltCd”: ”ST09”, ”outStatCd”: ”0031”}?  
   **A:** Upon checking the relevant information, it appears that the account you requested for name verification API has 16 digits, which exceeds the maximum limit of 15 digits allowed for account numbers. This likely caused the error you encountered. Typically, Korean won accounts have 14-digit account numbers, but after consulting with the bank, it was confirmed that special accounts, such as foreign currency accounts, may have 16 digits or different lengths. Please take note of this information.

3. **Q:** On the 1st, 37 transactions were initiated at the same time, 29 of them returned successfully, and only 8 of them returned abnormal. Can you help check why some are affected, some are not affected?  
   **A:** This happened because too many requests were made in a short period of time. To prevent this, I recommend you send transactions every 5 seconds.

4. **Q:** Can the recipient address be written in Korean if it's within Korea?  
   **A:** The recipient address cannot be written in Korean; it must be converted to English and provided. It has been confirmed that an error will occur if the recipient address is written in Korean. (with Financial Development Team)

5. **Q:** What information should be entered in the Recipient Name? Is it the account holder's name or the corporate name? If the recipient is a corporation and there is no Korean name for the corporation, can the "Recipient Korean Name" be omitted? Or should a random value be entered?  
   **A:** The recipient name field requires the account holder's name (e.g., corporate name for corporate accounts). If the recipient name is in English, it's acceptable to enter the same value for both "Recipient English Name" and "Recipient Korean Name."

6. **Q:** Can only lowercase alphabets be entered in the fields for Recipient English Name, Recipient Address, Recipient Bank Name, and Recipient Bank Address? Is it confirmed that neither uppercase alphabets nor Korean characters are allowed, regardless of whether it's a won or foreign currency transfer?  
   **A:** 
   - Lowercase alphabets cannot be entered.
   - Please refer to the Description section in the integration specification document.
   - Special characters such as [ ,, -, ., /, @, (, )] (total of 7) are allowed for input. Please take note of this.

7. **Q:** What will outStatCd and outRsltCd return for a successful transaction?  
   **A:** Success will only be outStatCd: 0021 and outRsltCd: 0000. When the transaction fails, it will be outStatCd: 0031 and outRsltCd: one error code from the result code.

8. **Q:** When we were testing FXRMT, we found a new return error "인보이스파일 정보 오류입니다", what does this mean?  
   **A:** If the transaction is for internal settlements, we can add test information to enable remittance without an invoice. If it is for a settlement to a third party, you would have to register your invoice on our side.

9. **Q:** Since we will deliver SWIFT BIC code on rcvrBankCd, if user input bankname is different from SWIFT code, can it lead to an error?  
   **A:** If you make a request via exchange & remittance API with the wrong SWIFT code, the request will be successfully sent. However, the remittance will be rejected from the bank side and you would be able to check the status of the remittance on Result Inquiry (V1). If the remittance fails due to the wrong SWIFT Code, the status would be 22: (Remittance Failure) Hecto Financial -> Local Bank Failure on Result Inquiry (V1).

10. **Q:** For one bank, for example, KBD, should we distinguish the different sub-branches, or use the same SWIFT code for all the KBD sub-branches?  
    **A:** In the case of "rcvrBankCd" please input it according to the details of the receiving bank. If the transaction is KRW -> KRW or USD -> KRW (meaning the receiving bank is a Korean bank) please input the three-digit bank code in the list previously provided. There is no need to input the SWIFT code. Please input the bank code in "rcvrBankCd". If the transaction is KRW -> USD (meaning the bank is an overseas bank), please input the swift code (BIC code) only in "rcvrBankCd". For KRW -> USD (if the receiving bank is an overseas bank), the swift code is needed and the swift code may vary depending on the branch of the bank which is why we request the user to provide the swift code of the specific branch of the bank.

11. **Q:** When do I need to send inFileNm and inFileData and are they required?  
    **A:** 
    - inFileNm and inFileData are requested on Exchange, remittance (B1).

12. On the api  fxTrans, It returned the following  error 'PY98: Invalid request parameter".
    
mchtTrdNo：43169d87deewwedsew7b08bere4dd5ea69782f1e83e121243.

request boby  : mchtId=M2439797&mchtTrdNo=43169d87deewwedsew7b08bere4dd5ea69782f1e83e121243&encCd=23&trdDt=&trdTm=&svcDivCd=FX&rateQuoteId=HCTO1NUZ0ABYJOVQV&sellCrcCd=KRW&sellAmt=xn313UzZ%2BqHl3EhKaZJCVw%3D%3D&buyCrcCd=&buyAmt=&remitCat=&remitAmt=&remitCrcCd=&rcvrNm=&rcvrNmKr=&rcvrBirthDt=&rcvrLiveNtnCd=&rcvrNtnCd=&rcvrAddr=&rcvrBankCd=&rcvrBankNm=&rcvrBankAddr=&rcvrAcntNo=&remitRsnCd=&rcvrAcntSumry=&invFileNm=&invFileData=

Could you help me fix this error?

Answer: the input parameter "HCTO1WY29105U55LS" of rateQuoteId is valid between 10:23:42~10:24:42 only, and the value entered after this valid period.

We are currenly using 1min currency so you need to be aware of this when you conduct the test.

## Balance Inquiry (B2)
1. **Q:** When do we have to send inFileNm, inFileData, and are they required?
 - For inFileData, there is no example in the API documentation, can you provide an example?
   
  **A:**
- inFileNm (Invoice File Name) and inFileData (Invoice File Data) are request items of Currency Exchange, Remittance (B1), and should be sent together when requesting.
- Invoice is required for foreign currency remittance (USD remittance) and can be omitted only if the recipient account is a 'settlement account' in the name of a pre-registered merchant. 
- For inFileData (Invoice File Data), there is no specific form so please send it according to the specification.

2. **Q:** Are the blcKrw and blcUsd in the response results real-time available balances
  **A:** These they represent real-time available balances

Test Plan for the 
DataFlex Web Service for Testing ISBN Numbers 
________________________________________
✅ Test Plan: SOAP API — IsValidISBN10
1. Overview
This test plan outlines the strategy for testing the IsValidISBN10 operation of the DataFlex SOAP web service. This API validates 10-digit ISBN numbers using checksum logic.
________________________________________
2. Objective
To ensure the IsValidISBN10 API accurately identifies valid and invalid ISBN-10 numbers via:
•	Functional correctness


•	Protocol compatibility (SOAP 1.1, SOAP 1.2, JSON)


•	Error handling


•	Performance and reliability


________________________________________
3. Test Scope
In Scope:
•	Functional testing for various ISBN formats (valid, invalid, edge cases)


•	Protocol testing (SOAP 1.1, SOAP 1.2, JSON)


•	Negative testing and error handling


•	Response structure and content validation


•	Load/Performance Testing (optional in staging)


Out of Scope:
•	Testing ISBN-13 numbers


•	UI Testing


•	Security/Penetration Testing


________________________________________
4. Test Environment
Component	Detail
Endpoint	http://webservices.daehosting.com/services/isbnservice.wso
Protocols Supported	SOAP 1.1, SOAP 1.2, JSON
Response Type	Boolean
Authentication	None
Tools	Postman / SOAP UI / JMeter / Curl / Custom Java or Python scripts
________________________________________
5. Test Data
Test Case Type	Sample ISBN	Expected Result
Valid ISBN	0306406152	true
Valid with X	048665088X	true
Invalid ISBN	1234567890	false
Invalid characters	ABCDEF1234	false / error
Too short	12345	error/false
Too long	1234567890123	error/false
Empty string	""	error/false
Null	null	error
________________________________________
6. Test Scenarios
6.1 Functional Tests
•	[✓] Validate correct response for known valid ISBN-10


•	[✓] Validate correct response for invalid ISBN-10


•	[✓] Verify behavior with ISBN ending in X


•	[✓] Test with lowercase x if allowed


•	[✓] Validate handling of null, empty string, special characters


6.2 Protocol/Transport Tests
•	[✓] Send valid request over SOAP 1.1 and assert proper response


•	[✓] Send valid request over SOAP 1.2 and assert proper response


•	[✓] Send valid request using JSON format and verify behavior


6.3 Negative Tests
•	[✓] Malformed SOAP envelope


•	[✓] Missing sISBN element


•	[✓] Invalid Content-Type


•	[✓] Non-numeric or alphanumeric ISBNs


•	[✓] Long payloads (DoS-type check)


6.4 Response Validation
•	[✓] HTTP 200 status check


•	[✓] Content-Type matches the protocol


•	[✓] XML/JSON schema matches documented structure


•	[✓] Boolean response is correct (true/false)


6.5 Performance Tests (Optional)
•	[✓] 100 concurrent requests — should not fail or delay


•	[✓] Average response time under 500ms


________________________________________
7. Test Approach
Phase	Tool	Description
Manual Testing	Postman, SOAP UI	Manual execution of all scenarios across protocols
Automation	Python + zeep / Java + TestNG	Automate basic validation using scripts
Load Testing	JMeter / Locust	Optional stress testing with varied payloads
________________________________________
8. Roles & Responsibilities
Role	Responsibility
QA Engineer	Design & execute test cases
Automation Engineer	Automate SOAP/JSON scenarios
Developer (Optional)	Fix backend issues based on test results
________________________________________
9. Entry and Exit Criteria
Entry Criteria:
•	API documentation is available


•	Endpoint is stable and responsive


•	Test environment is accessible


Exit Criteria:
•	All functional and negative test cases executed


•	All critical defects resolved


•	API response verified across all supported protocols


________________________________________
10. Risks & Mitigation
Risk	Mitigation
Service downtime	Check availability before execution
Invalid/missing documentation	Use actual responses to build test cases
Protocol mismatch	Test against all Content-Type variations
________________________________________
11. Sample SOAP Request (SOAP 1.1)
POST /services/isbnservice.wso HTTP/1.1
Host: webservices.daehosting.com
Content-Type: text/xml; charset=utf-8

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <IsValidISBN10 xmlns="http://webservices.daehosting.com/ISBN">
      <sISBN>0306406152</sISBN>
    </IsValidISBN10>
  </soap:Body>
</soap:Envelope>

________________________________________
12. Reporting & Metrics
•	Test Execution Report


•	Defect Summary Report


•	Protocol-wise success/failure matrix


•	Performance test graphs (if applicable)


________________________________________


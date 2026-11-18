# GlobalTrade API Prototype
 
Postman-based verification of the GlobalTrade Corp User Management API data contract, using the ReqRes mock service (`https://reqres.in`).
 
**Engineer:** Basant Saadeldin  
**Prepared for:** Jamal Tariq, QA Automation Lead, OmniSync Solutions
 
## Overview
 
This project validates the request-response lifecycle of two endpoints before GlobalTrade integrates them into their live systems. The tests check HTTP status codes, the JSON wire format of the responses, and the use of a Bearer token for authentication.
 
| Request | Endpoint | Assertions |
|---|---|---|
| GET - Retrieve Users | `GET {{base_url}}/api/users?page=2` | Status code is 200 OK |
| POST - Create New User | `POST {{base_url}}/api/users` | Status code is 201 Created; response parses with `pm.response.json()`; response contains an `id` property |
 
The POST request sends this payload with the headers `Content-Type: application/json` and `Authorization: Bearer gt_proto_9932abc` (a simulated token):
 
```json
{
    "name": "Alex Mercer",
    "job": "Operations Manager"
}
```
 
## Project Structure
 
```
globaltrade-api-prototype/
├── README.md
├── deliverables/
│   ├── GlobalTrade_User_API_Verification.postman_collection.json
│   ├── GlobalTrade_Dev_Environment.postman_environment.json
│   └── test_execution_report.md
└── references/
    ├── api_contract_specification.md
    └── test_execution_report_template.md
```
 
## How to Run
 
1. Open Postman and create a workspace (for example `GlobalTrade API Prototype`).
2. Click **Import** and add both JSON files from `deliverables/`.
3. Select the **GlobalTrade Dev** environment in the top-right dropdown.
4. Set the `api_key` environment variable to your own ReqRes API key (see below).
5. Send the **GET** request, then the **POST** request, and check the **Test Results** tab. All four tests should pass.
## Environment Variables
 
| Variable | Value | Purpose |
|---|---|---|
| `base_url` | `https://reqres.in` | Base URL used by every request via `{{base_url}}`, so no URL is hardcoded |
| `api_key` | `<your-api-key>` | ReqRes API key, sent in the `x-api-key` header |
 
To test a different environment, change only `base_url`.
 
## Note on the ReqRes API Key
 
ReqRes now requires an API key on requests. Without it, the service returns 401 or 403. Both requests send the key in an `x-api-key: {{api_key}}` header. The exported environment contains a placeholder instead of a real key, so you need to add your own before running.
 
## Results
 
All tests passed in the baseline run. The full details are in `deliverables/test_execution_report.md`.
 

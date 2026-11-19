# Test Execution Report: GlobalTrade User API Verification

**Prepared for:** Jamal Tariq, QA Automation Lead (OmniSync Solutions)  
**Client:** GlobalTrade Corp  
**Engineer:** Basant Saadeldin  
**Date:** 2026-11-19  

---

## Postman Workspace Setup

**Workspace Name:** GlobalTrade API Prototype  
**Collection Name:** GlobalTrade - User API Verification  
**Setup Description:**  
A dedicated workspace was created for the GlobalTrade project to isolate mock testing from other client work. The collection holds two requests (GET Retrieve Users, POST Create New User), each with automated assertions in the Scripts (After response) tab, and was run against the `GlobalTrade Dev` environment.

## Environment Variables Used

* **Environment Name:** GlobalTrade Dev
* **Variables Defined:**
  * `base_url`: `https://reqres.in`
  * `api_key`: ReqRes API key (value kept private and not included in the exported file)
* **Implementation:**  
Both requests use `{{base_url}}` as the host, so no URL is hardcoded (`{{base_url}}/api/users?page=2&x-api-key={{api_key}}` and `{{base_url}}/api/users?x-api-key={{api_key}}`). The collection can be pointed at GlobalTrade's real Dev environment by changing only the environment values.

## Test Assertions Summary

### 1. GET - Retrieve Users
* **Endpoint:** `GET {{base_url}}/api/users?page=2`
* **Objective:** Validate the API successfully returns a list of users.
* **Assertions:**
  * [x] HTTP Status Code is 200 OK
  * *Chai BDD Snippet used:*
    ```javascript
    pm.test('Verify that the retrieve users endpoint returns a 200 OK status', function () {
        pm.response.to.have.status(200);
    });
    ```
* **Actual status code observed:** 200 OK
* **Execution Status:** Pass (1/1 tests passed)

### 2. POST - Create New User
* **Endpoint:** `POST {{base_url}}/api/users`
* **Objective:** Validate that a new user can be created when a valid JSON payload and Bearer token are provided.
* **Authentication:** Simulated Bearer Token (`Authorization: Bearer gt_proto_9932abc`) applied in Headers, alongside `Content-Type: application/json`.
* **Payload Used:**
  ```json
  {
      "name": "Alex Mercer",
      "job": "Operations Manager"
  }
  ```
* **Assertions:**
  * [x] HTTP Status Code is 201 Created
  * [x] Response body parsed successfully using `pm.response.json()`
  * [x] Response JSON payload contains the `id` property
  * *Chai BDD Snippet used:*
    ```javascript
    pm.test('Verify that the user creation endpoint returns a 201 Created status', function () {
        pm.response.to.have.status(201);
    });

    pm.test('Verify that the user creation response body is valid JSON and parses into an object', function () {
        const responseData = pm.response.json();
        pm.expect(responseData).to.be.an('object');
    });

    pm.test('Verify that the user creation response payload contains an id property', function () {
        const responseData = pm.response.json();
        pm.expect(responseData).to.have.property('id');
    });
    ```
* **Actual status code observed:** 201 Created (response contained `id: "488"` and `createdAt`)
* **Execution Status:** Pass (3/3 tests passed)

## Observations

**General Notes:**  
Both endpoints met the baseline data contract: GET returned 200 OK with a JSON user list, and POST returned 201 Created echoing `name` and `job` together with a generated `id` and `createdAt`. Response times were low (about 135-140 ms). The responses also include extra fields not in the specification (`support`, `_meta`), which do not affect the assertions but should be noted when moving to GlobalTrade's real API.

**Challenges & Solutions:**  
1. ReqRes now requires an API key, so I added an `api_key` environment variable and sent it as the `x-api-key` query parameter on both requests.  
2. ReqRes briefly rate-limited some repeated runs (429). I kept the assertions strict (200 and 201) and re-ran the requests until the service returned the contract status codes.  
3. The `id` assertion requires the body to be parsed first, so I store `pm.response.json()` in a local `const responseData` before calling `pm.expect()`.  
4. I verified the Bearer token format in the Postman Console, where the header appears as `Authorization: Bearer gt_proto_9932abc`.

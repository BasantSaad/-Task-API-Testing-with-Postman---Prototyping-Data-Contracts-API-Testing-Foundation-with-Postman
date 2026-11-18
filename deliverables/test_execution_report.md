# Test Execution Report: GlobalTrade User API Verification

**Prepared for:** Jamal Tariq, QA Automation Lead (OmniSync Solutions)  
**Client:** GlobalTrade Corp  
**Engineer:** [Your Name / Junior QA Engineer]  
**Date:** 2026-11-18  

---

## Postman Workspace Setup

**Workspace Name:** GlobalTrade API Prototype  
**Collection Name:** GlobalTrade - User API Verification  
**Setup Description:**  
A dedicated workspace was created for the GlobalTrade project to isolate mock testing from other client work. The collection holds two requests (GET Retrieve Users, POST Create New User), each with automated assertions in the Tests tab, and is run against the `GlobalTrade Dev` environment.

## Environment Variables Used

* **Environment Name:** GlobalTrade Dev
* **Variables Defined:**
  * `base_url`: `https://reqres.in`
* **Implementation:**  
Both requests use `{{base_url}}` as the host (`{{base_url}}/api/users?page=2` and `{{base_url}}/api/users`). No URL is hardcoded, so the collection can be pointed at GlobalTrade's real Dev environment by changing only the environment value.

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
        pm.expect(pm.response.code).to.equal(200);
    });
    ```
* **Execution Status:** [Pass / Fail — record result from your Postman run]

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
    pm.test('Verify that the user creation response payload contains an id property', function () {
        const responseData = pm.response.json();
        pm.expect(responseData).to.have.property('id');
    });
    ```
* **Execution Status:** [Pass / Fail — record result from your Postman run]

## Observations

**General Notes:**  
[After running the collection, summarize stability here, e.g. whether both endpoints met the baseline contract: 200 on GET, 201 plus `id` and `createdAt` on POST.]

**Challenges & Solutions:**  
The `id` assertion requires the body to be parsed first, so the tests store `pm.response.json()` in a local `const responseData` before calling `pm.expect()`. The Bearer token format was verified by checking the request in the Postman Console, where the header should read `Authorization: Bearer gt_proto_9932abc`. Note: ReqRes may now require an `x-api-key` header; if requests return 401/403, add that header and note it here.

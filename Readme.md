# Mock Server API Testing with Postman and Newman

This repository contains a complete API testing setup using Postman, Newman, and `json-server`. It demonstrates how to create a local mock server and perform automated end-to-end CRUD testing of the API endpoints, followed by generating a professional HTML report.

## 📌 Project Overview
This project simulates a fully functional REST API for managing `users` using `json-server`. It includes a Postman collection (`Mock_Server_Testing.postman_collection.json`) containing various requests to perform Create, Read, Update, and Delete (CRUD) operations, along with automated tests written in JavaScript.

### 🎯 Features Tested:
1. **Create User** (`POST`): Creates a new user using dynamic random data (`{{$randomFullName}}`, `{{$randomEmail}}`). Validates status code 201 and stores the generated `user_id` as an environment variable.
2. **Get Single User** (`GET`): Retrieves the user created in the previous step to verify successful data persistence.
3. **Get User List** (`GET`): Fetches all users and validates that the response is an array.
4. **Update User Data** (`PUT`): Completely updates the user data and validates the updated fields.
5. **Get Updated Data** (`GET`): Verifies if the `PUT` operation successfully modified the record on the server.
6. **Update Partial Data** (`PATCH`): Partially updates the user's role and validates the changes.
7. **Get Updated Patch Data** (`GET`): Verifies the partial update.
8. **Delete User Data** (`DELETE`): Deletes the user using the saved environment variable `user_id`.
9. **Verify Deleted Data** (`GET`): Attempts to fetch the deleted user and verifies that a 404 Not Found error is returned.

## 🚀 Prerequisites
To run this project on your local machine, you need to have [Node.js](https://nodejs.org/) installed.

## 🛠️ Setup Instructions

### 1. Creating the Mock Server

We use `json-server` to mock a full fake REST API with zero coding.

**Step 1:** Install `json-server` globally on your machine.
```bash
npm install -g json-server
```

**Step 2:** Ensure you have a `db.json` file in your root directory. This file acts as your database. Our `db.json` starts with an empty `users` array or dummy data.

**Step 3:** Start the JSON Server.
```bash
json-server --watch db.json
```
*Note: By default, the server will start on `http://localhost:3000`.*

**Step 4:** The Postman collection `Mock_Server_Testing.postman_collection.json` has already been created and exported in this repository.

### 2. Newman Integration for Automated Execution

Newman is a command-line collection runner for Postman. It allows you to run and test a Postman collection directly from the command line.

**Step 1:** Install Newman globally.
```bash
npm install -g newman
```

**Step 2:** Install the `htmlextra` reporter for generating professional HTML test reports.
```bash
npm install -g newman-reporter-htmlextra
```

**Step 3:** Run the Postman Collection via Newman.
Ensure your mock server is running (`json-server --watch db.json`), then execute the following command:
```bash
newman run Mock_Server_Testing.postman_collection.json --env-var "base_url=http://localhost:3000" -r htmlextra --reporter-htmlextra-export ./results/report.html
```

## 📊 Test Results & Analysis

When the above Newman command finishes executing, it will generate a rich HTML report in the `./results/report.html` file. 

The test suite thoroughly validates:
- **Status Codes:** Ensures endpoints return expected status codes (200 OK, 201 Created, 404 Not Found).
- **Data Integrity:** Cross-checks if the data created/updated matches the payload sent.
- **Variable Chaining:** Demonstrates dynamically passing variables between requests using `pm.environment.set` and `pm.environment.get`. 
- **Array and Type Verification:** Validates the structural integrity of the JSON responses (e.g., checking if the response is a valid array).

Open `./results/report.html` in any web browser to view detailed assertions, request payloads, response bodies, and the overall pass/fail percentage of the test runs.

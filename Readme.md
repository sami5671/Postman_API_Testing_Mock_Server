<div align="center">
  <h1>🚀 Automated API Testing with Postman & Newman</h1>
  <p>
    <i>A comprehensive, end-to-end API testing framework utilizing JSON Server for mocking, Postman for collection authoring, and Newman for automated CLI execution and reporting.</i>
  </p>

  <!-- Badges -->
  <img src="https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white" alt="Postman" />
  <img src="https://img.shields.io/badge/Newman-000000?style=for-the-badge&logo=postman&logoColor=FF6C37" alt="Newman" />
  <img src="https://img.shields.io/badge/JSON_Server-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="JSON Server" />
  <img src="https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js" />
</div>

<br />

## 📋 Table of Contents

- [📖 Project Overview](#-project-overview)
- [🏗️ System Architecture](#️-system-architecture)
- [🧪 Test Scenarios & API Endpoints](#-test-scenarios--api-endpoints)
- [⚙️ Prerequisites](#️-prerequisites)
- [🚀 Setup & Installation](#-setup--installation)
  - [1. Mock Server Setup](#1-mock-server-setup)
  - [2. Automated Testing with Newman](#2-automated-testing-with-newman)
- [📊 Reporting & Analysis](#-reporting--analysis)
- [📂 Repository Structure](#-repository-structure)

---

## 📖 Project Overview

This repository serves as a blueprint for conducting fully automated REST API testing. Instead of relying on a live production backend, this project utilizes **`json-server`** to instantly deploy a local, stateful mock server.

The accompanying Postman collection (`Mock_Server_Testing.postman_collection.json`) dynamically tests a full **CRUD (Create, Read, Update, Delete)** lifecycle for a `users` resource, implementing variable chaining, automated assertions, and environment management.

---

## 🏗️ System Architecture

1. **Mock Backend (`json-server`)**: Provides a dynamic REST API simulating real-world database behaviors (saving, updating, and deleting data) using a single `db.json` file.
2. **Test Scripts (`Postman`)**: Contains pre-request scripts and test assertions written in JavaScript (using the Chai assertion library) to validate status codes, schema compliance, and data integrity.
3. **CLI Runner & Reporter (`Newman` & `htmlextra`)**: Executes the Postman collection headlessly from the terminal and generates highly detailed, interactive HTML execution reports.

---

## 🧪 Test Scenarios & API Endpoints

The test suite systematically runs the following workflow, ensuring proper data persistence and mutation across requests:

| #   | Request Name            | Method   | Endpoint             | Description & Assertions                                                                                                                                        |
| --- | ----------------------- | -------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Create User**         | `POST`   | `/users`             | Generates a user with dynamic fake data (`{{$randomFullName}}`). Asserts HTTP `201 Created` and saves the newly generated `user_id` to the Postman environment. |
| 2   | **Get Single User**     | `GET`    | `/users/{{user_id}}` | Fetches the newly created user. Asserts HTTP `200 OK` and cross-verifies if the ID matches the environment variable.                                            |
| 3   | **Get User List**       | `GET`    | `/users`             | Retrieves all users in the database. Asserts HTTP `200 OK` and validates that the response body is a JSON Array.                                                |
| 4   | **Update User Data**    | `PUT`    | `/users/{{user_id}}` | Replaces the entire user object. Asserts HTTP `200 OK` and validates that the `name` field has been fully updated.                                              |
| 5   | **Get Updated Data**    | `GET`    | `/users/{{user_id}}` | Fetches the user again to ensure the `PUT` operation was persistently saved on the mock server.                                                                 |
| 6   | **Update Partial Data** | `PATCH`  | `/users/{{user_id}}` | Updates only specific fields (e.g., `role`). Asserts HTTP `200 OK` and checks that the targeted field is successfully updated.                                  |
| 7   | **Get Updated Patch**   | `GET`    | `/users/{{user_id}}` | Fetches the user to verify the `PATCH` operation persisted successfully.                                                                                        |
| 8   | **Delete User Data**    | `DELETE` | `/users/{{user_id}}` | Deletes the specific user from the database. Asserts HTTP `200 OK`.                                                                                             |
| 9   | **Verify Deleted Data** | `GET`    | `/users/{{user_id}}` | Attempts to fetch the deleted user. **Crucially asserts HTTP `404 Not Found`**, verifying the resource no longer exists.                                        |

---

## ⚙️ Prerequisites

Ensure the following dependencies are installed on your local machine:

- [Node.js](https://nodejs.org/) (v14 or higher recommended)
- [Postman](https://www.postman.com/downloads/) (Optional: for viewing/editing the collection manually)

---

## 🚀 Setup & Installation

### 1. Mock Server Setup

The backend is driven entirely by `json-server`.

**Step 1:** Install `json-server` globally:

```bash
npm install -g json-server
```

**Step 2:** Start the Mock Server. Navigate to the project root and run:

```bash
json-server --watch db.json
```

> **Note:** The server will start on `http://localhost:3000`. Keep this terminal window open!

### 2. Automated Testing with Newman

Newman allows you to execute the entire test lifecycle locally with a single command.

**Step 1:** Install Newman and the HTMLExtra reporter globally:

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

**Step 2:** Execute the Collection. Open a _new_ terminal window (leave the mock server running) and execute:

```bash
newman run Mock_Server_Testing.postman_collection.json \
  --env-var "base_url=http://localhost:3000" \
  -r htmlextra \
  --reporter-htmlextra-export ./results/API_Test_Report.html
```

---

## 📊 Reporting & Analysis

After the `newman run` command completes, an interactive execution report is generated at `./results/API_Test_Report.html`.

This report provides a granular analysis of the test run, including:

- **Execution Overview:** Total requests sent, data transferred, and overall pass/fail percentage.
- **Request Details:** Full visibility into Request Headers, Request Bodies, Response Headers, and Response Bodies for every single endpoint.
- **Failed Tests:** A dedicated section highlighting any failed assertions, helping quickly isolate API defects.
- **Environment State:** A snapshot of variables (like `user_id`) as they were dynamically captured during runtime.

---

## 📂 Repository Structure

```text
📦 Mock-Server-API-Testing
 ┣ 📂 results/
 ┃ ┗ 📜 API_Test_Report.html                # Generated Newman execution report
 ┣ 📜 Mock_Server_Testing.postman_collection.json # Postman Test Collection
 ┣ 📜 Local_API.postman_environment.json    # Exported Environment variables
 ┣ 📜 db.json                               # JSON Server Database file
 ┗ 📜 Readme.md                             # Project Documentation
```

<br />

<div align="center">
  <i>Developed for Quality Assurance and Automated API Validation.</i>
</div>

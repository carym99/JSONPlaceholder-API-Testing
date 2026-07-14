# JSONPlaceholder API Testing

[![JSONPlaceholder API Tests](https://github.com/carym99/JSONPlaceholder-API-Testing/actions/workflows/api-tests.yml/badge.svg)](https://github.com/carym99/JSONPlaceholder-API-Testing/actions/workflows/api-tests.yml)

## Project Overview

This project contains an automated API test suite for the JSONPlaceholder API.

The test suite was designed in Postman and executed using Newman to validate API functionality, HTTP status codes, response bodies, headers, data types, negative scenarios, error handling, and response-time thresholds.

The test suite is integrated with GitHub Actions to automatically execute API regression tests as part of a CI pipeline.

## Base URL

`https://jsonplaceholder.typicode.com`

## Tools Used

- Postman
- Newman
- newman-reporter-htmlextra
- GitHub Actions
- Git
- GitHub
- PowerShell

## Test Scope

The API test suite covers:

- Functional API testing
- Positive testing
- Negative testing
- HTTP status code validation
- Response body validation
- Header validation
- Data type validation
- Response time validation
- Error handling validation

## Operations Tested

The following API operations were tested:

- GET - List all resources
- GET - Retrieve a resource by ID
- POST - Create a resource
- PUT - Update an entire resource
- PATCH - Partially update a resource
- DELETE - Delete a resource
- GET - Filter resources by user
- GET - Retrieve nested resources

## Negative Test Scenarios

Negative API scenarios include:

- Invalid resource ID
- Non-numeric resource ID
- Empty request body
- Malformed JSON payload

## Test Environment

The Postman environment uses the following reusable variables:

- `baseUrl`
- `postId`
- `userId`

These variables allow API requests to be reused and executed consistently across Postman and Newman.

## Running the Tests with Newman

From the project directory, run:

```powershell
newman run "JSONPlaceholder API Testing.postman_collection.json" -e "JSONPlaceholder Environment.postman_environment.json" --env-var "baseUrl=https://jsonplaceholder.typicode.com" --env-var "postId=1" --env-var "userId=1" --reporters "cli,htmlextra" --reporter-htmlextra-export "JSONPlaceholder_API_Test_Report.html"
```

The command:

- Executes the Postman collection.
- Loads the Postman environment.
- Supplies required environment variables.
- Displays Newman CLI test results.
- Generates a detailed HTML execution report.

## Test Execution Summary

Latest GitHub Actions CI execution:

| Metric | Result |
|---|---:|
| Total Iterations | 1 |
| Total Requests | 12 |
| Failed Requests | 0 |
| Test Scripts | 12 |
| Failed Test Scripts | 0 |
| Total Assertions | 94 |
| Failed Assertions | 3 |
| Skipped Tests | 0 |
| Total Run Duration | 613 ms |
| Average Response Time | 28 ms |
| Minimum Response Time | 15 ms |
| Maximum Response Time | 97 ms |

All 12 API requests executed successfully.

Three assertions failed and were linked to one negative-test defect involving malformed JSON handling.

## CI/CD Integration

The API test suite is integrated with GitHub Actions to provide automated API regression testing using Newman.

The CI pipeline is triggered automatically on:

- Pushes to the `main` branch
- Pull requests targeting the `main` branch
- Manual workflow execution using `workflow_dispatch`

### CI Pipeline Workflow

The GitHub Actions pipeline performs the following steps:

1. Checks out the repository.
2. Sets up Node.js.
3. Installs Newman and `newman-reporter-htmlextra`.
4. Executes the Postman API test collection.
5. Runs all API assertions.
6. Generates a Newman HTML test report.
7. Uploads the HTML report as a GitHub Actions artifact.

Failed Newman assertions return a non-zero exit code and fail the CI quality gate.

The HTML test report is uploaded using `if: always()`. This ensures test evidence remains available for analysis even when API assertions fail.

## Current CI Result

The current CI pipeline fails at the API test execution stage because three assertions detect unexpected malformed JSON handling.

The CI quality gate correctly identifies the failed assertions and returns exit code `1`.

The HTML execution report is still generated and uploaded as a GitHub Actions artifact.

This behaviour ensures API defects are visible during continuous integration rather than being silently ignored.

## Defect Identified

### API returns 500 Internal Server Error and exposes internal stack trace when malformed JSON is submitted

**Severity:** High  
**Priority:** High  
**Status:** Open

### Description

When a malformed JSON payload is submitted to the create-resource endpoint, the API returns `500 Internal Server Error` instead of handling the invalid client request with `400 Bad Request`.

The response also exposes internal stack-trace and server implementation information.

### Actual Result

- API returns HTTP `500 Internal Server Error`.
- Internal stack-trace information is exposed.
- Server implementation details are visible in the response.

### Expected Result

- API should return HTTP `400 Bad Request`.
- A safe and meaningful validation error should be returned.
- Internal stack traces and server implementation details should not be exposed.

### Failed Assertions

1. Malformed JSON should return 400 Bad Request.
2. Response should not expose stack trace.
3. Response should not return Internal Server Error.

## HTML Test Report

A detailed Newman HTML execution report is generated as:

`JSONPlaceholder_API_Test_Report.html`

For GitHub Actions executions, the HTML report is uploaded as the following workflow artifact:

`newman-api-test-report`

The report contains:

- Request execution details
- HTTP status codes
- Response times
- Assertion results
- Failed test details
- Request and response information

## QA Conclusion

The API test suite successfully executed 12 API requests with no request execution failures or skipped tests.

A total of 94 assertions were executed. Three assertions failed and were linked to one negative-test defect involving malformed JSON handling.

The GitHub Actions CI pipeline correctly detected the assertion failures and failed the API quality gate while preserving the Newman HTML execution report as a workflow artifact.

The project demonstrates functional API testing, positive and negative testing, response validation, HTTP status code validation, header validation, data type validation, response-time validation, error-handling validation, Newman CLI execution, and CI integration using GitHub Actions.
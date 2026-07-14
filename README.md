# JSONPlaceholder API Testing

## Project Overview

This project contains an API test suite for the JSONPlaceholder API. The
suite was created in Postman and executed using Newman to validate
functionality, responses, HTTP status codes, headers, data types,
negative scenarios, and response times.

## Base URL

`https://jsonplaceholder.typicode.com`

## Tools Used

-   Postman
-   Newman
-   newman-reporter-htmlextra
-   PowerShell

## Test Scope

-   Functional API testing
-   Positive and negative testing
-   HTTP status code validation
-   Response body and header validation
-   Data type validation
-   Response time validation
-   Error handling validation

## Operations Tested

-   GET - List all resources
-   GET - Retrieve a resource by ID
-   POST - Create a resource
-   PUT - Update an entire resource
-   PATCH - Partially update a resource
-   DELETE - Delete a resource
-   GET - Filter resources by user
-   GET - Retrieve nested resources

Negative scenarios include invalid resource ID, non-numeric ID, empty
request body, and malformed JSON.

## Test Environment

Reusable Postman variables: - `baseUrl` - `postId` - `userId`

## Running the Tests with Newman

``` powershell
newman run "JSONPlaceholder API Testing.postman_collection.json" -e "JSONPlaceholder Environment.postman_environment.json" --env-var "baseUrl=https://jsonplaceholder.typicode.com" --env-var "postId=1" --env-var "userId=1" --reporters "cli,htmlextra" --reporter-htmlextra-export "JSONPlaceholder_API_Test_Report.html"
```

## Test Execution Summary

  Metric                         Result
  ----------------------- -------------
  Total Iterations                    1
  Total Requests                     12
  Failed Requests                     0
  Test Scripts                       12
  Failed Test Scripts                 0
  Total Assertions                   94
  Failed Assertions                   3
  Skipped Tests                       0
  Total Run Duration        4.2 seconds
  Average Response Time          238 ms

## Defect Identified

### API returns 500 Internal Server Error and exposes internal stack trace when malformed JSON is submitted

**Severity:** High\
**Priority:** High\
**Status:** Open

### Description

When a malformed JSON payload is submitted to the create-resource
endpoint, the API returns `500 Internal Server Error` instead of
handling the invalid client request with `400 Bad Request`. The response
also exposes internal stack-trace and implementation information.

### Actual Result

-   API returns HTTP `500 Internal Server Error`.
-   Internal stack-trace information is exposed.
-   Server implementation details are visible.

### Expected Result

-   API should return HTTP `400 Bad Request`.
-   A safe and meaningful validation error should be returned.
-   Internal stack traces and implementation details should not be
    exposed.

### Failed Assertions

1.  Malformed JSON should return 400 Bad Request.
2.  Response should not expose stack trace.
3.  Response should not return Internal Server Error.

## HTML Test Report

The Newman HTML report is generated as
`JSONPlaceholder_API_Test_Report.html`. Open it in a browser to review
the execution dashboard, requests, responses, assertions, and failed
tests.

## QA Conclusion

The API test suite successfully executed all 12 requests with no request
execution failures or skipped tests. A total of 94 assertions were
executed, with 3 failed assertions linked to one negative-test defect
involving malformed JSON handling.

The execution demonstrates functional, positive, negative,
response-validation, performance-threshold, and error-handling coverage
using Postman and Newman.

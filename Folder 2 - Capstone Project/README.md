# Capstone Project 3: API Automation Framework with Performance Benchmarking

**[▶️ Click Here to Watch the Video Presentation](https://drive.google.com/file/d/1WN_ISbbfq0WuUzW11AhVPHsCi5_jU36S/view?usp=sharing)**

---

## 1. Problem Statement
Automate the User Management lifecycle of a RESTful web service across Create, Read, Update, and Delete (CRUD) operations, ensuring both functional integrity and latency compliance.

## 2. Objective
Build an end-to-end BDD API testing framework using Python `Requests` and `Behave`. Beyond functional status code validation, the framework integrates dynamic test data generation, mathematical schema validation, and visual latency analytics using `matplotlib`.

## 3. Tools, Software, and Concepts Used
* **Language:** Python 3
* **Libraries:** Requests, Behave BDD, Matplotlib, Faker, JSONSchema, Allure-Behave
* **Target Application:** JSONPlaceholder REST API (`https://jsonplaceholder.typicode.com/`)
* **Innovations:** Dynamic Data Generation, Strict Schema Validation, Hook-driven Analytics Visualization

## 4. Implementation Structure & Source Code

### A. Feature File (`features/api_user_management.feature`)
```gherkin
Feature: API User Management and Performance Benchmarking

  Scenario: GET all users and benchmark time
    When I send a GET request to "/users"
    Then the response status code should be 200
    And the response time should be less than 2000 ms
    And the response payload should match the user schema

  Scenario: POST a new user
    When I send a POST request to "/users" with mock data
    Then the response status code should be 201
    And the response time should be less than 2000 ms
    And the response payload should match the user schema

  Scenario: PUT to update a user
    When I send a PUT request to "/users/1" with mock data
    Then the response status code should be 200
    And the response payload should match the user schema

  Scenario: DELETE a user
    When I send a DELETE request to "/users/1"
    Then the response status code should be 200
```
B. Environment Hooks (features/environment.py)
```Python
import matplotlib.pyplot as plt
from features.utils.api_client import APIClient

def before_all(context):
    context.base_url = "[https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)"
    context.client = APIClient(context.base_url, "dummy-bearer-token-123")
    context.api_times = {}

def after_all(context):
    endpoints = list(context.api_times.keys())
    times = list(context.api_times.values())
    
    plt.figure(figsize=(10, 6))
    plt.bar(endpoints, times, color=['#4C72B0', '#55A868', '#C44E52', '#8172B3'])
    plt.xlabel('API Actions')
    plt.ylabel('Response Time (milliseconds)')
    plt.title('API Performance Benchmark - JSONPlaceholder')
    plt.tight_layout()
    plt.savefig('api_performance_visual.png')
```
C. Core API Client (features/utils/api_client.py)
```Python
import requests

class APIClient:
    def __init__(self, base_url, token):
        self.base_url = base_url
        self.headers = {
            "Authorization": f"Bearer {token}",
            "Content-Type": "application/json"
        }

    def post(self, endpoint, payload):
        return requests.post(f"{self.base_url}{endpoint}", json=payload, headers=self.headers)

    def get(self, endpoint):
        return requests.get(f"{self.base_url}{endpoint}", headers=self.headers)

    def put(self, endpoint, payload):
        return requests.put(f"{self.base_url}{endpoint}", json=payload, headers=self.headers)

    def delete(self, endpoint):
        return requests.delete(f"{self.base_url}{endpoint}", headers=self.headers)
```
D. Step Definitions (features/steps/api_steps.py)
```Python
from behave import given, when, then
from features.utils.payload_generator import generate_user_payload
from features.utils.schema_validation import validate_user_schema

@when('I send a GET request to "{endpoint}"')
def step_get_request(context, endpoint):
    context.response = context.client.get(endpoint)
    context.api_times[f"GET {endpoint}"] = context.response.elapsed.total_seconds() * 1000

@when('I send a POST request to "{endpoint}" with mock data')
def step_post_request(context, endpoint):
    context.payload = generate_user_payload()
    context.response = context.client.post(endpoint, context.payload)
    context.api_times[f"POST {endpoint}"] = context.response.elapsed.total_seconds() * 1000

@when('I send a PUT request to "{endpoint}" with mock data')
def step_put_request(context, endpoint):
    context.payload = generate_user_payload()
    context.response = context.client.put(endpoint, context.payload)
    context.api_times[f"PUT {endpoint}"] = context.response.elapsed.total_seconds() * 1000

@when('I send a DELETE request to "{endpoint}"')
def step_delete_request(context, endpoint):
    context.response = context.client.delete(endpoint)
    context.api_times[f"DELETE {endpoint}"] = context.response.elapsed.total_seconds() * 1000

@then('the response status code should be {status_code:d}')
def step_verify_status(context, status_code):
    assert context.response.status_code == status_code

@then('the response time should be less than {ms:d} ms')
def step_verify_time(context, ms):
    elapsed = context.response.elapsed.total_seconds() * 1000
    assert elapsed < ms

@then('the response payload should match the user schema')
def step_verify_schema(context):
    data = context.response.json()
    if isinstance(data, list):
        validate_user_schema(data[0])
    else:
        validate_user_schema(data)
```

## 5. Execution & Visual Output
Automated Latency Benchmark:
<img width="1000" height="600" alt="image" src="https://github.com/user-attachments/assets/5fb7855d-ec94-4e03-8019-8c23686e3d21" />
Terminal window:
<img width="1401" height="187" alt="image" src="https://github.com/user-attachments/assets/523bd672-9073-4ea9-8d2f-81875ce5ce0e" />

## 6. Result, Observation, and Conclusion
The test suite successfully validated all 4 CRUD scenarios against the JSONPlaceholder endpoints. The GET, POST, PUT, and DELETE calls executed within the established SLA thresholds. The test hook automatically aggregated response metrics into a bar chart, confirming that latency monitoring can be seamlessly embedded into automated regression suites. Furthermore, the integration of Faker and jsonschema ensured dynamic test data and strict structural integrity across all assertions, demonstrating a highly scalable and reusable framework architecture.

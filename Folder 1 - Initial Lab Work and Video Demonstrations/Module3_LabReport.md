# Module 3: Python BDD Restful Automations

---
# Experiment 1: Selenium Web Automation with Behave BDD

**1. Problem Statement**
Automate a basic web navigation scenario using Behavior-Driven Development (BDD) to ensure business-readable test definitions execute properly against a web UI.

**2. Objective**
To establish the Python Behave framework, write a Gherkin feature file, and map step definitions to Selenium WebDriver actions.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

Behave Framework

Concepts: Gherkin Syntax (Feature, Scenario, Given, When, Then), Step Definitions.

**4. Implementation Steps & Source Code**

File: features/login.feature
```
Gherkin
Feature: Basic Web Navigation
  Scenario: Verify page title
    Given I launch the browser
    When I navigate to the test automation practice site
    Then the page title should contain "Automation"
```
File: features/steps/login_steps.py
```python
from behave import given, when, then
from selenium import webdriver

@given('I launch the browser')
def step_impl_launch(context):
    context.driver = webdriver.Chrome()
    context.driver.maximize_window()

@when('I navigate to the test automation practice site')
def step_impl_navigate(context):
    context.driver.get("https://testautomationpractice.blogspot.com/")

@then('the page title should contain "{text}"')
def step_impl_verify(context, text):
    assert text in context.driver.title
    context.driver.save_screenshot("module3_exp1_output.png")
    context.driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="image" src="https://github.com/user-attachments/assets/7359fff0-430a-45cd-a7bb-af34e12ed77f" />


<img width="1311" height="452" alt="Screenshot 2026-09-19 230507" src="https://github.com/user-attachments/assets/3e833ec3-4e04-4626-9584-7393c8632be4" />



**6. Result, Observation, and Conclusion**
The Behave test runner successfully parsed the Gherkin feature file and executed the underlying Selenium Python code. The terminal output confirms the step-by-step execution, and the browser automation successfully validated the web page title, demonstrating a fully functional BDD framework setup. **

---

# Experiment 2: Data-Driven API Automation with Python Behave

**1. Problem Statement**
Automate the validation of a RESTful API using multiple sets of test data to confirm the server returns correct HTTP status codes for both valid and invalid endpoints.

**2. Objective**
To integrate the Python requests library within the Behave BDD framework and utilize Scenario Outline to achieve test data-driven automation without a web UI.   

**3. Tools, Software, and Concepts Used**   

Python 3   

Behave Framework, Python Requests Library   

Concepts: HTTP GET requests, Status Code assertions, BDD Data-Driven Testing (Scenario Outline, Examples).   

**4. Implementation Steps & Source Code**   

File: features/api_test.feature
```Gherkin
Feature: Data Driven API Validation
  Scenario Outline: Validate multiple API endpoints
    Given I set the API endpoint to "<url>"
    When I send a GET request to the endpoint
    Then the response status code should exactly be <status_code>

    Examples:
      | url                                | status_code |
      | https://reqres.in/api/users/2      | 200         |
      | https://reqres.in/api/users/23     | 404         |
```
File: features/steps/api_steps.py

```python
from behave import given, when, then
import requests

@given('I set the API endpoint to "{url}"')
def step_impl_set_url(context, url):
    context.api_url = url

@when('I send a GET request to the endpoint')
def step_impl_send_get(context):
    context.response = requests.get(context.api_url)

@then('the response status code should exactly be {status_code:d}')
def step_impl_verify_status(context, status_code):
    assert context.response.status_code == status_code
```

**5. Output**
<img width="1318" height="673" alt="image" src="https://github.com/user-attachments/assets/942e9b0d-762c-4a80-b88a-fbc887ff0ba4" />


**6. Result, Observation, and Conclusion**
The framework successfully read the external datasets from the Gherkin Examples table and executed the HTTP GET requests iteratively. The terminal output confirms that both scenarios passed, properly validating that the API returns a 200 status code for valid data and a 404 status code for invalid data.

---

# Experiment 3: Selenium Page Object Model (POM) in Python Behave

**1. Problem Statement**
Restructure a standard BDD web automation test to utilize the Page Object Model design pattern for better maintainability and locator separation.

**2. Objective**
To demonstrate advanced framework architecture by combining Behave step definitions with a dedicated Python class containing web element locators and action methods.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

Behave Framework

Concepts: Page Object Model (POM), BDD integration, OOP principles.

**4. Implementation Steps & Source Code**

File: features/pages/login_page.py

```Python
from selenium.webdriver.common.by import By

class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        self.username_input = (By.ID, "input-email")
        self.password_input = (By.ID, "input-password")
        self.login_btn = (By.XPATH, "//input[@value='Login']")

    def enter_credentials(self, user, pwd):
        self.driver.find_element(*self.username_input).send_keys(user)
        self.driver.find_element(*self.password_input).send_keys(pwd)

    def click_login(self):
        self.driver.find_element(*self.login_btn).click()
```
File: features/pom_login.feature

```Gherkin
Feature: POM Login Validation
  Scenario: Valid Login via POM
    Given I navigate to the login page
    When I submit valid credentials
    Then the account dashboard is displayed
File: features/steps/pom_steps.py
```
```Python
from behave import given, when, then
from selenium import webdriver
from features.pages.login_page import LoginPage

@given('I navigate to the login page')
def step_impl_nav_login(context):
    context.driver = webdriver.Chrome()
    context.driver.maximize_window()
    context.driver.get("https://tutorialsninja.com/demo/index.php?route=account/login")
    context.login_page = LoginPage(context.driver)

@when('I submit valid credentials')
def step_impl_submit(context):
    context.login_page.enter_credentials("dummytest@email.com", "Password123!")
    context.login_page.click_login()

@then('the account dashboard is displayed')
def step_impl_dashboard(context):
    assert "Account" in context.driver.title
    context.driver.save_screenshot("module3_exp3_output.png")
    context.driver.quit()
```
**5. Output**
<img width="1920" height="842" alt="module3_exp3_output" src="https://github.com/user-attachments/assets/d2774086-b5dd-4117-a82a-b0bdc84a8665" />
<img width="1308" height="417" alt="image" src="https://github.com/user-attachments/assets/b0e8f232-3f4c-4cba-a3f8-cce78921602f" />


**6. Result, Observation, and Conclusion**
The step definitions successfully instantiated the LoginPage object. Locators and actions were cleanly separated from the test logic through encapsulation, and the BDD framework executed the end-to-end workflow seamlessly, proving the integration of the Page Object Model within Behave.

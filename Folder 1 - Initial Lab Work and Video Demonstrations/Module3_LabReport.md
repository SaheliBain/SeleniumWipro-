**Module 3**

**Experiment 1: Selenium Web Automation with Behave BDD**

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

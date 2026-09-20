# Module 4: Robot Framework

---

# Experiment 1: Basic Syntax and Keywords

**1. Problem Statement**
Automate a web browser using the Robot Framework to navigate to a URL, input data into a form field, and verify the presence of a specific web element.

**2. Objective**
To establish the Robot Framework environment and utilize built-in SeleniumLibrary keywords such as "Open Browser", "Input Text", and "Page Should Contain Element".

**3. Tools, Software, and Concepts Used**
* Python 3
* Robot Framework
* SeleniumLibrary
* Concepts: Keyword-driven testing, `.robot` file structure, basic syntax.

**4. Implementation Steps & Source Code**

**File: `exp1_basic_syntax.robot`**
```robot
*** Settings ***
Library    SeleniumLibrary

*** Variables ***
${URL}    [https://testautomationpractice.blogspot.com/](https://testautomationpractice.blogspot.com/)
${BROWSER}    Chrome

*** Test Cases ***
Web Navigation And Verification
    Open Browser    ${URL}${BROWSER}
    Maximize Browser Window
    Input Text    id=name    Saheli Bain
    Page Should Contain Element    id=name
    Capture Page Screenshot    module4_exp1_output.png
    Close Browser
```
**5. Output**
<img width="1920" height="842" alt="module4_exp1_output" src="https://github.com/user-attachments/assets/077f5e8b-0fe9-4a73-abb1-6df2fa4c17b0" />

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/5a31a21e-b93c-4afe-9c83-fefbe7d7712e" />

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/66e69eb8-6041-48d5-9725-b7fa03497a6e" />

**6. Result, Observation, and Conclusion**

The Robot Framework successfully launched the Chrome browser and parsed the keyword-driven script. The "Input Text" keyword correctly populated the form with the specified string, and the "Page Should Contain Element" keyword successfully validated the DOM, demonstrating a functional setup of the SeleniumLibrary.

---

# Experiment 2: Variables and Data-Driven Automation

**1. Problem Statement**
Implement a test case utilizing variables to store sensitive data (usernames, passwords) and execute a data-driven test that performs identical web interactions across multiple distinct datasets.

**2. Objective**
To separate test data from test logic in the Robot Framework by reading external resource files and utilizing the Test Template setting to loop through multiple scenarios automatically.

**3. Tools, Software, and Concepts Used**

Python 3

Robot Framework

SeleniumLibrary

Concepts: Data-Driven Testing (DDT), Test Template, Resource Files, Variable scoping.

**4. Implementation Steps & Source Code**

File: test_data.robot

```Robot Framework
*** Variables ***
${LOGIN_URL}        https://tutorialsninja.com/demo/index.php?route=account/login
${BROWSER}          Chrome
${VALID_USER}       dummytest@email.com
${VALID_PASS}       Password123!
${INVALID_USER}     wronguser@email.com
${INVALID_PASS}     WrongPass456
```
File: exp2_datadriven.robot

```Robot Framework
*** Settings ***
Library           SeleniumLibrary
Resource          test_data.robot
Test Template     Attempt Login

*** Test Cases ***                USERNAME            PASSWORD
Valid Credential Login            ${VALID_USER}       ${VALID_PASS}
Invalid Credential Login          ${INVALID_USER}     ${INVALID_PASS}

*** Keywords ***
Attempt Login
    [Arguments]    ${username}    ${password}
    Open Browser    ${LOGIN_URL}    ${BROWSER}
    Maximize Browser Window
    Input Text      id=input-email       ${username}
    Input Text      id=input-password    ${password}
    Click Element   xpath=//input[@value='Login']
    Capture Page Screenshot
    Close Browser
```

**5. Output**
<img width="1920" height="842" alt="selenium-screenshot-1" src="https://github.com/user-attachments/assets/6c8e2c33-d62f-4b1f-95dc-71e13b384d4c" />

<img width="1920" height="842" alt="selenium-screenshot-2" src="https://github.com/user-attachments/assets/50b2d7ae-1f37-43fe-9ab3-6de06c5188d9" />
<img width="1170" height="452" alt="image" src="https://github.com/user-attachments/assets/faa29cfc-9b52-4718-8e29-156fe0ed4831" />

**6. Result, Observation, and Conclusion**

The Robot Framework successfully imported external variables from test_data.robot and mapped them to the main script. The Test Template setting efficiently acted as a loop, executing the "Attempt Login" keyword twice using the provided arguments. The generation of two separate screenshots (selenium-screenshot-1.png and selenium-screenshot-2.png) visually confirms that both sets of test data were processed iteratively.

---

# Experiment 3: Custom Keywords and Libraries

**1. Problem Statement**
Enhance the readability and maintainability of an automation script by grouping low-level Selenium commands into higher-level, user-defined custom keywords.

**2. Objective**
To construct custom keywords using the `*** Keywords ***` section in Robot Framework, pass arguments to these keywords, and execute a modular test case that reads like plain English.

**3. Tools, Software, and Concepts Used**
* Python 3
* Robot Framework
* SeleniumLibrary
* Concepts: User-Defined Keywords, Procedural vs. Gherkin readability, Abstraction.

**4. Implementation Steps & Source Code**

**File: `exp3_custom_keywords.robot`**
```robot
*** Settings ***
Library    SeleniumLibrary

*** Variables ***
${URL}             [https://tutorialsninja.com/demo/](https://tutorialsninja.com/demo/)
${BROWSER}         Chrome
${SEARCH_TERM}     MacBook

*** Test Cases ***
Product Search With Custom Keywords
    Launch E-Commerce Application
    Search For A Product    ${SEARCH_TERM}
    Verify Product Results Are Displayed
    Close The Application

*** Keywords ***
Launch E-Commerce Application
    Open Browser    ${URL}    ${BROWSER}
    Maximize Browser Window

Search For A Product
    [Arguments]    ${product_name}
    Input Text    name=search    ${product_name}
    Click Button    xpath=//button[contains(@class, 'btn-default')]

Verify Product Results Are Displayed
    Page Should Contain Element    xpath=//a[text()='MacBook']
    Capture Page Screenshot    module4_exp3_output.png

Close The Application
    Close Browser
```

**5. Output**
<img width="1920" height="842" alt="module4_exp3_output" src="https://github.com/user-attachments/assets/a9b726cd-0226-4720-83c4-57086e33dd26" />
<img width="1311" height="418" alt="image" src="https://github.com/user-attachments/assets/2e625f08-63e2-4518-9728-76cc538dd0c7" />


**6. Result, Observation, and Conclusion**

The test case successfully executed the abstracted custom keywords. By moving the raw Selenium commands (Open Browser, Input Text, Click Button) into the *** Keywords *** section, the main test case became significantly more readable. The script successfully navigated the e-commerce site, searched for the defined product argument, and validated the results screen.

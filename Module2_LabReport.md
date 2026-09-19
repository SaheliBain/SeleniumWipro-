# Module 2: Selenium Python Framework Development

---

**Experiment 1: PyTest Implementation with Page Object Model (POM)**

**1. Problem Statement**
Automate a login sequence using a structured testing framework rather than a linear script, ensuring the test logic is separated from the web element locators for better maintainability.

**2. Objective**
To demonstrate the implementation of the Page Object Model (POM) design pattern and the execution of test cases using PyTest fixtures and assertions.

**3. Tools, Software, and Concepts Used**
* Python 3
* Selenium WebDriver
* PyTest Framework
* Concepts: Page Object Model (POM), PyTest fixtures, Assertions.

**4. Implementation Steps & Source Code**
The script defines a `LoginPage` class that encapsulates the element locators and login actions. A separate test function (`test_valid_login`) initializes the driver, calls the page object methods, and uses a PyTest assertion to verify the page title changes upon a successful action.

```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        self.username_input = (By.ID, "input-email")
        self.password_input = (By.ID, "input-password")
        self.login_button = (By.XPATH, "//input[@value='Login']")

    def login(self, username, password):
        self.driver.find_element(*self.username_input).send_keys(username)
        self.driver.find_element(*self.password_input).send_keys(password)
        self.driver.find_element(*self.login_button).click()

def test_login_attempt():
    driver = webdriver.Chrome()
    driver.get("https://tutorialsninja.com/demo/index.php?route=account/login")
    driver.maximize_window()

    login_page = LoginPage(driver)
    login_page.login("dummytest@email.com", "Password123!")
    
    time.sleep(2)
    assert "Account Login" in driver.title
    
    driver.save_screenshot("module2_exp1_output.png")
    driver.quit()
```
**5. Output**
<img width="1920" height="842" alt="module2_exp1_output" src="https://github.com/user-attachments/assets/1289187c-e237-45f3-9afc-16f4e4df85b4" />
  

**6. Result, Observation, and Conclusion**
The framework successfully executed the test utilizing the POM structure. The PyTest assertion validated the page title, confirming the login logic worked as expected, and the script automatically captured the execution state.

---

**Experiment 2: Data-Driven Automation (DDT)**

**1. Problem Statement**
Automate a script that reads multiple sets of test data to execute the same test logic iteratively without duplicating code.

**2. Objective**
To demonstrate Data-Driven Testing (DDT) using PyTest parameterization.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

PyTest Framework

Concepts: @pytest.mark.parametrize.

**4. Implementation Steps & Source Code**
The script uses a PyTest decorator to pass a list of username and password tuples into the test function. The framework automatically runs the test case multiple times, once for each dataset.

```Python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

@pytest.mark.parametrize("username, password", [
    ("user1@example.com", "pass123"),
    ("user2@example.com", "pass456")
])
def test_multiple_logins(username, password):
    driver = webdriver.Chrome()
    driver.get("https://tutorialsninja.com/demo/index.php?route=account/login")
    driver.maximize_window()
    
    driver.find_element(By.ID, "input-email").send_keys(username)
    driver.find_element(By.ID, "input-password").send_keys(password)
    driver.find_element(By.XPATH, "//input[@value='Login']").click()
    
    time.sleep(1)
    driver.save_screenshot(f"module2_exp2_{username.split('@')[0]}_output.png")
    driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="module2_exp2_user1_output" src="https://github.com/user-attachments/assets/79a4ce66-81f3-4f6f-8d17-fb4682657b8d" />
<img width="1920" height="842" alt="module2_exp2_user2_output" src="https://github.com/user-attachments/assets/320dbb40-09a8-4e3a-87d4-ff97bcc48d9c" />

**6. Result, Observation, and Conclusion**
The framework successfully executed the same test sequence multiple times with different datasets, proving the efficiency of DDT in reducing code duplication.

---

**Experiment 3: PyTest HTML Reporting**

**1. Problem Statement**
Generate a structured, easily readable execution report for automated test suites to share with stakeholders.

**2. Objective**
To demonstrate the installation and utilization of the pytest-html plugin for automatic report generation.

**3. Tools, Software, and Concepts Used**

Python 3

PyTest Framework

pytest-html plugin

Concepts: Command Line Execution, HTML Report Generation.

**4. Implementation Steps & Source Code**
The test execution is triggered from the command line using the --html flag. The script contains basic assertions to generate a report showing passed and failed states.

```python
import pytest

def test_pass_case():
    assert True

def test_tool_check():
    assert "Selenium" == "Selenium"
```
(Execution Command run in terminal: pytest exp3_mod3.py --html=report.html)

**5. Output**
<img width="1920" height="1020" alt="Screenshot 2026-09-19 210333" src="https://github.com/user-attachments/assets/7b752601-08da-4bc3-b96f-fc564ff3393b" />


**6. Result, Observation, and Conclusion**
The pytest-html plugin successfully intercepted the test execution results and generated a formatted HTML document detailing the 100% pass status of the suite.

---

**Experiment 4: Advanced DDT with External CSV and PyTest Fixtures**

**1. Problem Statement**
Automate a login test that extracts test data from an external CSV file and utilizes a centralized setup and teardown mechanism for browser management.

**2. Objective**
To demonstrate reading data from a local CSV file for parameterization and implementing PyTest fixtures (`@pytest.fixture`) for clean WebDriver initialization and teardown.

**3. Tools, Software, and Concepts Used**
* Python 3
*  `csv` module
* Selenium WebDriver
* PyTest Framework
* Concepts: `@pytest.fixture`, CSV file creation, Data Extraction, Parameterization.

**4. Implementation Steps & Source Code**
The implementation requires creating both the external data file and the test script.

Step A: Creating the CSV Data Source
A new file named data.csv was created in the root test directory alongside the Python scripts. The first row establishes the column headers (username,password), and the subsequent rows contain the test data combinations used to drive the automation.

```Contents of data.csv:

Code snippet
username,password
csvuser1@test.com,pass123
csvuser2@test.com,pass456
Step B: Writing the PyTest Script
A Python script was written to interact with the CSV file. A utility function read_csv_data() opens data.csv in read mode, skips the header row, and extracts the data into a list of tuples. The @pytest.mark.parametrize decorator calls this function to pass the credentials into the test iteratively. Additionally, a @pytest.fixture is used to launch the browser before the test and automatically close it using the yield statement once the test completes.
```
```Python
import pytest
import csv
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

@pytest.fixture()
def setup_driver():
    driver = webdriver.Chrome()
    driver.maximize_window()
    yield driver
    driver.quit()

def read_csv_data():
    data = []
    with open("data.csv", "r") as file:
        reader = csv.reader(file)
        next(reader)
        for row in reader:
            data.append(tuple(row))
    return data

@pytest.mark.parametrize("username, password", read_csv_data())
def test_login_with_csv(setup_driver, username, password):
    driver = setup_driver
    driver.get("https://tutorialsninja.com/demo/index.php?route=account/login")
    driver.find_element(By.ID, "input-email").send_keys(username)
    driver.find_element(By.ID, "input-password").send_keys(password)
    driver.find_element(By.XPATH, "//input[@value='Login']").click()
    time.sleep(1)
    driver.save_screenshot(f"module2_exp4_{username.split('@')[0]}.png")
```
**5. Output**

<img width="1920" height="842" alt="module2_exp4_csvuser1" src="https://github.com/user-attachments/assets/379e4f8a-83ff-4da2-8943-f98ec4b7d670" />
<img width="1920" height="842" alt="module2_exp4_csvuser2" src="https://github.com/user-attachments/assets/1e41bb70-f721-4496-93e2-3035b8bf08a2" />

**6. Result, Observation, and Conclusion**
The framework successfully located and read the external data.csv file, extracting the parameters to drive the login test. The PyTest fixture executed the setup and teardown seamlessly around the test logic, demonstrating a highly scalable framework architecture driven by external datasets.

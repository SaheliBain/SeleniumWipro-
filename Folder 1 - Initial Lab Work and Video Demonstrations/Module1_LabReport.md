# Module 1: Automation with Selenium

---

**Experiment 1: Core Fundamentals & Locators**

**1. Problem Statement**
Automate the interaction with basic web form elements using standard locators to simulate a user entering data.

**2. Objective**
To demonstrate browser initialization, navigation, and element identification using core `By.ID` locators.

**3. Tools, Software, and Concepts Used**
* Python 3
* Selenium WebDriver
* Concepts: DOM locators (`By.ID`), Web Element interactions (`send_keys`).

**4. Implementation Steps & Source Code**
The script initializes the browser, navigates to the practice form, locates the standard text input fields by their HTML ID attributes, and injects test data.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

driver = webdriver.Chrome()
driver.get("https://testautomationpractice.blogspot.com/")
driver.maximize_window()

driver.find_element(By.ID, "name").send_keys("Saheli Bain")
driver.find_element(By.ID, "email").send_keys("saheli123@gmail.com")

time.sleep(2)
driver.save_screenshot("module1_exp1_output.png")
driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="module1_exp1_output" src="https://github.com/user-attachments/assets/3567ec71-4250-4ea1-b3a4-70aba65fe79c" />


**6. Result, Observation, and Conclusion**
The script executed successfully, demonstrating reliable interaction with standard DOM elements using ID locators.

---

**Experiment 2: Advanced Interactions - Drag and Drop**

**1. Problem Statement**
Automate a browser interaction that requires clicking and holding a web element, dragging it across the screen, and releasing it precisely into a designated target container.

**2. Objective**
To demonstrate the initialization of the Selenium WebDriver and the application of ActionChains to perform complex, multi-step mouse events.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

Concepts: ActionChains (drag_and_drop), DOM locators (By.ID).

**4. Implementation Steps & Source Code**
The script locates the source element and the target drop-zone using their HTML IDs, then chains the drag-and-drop actions together to execute the movement.

```Python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
import time

driver = webdriver.Chrome()
driver.get("https://testautomationpractice.blogspot.com/")
driver.maximize_window()
time.sleep(3)

source = driver.find_element(By.ID, "draggable")
target = driver.find_element(By.ID, "droppable")

actions = ActionChains(driver)
actions.drag_and_drop(source, target).perform()

time.sleep(2)
driver.save_screenshot("module1_output.png")
driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="module1_output" src="https://github.com/user-attachments/assets/32067d02-16fe-4da8-bf68-bd6377eed190" />


**6. Result, Observation, and Conclusion**
The script executed successfully. The ActionChains correctly simulated the physical mouse click-and-hold movement, securely placing the draggable element inside the target box.

---

**Experiment 3: Keyboard Actions and Element Focus**

**1. Problem Statement**
Automate the process of typing text into a specific input panel, highlighting it, copying it, and pasting it into a secondary input panel using simulated keyboard shortcuts.

**2. Objective**
To demonstrate advanced usage of ActionChains combined with Keys to simulate physical keyboard strokes.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

Concepts: ActionChains (key_down, key_up), JavaScript Executor (focus).

**4. Implementation Steps & Source Code**
The script locates the input panels using their IDs and uses ActionChains to type, copy, and paste the string "Welcome to Selenium" from one side to the other.

```Python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
import time

driver = webdriver.Chrome()
driver.get("https://text-compare.com/")
driver.maximize_window()
time.sleep(3)

left_box = driver.find_element(By.ID, "inputText1")
right_box = driver.find_element(By.ID, "inputText2")

driver.execute_script("arguments[0].focus();", left_box)
act = ActionChains(driver)
act.send_keys("Welcome to Selenium").perform()
time.sleep(2)

act.key_down(Keys.CONTROL).send_keys("a").key_up(Keys.CONTROL).perform()
act.key_down(Keys.CONTROL).send_keys("c").key_up(Keys.CONTROL).perform()

driver.execute_script("arguments[0].focus();", right_box)
act.key_down(Keys.CONTROL).send_keys("v").key_up(Keys.CONTROL).perform()

time.sleep(2)
driver.save_screenshot("module1_exp3_output.png")
driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="module1_exp3_output" src="https://github.com/user-attachments/assets/59c63eba-13ce-4eb9-bf58-cd1e76519cb1" />


**6. Result, Observation, and Conclusion**
The script successfully simulated the keyboard shortcuts. The text was dynamically typed, highlighted, and pasted into the second panel without utilizing the standard send_keys() directly on the element.

---

**Experiment 4: Handling JavaScript Alerts**

**1. Problem Statement**
Automate the interaction with a browser-native JavaScript popup alert.

**2. Objective**
To demonstrate the use of Selenium's switch_to.alert interface to manage un-inspectable browser dialogs.

**3. Tools, Software, and Concepts Used**

Python 3

Selenium WebDriver

Concepts: switch_to.alert.

**4. Implementation Steps & Source Code**
The script clicks a button that triggers a simple alert, switches the WebDriver's focus to the alert dialog, and accepts it.

``` Python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

driver = webdriver.Chrome()
driver.get("https://testautomationpractice.blogspot.com/")
driver.maximize_window()

driver.find_element(By.XPATH, "//button[text()='Simple Alert']").click()
time.sleep(1)

alert = driver.switch_to.alert
print("Alert message:", alert.text)
time.sleep(2)
alert.accept()

time.sleep(2)
driver.save_screenshot("module1_exp4_output.png")
driver.quit()
```

**5. Output**
<img width="1920" height="842" alt="module1_exp4_output" src="https://github.com/user-attachments/assets/37c09068-7f62-4ce4-a581-bcb848d386ca" />

<img width="742" height="101" alt="image" src="https://github.com/user-attachments/assets/53b11abe-9ef2-4578-9ff4-20ecd920d8fc" />


**6. Result, Observation, and Conclusion**
The script successfully detected the browser-level alert, switched focus, and closed it, verifying the ability to bypass standard DOM elements when dealing with native browser dialogs.

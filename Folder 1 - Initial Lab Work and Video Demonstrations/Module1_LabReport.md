**Module 1: Automation with Selenium**
**Experiment 1: Advanced Interactions - Drag and Drop**

**1. Problem Statement**
Automate a browser interaction that requires clicking and holding a web element, dragging it across the screen, and releasing it precisely into a designated target container.

**2. Objective**
To demonstrate the initialization of the Selenium WebDriver and the application of `ActionChains` to perform complex, multi-step mouse events.

**3. Tools, Software, and Concepts Used**
* Python 3
* Selenium WebDriver
* Concepts: ActionChains (drag_and_drop), DOM locators (By.ID).

**4. Implementation Steps & Source Code**
The script initializes the Chrome driver and navigates to the practice page. It locates the source element and the target drop-zone using their HTML IDs, then chains the drag-and-drop actions together to execute the movement.
```python
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
<img width="1920" height="842" alt="module1_output" src="https://github.com/user-attachments/assets/e63dd3eb-e513-4de7-ada4-da8d1b4ba9ad" />


**6. Result, Observation, and Conclusion**
The script executed successfully. The ActionChains correctly simulated the physical mouse click-and-hold movement, securely placing the draggable element inside the target box without manual intervention.

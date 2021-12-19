# Selenium WebDriver 学习笔记

## 浏览器驱动

- Firefox：geckodriver
- Chrome：chromedriver
- Edge：MicrosoftWebDriver

## 启动和访问

- 使用库：`from selenium import webdriver`

- 启动浏览器：`driver = webdriver.Firefox(executable_path=r'./geckodriver')`

  - 括号中是参数，可以留空。

  - 无头模式启动：

    ```python
    from selenium.webdriver.chrome.options import Options
    chrome_options = Options()
    chrome_options.add_argument('--headless')
    driver = webdriver.Chrome(chrome_options=chrome_options)
    ```

    

    ```python
    from selenium import webdriver
    #创建Chrome浏览器设置变量
    chrome_options = webdriver.ChromeOptions()
    #无界面模式
    chrome_options.add_argument('--headless')
    #实例化Chrome driver
    driver=webdriver.Chrome(chrome_options=chrome_options)
    ```

  - 防止检测：防止网页调用 `window.navigator.webdriver` 得知使用了 webdriver。

    ```python
    from selenium.webdriver import Chrome
    from selenium.webdriver import ChromeOptions
    
    options = ChromeOptions()
    options.add_experimental_option('excludeSwitches', ['enable-automation'])
    options.add_experimental_option('useAutomationExtension', False)
    
    bro = Chrome(options=options)
    bro.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
      "source": """
        Object.defineProperty(navigator, 'webdriver', {
          get: () => undefined
        })
      """
    })
    ```

- 打开某个网页：`driver.get("https://baidu.com/")`
  - 将等待页面加载完毕。但是如果有较多复杂的 Ajax 可能使判断不准确。
  
- 退出：`driver.close()` 关闭单个标签页，`driver.quit()` 关闭浏览器。

## 元素选择

诸如 `driver.find_element_by_id()` 的查找方法已经被弃用，建议使用 By 的方式。

### By 属性查找

```python
from selenium.webdriver.common.by import By

driver.find_element(By.XPATH, '//button[text()="Some text"]')
driver.find_elements(By.XPATH, '//button')
```

默认的 By 成员：

```python
ID = "id"
XPATH = "xpath"
LINK_TEXT = "link text"
PARTIAL_LINK_TEXT = "partial link text"
NAME = "name"
TAG_NAME = "tag name"
CLASS_NAME = "class name"
CSS_SELECTOR = "css selector"
```

## 元素操作

- 点击按键/输入文本：`element.send_keys("some text")`
  - `element.send_keys("some text", Keys.ARROW_DOWN)`
- 清除文本框文本：`element.clear()`

### 鼠标操作

```python
element.click()
element.double_click()
```

#### 元素拖拽 

```python
element = driver.find_element_by_name("source")
target = driver.find_element_by_name("target")

from selenium.webdriver import ActionChains
action_chains = ActionChains(driver)
action_chains.drag_and_drop(element, target).perform()
```

### 表单操作

- 提交表单：`element.submit()` 查找元素所属的表单，如果存在则直接提交表单，否则抛出异常。

### 下拉选项栏 select 操作

```python
from selenium.webdriver.support.ui import Select

select = Select(driver.find_element_by_name('name'))
select.select_by_index(index) # 选择编号 index 的选项
select.select_by_visible_text("text") # 选择文本为 text 的选项
select.select_by_value(value) # 选择值为 value 的选项

all_selected_options = select.all_selected_options # 全选
options = select.options
select.deselect_all() # 全不选
```



## Cookie

```python
cookie = {'name' : 'foo', 'value' : 'bar'}
driver.add_cookie(cookie)
```



## 等待加载

### 显式等待

- 等待一定条件之后执行。

- 等待固定时间：`time.sleep()`

- 等待元素出现，最多等待 10 秒否则抛出异常：

  ```python
  from selenium import webdriver
  from selenium.webdriver.common.by import By
  from selenium.webdriver.support.ui import WebDriverWait
  from selenium.webdriver.support import expected_conditions as EC
  
  driver = webdriver.Firefox()
  driver.get("http://somedomain/url_that_delays_loading")
  try:
      element = WebDriverWait(driver, 10).until(
          EC.presence_of_element_located((By.ID, "myDynamicElement"))
      )
  finally:
      driver.quit()
  ```

  - WebDriverWait 默认情况下会每500毫秒调用一次 ExpectedCondition 直到结果成功返回。 ExpectedCondition 成功的返回结果是一个布尔类型的 true 或是不为 null 的返回值。

- 预期条件：ExpectedCondition 预置了若干预期条件：

  ```python
  from selenium.webdriver.support import expected_conditions as EC
  
  wait = WebDriverWait(driver, 10)
  element = wait.until(EC.element_to_be_clickable((By.ID,'someid')))
  ```



### 隐式等待





## 浏览器控制

- 控制窗口大小：`driver.set_window_size(480, 800)`
- 后退/前进：`driver.back()/driver.forward()`
- 刷新：`driver.refresh()`



### 窗口切换

```python
driver.switch_to_window("windowName")

for handle in driver.window_handles:
    driver.switch_to_window(handle)

driver.switch_to_frame("frameName.0.child")
```

### 保存截图

```
driver.save_screenshot('screenshot.png')
```
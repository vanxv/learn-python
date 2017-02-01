#1.安装selenium，install Selenium
**python3**
```Python
pip3 install selenium
```

**python2**
```Python
pip install selenium
```
**windows**
```
C:\Python35\Scripts\pip.exe install selenium
```

**if install errror**
goto google "selenium install mac/windows/linux"

[点击下载Chrome自动浏览器](https://chromedriver.storage.googleapis.com/index.html?path=2.27/)


#自动注册qq的系统
```python
from selenium import webdriver
import time
from selenium.webdriver.support.ui import WebDriverWait # available since 2.4.0
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome('/users/VANXV/downloads/chromedriver')
##above open chrome browser（open path）
##打开浏览器的路径。
driver.get("https://ssl.zc.qq.com/chs/index.html")
##openurl
assert "QQ" in driver.title
##确定标题中有QQ
elem = driver.find_element_by_name("nick")
#找到html中name含有nick的标签
elem.send_keys("琳卡卡")
#输入值
elem = driver.find_element_by_name("password")
elem.send_keys("1q2w3e4r")
elem = driver.find_element_by_name("pass_again")
elem.send_keys("1q2w3e4r")
time.sleep( 1 )
#等待1秒
elem = driver.find_element_by_id("year_value")
elem.clear()
#清理原来的值
elem.send_keys("1986")
time.sleep( 1 )

elem = driver.find_element_by_id("month_value")
elem.clear()
elem.send_keys("1")
time.sleep( 1 )

elem = driver.find_element_by_id("day_value")
elem.clear()
elem.send_keys("7")
time.sleep( 1 )

elem = driver.find_element_by_name("phone_num")
elem.send_keys("18606622210")
time.sleep( 1 )
elem = driver.find_element_by_id("getverima").click()
#点击值
assert "QQ." not in driver.page_source

```

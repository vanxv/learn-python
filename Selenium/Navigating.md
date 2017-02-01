#3航行 Navigating


##3.1. 与页面交互Interacting with the page
Just being able to go to places isn’t terribly useful.
What we’d really like to do is to interact with the pages,
or, more specifically, the HTML elements within a page. First of all, we need to find one. WebDriver offers a number of ways to find elements. For example, given an element defined as:
页面交互的办法是通过WebDriver可以识别的元素。
举例：
```HTML
<input type="text" name="passwd" id="passwd-id" />
```

```python
element = driver.find_element_by_id("passwd-id")
element = driver.find_element_by_name("passwd")
element = driver.find_element_by_xpath("//input[@id='passwd-id']")
```
上面3种办法都是找到input这个模块

```python
element.send_keys(" and some", Keys.ARROW_DOWN)
```
后面的 **keys.ARROW_DOWN** 是模拟鼠标
**and some** 输入关键词

```python
element.clear()
```
如果框里面有文字，可以用这个指令清理


##3.2寻找表格
We’ve already seen how to enter text into a textarea or text field, but what about the other elements? You can “toggle” the state of drop down, and you can use “setSelected” to set something like an OPTION tag selected. Dealing with SELECT tags isn’t too bad:

我们准备看见怎样的输入text在文字框，但是其他元素的呢？你可以切换或者下拉。使用setSelected设置这样的选择项
```python
element = driver.find_element_by_xpath("//select[@name='name']")
all_options = element.find_elements_by_tag_name("option")
for option in all_options:
    print("Value is: %s" % option.get_attribute("value"))
    option.click()
```

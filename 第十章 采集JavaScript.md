## 核心模块：selenium.webdriver

### 核心原理：webdriver模仿浏览器发出各种指令

### 事例

```python
driver = webdriver.PhantomJS(executable_path='your_path')
driver.get('http://www.jnugeek.cn/')
```
- 这里就相当于模仿浏览器发送get请求
- PhantomJS是一个没有GUI的浏览器
- 说白了还是间接用浏览器

### 关于selenium

- selenium有一个类似于BeautifulSoa的选择器
- 可以很方便的定位到网页的某个元素
- 而且他的命名也十分清楚明了

```python
driver.find_element_by_tag_name('div')
```
### 总之，在浏览器面前
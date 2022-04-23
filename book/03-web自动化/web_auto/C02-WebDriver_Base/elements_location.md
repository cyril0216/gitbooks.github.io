## 元素定位

### 目标

```
1. 了解元素各种定位方法
2. 掌握id、name、class_name、tag_name、link_text、partial_link_text定位的使用
```

## 1. 为什么要学习元素定位方式？

```
1. 让程序操作指定元素，就必须先找到此元素；
2. 程序不像人类用眼睛直接定位到元素；
3. WebDriver提供了八种定位元素方式
```

## 2. WebDriver 元素定位方式

```
1. id
2. name
3. class_name
4. tag_name
5. link_text
6. partial_link_text
```

### 定位方式分类-汇总：

```
1). id、name、class_name：为元素属性定位
2). tag_name：为元素标签名称
3). link_text、partial_link_text：为超链接定位(a标签)
4). Xpath：为元素路径定位
5). Css：为CSS选择器定位
```

### 案例-1 [注册页面](../images/素材-注册页面.rar)

```
1. 为了更好的学习这八种方式和网络的关系，我们在案例-1注册页面上来练习自动化脚本设计，提高学习效率和
   脚本执行速率
2. 语言使用Python
3. 开发工具使用Pycharm
4. selenium使用2.48.0
```

### 2.1  id定位

```
实现案例-1需求：
    1). 打开注册A.html页面，使用id定位，自动填写(账号A：admin、密码A:123456)
    2). 填写完毕后，3秒钟关闭浏览器窗口     
```

实现方法:

> find_element_by_if()

实现

```
from selenium import webdriver
from time import sleep
driver=webdriver.Firefox()
url='E:\\测试\\课件\\Web自动化\\Web自动化课件\\02img\\注册A.html'
driver.get(url)
user=driver.find_element_by_id("userA")
user.send_keys("admin")
pwd=driver.find_element_by_id("passwordA")
pwd.send_keys("123456")
sleep(3)
driver.quit()
```

### 2.2  name属性定位

实现案例-1需求：
1). 打开注册A.html页面，使用id定位，自动填写(账号A：admin、密码A:123456)
2). 填写完毕后，3秒钟关闭浏览器窗口  

实现方法

> find_element_by_name

### 2.3 class_name 定位

```
实现案例-1需求：
    通过class_name定位电话号码A，并发送18611111111 
```

实现方法

> find_element_by_class_name()

### 2.4 tag_name 定位

```
实现案例-1需求：
    1). 打开注册A.html页面，使用tag_name定位，自动填写(账号A：admin)
    2). 填写完毕后，3秒钟关闭浏览器窗口
```

实现方法

> find_element_by_tag_name()    返回:符合条件的第一个标签

### 2.5 link_text定位

说明: **专门定位超链接文本**(<a>标签</a>), 全部匹配

```
实现案例-1需求：
    1). 打开注册A.html页面，使用link_text定位(访问 新浪 网站)超链接
    2). 3秒钟关闭浏览器窗口   
```

实现方法

> find_element_by_link_text()

### 2.6 partial_link_text

说明: partial_link_text定位是对link_text定位的补充，partial_like_text为模糊匹配；

```
实现案例-1需求：
    1). 打开注册A.html页面，使用partial_link_text定位(访问 新浪 网站)超链接
    2). 3秒钟关闭浏览器窗口 
```

实现方法

> find_element_by_partial_link_text()

## 3. 获取元素列表

![获取元素列表](/images/elements.png)

```
作用：
    1). 查找定位所有符合条件的元素
    2). 返回的定位元素格式为数组(列表)格式；
说明：
    1). 列表数据格式的读取需要指定下标(下标从0开始)
```

**使用说明**

和获取单个元素find_element_by_xxx 的使用方法一样. 例如: 

```
说明：使用tag_name获取第二个元素(密码框)

代码：
    ...
    driver.find_elements_by_tag_name("input")[1].send_keys("123456")
    ...
```

## 思考？

```
1. 在实际项目中标签没有id、name、class属性改如何定位？
2. id、name、class属性值为动态获取，随着刷新或加载而变化，改如何定位？
```
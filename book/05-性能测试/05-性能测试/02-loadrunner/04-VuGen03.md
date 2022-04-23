## <center>VuGen-手动编写脚本</center>

#### 目标

    1. 掌握C语言的注释方法
    2. 掌握基于LR手动编辑脚本的方法

首先科普下C语言中的注释；

    1. //：单行注释  如：//登录             
    2. /**/：多行注释  如：/*多行    注释*/    

    提示：
        1). Ctrl + Alt + C 为单行注释快捷键；
        2). Ctrl + Alt + U 为取消单行注释快捷键；

#### 一、手动编写脚本

##### 1.1 为什么要手写脚本

1. 避免LR由于兼容性导致录制异常或失败；<br>
2. 手写脚本无冗余代码，更有利于脚本的增强(插入事务、参数化)和维护

##### 1.2 手动编写脚本使用编写语言

1. JAVA<br>
2. C语言【推荐】

提示：
    1. LoadRunner中虽然默认是使用C语言录制/编写脚本，但我们无需专门去学C语言；
    2. 只需注意下基础语法和掌握LR中常用的几个请求函数的用法就可以编写常用的脚本；

##### 1.3 脚本请求 函数

1. web_url()
2. web_submit_data()
3. web_custom_request()

提示：

    1. web_url：GET请求使用
    2. web_submit_data：GET、POST请求
    3. web_custom_request：定制请求（GET\POST\PUT\DELETE）等，接口请求使用此函数

###### 1.3.1 web_url() 函数【重点】
只能做GET请求；

语法：

    web_url("request name","URL=http://***",LAST); 
    1). request name：标记请求名称，比如：首页
    2). URL=http://xxx：URL=为固定格式；http://xxx为GET请求地址
    3). LAST结束标志；

###### 1.3.2 web_submit_data()函数【重点】
默认设置是POST请求；也可以做GET请求。

语法：

    web_submit_data(StepName, Action=http://xxx, <List of Attributes>, 
    ITEMDATA,<List of data>, LAST); 
    1). StepName：标记请求名称， 比如：登录
    2). Action=http://xxx：Action=为固定格式；http://xxx：为提交地址
    3). List of Attributes:其他属性 如："Method=POST"
    4). List of data:其他参数 如："username=jojo"
    5). LAST：结束符

提示：

    1. POST提交参数比较多，但我们不需用一个个记，使用插入函数方法，在函数对话框内输入即可；
    2. 录制HTML页面时，Mode推荐使用HTML；

###### 1.3.3 web_custom_request() 函数【掌握】
自定义请求方法(GET/POST/PUT/DELETE)，用于实现接口测试。

语法：

    web_custom_request(RequestName, <List of Attributes>,LAST); 
    1). RequestName：请求名称
    2). List of Attributes：属性列表，如："Method=POST"
    3). LAST：结束符

提示：

    一般做接口压测时使用；

练习

    手动编写 iWebShop 的登录脚本；

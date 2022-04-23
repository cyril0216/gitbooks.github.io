## <center>VuGen-关联</center>

### 目标

1. 理解关联的概念<br>
2. 掌握关联函数的使用<br>
3. 了解LR自动关联的使用

### 一、为什么要学习关联？

![关联](/images/correlation_01.png)

    1. 获取动态加载的数据，例如：Session ID（会话ID）
    2. 测试的时候需要获取页面上指定的数据，注意是获取，而不是查找

    说明：
        Session ID：客户端与服务器交互时，服务器生成的一个唯一标识码;
        (通过标识码服务器可以区分多次交互对象是否同一客户端，浏览器只要不关闭标识码就不变)

### 二、什么是关联（correlation）？

动态获取指定的数据，并把获取的数据通过参数化的方式在另一处引用

### 三、LoadRunner 常用的关联方式

1. 手动关联【推荐】<br>
2. 自动关联【了解】

#### 3.1 手动关联【重点】

手动关联就是直接调用关联函数；

##### 关联函数 web_reg_save_param()

    1). 执行顺序：LR中函数内含reg为注册函数，注册函数是在下一个Action Function完成时执行。
    2). 函数位置：注册函数【必须】放到下一个Action Function(要进行关联数据所在的函数)函数之前；
    3). 如果关联的参数值超过256个字符，需要先调用 web_set_max_html_param_len("1024") 改变参数长度；

#### 案例 1

![关联](/images/correlation_04.png)

使用 web_reg_save_param() 函数把红框选中内容关联给Welcome参数

#### 案例 1 脚本
    Action()
    {
        web_url("WebTours",
            "URL=http://127.0.0.1:1080/WebTours/",
            "TargetFrame=",
            "Resource=0",
            "RecContentType=text/html",
            "Referer=",
            "Snapshot=t1.inf",
            "Mode=HTML",
            LAST);
        return 0;
    }
#### 案例1 操作分析

    1. 在web_url函数之前插入关联函数 web_reg_save_param()
    2. 调用 lr_eval_string() 来读取关联函数的参数引用名称，并通过 lr_output_message() 函数输出到日志

#### 案例1 操作实施
##### 1) 插入 web_reg_save_param()方法
![关联](/images/correlation_05.png)

    1. 在web_url函数之前空白行，点击鼠标右键
    2. 选择Insert菜单 -> New Step

##### 2) 输入要查找的函数：

![关联](/images/correlation_06.png)

    1). 图1：输入要查找函数的关键词或全称
    2). 图2：找到匹配出的函数，双击或点击图3
    3). 图3：选中图2，点击图3进行选中函数参数设置

##### 3) 设置函数参数

![关联](/images/correlation_07.png)

参数：

    1). Parameter Name:参数名称（被引用时使用）
    2). Left Boundary(LB):左边界
    3). Right Boundary(RB):右边界
    4). Not Found:关联失败时，处理方式
    5). Search in:匹配搜索响应代码范围

注意：

    单引号(')、双引号(")、反斜杠(\)、回车换行等需要转义字符，加上转义字符[\]

##### 4) 如何查看页面元素响应代码？
1. 通过LR查看响应代码<br>
2. 通过页面查看源代码

###### 1). 通过LR查看 【注意：必须是录制脚本-手动编写脚本没有快照】 

![关联](/images/correlation_08.png)

提示：

    1. 结构为Tree
    2. url：为WebTours
    3. 页面为/WebTours/home.html

###### 2). 通过页面源代码查看 关联

地址：http://127.0.0.1:1080/WebTours/home.html
参数化并输出到日志

![关联](/images/correlation_09.png)

代码：

    Action()
    {
        web_reg_save_param("Webcome",
            "LB=</H1>\n",
            "RB=\n"
            "<br>",
            "NotFound=ERROR",
            "Search=All",
            LAST);

        web_url("WebTours",
            "URL=http://127.0.0.1:1080/WebTours/",
            "TargetFrame=",
            "Resource=0",
            "RecContentType=text/html",
            "Referer=",
            "Snapshot=t1.inf",
            "Mode=HTML",
            LAST);

        lr_output_message("Webcome值为：%s",lr_eval_string("{Webcome}"));

        return 0;
    }

###### 3.2 自动关联 【了解】

    1. 自动关联LoadRunner提供两种方式：
        1). 录制时关联-在录制脚本的时候如果有符合关联规则的就自动进行关联；
        注意：此种方式依赖于LR自带的规则或新建的自定义规则
        2). 录制后关联-脚本录制完成并且运行完一次后，打开扫描工具，进扫描需要关联的数据进行关联；
        注意：
            ① 利用扫描工具必须要先运行一遍脚本
            ② 扫描关联：很多时候LR扫描不到需要关联的信息（比如手写的脚本），所以一般不推荐使用；
    2. 自动关联调用的是关联函数 web_reg_save_param_ex()；
    (web_reg_save_param_ex与web_reg_save_param功能大致相同，它也可以使用正则表达式和过滤器)

###### 1) 自动关联-启用自动关联设置

![关联](/images/correlation_02.png)

启动自动关联位置：工具菜单(Tool) -> Recording Option

###### 2) 自动关联-扫描工具

![关联](/images/correlation_03.png)


1. 启动：打开扫描工具-Ctrl+F8 或者菜单栏(Vuser) -> Scan Script for Correlations<br>
2. 扫描：扫描之前先运行一遍脚本<br>
3. 关联：图中个第二步Correlation Results内进行关联；

#### 自动关联总结

    1. 自动关联-录制时关联：采用自带或设定的规则在录制脚本的时候自动关联
    2. 自动关联-录制后关联：前后运行两次，搜索两次不同之处为需要关联的数据，进行确认关联
    3. 自动关联无论哪种方式，都是采用调用关联函数的方法，所以工具智能人更智能，因此推荐优先使用手动关联

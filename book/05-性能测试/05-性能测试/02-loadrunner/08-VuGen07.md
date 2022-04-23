## <center>VuGen-事务</center>

#### 目标
1. 了解什么是事务
2. 掌握事务的常用方法

#### 一、为什么学习事务？

需求：

    1. 统计订票网站-登录业务需要的具体时间；
    2. 登录账户不是jojo，则登录业务失败；

问题：

    统计时间有很多办法可以做到，但如何把登录业务标记成功或失败呢？

#### 二、事务
事务（Transaction）是用户在应用软件上操作的一个业务或多种业务集合(Actions)的统称；

作用：

    1. 通过事务可标记业务开始和结束，并且可统计相应业务的操作时间(事务时间)【重点】
    2. 通过事务状态机制，可判断相应业务执行的成功与失败；【了解】
    3. 通过事务来衡量服务器性能；

#### 三、添加事务
LR添加事务有两种方式：

    1). 录制脚本的时候添加【了解】
    2). 运行脚本的时候添加【推荐】

##### 3.1 添加事务-录制时添加【了解】

![插入开始事务](/images/Transaction_02.png)

#####3.2 添加事务-运行脚本时添加【重点】

![插入开始事务](/images/Transaction_03.png)

一、插入事务开始 lr_start_transaction("Name")

    1. 插入方式：
        1). 快捷键：Ctrl + T
        2). 鼠标右键Insert -> Start Transaction
        或 工具栏菜单(Insert) -> Start Transaction
    2. 参数：
        Name:事务名称

二、插入事务结束 lr_end_transaction("Name", "status")

    1. 插入方式：
        1). 快捷键：Ctrl + D
        2). 鼠标右键Inert -> End Transaction
        或 工具栏菜单(Insert) -> End Transaction
    2. 参数：
        Name:事务名称和开始事务名相同；
        status:标志事务状态
            1). LR_AUTO: 自动-由LR判断【推荐】
            2). LR_PASS：通过
            3). LR_FAIL: 失败
            4). LR_STOP: 停止

注意：

    1. 插入事务时，尽量不要包含 lr_think_time() 函数；
    2. 事务名称可用中文或英文，但建议使用英文；
    3. 事务的开始和结束名称必须相同；例如：登录(Login)
    4. 事务必须成对出现。
    例如：lr_start_transaction("登录"); <-> lr_end_transaction("登录", LR_AUTO);

##### 3.3 事务响应时间

![响应时间](/images/Transaction_06.png)

1). 一个事务的时间是指事务持续时间(Duration Time)从事务的开始到结束；<br>
2). 事务时间包含：
    1. 响应时间
    2. 浪费时间(Wasted Time)
    3. 思考时间(Think Time)
    4. 事务自身执行时间
3). 在运行脚本后，运行日志内有事务时间、Wasted Time时间，单位为秒；

######1) 响应时间

![响应时间](/images/Transaction_01.png)

    1. 响应时间=T1+T2+T3+T4+T5+T6+T7
    2. 客户端浏览器解析渲染响应数据不算响应时间(Web项目除外)

######2) Think Time(思考时间)

lr_think_time()函数

######3) Wasted Time(浪费时间)

LR自动会把一些函数执行的时间统计为浪费时间，比如（关联、检查点）等函数

######4) 事务函数执行时间

lr_start_transaction("")和lr_end_transaction("", LR_FAIL)这两句代码执行的时间

##### 3.4 事务时间 汇总

1. 站在代码角度，事务的开始与结束，其实就是指定一段代码为一个事务<br>
2. 而代码段内可以包含思考时间、业务代码、事务函数，所以说事务时间包含响应(业务)时间、思考时间、事务自身、其他函数(浪费时间) <br>
3. 事务时间 = 响应时间 + 浪费时间 + 思考时间 + 事务代码的执行时间

#### 四、需求 解决方案分析

1. 在登录函数之前插入开始事务
2. 获取登录后的用户名
3. 判断登录用户名是否为jojo，如果是插入结束事务并且事务状态为：LR_PASS，否则，插入结束事务，事务状态为：
LR_FAIL；

问题：如何判断登录名是否为jojo？

##### 4.1 strcmp函数
比较两个字符串(匹配大小写)是否相等，并返回int型数值<br>
用法：result = strcmp(string1, string2) 

    result:
        1). <0 string1 小于 string2;
        2). =0 string1 等于 string2;
        3). >0 string1 大于 string2;

提示：如果要忽略大小写请使用 stricmp函数，用法参考strcmp

##### 4.2 需求1 解决代码

    Action()
    {    
        // 注意变量必须定义 在开头
        int result;

        lr_start_transaction("登录");

        // 关联 session
        web_reg_save_param("Session",
            "LB=name=userSession value=",
            "RB=>",
            LAST);

        // 打开URL
        web_submit_data("web_submit_data",
            "Action=http://127.0.0.1:1080/WebTours/",
            "Method=GET",
            "TargetFrame=",
            "Referer=",
            "Mode=HTML",
            ITEMDATA,
            LAST);

        // 获取登录后的用户名
        web_reg_save_param("username",
            "LB=Welcome, <b>",
            "RB=</b>,",
            LAST);

        // 登录
        web_submit_data("登录",
            "Action=http://127.0.0.1:1080/WebTours/login.pl",
            "Method=POST",
            "TargetFrame=",    
            "Referer=",
            "Mode=HTML",
            ITEMDATA,
            "Name=userSession", "Value={Session}", ENDITEM,
            "Name=username", "Value=jojo", ENDITEM,
            "Name=password", "Value=bean", ENDITEM,
            LAST);

        // 判断登录用户是否为jojo 0:为相等  ASCII码
        result = strcmp(lr_eval_string("{username}"),"jojo");
        if(result==0){
            lr_end_transaction("登录", LR_PASS);
            lr_output_message("结果为：%d,通过啦！",result);
        }else{
            lr_end_transaction("登录", LR_FAIL);
            lr_output_message("结果为：%d,错误啦！",result);
        }

        return 0;
    }
##### 4.2 需求1 执行结果

![插入开始事务](/images/Transaction_05.png)

##### 4.3 需求1 相关资料
响应登录信息

![插入开始事务](/images/Transaction_04.png)

## <center>VuGen-集合点</center>
#### 目标
1. 了解集合点的概念
2. 掌握集合点的使用

#### 一、为什么要学习集合点？
需求：

    1. 10个用户同时登录订票网站，统计服务器处理登录事务性能；

##### 1.1 问题
1. 10个用户同时登录，如何保证10个虚拟用户同时(忽略绝对论，最大程度去模拟)去操作？

##### 1.2 集合点(rendezvous)
在指定的地点集合指定虚拟用户(Vuser)，条件满足时集合的虚拟用户，同时去操作同一事务

    1. 在VuGen脚本内可以插入集合点，要运行集合点必须配合下一章《Controller》知识点，我们在这里先学习
    如何基于VuGen创建集合点；
    2. 前面了解的并发测试就是基于集合点来完成【重点】

##### 1.3 集合点 创建

![基于VuGen插入集合点](/images/Rendezvous_01.png)

方式：

    1. 指定插入位置，鼠标右键 -> Insert -> Rendezvous【推荐】
    2. 工具栏菜单(Insert) -> Rendezvous

参数：

    Rendezvous Name:集合点名称；在Controller场景运行时使用；例如：Login
提示：

    1. 创建集合点，相当于找好集合的地方，那么到底应该哪些用户及集合多少用户需要单独设置；
    2. 集合点的具体设置和运行，必须在Controller工具内完成；
    3. 由于Controller工具我们目前还没有学习，所以我们先在这里先学习使用它如何设置集合点。

##### 1.4 集合点设置
######1). 基于当前脚本 创建Controller场景

![打开Controller](/images/Rendezvous_02.png)


菜单工具(Tools) -> Create Controller Scenario <br>
(基于当前脚本直接创建场景)

######2). 选择场景模式及虚拟用户数

![打开Controller](/images/Rendezvous_03.png)

设置：

    1. 场景模式：Manual Scenario(手工场景)
    2. Number of Vusers：虚拟用户数

提示：

    场景模式：我们还没了解，在这里暂时使用下手工场景
    虚拟用户数：为并发用户数，我们这里以需求10个并发登录为例

######3) Controller工具内设置集合策略
打开集合点设置

![打开Controller](/images/Rendezvous_04.png)

Controoler工具-菜单(Scenario) -> Rendezvous

设置集合点

![打开Controller](/images/Rendezvous_06.png)

说明：

    1. 创建场景时10个用户，默认10个用户都参与集合点
    2. 可指定用户不参与集合点 选择用户-单击Disable VUser按钮
    3. Policy...：设置集合用户策略

#### Policy集合策略

![打开Controller](/images/Rendezvous_07.png)

    1. 集合策略：
        1). 当所有虚拟用户中的 x% 到达集合点时释放;
        2). 当所有正在运行的虚拟用户中的 x% 到达集合点时释放;
        3). 当 x 个虚拟用户到达集合点时释放;【推荐】
    2. 时间：
        1). Timeout between Vusers：默认30秒；虚拟用户之间的超时时长;
    3. 提示：
        1). 集合点的插入和场景设置我们就学习完了，集合点的执行要在学习完Controller工具后，再进行具体操作；

##### 1.5 需求1 代码

    Action()
    {
        web_url("WebTours",
            "URL=http://127.0.0.1:1080/WebTours/",
            "Resource=0",
            "RecContentType=text/html",
            "Referer=",
            "Snapshot=t1.inf",
            "Mode=HTML",
            LAST);

        lr_think_time(8);

        # 集合点
        lr_rendezvous("Login");

        # 登录事务开始
        lr_start_transaction("登录");

        web_submit_form("login.pl",
            "Snapshot=t2.inf",
            ITEMDATA,
            "Name=username", "Value=jojo", ENDITEM,
            "Name=password", "Value=bean", ENDITEM,
            "Name=login.x", "Value=68", ENDITEM,
            "Name=login.y", "Value=12", ENDITEM,
            LAST); 

        # 登录事务结束
        lr_end_transaction("登录", LR_AUTO);

        lr_think_time(19);

        web_image("SignOff Button",
            "Alt=SignOff Button",
            "Snapshot=t3.inf",
            LAST);

        return 0;
    }

##### 1.6 集合点设置 注意事项

1. 集合点只能在手工场景模式内使用<br>
2. 多个脚本需要同步并发时，可以在不同脚本内设置相同的集合点，再在一个场景内搭建时使用这些脚本<br>
3. 如果脚本内没有设置集合点，在Controller内集合点策略设置功能不生效！<br>
4. 集合点只能在Action部分添加，在 vuser_init 和 vuser_end 中则无法添加！

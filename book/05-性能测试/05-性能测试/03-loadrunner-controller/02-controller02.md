## <center>Goal-Oriented Scenario(目标场景)</center>

#### 目标

了解目标场景的设计方案

#### 一、目标场景 【了解】

#####1.1 目标场景
目标场景设计就是定义要实现的【测试目标】，LR会根据根据这些目标自动构建场景。

LR提供了5种测试目标：：
    
    1、虚拟用户数
    2、每秒点击数
    3、每秒事务数
    4、每分钟页面数
    5、事务响应时间
    (其中每秒点击数、每分钟页面数只适合Web项目)

#####1.2 如何选择启动目标场景

######1) 创建目标场景

![目标场景](/images2/Goal_01.png)

1. 启动Controller
2. Select Scenario Type：选择Goal-Oriented Scenario
3. 根据测试用例给目标场景添加相应业务(单一业务或混合业务)，双击(Available)有效脚本，或选
中点击Add ==>> 按钮

(单一业务：单个业务脚本；混合业务：多个业务脚本【重点】）

######2) 目标场景 图

![目标场景](/images2/Goal_02.png)

**红框选中的为接下来需要关注的地方**

#####1.3 目标场景 计划与编辑

1. 启动编辑场景
2. 编辑场景

######1) 启动编辑场景

![目标场景](/images2/Goal_03.png)

1. 场景默认数据：为 Controller 默认创建目标场景时设计的数据
2. Edit Scenario Goal：编辑目标场景-修改目标场景数据

######2）编辑场景

![目标场景](/images2/Goal_04.png)

    标注1：新建场景、修改现有场景名称、设置场景启动时间
    标注2：设置目标场景的测试目标类型及目标范围【相对目标场景 重点】
    标注3：场景运行时间及策略设计
    标注4：负载行为(会根据场景目标变化对应内容)

######标注 1 (新建场景、修改现有场景名称、设置场景启动时间)

![目标场景](/images2/Goal_05.png)

1. Scenario Start Time：设计完场景后，场景启动时间；一般默认即可

    1). Without delay：立即运行场景（默认）
    2). With a delay of HH:MM:SS：等待指定时间运行场景
    3). At HH:MM:SS on Y/M/D：指定X年X月X日X时X分X秒运行场景

2. New：创建一个新的计划场景

######标注 2 【目标场景的核心】

![目标场景](/images2/Goal_07.png)


    1. Goal Type：本次性能测试的测试目标
    2. 由于此选项比较重要，在接下来第二小节【二、目标场景5种 测试目标】中单独讲解

######标注 3 (场景运行时间及策略设计)

![目标场景](/images2/Goal_08.png)

1. 标示1(Run time)：场景达到目标后，继续运行多长时间；（一般10-30分钟）
2. 标示2(If target cannot be reached)：如果达不到设定目标时的处理方式

    1). Stop scenario and save results 停止并保存运行结果<br>
    2). Continue Scenario without reaching 继续执行场景直到达到目标位置 【默认】

提示：
    以上选项如没有特殊业务需求，默认即可；

######标注 4 (负载行为)

![目标场景](/images2/Goal_09.png)

1. Automatic：系统自动设置【默认】
2. Reach target number of hits per second 00:10:00 设定10分钟后达到指定的每秒点击数
3. Step up by 20 hits per second every 00:02:00 表示以每2分钟加载20个点击数

    提示：以上选项会根据不同的场景目标进行变化，目标类型不同选项会有所差异，但都大同小异

#### 二、目标场景5种 测试目标

1. Virtual users             (虚拟用户)
2. Hits per Second           (每秒点击数)
3. Transactions per Second   (每秒事务数)
4. Transaction Response time (事务响应时间)
5. Pages per Minute          (每分钟页面数)

    提示： Hits per Second 和 Pages per Minute 只适合用于 Web 项目

#####2.1 Virtual users(虚拟用户)

![虚拟用户](/images2/Goal_Type_01.png)

Virtual user 目标：测试应用程序是否可以同时运行指定数量的虚拟用户数

#####2.2 Hits per Second(每秒点击数)

![每秒点击率](/images2/Goal_Type_02.png)

1. Hits per Second 目标：测试服务器的每秒点击数<br>
(指定每秒点击数目标以及达到这一目标的最小用户数和最大用户数)
2. 负载行为（Load Behavior）应选择 自动加载(Automatic)
3. Hits per Second只适合Web项目【注意】

#####2.3 Transactions per Second(每秒事务数)

![每秒事务数](/images2/Goal_Type_03.png)

1. Transactions per Second 目标：测试服务器每秒事务数(指定事务)<br>
(指定每秒完成事务数目标以及达到这一目标的最小用户数和最大用户数)
2. 在VuGen中必须插入相应的事务才能够进行设置！

#####2.4 Transaction Response Time(事务响应时间)

![事务响应时间](/images2/Goal_Type_04.png)

Transaction Response Time 目标：测试在不超过预期事务响应时间(目标)的情况下可以运行多少个用户，必须指定预期时间、最少用户、最大用户。<br>
(如：需求100个人使用支付软件付款，在3秒钟内完成；预期目标时间为3秒，最少用户80，最多用户180)

#####2.5 Pages per Minute(每分钟页面数)

![每分钟页面数](/images2/Goal_Type_05.png)

1. Pages per Minute 目标：测试服务器每分钟页面数
(指定每分钟完成页面数目标以及达到这一目标的最小用户数和最大用户数)
2. Pages per Minute只适合Web项目【注意】

提示:

    1. 目标场景主要采用内在工具来最大限度完成测试目标，工具很智能，但人更智能。在实际工作场景中使用最多的还是手工场景；
    2. 目标场景和手工场景都只是测试场景的搭建模式，场景的运行和监控都是一样。

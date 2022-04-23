## <center>LoadRunner介绍</center>

### 一、LoadRunner简介

LoadRunner是一种工业级标准的性能负载测试工具；可以模拟上千万用户实施测试，并在测试时可实时检测应用服务器及服务器硬件的各种数据，来查找和确认存在的性能瓶颈；

支持多种协议，如：Web(HTTP/HTML)、Windows Sockets、FTP、ODBC、MS SQL Server等协议

### 二、LoadRunner组成【非常重要】

组成：
    1. Virtual User Generator（VuGen）
    2. Controller 
    3. Analysis 

提示：
    1. VuGen：脚本生成器-脚本录制、编辑
    2. Controller：控制器-设计场景、运行、监控
    3. Analysis：测试结果分析

### 三、 理解LoadRunner的三大工具（扩展）

为了更好的理解LoadRunner性能测试三大工具作用，我们先回顾下不使用测试工具时，如果进行性能测试...

需求：
    
    软件系统支持100人同时登录

实施：
    1. 找100个员工及100台电脑，每个员工注册账号OK，环境OK，输入账号、密码完成...鼠标放到登录按钮上；
    2. 领导拿个大喇叭，高喊:1...2...3
    3. 相关人员统计平均登录时间及登录前、登陆后的服务器CPU、内存等使用情况

LoadRunner的执行原理：【重点】

    1. VuGen：相当于打开登录页面、输入账号、输入密码、点击登录
    2. Controller：手拿大喇叭的领导...
    3. Analysis：相当与各个统计数据的相关人员
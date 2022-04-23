## Web自动化测试

### 目标

```
1. 了解什么是Web自动化测试
2. 了解Web自动化测试常用工具   
```

## 1. 什么是Web自动化测试？

```
概念：让程序代替人为自动验证Web项目功能的过程
```

## 2. 什么项目适合做自动化测试？

```
1.需求变动不频繁
2.项目周期长
3.项目需要回归测试
```

## 3. 如何进行自动化测试？(主流测试-工具)

```
1. QTP（收费）
        QTP是商业的功能测试工具，收费，支持web，桌面自动化测试。
2. Selenium（开源）【本阶段学习】
        Selenium是开源的web自动测试工具，免费，主要做功能测试。
3. Jmeter（开源、Web、接口、性能, 接口测试使用）
        Jmeter是由Apache公司使用Java平台开发的一款测试工具，支持（Web、接口测试、性能测试）
        提示：Web测试在通信层(无UI界面)
5. Loadrunner（收费、Web、性能）
        Loadrunner是商业性能测试工具，收费，功能强大，适合做复杂场景的性能测试
6. Robot framework
        Robot Framework是一个基于Python可扩展地(关键字驱动)的测试自动化框架；
```

### 3.1 主流工具-汇结：

```
    Web自动化测试：selenium、robot framework
    App端自动化测试：Appium、Monkeyrunner、UIautomation
    PC客户端（win32）自动化测试：QTP
    接口自动化测试：Jmeter、Postman、httpUnit、RESTClient
    云测平台：Testin Testbird
    性能测试：Jmeter、LoadRunner
```

## 4. 什么是Selenium？

```
概念： Selenium是一个用于Web应用程序测试的工具；中文的意思（硒）
```

### 4.1 Selenium特点

```
1. 开源软件：源代码开放可以根据需要来增加工具的某些功能
2. 跨平台：linux 、windows 、mac
3. 核心功能：就是可以在多个浏览器上进行自动化测试
4. 多语言：Java、Python、C#、JavaScript、Ruby等
5. 成熟稳定：目前已经被google , 百度， 腾讯等公司广泛使用
6. 功能强大：能够实现类似商业工具的大部分功能，因为开源性，可实现定制化功能
```

### 4.2 Selenium家族(发展史) 【了解】

![senium](/images/selenium.png)

```
重点：    
    1. SeleniumIDE
    2. Selenium2.0(WebDriver)
```
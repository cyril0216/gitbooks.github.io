## <center>VuGen-参数化</center>

#### 目标

1. 理解参数化的使用场景<br>
2. 掌握参数化的使用方式

#### 一、什么是参数化
根据需求动态的获取数据的过程

#### 二、为什么要参数化？
1. 减少重复代码<br>
2. 数据代码进行分离，方便维护

需求1

    说明：由于虚拟机运行速度原因，我们使用 lr_output_message() 函数，来学习如何使用参数化
    要求：输出 我要去北京；我要去上海；我要去广州；我要去深圳；

#### 三、参数化操作

##### 3.1 打开参数化菜单

![参数化](/images/parameter_01.png)

    选中要参数化的文本 -> 鼠标右键 -> Replace with a Parameter
    或者
    菜单 Insert -> New Parameter...

##### 3.2 Replace with a Parameter 打开的窗口

![参数化](/images/parameter_02.png)

    1. Parameter name:参数化引用名称（参数化时使用）
    2. Parameter type:File 【推荐】
    3. Properties:属性-点击此按钮，可直接设置参数化引用名称对应的值；【点击】

    提示：
        1). 点击OK；参数化引用名称具体的值，需要再次设置；

##### 3.3 Properties 设置面板

![参数化](/images/parameter_03_1.png)

    1. Parameter type: 参数类型 File 【重点】
    2. File:参数保存的文件类型及位置，一般为txt;【推荐】
    3. Add Row:添加行，在当前表格编辑数据；【可选】
    4. Edit with Notepad:在记事本内编辑数据；【推荐】
    5. Select next row:运行或迭代时对行的选择方式 
    6. Update value on:运行或迭代时对值的选取方式

    提示：
        1). 其中5.Select next row 与 6.Update value on为组合关系，不同组合效果不同，我们单独来学习

##### 需求1 解决方案

1. 创建指定路径及txt文本格式 如：C:\lr11\scripts\city.txt
2. 使用 Edit with Notepad 编辑数据：北京、上海、广州、深圳
3. 编写脚本 
4. 迭代次数 4

问题

    lr_output_message(“str”):函数为把str字符串输出到日志，但它自身并不能解析读取参数；

##### 函数(设置参数、读取参数)

1. lr_save_string() 
2. lr_eval_string()

###### 1. lr_save_string()

把字符串保存到指定的参数中<br>
格式：lr_save_string("字符串","参数名")<br>
示例：lr_save_string("北京","city");

###### 2. lr_eval_string()

读取变量的值，并以Char类型返回;<br>
格式：lr_eval_string("{参数名}");<br>
示例：lr_eval_string("{city}");

    提示：
        1). 参数名称使用花括号{}包括
        2). 参数使用双引号包括

###### 需求1 代码示例
    Action()
    {
        lr_output_message("我要去%s",lr_eval_string("{NewParam}"));
        return 0;
    }

##### 3.4 Select next row 与 Update value on

###### Select next row

读取参数文件行的方式

方式：

    1. Sequential：顺序，按照行的顺序读取数据，如果迭代次数大于行数，再次从第一行开始读取
    2. Random：随机，随机读取行
    3. Unique：唯一，每次读取唯一行，不重复；
    4. Same line as xxx  取参数同行(需要两个以上参数，才能显示和使用)

注意：

    使用 Unique 选项时会激活 When out of values:
    1) Abort Vuser：中止虚拟用户
    2) Continue in a cyclic manner：以循环的方式继续
    3) Continue with last value：继续使用最后一个值

###### Update value on

参数值更新的方式

方式：

    1. Each iteration：每次迭代以后更新
    2. Each occurrence：每次出现参数时更新
    3. Once：每出现一个虚拟用户(线程)更新一次

###### 组合

1. 行顺序 + 每次迭代更新值    Sequential + Each iteration
2. 行顺序 + 每次参数出现更新值    Sequential + Each occurrence
3. 行顺序 + 更新一次(单用户)    Sequential + Once

说明：
        1. 组合我们在这里就不穷举了，以行的顺序配合值的3种更新方式做演示
        2. 因为组合内含occurrence，所以必须要两个Action，我们新增两个（Action1、Action2）两个代码相同

###### 代码
    Action1()
    {
        lr_output_message("我要去%s",lr_eval_string("{NewParam}"));
        return 0;
    }

    Action2()
    {
        lr_output_message("我要去%s",lr_eval_string("{NewParam}"));
        return 0;
    }

###### Sequential + Each iteration
行为顺序，值为迭代更新一次；

预期：

    1. 第一次迭代：Action1:北京；Action2:北京
    2. 第二次迭代：Action1:上海；Action2:上海
    3. 第三次迭代：Action1:广州；Action2:广州
    4. 第四次迭代：Action1:深圳；Action2:深圳

###### Sequential + Each occurrence
行为顺序，值为出现一次更新一次

预期：

    1. 第一次迭代：Action1:北京；Action2:上海
    2. 第二次迭代：Action1:广州；Action2:深圳
    3. 第三次迭代：Action1:北京；Action2:上海
    4. 第四次迭代：Action1:广州；Action2:深圳

###### Sequential + Once
行为顺序，值为单用户只更新一次

预期：

    1. 第一次迭代：Action1:北京；Action2:北京
    2. 第二次迭代：Action1:北京；Action2:北京
    3. 第三次迭代：Action1:北京；Action2:北京
    4. 第四次迭代：Action1:北京；Action2:北京

#### 批量获取参数时，如果保证参数不重复？（扩展）

    思路：添加时间戳
    说明：web_save_timestamp_param("time_stamp",LAST) 为时间戳函数，返回：1970到现在的毫秒数；
    (time_stamp：参数为获取值后保存的参数名称，被引用时使用)

示例：

    Action()
    {
        web_save_timestamp_param("time_stamp",LAST);
        lr_output_message("我要去%s",lr_eval_string("{NewParam}_{time_stamp}"));
        return 0;
    }

#### 其他常用参数类型（了解）

    parameter type:
        1. Date/Time
        2. Random Number

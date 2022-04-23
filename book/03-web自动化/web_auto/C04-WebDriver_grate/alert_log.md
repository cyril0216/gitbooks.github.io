# 附件-断言资料

#### 基本布尔型断言

| 序号 | 断言方法                                | 断言描述                                     |
| ---- | --------------------------------------- | -------------------------------------------- |
| 1    | assertEqual(arg1, arg2, msg=None)       | 验证arg1=arg2，不等则fail 【常用】           |
| 2    | assertNotEqual(arg1, arg2, msg=None)    | 验证arg1 != arg2, 相等则fail                 |
| 3    | assertTrue(expr, msg=None)              | 验证expr是true，如果为false，则fail 【常用】 |
| 4    | assertFalse(expr,msg=None)              | 验证expr是false，如果为true，则fail 【常用】 |
| 5    | assertIs(arg1, arg2, msg=None)          | 验证arg1、arg2是同一个对象，不是则fail       |
| 6    | assertIsNot(arg1, arg2, msg=None)       | 验证arg1、arg2不是同一个对象，是则fail       |
| 7    | assertIsNone(expr, msg=None)            | 验证expr是None，不是则fail                   |
| 8    | assertIsNotNone(expr, msg=None)         | 验证expr不是None，是则fail                   |
| 9    | assertIn(arg1, arg2, msg=None)          | 验证arg1是arg2的子串，不是则fail             |
| 10   | assertNotIn(arg1, arg2, msg=None)       | 验证arg1不是arg2的子串，是则fail             |
| 11   | assertIsInstance(obj, cls, msg=None)    | 验证obj是cls的实例，不是则fail               |
| 12   | assertNotIsInstance(obj, cls, msg=None) | 验证obj不是cls的实例，是则fail               |

#### 比较断言

| 序号 | 断言方法                                                     | 断言描述                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | assertAlmostEqual (first, second, places = 7, msg = None, delta = None) | 验证first约等于second。 palces: 指定精确到小数点后多少位，默认为7 |
| 2    | assertNotAlmostEqual (first, second, places, msg, delta)     | 验证first不约等于second。 palces: 指定精确到小数点后多少位，默认为7 注： 在上述的两个函数中，如果delta指定了值，则first和second之间的差值必须≤delta |
| 3    | assertGreater (first, second, msg = None)                    | 验证first > second，否则fail                                 |
| 4    | assertGreaterEqual (first, second, msg = None)               | 验证first ≥ second，否则fail                                 |
| 5    | assertLess (first, second, msg = None)                       | 验证first < second，否则fail                                 |
| 6    | assertLessEqual (first, second, msg = None)                  | 验证first ≤ second，否则fail                                 |
| 7    | assertRegexpMatches (text, regexp, msg = None)               | 验证正则表达式regexp搜索匹配的文本text。 regexp：通常使用re.search() |
| 8    | assertNotRegexpMatches (text, regexp, msg = None)            | 验证正则表达式regexp搜索不匹配的文本text。 regexp：通常使用re.search() 说明：两个参数进行比较（＞、≥、＜、≤、约等、不约等） |

#### 复杂断言

| 序号 | 断言方法                                      | 断言描述                                                     |
| ---- | --------------------------------------------- | ------------------------------------------------------------ |
| 1    | assertListEqual(list1, list2, msg = None)     | 验证列表list1、list2相等，不等则fail，同时报错信息返回具体的不同的地方 |
| 2    | assertTupleEqual (tuple1, tuple2, msg = None) | 验证元组tuple1、tuple2相等，不等则fail，同时报错信息返回具体的不同的地方 |
| 3    | assertSetEqual (set1, set2, msg = None)       | 验证集合set1、set2相等，不等则fail，同时报错信息返回具体的不同的地方 |
| 4    | assertDictEqual (expected, actual, msg = None | 验证字典expected、actual相等，不等则fail，同时报错信息返回具体的不同的地方 |


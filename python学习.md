#python学习入门
写在前面：学习Python其实没啥功利性的目的，之前有几次同事问我会不会用Python帮忙处理下问题。我回答：不会。感觉不是很好，平时复杂一些的程序用java来写，也学习AWK&sed来处理一些文本。
<br>
多学一点也没啥坏处，给自己的工具箱里也多添一件工具。
按照《Dive Into Python》来做下笔记，有的地方写了自己的一些看法，方便自己后续的复习。

### 第一天 第一个Python程序
1 函数声明<br>
def buildConnectionString(params):<br>
函数没有定义返回的数据类型，Python不需要指定返回值的数据类型；甚至不需要指定是否有返回值。<br><br>
2 类型指定<br>
在Java、C++ 和其他静态类型语言中，必须要指定函数返回值和每个函数参数的数据类型。在Python中，永远也不需要明确指定任何东西的数据类型。
Python会根据赋给它的值在内部将其数据类型记录下来。<br><br>
3 文档化函数<br>
```python
def buildConnectionString(params):
"""Build a connection string from a dictionary of parameters.

Returns string."""
```
三重引号表示一个多行字符串。在开始与结束引号间的所有东西都被视为单个字符串的一部分，包括硬回车和其它的引号字符。您可以在任何地方使用它们，但是您可能会发现，它们经常被用于定义doc string。它们用来说明函数可以做什么。如果存在doc string，它必须是一个函数要定义的第一个内容(也就是说，在冒号后面的第一个内容)。<br><br>
4 使用模块的方法<br>
当使用在被导入模块中定义的函数时，必须包含模块的名字。（类似于使用java类的static方法）<br><br>
5 python对象<br>
在 Python 中，定义是松散的;某些对象既没有属性也没有方法。但是万物皆对象从感性上可以解释为：一切都可以赋值给变量或作为参数传递给函数。在 Python 中万物皆对象。<br><br>
6 代码缩进<br>
Python使用硬回车来分割语句，冒号和缩进来分割代码块。C++和Java使用分号来分割语句,花括号来分割代码块。<br><br>
7 if语句<br>
```python
if __name__ == "__main__":
```
首先，if表达式无需使用圆括号括起来。其次,if语句以冒号结束，紧跟其后的是缩进代码。<br><br>
### 第二天 内置数据类型
1 Dictionary<br>
Dictionary是Python的内置数据类型之一，它定义了键和值之间一对一的关系。很像像Java中的 Hashtable类的实例。Dictionary没有元素顺序的概念。并且dictionary的key是大小写敏感的。 <br>
```python
>>> d = {"server":"mpilgrim", "database":"master"} 
>>> d
{'server': 'mpilgrim', 'database': 'master'}
>>> d["server"]
'mpilgrim'
```

删除元素：<br>
d.clear():清空元素。<br>
del d[key]:删除独立元素。<br>

大括号扩起来，逗号分隔，方括号获取。<br>

2 List<br>
它更像是java中的ArrayList类，它可以保存任意对象，并且可以在增加新元素时动态扩展。<br>
```python
>>> li = ["a", "b", "mpilgrim", "z", "example"]
>>> li
['a', 'b', 'mpilgrim', 'z', 'example']
```

负数索引从list的尾部开始向前计数来存取元素,可以这样理解： li[-n] == li[len(li) - n]。<br>
list的分片：<br>
list[begin:end]（包括begin,不包括end）<br>

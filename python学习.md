#python学习入门
写在前面：学习Python其实没啥功利性的目的，之前有几次同事问我会不会用Python帮忙处理下问题。我回答：不会。感觉不是很好，平时复杂一些的程序用java来写，也学习AWK&sed来处理一些文本。
<br>
多学一点也没啥坏处，给自己的工具箱里也多添一件工具。
目的是让有java基础的人，可以快速使用Python。

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
向list中增加元素：<br>
```python
>>> li
['a', 'b', 'mpilgrim', 'z', 'example']
>>> li.append("new")      
>>> li
['a', 'b', 'mpilgrim', 'z', 'example', 'new']
>>> li.insert(2, "new") 
>>> li
['a', 'b', 'new', 'mpilgrim', 'z', 'example', 'new']
>>> li.extend(["two", "elements"])
>>> li
['a', 'b', 'new', 'mpilgrim', 'z', 'example', 'new', 'two', 'elements']
```

注意:append向list的末尾追加单个元素,即不论是什么，都当作一个单独的对象；而extend用来连接 list。<br>

在list中搜索：<br>
```python
>>> li
['a', 'b', 'new', 'mpilgrim', 'z', 'example', 'new', 'two', 'elements']
>>> li.index("example")
5
>>> li.index("new")
2
>>> li.index("c") 
Traceback (innermost last):
 File "<interactive input>", line 1, in ?
ValueError: list.index(x): x not in list
>>> "c" in li 
False
```

注意：与java不同，在如果在list中没有找到值，Python会引发一个异常。并且注意布尔值的False和True。<br>
<br>
从list中删除元素：<br>
```python
>>> li
['a', 'b', 'new', 'mpilgrim', 'z', 'example', 'new', 'two', 'elements']
>>> li.remove("z") 
>>> li
['a', 'b', 'new', 'mpilgrim', 'example', 'new', 'two', 'elements']
>>> li.remove("c") 
Traceback (innermost last):
File "<interactive input>", line 1, in ?
ValueError: list.remove(x): x not in list
>>> li.pop()   
'elements'
>>> li
['a', 'b', 'mpilgrim', 'example', 'new', 'two']
```
remove从list中删除一个值的首次出现。<br>
pop 是一个有趣的东西。它会做两件事：删除list的最后一个元素，然后返回删除元素的值。<br>

使用list的运算符:<br>
```python
>>> li = ['a', 'b', 'mpilgrim']
>>> li = li + ['example', 'new'] 
>>> li
['a', 'b', 'mpilgrim', 'example', 'new']
>>> li += ['two']     
>>> li
['a', 'b', 'mpilgrim', 'example', 'new', 'two']
>>> li = [1, 2] * 3  
>>> li
[1, 2, 1, 2, 1, 2]
```

Lists也可以用+运算符连接起来。list= list+otherlist相当于list.extend(otherlist)。但+运算符把一个新(连接后)的list作为值返回， 而extend只修改存在的list。也就是说，对于大型 list来说，extend的执行速度要快一些。<br>
（有点像java中的String拼接和StringBuffer的append方法）<br>
"*"运算符可以作为一个重复器作用于list。 <br>

3 Tuple介绍<br>
Tuple是不可变的list。定义tuple与定义list的方式相同，但整个元素集是用小括号包围的，而不是方括号。与list一样分片(slice)也可以使用。注意当分割一个list时，会得到一个
新的list；当分割一个tuple时，会得到一个新的tuple。<br>
Tuple没有index方法。然而，您可以使用in来查看一个元素是否存在于 tuple中。<br>
tuple比list操作速度快。如果定义了一个值的常量集，并且唯一要用它做的是不断地遍历它，请使用 tuple代替list。<br>
Tuples可以在dictionary中被用做key，但是list不行。实际上，事情要比这更复杂。Dictionary key必须是不可变的。<br>
<br>
Tuple可以转换成list，反之亦然。内置的tuple函数接收一个list，并返回一个有着相同元素的tuple。而list函数接收一个tuple返回一个list。从效果上看，tuple冻结一个list，而list解冻一个tuple。<br>
<br>

4 变量声明<br>
变量通过首次赋值产生，当超出作用范围时自动消亡。<br>
当一条命令用续行符(“\”)分割成多行时，后续的行可以以任何方式缩进。严格地讲，在小括号，方括号或大括号中的表达式可以用或者不用续行符 (“\”) 分割成多行。<br>
Python不允许您引用一个未被赋值的变量，试图这样做会引发一个异常。<br>
```python
>>> v = ('a', 'b', 'e')
>>> (x, y, z) = v  
>>> x
'a'
```
将一个tuple赋值给另一个tuple，会按顺序将v的每个值赋值给每个变量。<br>
```python
>>> range(7)
[0, 1, 2, 3, 4, 5, 6]
```
内置的range函数返回一个元素为整数的list,但不包含上限值。<br>
<br>
5 格式化字符串<br>
```python
>>> k = "uid"
>>> v = "sa"
>>> "%s=%s" % (k, v)
'uid=sa'
```
与java不同的是，试图将一个字符串同一个非字符串连接会引发一个异常。 <br>

6 映射list<br>
python可以通过对list中的每个元素应用一个函数，从而将一个list映射为另一个list。<br>
```python
>>> li = [1, 9, 8, 4]
>>> [elem*2 for elem in li]  
[2, 18, 16, 8]
>>> li            
[1, 9, 8, 4]
>>> li = [elem*2 for elem in li] 
>>> li
[2, 18, 16, 8]
```
需要注意是，对list的解析并不改变原始的list<br>
```python
dictionary的keys,values和items函数<br>
>>> params = {"server":"mpilgrim", "database":"master", "uid":"sa", "pwd":"secret"}
>>> params.keys()  
['server', 'uid', 'database', 'pwd']
>>> params.values() 
['mpilgrim', 'sa', 'master', 'secret']
>>> params.items()
[('server', 'mpilgrim'), ('uid', 'sa'), ('database', 'master'), ('pwd', 'secret')]
```
7 连接list与分割字符串<br> 
把list中的元素以“分号”进行连接，输出成字符串。<br>
return ";".join(["%s=%s" % (k, v) for k, v in params.items()])<br>
join只能用于元素是字符串的list；它不进行任何的强制类型转换。连接一个存在一个或多个非字符串元素的list将引发一个异常。<br>
与join相反的是split，把字符串转成list。<br>
```python
>>> s.split(";") 
['server=mpilgrim', 'uid=sa', 'database=master', 'pwd=secret']
```
### 第三天 自省的威力
恩，你有更好的求助资源，我的智囊团钱庄同学啊。。。
#python学习入门
写在前面：给自己的工具箱里也多添一件工具没啥坏处。
目的是让有java、c++基础的人，可以快速使用Python(相同部分不再赘述)。

### 一、基础 
* 在计算机内部，Python解释器把源代码转换成称为字节码的中间形式，然后再把它翻译成计算机使用的机器语言并运行。
<br>
* 我们将看一下如何用Python编写运行一个传统的“Hello World”程序。通过它，你将学会如何编写、保存和运行Python程序。源文件：<br>
```
#!/usr/bin/python
# Filename : helloworld.py
print 'Hello World'
```
运行及输出：<br>
```
$ python helloworld.py
Hello World
```

* 注意Python是大小写敏感的. <br>
* 任何在#符号右面的内容都是注释。<br>
* Python是大小写敏感的。<br>
* help(str)会显示str类的帮助。按q退出帮助。<br>
* 在Python中有4种类型的数——整数、长整数、浮点数(如52.3E-4)和复数(如(-5+4j)。<br>

* 在Java、C++ 和其他静态类型语言中，必须要指定函数返回值和每个函数参数的数据类型。在Python中，永远也不需要明确指定任何东西的数据类型。Python会根据赋给它的值在内部将其数据类型记录下来。<br>

* 你可以用单引号指示字符串.在双引号中的字符串与单引号中的字符串的使用完全相同。利用三引号，你可以指示一个多行的字符串。行末的单独一个反斜杠表示字符串在下一行继续，而不是开始一个新的行。如果你想要指示某些不需要如转义符那样的特别处理的字符串，那么你需要指定一个自然字符串。自然字符串通过给字符串加上前缀r或R来指定。Python允许你处理Unicode文本——你只需要在字符串前加上前缀u或U。<br>

* 如果你把两个字符串按字面意义相邻放着，他们会被Python自动级连。<br>
* 在Python中没有专门的char数据类型。<br>
* 变量第一个字符必须是字母表中的字母（大写或小写）或者一个下划线。其他部分可以由字母（大写或小写）、下划线或数字组成。并且对大小写敏感的。<br>

* 使用变量时只需要给它们赋一个值。不需要声明或定义数据类型。<br>

* 空白在Python中是重要的。事实上行首的空白是重要的。它称为缩进。同一层次的语句必须有相同的缩进。每一组这样的语句称为一个块。不要混合使用制表符和空格来缩进，因为这在跨越不同的平台的时候，无法正常工作。强烈建议在每个缩进层次使用单个制表符或两个或四个空格。
选择这三种缩进风格之一。更加重要的是，选择一种风格，然后一贯地使用它，即只使用这一种风格。<br>

* **:代表幂运算符。//:取整除,返回商的整数部分。not,and,or分别代表布尔非，与，或。<br>

* if语句:<br>
```python
if guess == number:
    print '1'
elif guess < number:
    print '2'
else:
    print '3' 
```
注意:elif，没有括号，以及有冒号。<br>

* 在Python中没有switch语句。<br>
* while语句：<br>
```python
while running:
    ....
else
    ....
```
你可以在while循环中使用一个else从句，但else块事实上是多余的。<br>

* for语句:
```python
for i in range(1, 5):
    print i
else:
    print 'The for loop is over'
```
for也可以使用一个else从句。<br>
注意:range(1,5)给出序列[1, 2, 3, 4]。默认地，range的步长为1。如果我们为range提供第三个数，那么它将成为步长。例如，range(1,5,2)给出[1,3]。记住，range 向上 延伸到第二个数，即它不包含第二个数。<br>
while和for中可以使用break和continue语句。<br>

* True和False被称为布尔类型。<br>
* 输入字符串的长度通过内建的len函数取得。<br>

* 函数声明：<br>
```python
def sayHello():
    print 'Hello World!'
```
函数没有定义返回的数据类型，Python不需要指定返回值的数据类型；甚至不需要指定是否有返回值。<br>

* 通过global语句，可以为定义在函数外的变量赋值的。<br>
```python
global x
```

* 使用默认参数值：
```python
def say(message, times = 1):
    print message * times

say('Hello')
say('World', 5)
```
输出：<br>
```python
$ python func_default.py
Hello
WorldWorldWorldWorldWorld
```
注：Python中，字符串*整数N，会将这个字符串拼接N遍。<br>

* 关键参数<br>
如果你的某个函数有许多参数，而你只想指定其中的一部分，那么你可以通过命名来为这些参数赋值——这被称作关键参数——我们使用名字而不是位置来给函数指定实参。
这样做有两个优势:一，由于我们不必担心参数的顺序，使用函数变得更加简单了。二、假设其他参数都有默认值，我们可以只给我们想要的那些参数赋值。

```python
def func(a, b=5, c=10):
    print 'a is', a, 'and b is', b, 'and c is', c

func(3, 7)
func(25, c=24)
func(c=50, a=100)
```
输出：<br>
```
$ python func_key.py
a is 3 and b is 7 and c is 10
a is 25 and b is 5 and c is 24
a is 100 and b is 5 and c is 50
```

* 没有返回值的return语句等价于return None。None是Python中表示没有任何东西的特殊类型。<br>

* pass语句在Python中表示一个空的语句块。(等同于java中的单独分号。)<br>

* 文档字符串：<br>
三重引号表示一个多行字符串。在开始与结束引号间的所有东西都被视为单个字符串的一部分，包括硬回车和其它的引号字符。您可以在任何地方使用它们，但是您可能会发现，它们经常被用于定义doc string。它们用来说明函数可以做什么。如果存在doc string，它必须是一个函数要定义的第一个内容(也就是说，在冒号后面的第一个内容)。<br>
之前的help()所做的只是抓取函数的__doc__属性。
，你也可以通过functionname._doc_ 来进行调用。<br>

* 引入模块：import 模块名<br>
输入一个模块相对来说是一个比较费时的事情，所以通过引入创建以.pyc作为扩展名的字节编译的文件会更快一些。另外，这些字节编译的文件也是与平台无关的。<br>
<br>
如果你想要直接输入argv变量到你的程序中（避免在每次使用它时打sys.），那么你可以使用from sys import argv语句。如果你想要输入所有sys模块使用的名字，那么你可以使用from sys import *语句。这对于所有模块都适用。一般说来，应该避免使用from..import而使用import语句，因为这样可以使你的程序更加易读，也可以避免名称的冲突。<br>
记住这个模块应该被放置在我们输入它的程序的同一个目录中，或者在sys.path所列目录之一。<br>

* 每个模块都有一个名称，在模块中可以通过语句来找出模块的名称。可以通过模块的```__name__```属性完成。每个Python模块都有它的_```_name__```，如果它是```__main__```，这说明这个模块被用户单独运行.
```python
#!/usr/bin/python
# Filename: using_name.py

if __name__ == '__main__':
    print 'This program is being run by itself'
else:
    print 'I am being imported from another module'
```
运行：<br>
```
$ python using_name.py
This program is being run by itself

$ python
>>> import using_name
I am being imported from another module
>>>
```

* 你可以使用内建的dir函数来列出模块定义的标识符。标识符有函数、类和变量。
当你为dir()提供一个模块名的时候，它返回模块定义的名称列表。

* del用来删除一个变量/名称。例如del a，你将无法再使用变量a——它就好像从来没有存在过一样。<br>

### 二、数据结构
* 在Python中有三种内建的数据结构——列表、元组和字典。<br>

* List<br>
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

注意：与java不同，在如果在list中没有找到值，Python会引发一个异常。并且注意布尔值的False和True。
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
*运算符可以作为一个重复器作用于list。 <br>

* 元组最通常的用法是用在打印语句中：<br>
```python
age = 22
name = 'Swaroop'
print '%s is %d years old' % (name, age)
```

Tuple是不可变的list。定义tuple与定义list的方式相同，但整个元素集是用小括号包围的，而不是方括号。与list一样分片(slice)也可以使用。注意当分割一个list时，会得到一个新的list；当分割一个tuple时，会得到一个新的tuple。<br>

Tuple没有index方法。然而可以使用in来查看一个元素是否存在于 tuple中。<br>

tuple比list操作速度快。如果定义了一个值的常量集，并且唯一要用它做的是不断地遍历它，请使用 tuple代替list。<br>

Tuple可以转换成list，反之亦然。内置的tuple函数接收一个list，并返回一个有着相同元素的tuple。而list函数接收一个tuple返回一个list。从效果上看，tuple冻结一个list，而list解冻一个tuple。<br>
<br>

* Dictionary<br>
Dictionary定义了键和值之间一对一的关系。很像像Java中的 Hashtable类的实例。Dictionary没有元素顺序的概念。键值对在字典中以这样的方式标记：d = {key1 : value1, key2 : value2 }。注意它们的键/值对用冒号分割，而各个对用逗号分割，所有这些都包括在花括号中。记住字典中的键/值对是没有顺序的。 <br>
```python
ab = {       'Swaroop'   : 'swaroopch@byteofpython.info',
             'Larry'     : 'larry@wall.org',
             'Matsumoto' : 'matz@ruby-lang.org',
             'Spammer'   : 'spammer@hotmail.com'
     }

print "Swaroop's address is %s" % ab['Swaroop']

# Adding a key/value pair
ab['Guido'] = 'guido@python.org'

# Deleting a key/value pair
del ab['Spammer']

print '\nThere are %d contacts in the address-book\n' % len(ab)
for name, address in ab.items():
    print 'Contact %s at %s' % (name, address)

if 'Guido' in ab: # OR ab.has_key('Guido')
    print "\nGuido's address is %s" % ab['Guido']
```
删除元素：<br>
d.clear():清空元素。<br>
del d[key]:删除独立元素。<br>

* 变量声明<br>
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

* 列表、元组和字符串都是序列，但是序列是什么，它们为什么如此特别呢？序列的两个主要特点是索引操作符和切片操作符。索引操作符让我们可以从序列中抓取一个特定项目。切片操作符让我们能够获取序列的一个切片，即一部分序列。索引操作不多赘述。<br>
list[1:3]返回从位置1开始，包括位置2，但是停止在位置3的一个序列切片。<br>

* 映射list<br>
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
* 连接list与分割字符串 
把list中的元素以“分号”进行连接，输出成字符串。<br>
return ";".join(["%s=%s" % (k, v) for k, v in params.items()])<br>
join只能用于元素是字符串的list；它不进行任何的强制类型转换。连接一个存在一个或多个非字符串元素的list将引发一个异常。<br>
与join相反的是split，把字符串转成list。<br>
```python
>>> s.split(";") 
['server=mpilgrim', 'uid=sa', 'database=master', 'pwd=secret']
```

* 对象与参考
```python
print 'Simple Assignment'
shoplist = ['apple', 'mango', 'carrot', 'banana']
mylist = shoplist # mylist is just another name pointing to the same object!

del shoplist[0]

print 'shoplist is', shoplist
print 'mylist is', mylist
# notice that both shoplist and mylist both print the same list without
# the 'apple' confirming that they point to the same object

print 'Copy by making a full slice'
mylist = shoplist[:] # make a copy by doing a full slice
del mylist[0] # remove first item

print 'shoplist is', shoplist
print 'mylist is', mylist
# notice that now the two lists are different
```
输出：<br>
```
$ python reference.py
Simple Assignment
shoplist is ['mango', 'carrot', 'banana']
mylist is ['mango', 'carrot', 'banana']
Copy by making a full slice
shoplist is ['mango', 'carrot', 'banana']
mylist is ['carrot', 'banana']
```
你需要记住的只是如果你想要复制一个列表或者类似的序列或者其他复杂的对象（不是如整数那样的简单 对象 ），那么你必须使用切片操作符来取得拷贝。<br>

* 一些字符串的方法：
startwith方法是用来测试字符串是否以给定字符串开始。in操作符用来检验一个给定字符串是否为另一个字符串的一部分。find方法用来找出给定字符串在另一个字符串中的位置，或者返回-1以表示找不到子字符串。<br>

### 三、解决问题
* 面向对象编程<br>
域有两种类型——属于每个实例/类的对象或属于类本身。它们分别被称为实例变量和类变量。
类使用class关键字创建。类的域和方法被列在一个缩进块中。<br>
类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称，但是在调用这个方法的时候你不为这个参数赋值，Python会提供这个值。这个特别的变量指对象本身，按照惯例它的名称是self。<br>
```python
class Person:
    def sayHi(self):
        print 'Hello, how are you?'

p = Person()
p.sayHi()
```
Python中的self等价于Java、C#中的this。<br>

* __init__方法
```__init__```方法在类的一个对象被建立时，马上运行。这个方法可以用来对你的对象做一些你希望的 初始化 。注意，这个名称的开始和结尾都是双下划线。<br>
```python
class Person:
    def __init__(self, name):
        self.name = name
    def sayHi(self):
        print 'Hello, my name is', self.name

p = Person('Swaroop')
p.sayHi()

# This short example can also be written as Person('Swaroop').sayHi()
```
__init__方法类似于C++、C#和Java中的 constructor 。<br>

* 类和对象的变量
类的变量 由一个类的所有对象（实例）共享使用。只有一个类变量的拷贝，所以当某个对象对类的变量做了改动的时候，这个改动会反映到所有其他的实例上。<br>

对象的变量 由类的每个对象/实例拥有。因此每个对象有自己对这个域的一份拷贝，即它们不是共享的，在同一个类的不同实例中，虽然对象的变量有相同的名称，但是是互不相关的。<br>

* 如同```__init__```方法一样，还有一个特殊的方法```__del__```，它在对象消逝的时候被调用。对象消逝即对象不再被使用，它所占用的内存将返回给系统作它用。当对象不再被使用时，```__del__```方法运行，但是很难保证这个方法究竟在什么时候运行。

* 继承
```python
#!/usr/bin/python
# Filename: inherit.py

class SchoolMember:
    '''Represents any school member.'''
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print '(Initialized SchoolMember: %s)' % self.name

    def tell(self):
        '''Tell my details.'''
        print 'Name:"%s" Age:"%s"' % (self.name, self.age),

class Teacher(SchoolMember):
    '''Represents a teacher.'''
    def __init__(self, name, age, salary):
        SchoolMember.__init__(self, name, age)
        self.salary = salary
        print '(Initialized Teacher: %s)' % self.name

    def tell(self):
        SchoolMember.tell(self)
        print 'Salary: "%d"' % self.salary

class Student(SchoolMember):
    '''Represents a student.'''
    def __init__(self, name, age, marks):
        SchoolMember.__init__(self, name, age)
        self.marks = marks
        print '(Initialized Student: %s)' % self.name

    def tell(self):
        SchoolMember.tell(self)
        print 'Marks: "%d"' % self.marks

t = Teacher('Mrs. Shrividya', 40, 30000)
s = Student('Swaroop', 22, 75)

print # prints a blank line

members = [t, s]
for member in members:
    member.tell() # works for both Teachers and Students
```
输出：<br>
```
$ python inherit.py
(Initialized SchoolMember: Mrs. Shrividya)
(Initialized Teacher: Mrs. Shrividya)
(Initialized SchoolMember: Swaroop)
(Initialized Student: Swaroop)

Name:"Mrs. Shrividya" Age:"40" Salary: "30000"
Name:"Swaroop" Age:"22" Marks: "75"
```
注意：Python不会自动调用基本类的constructor，你得亲自专门调用它。<br>

* 输入输出
你可以通过创建一个file类的对象来打开一个文件，分别使用file类的read、readline或write方法来恰当地读写文件。对文件的读写能力依赖于你在打开文件时指定的模式。最后，当你完成对文件的操作的时候，你调用close方法来告诉Python我们完成了对文件的使用。具体例子：<br>
```python
#!/usr/bin/python
# Filename: using_file.py

poem = '''\
Programming is fun
When the work is done
if you wanna make your work also fun:
        use Python!
'''

f = file('poem.txt', 'w') # open for 'w'riting
f.write(poem) # write text to file
f.close() # close the file

f = file('poem.txt')
# if no mode is specified, 'r'ead mode is assumed by default
while True:
    line = f.readline()
    if len(line) == 0: # Zero length indicates EOF
        break
    print line,
    # Notice comma to avoid automatic newline added by Python
f.close() # close the file
```
模式可以为读模式（'r'）、写模式（'w'）或追加模式（'a'）。如果我们没有指定模式，读模式会作为默认的模式。

* 存储器
Python提供一个标准的模块，称为pickle。使用它你可以在一个文件中储存任何Python对象，之后你又可以把它完整无缺地取出来。这被称为持久地储存对象。
```python
#!/usr/bin/python
# Filename: pickling.py

import cPickle as p

shoplistfile = 'shoplist.data'

shoplist = ['apple', 'mango', 'carrot']

f = file(shoplistfile, 'w')
p.dump(shoplist, f) # dump the object to a file
f.close()

del shoplist 

# Read back from the storage
f = file(shoplistfile)
storedlist = p.load(f)
print storedlist
```
```
$ python pickling.py
['apple', 'mango', 'carrot']
```
import..as语法是一种便利方法，以便于我们可以使用更短的模块名称。<br>

为了在文件里储存一个对象，首先以写模式打开一个file对象，然后调用储存器模块的dump函数，把对象储存到打开的文件中。这个过程称为储存。
接下来，我们使用pickle模块的load函数的返回来取回对象。这个过程称为取储存 。<br>

* 异常的处理
```python
#!/usr/bin/python
# Filename: try_except.py

import sys

try:
    s = raw_input('Enter something --> ')
except EOFError:
    print '\nWhy did you do an EOF on me?'
    sys.exit() # exit the program
except:
    print '\nSome error/exception occurred.'
    # here, we are not exiting the program

print 'Done'
```
若是希望无论异常发生与否的情况下都都可以执行某些功能，这可以使用finally块来完成。

参考资料:
[简明Python教程](http://woodpecker.org.cn/abyteofpython_cn/chinese/index.html)<br>
[Dive into python](http://woodpecker.org.cn/diveintopython/)


## python基础语法 ##
### 输入输出 ###
#### 输出 ####
用print()在括号中加上字符串，输出指定的文字。如
<pre>>>> print('hello, world')`  </pre>
接受多个字符串，用逗号“,”隔开，连成一串输出：
<pre>>>> print('The quick brown fox', 'jumps over', 'the lazy dog')
The quick brown fox jumps over the lazy dog</pre>  
print()会依次打印每个字符串，遇到逗号“,”会输出一个空格，因此，输出的字符串是这样拼起来的：
#### 输入 ####
提供了一个input()，可以让用户输入字符串，并存放到一个变量里。如：  
<pre>>>> name = input()
Michael
>>> input('birth')
birth1993
'1993'</pre>
input()返回的数据类型是str，str使用int()函数来完成和整数的转换。

### 数据类型 ###
#### 整数 ####
Python可以处理任意大小的整数，十六进制用0x前缀和0-9，a-f表示。
#### 浮点数 ####
对于很大或很小的浮点数，就必须用科学计数法表示，把10用e替代，1.23x109就是1.23e9，或者12.3e8，0.000012可以写成1.2e-5，等等。浮点数运算则可能会有四舍五入的误差。
#### 字符串 ####
字符串是以单引号'或双引号"括起来的任意文本。如果'本身也是一个字符，那就可以用""括起来。如果字符串内部既包含'又包含"怎么办？可以用转义字符\来标识。  
<pre>'I\'m \"OK\"!'</pre>  
表示  
<pre>I'm "OK"!</pre>        
如果字符串里面有很多字符都需要转义，就需要加很多\，为了简化，Python还允许用r''表示''内部的字符串默认不转义。如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用'''...'''的格式表示多行内容。
#### 布尔值 ####
一个布尔值只有True、False两种值。布尔值可以用and、or和not运算。
#### 空值 ####
空值是Python里一个特殊的值，用None表示。
#### 变量 ####
在Python中，等号=是赋值语句，可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量。
#### 常量 ####
在Python中，通常用全部大写的变量名表示常量。在Python中，//是除法取整，%是除法取余。
### 字符编码 ###
在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：  
![](http://i.imgur.com/fK8n3r1.png)
#### python字符串 ####
对于单字符编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符。  
由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
Python对bytes类型的数据用带b前缀的单引号或双引号表示：  
<pre>x = b'ABC'</pre>  
要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。  
以Unicode表示的str通过encode()方法可以编码为指定的bytes，例如：  
<pre>>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)</pre> 
纯英文的str可以用ASCII编码为bytes，内容是一样的，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。  
要计算str包含多少个字符，可以用len()函数：
<pre>>>> len('ABC')
3
>>> len('中文')
2
</pre>
len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数：
<pre>>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
</pre>
当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：
<pre>
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
</pre>
#### 格式化 ####
Python中，用%实现格式化：
<pre>
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
</pre>
常见的占位符有：
<pre>%d	整数
%f	浮点数
%s	字符串
%x	十六进制整数</pre>
格式化整数和浮点数还可以指定是否补0和整数与小数的位数:
<pre>>>> '%2d-%02d' % (3, 1)
' 3-01'
>>> '%.2f' % 3.1415926
'3.14'
</pre>
字符串里面的%是一个普通字符就需要转义，用%%来表示一个%：
<pre>>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
</pre>
### list和tuple ###
#### list ####
list是一种有序的集合，可以随时添加和删除其中的元素。list里面的元素的数据类型也可以不同。list元素也可以是另一个list。如果一个list中一个元素也没有，就是一个空的list，它的长度为0。
<pre>>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']</pre>用len()函数可以获得list元素的个数。索引从0开始。取最后一个元素，还可以用-1做索引，直接获取最后一个元素：<pre>>>> classmates[-1]
'Tracy'
</pre>
list是一个可变的有序表，所以，可以往list中追加元素到末尾：
<pre>>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']</pre>
也可以把元素插入到指定的位置：
<pre>>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']</pre>
要删除list末尾的元素，用pop()方法：
<pre>>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']</pre>
要删除指定位置的元素，用pop(i)方法，其中i是索引位置：
<pre>>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']</pre>
要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：
<pre>
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
</pre>
#### tuple ####
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改。当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来。如果要定义一个空的tuple，可以写成()。有1个元素的tuple定义时必须加一个逗号,，来消除歧义。
<pre>
>>> t = (1, 2)
>>> t
(1, 2)
>>> t = ()
>>> t
()
>>> t = (1,)
>>> t
(1,)
</pre>
tuple中包含list，那么其中的list里的元素可以改变。
### 使用dict和set ###
#### dict ####
Python内置了字典dict，使用键-值（key-value）存储，具有极快的查找速度。
<pre>>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
</pre>
把数据放入dict的方法，除了初始化时指定外，还可以通过key放入。由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉。如果key不存在，dict就会报错：
<pre>>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88
</pre>
可以通过两种方式判断key是否存在。
<pre>>>> 'Thomas' in d
False
</pre>二是通过dict提供的get方法，如果key不存在，可以返回None，或者自己指定的value：
<pre>>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
</pre>
要删除一个key，用pop(key)方法，对应的value也会从dict中删除：
<pre>>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}</pre>
要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key:
<pre>>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'</pre>
#### set ####
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。要创建一个set，需要提供一个list作为输入集合：
<pre>>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}</pre>
重复元素在set中自动被过滤。
通过add(key)方法可以添加元素到set中。通过remove(key)方法可以删除元素。set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作。  
set的原理和dict一样，所以，同样不可以放入可变对象
### 条件判断 ###
if语句的完整形式是：
<pre>if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4></pre>
if语句执行有个特点，它是从上往下判断，如果在某个判断上是True，把该判断对应的语句执行后，就忽略掉剩下的elif和else。
### 循环 ###
Python的循环有两种，一种是for...in循环，依次把list或tuple中的每个元素迭代出来：
<pre>names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)</pre>
range()函数，可以生成一个整数序列，再通过list()函数可以转换为list。
<pre>>>> list(range(5))
[0, 1, 2, 3, 4]</pre>
第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。
<pre>sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)</pre>
### 函数 ###
几个函数：abs()绝对值。max(a,b)求最大值。类型转换int(),float(),str(),bool()。  
可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：
<pre>>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
</pre>
#### 定义函数 ####
在Python中，定义一个函数要使用def语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用return语句返回。
<pre>def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x</pre>
如果你已经把my_abs()的函数定义保存为abstest.py文件了，那么，可以在该文件的当前目录下启动Python解释器，用from abstest import my_abs来导入my_abs()函数，注意abstest是文件名（不含.py扩展名）。
#### 空函数 ####
如果想定义；一个什么都不做的空函数，可以用pass语句：
<pre>def nop():
    pass
</pre>
#### 参数检查 ####
如果参数个数不对，解释器会检查出来，抛出TypeError，但参数类型不对，无法检查，需要自定义。
#### 返回多个值 ####
<pre>import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny</pre>
<pre>>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
</pre>

其实，返回的是一个tuple，按位置赋予对应的值。
#### 参数设置 ####
默认参数。设置默认参数需要保证必选参数在前，默认参数在后。
<pre>def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s</pre>
默认参数必须指向不变对象！否则，在函数定义时，默认参数就计算出来了，如果指向一个可变对象，则有可能改变该对象。
<pre>def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L</pre>
#### 可变参数 ####
定义函数参数为可变参数，
<pre>def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum</pre>
<pre>>>> calc(1, 2)
5
>>> calc()
0
</pre>
在list或tuple前面加一个*号，把list或tuple的元素变成可变参数传进去:
<pre>>>> nums = [1, 2, 3]
>>> calc(*nums)
14
</pre>*nums表示把nums这个list的所有元素作为可变参数传进去。
#### 关键字参数 ####
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。
<pre>def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)</pre>
可以传入任意个数的关键字参数：
<pre>>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}</pre>
#### 命名关键字参数 ####
要限制关键字参数的名字，就可以用命名关键字参数，命名关键字参数必须传入参数名。例如，只接收city和job作为关键字参数。这种方式定义的函数如下：
<pre>def person(name, age, *, city, job):
    print(name, age, city, job)</pre>
如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了:
<pre>def person(name, age, *args, city, job):
    print(name, age, args, city, job)</pre>
#### 参数组合 ####
参数定义的顺序为必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
### 递归函数 ###

                                                                                                                
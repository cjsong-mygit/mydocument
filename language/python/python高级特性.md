## python高级特性 ##
### 切片 ##
取指定索引范围的操作，使用切片：
<pre>>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']</pre>
L[0:3]表示，从索引0开始取，直到索引3为止，但不包括索引3。
<pre>>>> L = list(range(100))</pre>
前10个数，每两个取一个：
<pre>>>> L[:10:2]
[0, 2, 4, 6, 8]</pre>
所有数，每5个取一个：
<pre>
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]</pre>
tuple也可以用切片操作，只是操作的结果仍是tuple。字符串也可以用切片操作，只是操作结果仍是字符串。
### 迭代 ###
默认情况下，dict迭代的是key。如果要迭代value，可以用for value in d.values()，如果要同时迭代key和value，可以用for k, v in d.items()。
<pre>>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print(key)
...
a
c
b</pre>
for循环里，同时引用了两个变量:
<pre>>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
</pre>
### 列表生成式 ###
<pre>>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']</pre>

### 生成器 ###
要创建一个generator，只要把一个列表生成式的[]改成()，就创建了一个generator：
<pre>>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81
</pre>
如果一个函数定义中包含yield关键字，那么这个函数就是一个generator：
<pre>def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
</pre>
generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。
<pre>>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
</pre>

### 迭代器 ###
可以直接作用于for循环的数据类型有以下几种：一类是集合数据类型，如list、tuple、dict、set、str等；一类是generator，包括生成器和带yield的generator function。这些可以直接作用于for循环的对象统称为可迭代对象：Iterable。可以使用isinstance()判断一个对象是否是Iterable对象：
<pre>>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
</pre>
可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator。可以使用isinstance()判断一个对象是否是Iterator对象：
<pre>>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
</pre>
生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator。把list、dict、str等Iterable变成Iterator可以使用iter()函数：
<pre>>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
</pre>
### 高阶函数 ###
#### map/reduce ####
map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。
<pre>>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]</pre>
reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是：
<pre>reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)</pre>
#### filter ####
filter()也接收一个函数和一个序列。filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。
<pre>
def is_odd(n):
    return n % 2 == 1
list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
</pre>
#### sorted ####
python内置的sorted()函数可以对list进行排序。它是一个高阶函数，可以接收一个key函数来实现自定义排序，比如按绝对值大小排序：
<pre>>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]</pre>
默认情况下，对字符串排序，是按照ASCII的大小比较的,要进行反向排序，不必改动key函数，可以传入第三个参数reverse=True:
<pre>>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']</pre>
### 返回函数 ### 
如果不需要立刻求和，而是在后面的代码中，根据需要再计算，这时可以不返回求和的结果，而是返回求和的函数：
<pre>def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum</pre>
调用函数f时，才真正计算求和的结果:
<pre>>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
>>> f()
25
</pre>
当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数，调用结果互不影响。  
返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。  
如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：
<pre>def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs</pre>
结果：
<pre>>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
</pre>
### 匿名函数 ###
<pre>>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]</pre>
关键字lambda表示匿名函数，冒号前面的x表示函数参数。匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果。
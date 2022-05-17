# python

Created: 2021-10-01 09:06:20 +0800

Modified: 2022-05-06 19:15:21 +0800

---


因为是动态变量 所以想要除出整数得用地板除 //

ord() 字符整数表示

chr() 整数字符表示

b'string' 每个字符占一个 字节

1个中文字符经过UTF-8编码后通常会占用3个字节

len() 默认字符数 转换为byte就是字节数



%d %f 等 格式化

print('%2d-%02d' % (3, 1))

f string {变量}

{变量名:格式化参数}

s = 3.14 * r ** 2

>>> print(f'The area of a circle with radius {r} is {s:.2f}')



list [] 可更改 有序

list = [a,b,c]

len(list) >>> 3

list.append( d )

list.insert( index,e )

list.pop(index)删除末尾/指定元素

list.sort() 排序

替换直接赋值

可以嵌套 类似于数组



tuple () 不可更改 相当于const array[]

只有一个元素时记得添个,

t = (1,)

tuple里面的list是可以更改的 类似指针思想



条件判断

if ... ><!= ... :



elif ....:



else:



类型转换

int(a) float(b)



循环 依次把list/tuple中的元素循环出来

for element in list:

print(element)



range(数量) 生成0开始的整数序列 0-100就要写101



while 与c同理

break

continue



dict {} 字典 键值存储

d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}

d['Mike'] = 90 直接赋值添加

d['Bob'] >>> 75



判断key存在

1.  通过in判断

'Bob' in d >>> True



2.  get()

d.get('a') >>> None

d.get('a',-1) >>> -1



同样也有pop()



dict的key不可变



set 集合 不能有重复的元素

创建一个set 需要提供一个list

全是key不存储value

add()

remove()

& | 集合可以做逻辑运算



字符串也是不可变的

replace()只是新建了一个字符串



简单函数

abs() max() int() float() str() bool()



a = abs 可以给函数起别名 有点像define typedef



定义函数def

pass 占位符



参数检查

isinstance(变量名,(数据类型))

raise TypeError('')



返回多个值 实际上是一个tuple

按位置赋给对应的值罢了



默认参数 fn(a,b=2)

没输入b时就是b=2

定义默认参数要牢记一点：**默认参数必须指向不变对象**！



可变参数 变量前面加一个*

声明和传入参数时都可以加上*



关键字参数 两个星号**

可以不传入也可以自动以dict形式传入

def person(name, age, **kw):

print('name:', name, 'age:', age, 'other:', kw)

>>> person('Adam', 45, gender='M', job='Engineer')

name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}





命名关键字参数

def person(name, age, *, city, job):

print(name, age, city, job)

和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了

**命名关键字参数必须传入参数名 因为python把后两个参数放到*这个可变参数里面去了 后面缺少参数**

可以设置缺省值



对于任意函数，都可以通过类似func(*args, **kw)的形式调用它，无论它的参数是如何定义的。





默认情况下，dict迭代的是key。如果要迭代value，可以用for value in d.values()，如果要同时迭代key和value，可以用for k, v in d.items()。

from collections.abc import Iterable



Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身：



>>> for i, value in enumerate(['A', 'B', 'C']):

... print(i, value)

...

0 A

1 B

2 C





列表生成式

[**x * x** for x in range(1, 11)] 外面的括号表示生成的是list

[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]



要生成的元素/表达式放for前面

for循环后面可以加判断 筛选



甚至可以两个循环一起来

>>> [m + n for m in 'ABC' for n in 'XYZ']

['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']



for循环其实可以有多个变量



可见，在一个列表生成式中，for**前面**的if ... else是表达式，而for**后面**的if是过滤条件，不能带else。



在前面判断 最后必须会有值留下来 比如 else None None仍然会在list中

后面判断 不满足的直接略过



列表生成式外面套个()就成为了生成器

next() 一个个生成返回值

生成器是可以迭代的

>>> g = (x * x for x in range(10))

>>> for n in g:

... print(n)

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



函数定义里面yield关键字类似return

但是每次遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。



调用该generator时，首先要生成一个generator对象，再用next()

func_gen = func()

next(func_gen)



用for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中

>>> g = fib(6)

>>> while True:

... try:

... x = next(g)

... print('g:', x)

... except StopIteration as e:

... print('Generator return value:', e.value)

... break

...

g: 1

g: 1

g: 2

g: 3

g: 5

g: 8

Generator return value: done



生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iter

把list、dict、str等Iterable变成Iterator可以使用iter()函数



那么函数名是什么呢？函数名其实就是指向函数的变量！对于abs()这个函数，完全可以把函数名abs看成变量，它指向一个可以计算绝对值的函数！





map(func,iterable)函数 将传入的函数应用到可迭代参数的每个元素 返回新的iterator

最后再用list()把iterator转换成list



python最好不要把变量起个内建函数的名字



reduce() 里面的函数必须接受两个参数 reduce把iterable每个元素依次做累积



匿名函数 lambda函数 ：

lambda 接收参数 : 表达式



和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。



sorted 排序函数

接收一个key函数来排序 如abs str.lower

反向reverse = True



函数对象有一个__name__属性，可以拿到函数的名字



在代码运行期间动态增加功能的方式，称之为"装饰器"（Decorator）



示范代码 基本按这样写

from functools import wraps

def decorator_name(f):

@wraps(f)

def decorated(*args, **kwargs):

if not can_run:

return print("Function will not run")cla

return f(*args, **kwargs)

return decorated

@decorator_name

def func():

return("Function is running")

can_run = True

print(func())

# Output: Function is running

can_run = False

print(func())

# Output: Function will not run



偏函数 functools.partial

functools.partial的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。



外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。







可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的__init__方法，在创建实例的时候，就把name，score等属性绑上去

class Student(object):



def __init__(self, name, score):

self.__name = name

self.__score = score



如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线__，在Python中，实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问



需要注意的是，在Python中，变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量



>>> bart = Student('Bart Simpson', 59)

>>> bart.get_name()

'Bart Simpson'

>>> bart.__name = 'New Name' # 设置__name变量！

>>> bart.__name

'New Name'

表面上看，外部代码"成功"地设置了__name变量，但实际上这个__name变量和class内部的__name变量不是一个变量！内部的__name变量已经被Python解释器自动改成了_Student__name，而外部代码给bart新增了一个__name变量。





当子类和父类都存在相同的run()方法时，我们说，子类的run()覆盖了父类的run()，在代码运行的时候，总是会调用子类的run()。这样，我们就获得了继承的另一个好处：多态。



要理解什么是多态，我们首先要对数据类型再作一点说明。当我们定义一个class的时候，我们实际上就定义了一种数据类型。我们定义的数据类型和Python自带的数据类型，比如str、list、dict没什么两样



新增一个Animal的子类，不必对run_twice()做任何修改，实际上，任何依赖Animal作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态。

由于Animal类型有run()方法，因此，传入的任意类型，只要是Animal类或者子类，就会自动调用实际类型的run()方法，这就是多态的意思



继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写。



动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的。



总是优先使用isinstance()判断类型，可以将指定类型及其子类"一网打尽"。



如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list，比如，获得一个str对象的所有属性和方法：



>>> dir('ABC')

['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']



仅仅把属性和方法列出来是不够的，配合getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态：

>>> class MyObject(object):

... def __init__(self):

... self.x = 9

... def power(self):

... return self.x * self.x

...

>>> obj = MyObject()

>>> hasattr(obj, 'x') # 有属性'x'吗？

True

>>> obj.x

9

>>> hasattr(obj, 'y') # 有属性'y'吗？

False

>>> setattr(obj, 'y', 19) # 设置一个属性'y'

>>> hasattr(obj, 'y') # 有属性'y'吗？

True

>>> getattr(obj, 'y') # 获取属性'y'

19

>>> obj.y # 获取属性'y'

19





一个正确的用法的例子如下：



def readImage(fp):

if hasattr(fp, 'read'):

return readData(fp)

return None

假设我们希望从文件流fp中读取图像，我们首先要判断该fp对象是否存在read方法，如果存在，则该对象是一个流，如果不存在，则无法读取。hasattr()就派上了用场。



请注意，在Python这类动态语言中，根据鸭子类型，有read()方法，不代表该fp对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要read()方法返回的是有效的图像数据，就不影响读取图像的功能。



如果Student类本身需要绑定一个属性呢？可以直接在class中定义属性，这种属性是类属性，归Student类所有



在编写程序的时候，千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。



如果我们想要限制实例的属性怎么办？比如，只允许对Student实例添加name和age属性。



为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性：



class Student(object):

__slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称



使用__slots__要注意，__slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的

除非在子类中也定义__slots__，这样，子类实例允许定义的属性就是自身的__slots__加上父类的__slots__。



setter getter property



多重继承

定义多个类，然后定义类时可以多几个参数，

class Dog(Mammal,Runnable):

...



通过多重继承，一个子类就可以同时获得多个父类的所有功能。



在设计类的继承关系时，通常，主线都是单一继承下来的，例如，Ostrich继承自Bird。但是，如果需要"混入"额外的功能，通过多重继承就可以实现，比如，让Ostrich除了继承自Bird外，再同时继承Runnable。这种设计通常称之为MixIn。



MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。





Python的class中还有许多这样有特殊用途的函数，可以帮助我们定制类。

__str__ 或 __repr__()

__iter__ 可迭代 __next__ 下一个值

class Fib(object):

def __init__(self):

self.a, self.b = 0, 1 # 初始化两个计数器a，b



def __iter__(self):

return self # 实例本身就是迭代对象，故返回自己



def __next__(self):

self.a, self.b = self.b, self.a + self.b # 计算下一个值

if self.a > 100000: # 退出循环的条件

raise StopIteration()

return self.a # 返回下一个值



__getitem__

Fib实例虽然能作用于for循环，看起来和list有点像，但是，把它当成list来使用还是不行

要表现得像list那样按照下标取出元素，需要实现__getitem__()方法



__getattr__

只有在没有找到属性的情况下，才调用__getattr__，已有的属性，比如name，不会在__getattr__中查找。

这实际上可以把一个类的所有属性和方法调用全部动态化处理了，不需要任何特殊手段。

这种完全动态调用的特性有什么实际作用呢？作用就是，可以针对完全动态的情况作调用。



__call__

一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用instance.method()来调用。能不能直接在实例本身上调用呢？在Python中，答案是肯定的。

任何类，只需要定义一个__call__()方法，就可以直接对实例进行调用。

通过callable()函数，我们就可以判断一个对象是否是"可调用"对象。



加上__call__和__getattr__后就可以在.后面调用这个类

Chain().users('michael') = Chain('users')('michael')

因为找不到users这个属性 直接认为是path属性

因为callable所以对实例直接调用，给的是path='michael'这个参数

返回的是Chain('usersmichael')



每次调用相当于重建一次实例 重新__init__一次



枚举类

from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

for name, member in Month.__members__.items():

print(name, '=>', member, ',', member.value)





如果需要更精确地控制枚举类型，可以从Enum派生出自定义类：



from enum import Enum, unique



@unique

class Weekday(Enum):

Sun = 0 # Sun的value被设定为0

Mon = 1

Tue = 2

Wed = 3

Thu = 4

Fri = 5

Sat = 6

@unique装饰器可以帮助我们检查保证没有重复值。



type()函数既可以返回一个对象的类型，又可以创建出新的类型，比如，我们可以通过type()函数创建出Hello类，而无需通过class Hello(object)...的定义



Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class

要创建一个class对象，type()函数依次传入3个参数：



1.  class的名称；

2.  继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；

3.  class的方法名称与函数绑定，这里我们把函数fn绑定到方法名hello上。

通过type()函数创建的类和直接写class是完全一样的，因为Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用type()函数创建出class。



正常情况下，我们都用class Xxx...来定义类，但是，type()函数也允许我们动态创建出类来，也就是说，动态语言本身支持运行期动态创建类，这和静态语言有非常大的不同，要在静态语言运行期创建类，必须构造源代码字符串再调用编译器，或者借助一些工具生成字节码实现，本质上都是动态编译，会非常复杂。







高级语言通常都内置了一套try...except...finally...的错误处理机制，Python也不例外

**try**:

print('try...')

r = 10 / int('a')

print('result:', r)

**except** ValueError **as** e:

print('ValueError:', e)

**except** ZeroDivisionError **as** e:

print('ZeroDivisionError:', e)

**else**:

print('no error')

**finally**:

print('finally...')

print('END')



Python的错误其实也是class，所有的错误类型都继承自BaseException，所以在使用except时需要注意的是，它不但捕获该类型的错误，还把其子类也"一网打尽"



使用try...except捕获错误还有一个巨大的好处，就是可以跨越多层调用，比如函数main()调用bar()，bar()调用foo()，结果foo()出错了，这时，只要main()捕获到了，就可以处理





python 错误层级

<https://docs.python.org/3/library/exceptions.html#exception-hierarchy>



记录错误

如果不捕获错误，自然可以让Python解释器来打印出错误堆栈，但程序也被结束了。既然我们能捕获错误，就可以把错误堆栈打印出来，然后分析错误原因，同时，让程序继续执行下去。



Python内置的logging模块可以非常容易地记录错误信息



import logging



def foo(s):

return 10 / int(s)



def bar(s):

return foo(s) * 2



def main():

try:

bar('0')

except Exception as e:

logging.exception(e)



main()

print('END')



如果要抛出错误，首先根据需要，可以定义一个错误的class，选择好继承关系，然后，用raise语句抛出一个错误的实例













logging的好处是它允许你指定记录信息的级别，有debug，info，warning，error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。



logging的另一个好处是通过简单的配置，一条语句可以同时输出到不同的地方，比如console和文件。





IO编程

文件读写

f = open(path,mode)

f.read(size)

f.close() 关闭文件 减少内存占用

f.write() 写入 #标识符要是"w"/"wb"才行



try:

f = open('/path/to/file', 'r')

print(f.read())

finally:

if f:

f.close()

两者作用相同

with open('/path/to/file', 'r') as f:

print(f.read())

with 自动调用close()



readline() 每次读入一行内容

readlines() 读取所有内容并返回list



for line in f.readlines():

print(line.strip()) # 把末尾的'n'删掉



file-like Object

像open()函数返回的这种有个read()方法的对象，在Python中统称为file-like Object。除了file外，还可以是内存的字节流，网络流，自定义流等等。file-like Object不要求从特定类继承，只要写个read()方法就行。



StringIO 就是在内存中创建的file-like Object，常用作临时缓冲。



二进制文件

前面讲的默认都是读取文本文件，并且是UTF-8编码的文本文件。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可：



>>> f = open('/Users/michael/test.jpg', 'rb')

>>> f.read()

b'xffxd8xffxe1x00x18Exifx00x00...' # 十六进制表示的字节



要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数，例如，读取GBK编码的文件：

>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')



保险起见 用with保证文件写入完毕且正常关闭

with open('/Users/michael/test.txt', 'w') as f:

f.write('Hello, world!')



StringIO

很多时候，数据读写不一定是文件，也可以在内存中读写。

StringIO顾名思义就是在内存中读写str。

要把str写入StringIO，我们需要先创建一个StringIO，然后，像文件一样写入即可



getvalue()方法用于获得写入后的str



要读取StringIO，可以用一个str初始化StringIO，然后，像读文件一样读取



BytesIO

StringIO操作的只能是str，如果要操作二进制数据，就需要使用BytesIO。



操作系统目录与文件 os模块

os.name()

os.environ() 全部环境变量

os.environ.get("key")



# 查看当前目录的绝对路径:

>>> os.path.abspath('.')

# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:

>>> os.path.join('/Users/michael', 'testdir')

# 然后创建一个目录:

>>> os.mkdir('/Users/michael/testdir')

# 删掉一个目录:

>>> os.rmdir('/Users/michael/testdir')

把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符。在Linux/Unix/Mac下，os.path.join()返回这样的字符串：

part-1/part-2

而Windows下会返回这样的字符串：

part-1part-2



同样的道理，要拆分路径时，也不要直接去拆字符串，而要通过os.path.split()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名



os.path.splitext()可以直接让你得到文件扩展名，很多时候非常方便





shutil 模块里有copyfile()函数



我们把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等



序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上。



反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。



pickle.dumps()方法把任意对象序列化成一个bytes，然后，就可以把这个bytes写入文件。或者用另一个方法pickle.dump()直接把对象序列化后写入一个file-like Object：

>>> f = open('dump.txt', 'wb')

>>> pickle.dump(d, f)

>>> f.close()



当我们要把对象从磁盘读到内存时，可以先把内容读到一个bytes，然后用pickle.loads()方法反序列化出对象，也可以直接用pickle.load()方法从一个file-like Object中直接反序列化出对象。



Python内置的json模块提供了非常完善的Python对象到JSON格式的转换。把Python对象变成一个JSON：

>>> import json

>>> d = dict(name='Bob', age=20, score=88)

>>> json.dumps(d)

'{"age": 20, "score": 88, "name": "Bob"}'



如何写入json到文件里

with open('/Users/frnks/test.json','w') as f:

f.write(json.dumps(d))



json只会处理dict类型的对象

json.dumps(s,default=convertfunc)



print(json.dumps(s, default=lambda obj: obj.__dict__))

偷懒写法

因为通常class的实例都有一个__dict__属性，它就是一个dict，用来存储实例变量。也有少数例外，比如定义了__slots__的class。



同样的道理，如果我们要把JSON反序列化为一个Student对象实例，loads()方法首先转换出一个dict对象，然后，我们传入的object_hook函数负责把dict转换为Student实例：

def dict2student(d):

return Student(d['name'], d['age'], d['score'])



>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'

>>> print(json.loads(json_str, object_hook=dict2student))







简易正则表达式

直接给出字符 精确匹配

d 一个数字 w 一个字母或数字 s 一个空格

. 可以匹配一个任意字符

匹配更长的字符 可用通配符*（任意个字符）

+表示至少一个字符

?表示0个或1个字符

{n}表示n个字符

{n,m}表示n-m个字符

特殊字符需要用转义

d{3}-d{3,8} 匹配如010-12345678



[0-9] 匹配一个数字

[0-9a-zA-z_] 可以匹配一个数字、字母或者下划线

[0-9a-zA-z_]+ 匹配至少由一个数字、字母或者下划线组成的字符串

[a-zA-Z_][0-9a-zA-Z_]*可以匹配由字母或下划线开头，后接任意个由一个数字、字母或者下划线组成的字符串，也就是Python合法的变量

[a-zA-Z_][0-9a-zA-Z_]{0, 19}更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）



(A|B)可以匹配A或B，所以(P|p)ython可以匹配'Python'或者'python'



^表示行的开头，^d表示必须以数字开头

$表示行的结束，d$表示必须以数字结束

^xxx$ 完全整行匹配



使用Python的r前缀，就不用考虑转义的问题



re模块

re.match(表达式字符串,字符串)

如果匹配成功，返回一个Match对象，否则返回None



切分字符串

split() 无法去除空格

re.split()

[s,;]+

把不规范的输入转化成正确的数组



分组

除了简单地判断是否匹配之外，正则表达式还有提取子串的强大功能。用()表示的就是要提取的分组（Group）

^(d{3})-(d{3,8})$分别定义了两个组，可以直接从匹配的字符串中提取出区号和本地号码

注意到group(0)永远是原始字符串，group(1)、group(2)......表示第1、2、......个子串



正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。举例如下，匹配出数字后面的0：

>>> re.match(r'^(d+)(0*)$', '102300').groups()

('102300', '')

用?采用非贪婪匹配

>>> re.match(r'^(d+?)(0*)$', '102300').groups()

('1023', '00')



如果一个正则表达式要重复使用几千次，出于效率的考虑，我们可以预编译该正则表达式，接下来重复使用时就不需要编译这个步骤了，直接匹配：



>>> import re

# 编译:

>>> re_telephone = re.compile(r'^(d{3})-(d{3,8})$')

# 使用：

>>> re_telephone.match('010-12345').groups()

('010', '12345')

>>> re_telephone.match('010-8086').groups()

('010', '8086')









datetime

now()

timestamp()

fromtimestamp() 本地时间

utcfromtimestamp() UTC时间

strptime('时间字符串','格式')

example = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')

<https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior>

（转换后没有时区信息）



datetime转换字符串

strftime('格式')



timedelta 计算时间差

+ timedelta(hours=10) 过了10小时

datetime属性tzinfo存储时区

timezone(timedelta(hours=x)) 创建时区UTC+x:00

replace(tzinfo=timezone(timedelta(hours=x)))



utcnow()

astimezone(timezone(timedelta(hours=8))) 将UTC转换为+8:00







collections

1.  namedtuple 创建一个tuple的带名子类

namedtuple('name',['a','b'])

n = name(1,2)

n.a == 1

n.b == 2

2.  deque 高效双向列表 适用于队列和栈

appendleft() popleft()

3.  defaultdict(lambda:-----) key不存在时返回特定函数

4.  OrderedDict 有序字典 按照插入顺序排列 可以实现FIFO的字典

5.  chainmap 多个dict串起来 按照顺序依次查找（优先级）

6.  Counter dict的子类



base64



先准备含64个字符的数组

对二进制数据取3个字节 共3x8=24个bit 再分为4组 每组6个bit

4个数字为索引 查表 获得相应的4个字符 就是编码后的字符串





struct

pack()

unpack()





hashlib

md5()

update()

hexdigest()



hmac

new()



urllib

urlopen()



ssl

SSLContext()



request

Request()



Pillow





requests

get('url',params={'':'',},headers='',timeout=)

Response类型

status_code

text

encoding

content

json

headers['']

cookies['']



post('url',data={},json=params,files=___)

注意文件要是rb模式 二进制读取





psutil

cpu_

count

times

...





数据库sqlite3

connect()

cursor()

execute()

使用Python的DB-API时，只要搞清楚Connection和Cursor对象，打开后一定记得关闭，就可以放心地使用。

使用Cursor对象执行insert，update，delete语句时，执行结果由rowcount返回影响的行数，就可以拿到执行结果。

使用Cursor对象执行select语句时，通过fetchall()可以拿到结果集。结果集是一个list，每个元素都是一个tuple，对应一行记录。

如果SQL语句带有参数，那么需要把参数按照位置传递给execute()方法，有几个?占位符就必须对应几个参数，例如：

cursor.execute('**select*** **from user where** name=? **and** pwd=?', ('abc', 'password'))



出错的话可以用try except finally



十

z

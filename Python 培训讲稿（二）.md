## Python 培训讲稿（二）

 - 控制流
 - 函数和模块
 - 命名空间和传值问题

---

上次我们对 Python 内置的数据元素做了一些简单地介绍，这次我们来讲一讲控制流和函数。

### 控制流

Python 的控制流有三种，if,while,for

除外，还有 break,continue，和 C 作用的一致

还有一个特别的 pass。

if ,while,for 控制流和上一章讲述的布尔运算和布尔值关系密切，实际上关于布尔运算，计算机院有专门的离散课程会有更详细的说明。这里并不具体展开。

在控制流中，所有运算结果皆为 True 或者运算对象存在，皆会进入控制流的运算中。

Python 的 if 写法如下：

    if statement:
        # do anything you want
    elif another statement:
        # do anything you like
    else:
        ...  

Python 没有大括号，Python 皆以**缩进**来供编译器识别相应的命名空间，
举个范例：

    >>> age = raw_input("input your age:")
    >>> if age <= 18:
    ...     print "you can't go to XiDianZhiJia"
    ... elif age <= 22:
    ...     print "naive"
    ... else:
    ...     print "are you still single?"

等同以下 C 代码：
    
    int main() {
        int age;
        scanf("%d",&age);
        if (age <= 18) {
            printf("you can't go to XiDianZhiJia\n");
        }
        else if (age <= 22) {
            printf("naive\n");
        else {
            printf("are you still single?")
        }
        return 0;
    }

这里我们的 statement 都是有明确结果的布尔运算，在 Python 中，数字 0 、空列表、空字典（其实是 Python 内置数据元素的初始化对象），皆视作 False；否则其余的对象（包括类，包括实例）都视作True。（其实 Python 类有相应内置函数可以设置布尔判定，如果你未接触过类可以默认这句话是对的）。

注意，所有控制流都不可以跨域，如下示例将会报错。

    if statementA: # 第一个 if
        if statementB: # 第二个 if
            # pass
    else: # 第一个 if
        elif statmentC # 第二个 if
    ...

这很容易理解，本身逻辑上也不成立。

Python 上的 while 和 C 语言一致，只不过没有括号。

    while statement:
        # do anything you like

示例：

    >>> a = 5
    >>> while a >= 0:
    ...     print a
    ...     a -= 1
    ... 
    # 最后打印 5 4 3 2 1，a 最后的值是 -1.

while 很容易理解。

而 Python 的 for ，不同于 C 的 for，类似于 C++ 的 for（也至少是 C++11）。Python 的 for ，准确的说不叫“循环”，而叫做“遍历”，而且 Python 的 for 应该叫做 for ... in .. 。（为了方便我们还是叫 for 吧）

上文中，我们谈到了遍历列表，其中使用了 for 语句来遍历。

实际上，列表、tuple、集合都可以使用 for 来遍历，（和布尔运算相同，类有相应的函数可以设置遍历，感兴趣的同学可以先搜索迭代器，或者[点这里](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143178254193589df9c612d2449618ea460e7a672a366000)）。

说这么多我们来讲个示例：

    >>> a = [i**2 for i in range(0,6)]
    >>> \# a = [0,1,4,9,16,25]
    >>> for i in a:
    ...     print i
    ... 

稍微扩展一下，再比如说我们有一个列表，里面的元素有列表也有数字

    >>> a = [1, [2,3],[3,4,5]]

我们如何单独打印出来数字呢？

上一次我们介绍了 type() 函数用来查看对象的类型，可能你会想到使用这个函数来区分开是列表还是数字。

我们来仔细使用一下 type() 函数。

    >>> a = 1
    >>> type(a)
     <type 'int'>
    >>> type(a) == <type 'int'>
    > # 报错，格式错误

其实，我们在命令行下看到的反馈，都是通过 repr() 函数（严格来说是对象的内置的\_\_repr\_\_函数）处理后看到的结果，比如：

    >>> a = type(1)
    >>> repr(a)
    "<type 'int'>"

可能你会还听过 str() 函数，有时候这两个函数输出的结果是一致的。但严格意义上来说，str() 的目的是将对象转化为字符串，而 repr() 的目的是将对象显示出来。（str = string, repr = represent）在命令行交互的模式下，你看到的每一步反馈都是经过了 repr() 函数处理过的结果，而实际运行中（直接编译运行代码文件），不仅不会有任何的反馈，而且都不会经过任何的处理。
所以如果你要坚持使用 type() 函数来区分列表和数字的话，应该这样处理。

    >>> queue = list() 
    >>> queue.append(a)
    >>> while len(queue) != 0:
    ...     i = queue[0]
    ...     if repr(type(i)) == '< type 'int' >':
    ...         print i # 是数字直接打印
    ...     else:
    ...         for j in i: 
    ...             queue.append(j) # 将列表中的元素放入 queue 中
    ...     queue.remove(i) # 删除 i
    ... 

我们采用了一个 queue 列表和 while 来迭代整个 l 列表。
但在实际使用中，几乎很少有人使用 type() 函数来判断对象的类型，Python 有一个专门的函数来判定对象类型 ， isinstance（instance 是实例的意思），用法如下：

    >>> a = 1
    >>> isinstance(a,int)
    True
    >>> isinstance(a,float)
    False

isinstance 接受两个参数，isinstance(object, class-or-type-or-tuple)，返回 bool。object 就是待判定类型的对象，第二个参数，既可以是 class（类，比方 int），也可以是一个 type（即 type() 函数的返回值），还可以是一个 tuple（用于多次判断），示例如下：

    >>> a = 1
    >>> b = 2
    >>> c = 3.0
    >>> s = 'hello xdmsc'
    >>> isinstance(a,int)
    True
    >>> isinstance(a,type(b))
    True
    >>> isinstance(a,(type(s),type(c),int))
    # 等价于
    # isinstance(a,type(s)) or isinstance(a,type(c)) or isinstance(a,int)
    True

isinstance 的介绍就到这里。在命令行交互中，isinstance() 虽然比 type() 复杂，但是其返回的信息比 type 更有价值（尤其使用了多态以后，这里不作过多展开）。

关于 break 和 continue 示例如下：

    >>> for i in range(1,10):
    ...     if i == 5:
    ...         continue # 直接结束本次循环，进入下一次循环
    ...     elif i == 7:
    ...         break # 结束所有循环，进入下一个代码块
    ...     print i
    ...
    1
    2
    3
    4
    6

关于 pass，其起到一个占位符的作用，主要就是跳过本段代码块（就是缩进的那一块），比如：

    >>> a = 2
    >>> if a != 2:
    ...     pass 
    ... else:
    ...     print i
    ...
    2

你可以尝试不添加 pass ，无论解释器还是在 ide 环境，都会报错。

Python 控制流就讲到这里。

### 函数

我们已经使用了不少的 Python 的内置函数和成员函数，现在我们来介绍如何定义一个函数

    >>> def mySum(l): # def 是定义函数的关键字
    ...     if len(l) == 0:
    ...         return 0
    ...     elif len(l) == 1:
    ...         return l[0]
    ...     return mySum(l[0]) + mySum(l[1:])
    ...     

这里我们定义一个 mySum 函数（先不用在意最后一句），你会发现，Python 定义函数不需要申明类型，这也是动态语言的优点（[延伸阅读](https://www.zhihu.com/question/19918532)）。

在 Python 中，return 是不强制要求的，无 return 的函数即算作返回值为 None。在函数执行过程中遇见 return 关键字，之后的过程将会直接跳过。也就是：

    >>> def test():
    ...     return 1
    ...     print "hello xdmsc"
    ...     

最后一句是永远不会执行的，我们可以针对性的利用这一点来美观我们的函数写法。

回到 mySum 函数中，这里我定义了一个递归函数，即他调用他自己。

比如在上面的例题，关于依次打印 [1,[2,3],[4,5,6]] 列表中的数字，使用递归函数的写法如下：

    >>> def myPrint(l):
    ...     if len(l) == 0:
    ...         return        # 返回 None，结束函数
    ...     i = l[0]
    ...     if isinstance(i,list):
    ...         myPrint(i)
    ...     else:
    ...         print i
    ...         myPrint(l[1:])

函数执行过程如下：

    myPrint([1,[2,3],[4,5,6]]) # 1 是数字，直接打印
    myPrint([[2,3],[4,5,6]]) # [2,3] 是列表，单独打印
    myPrint([[3],[4,5,6]])
    myPrint([[],[4,5,6]]) # 空列表
    myPrint([[4,5,6]]) # [4,5,6] 是列表
    myPrint([[5,6]])
    myPrint([[6]])
    myPrint([[]]) # 空列表
    myPrint([]) # 空列表

递归函数相较于之前的迭代函数，优点在于清晰易懂，缺点在于容易递归栈溢出。因为每一次调用递归函数，在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧，[延伸阅读](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/00137473836826348026db722d9435483fa38c137b7e685000)。如果对栈不是很了解的同学，你可以先忽略这一块知识，只要记住在使用递归函数时不要过分的让递归层数太大就行。（实际上，示例中的写法，增加两个词就实现了尾递归的过程）

相较于 C，Python 的函数还有一个优点就是允许多重返回值。示例：

    >>> def between(n):
    ...     return n-1,n+1
    ... 
    >>> x,y = between(4)
    >>> print x,y
    3 5

实际上，Python 实现多重返回值是通过返回一个 tuple 来实现的

    >>> between(4)
    (3,5)

像在上一章我们介绍字典时，讲到了字典的 items() 函数。

    >>> d = {i:i**2 for i in range(0,4)}
    >>> d.items()
    >>> {(0,0),(1,1),(2,4),(3,9)}
    >>> for i,j in d.items():
    ...     print i,j
    ... 
    0 0
    1 1
    2 4
    3 9

谈到多重返回值，我们再来说一下类似的一个写法。
在 Python 中，交换两个数的值，可以使用以下语法：

    >>> a = 1
    >>> b = 2
    >>> a,b = b,a

不同于 C 需要使用一个中间变量。实际上，在 Python 中，这种语法甚至快过使用中间变量的写法。

另外关于函数参数，有四点可以介绍。

第一就是默认参数。在定义函数时，你可以指定参数的默认值，例如：

    >>> def subn(a,n=5):
    ...     return a-n
    ... 
    >>> subn(10) # 直接调用时，默认n=5
    5
    >>> subn(10,2) # 指定 n 的值
    8

在一些默认参数比较多的函数中，例如：

    >>> def test(a,b=2,c=2):
    ...     return a-b+c
    ... 
    >>> test(5)
    5
    >>> test(5,1,2) # 同时改变了 b,c 值
    6
    >>> test(5,c=3) # 只改变了 c 的值
    6
    >>> test(a=5,b=0,c=0) # 指明参量名称
    5

在搭建工程时，指明函数参数名称是个好习惯，便于后来重新编辑时代码的易读性。

第二点就是可变参数，示例如下：

    >>> def mySum(l,n=0): # 这里采用了尾递归的形式
    ...     if len(l) == 0:
    ...         return n
    ...     elif len(l) == 1:
    ...         return n+l[0]
    ...     return mySum(l[1:],n+l[0])
    >>> mySum([1,2,3])
    6

在调用这个函数是，我们需要将需要计算的值包装成一个列表，或者一个 tuple，函数才能计算。

实际上，还有一个不同的写法，就是使用可变参数。

    >>> def mySum(*args): # 这里采用了迭代的形式
    ...     sum = 0
    ...     for i in args:
    ...         sum += i
    ...     return sum
    ... 
    >>> mySum(1,2,3)
    6

注意，这里我们就不用将参数包装成列表的形式，直接传入即可，其中的 *args ，会自动将我们传入的参数包装一个 tuple。

    >>> def test(*args):
    ...     print args
    ... 
    >>> test(1,2,3)
    (1,2,3)

这里例子很清晰的展示了这个过程。

另外，这里 *args 的名称可以随便更改，只要更改成 *+(名称) 即可，比方:

    >>> def test(*numbers):
    ...     print numbers
    ... 

都是一样的效果。

除了可以将参数包装成 tuple，还有种写法可以将参数连同传入的参数名称包装成字典。

    >>> def test(**kwargs):
    ...     print type(kwargs)
    ...     print kwargs.items()
    ... 
    >>> test(a=1,b=2,c=3)
    <type 'dict'>
    [(a,1),(b,2),(c,3)]

在工程中，一般都是 *args 和 **kwargs 一起使用。更多关于可变参数的资料，可以阅读[延伸阅读](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001374738449338c8a122a7f2e047899fc162f4a7205ea3000)。

第三点和第四点，分别是匿名函数和高阶函数，这两点是 Python 从 [函数式编程](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B8%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80) 学习延伸实现的，剩下部分不一定要求学会什么是函数式编程，同学们只需要体会另一种编程思维的魅力。

匿名函数也称作 lambda 函数。可以快速地不使用 def 申明来实现一个函数。

    >>> f = lambda x:x+1
    >>> f
    <function <lambda> at 0x105ca76e0>
    >>> f(5)
    6

在这里，f 函数没有名称，匿名函数嘛。

而高阶函数，可能大家听说过高阶导数（导数也是一种函数），示例如下：

    >>> def f1(*args):
    ...     return sum(args)
    ... 
    >>> def f2(*args):
    ...     return sum(args)+2
    ... 
    >>> def test(m,a,b,c): # m 是函数，a,b,c是参数
    ...     return m(a,b,c) # 返回 m 函数的返回值
    ... v
    >>> test(f1,1,2,3)
    6
    >>> test(f2,1,2,3)
    8

高阶函数在以后的类的设计模式中会使用到，感兴趣的同学可以去搜索一下装饰模式和装饰器。不过 Python 已经为我们提供了三种高阶函数，map,filter,reduce，这三个函数更多的作为一种工具来帮助我们。

    map(function,sequence) -> list
    filter(function,sequence) -> list,tuple,string
    reduce(function,sequence[,initial]) -> value

sequence 意为序列，即可遍历的数据结构（列表，tuple，string）。

map 函数，会对 sequence 中每一个元素都代入 function 中，然后使用返回值代替其位置。
filter 函数，它的 function 一般为是布尔返回值的函数，它会将 sequence 中的每一个元素代入 function 中，结果为真的将继续保留，结果为假的将会删除。
reduce 函数，会对重复调用，类似递归的过程。

话不多说，示例如下：

    >>> l = range(0,6)
    # l = [0,1,2,3,4,5]
    >>> map(lambda x:x+i,l)
    [1,2,3,4,5,6]
    >>> filter(lambda x:x % 2 == 0,l) # 筛选出偶数
    [0,2,4]
    >>> reduce(lambda x,y: x*10+y,l)
    12345

其中，filter 函数的返回值取决于 sequence 的类型。
reduce 函数的实现过程，拿示例中举例，过程如下：

    (((((0*10+1)*10+2)*10+3)*10+4)*10+5)

你可以数一数左边的括号，一共有五个，说明调用了五次。是不是类似递归的调用过程呢？

实际上，我们自己也可以实现 map,filter,reduce 函数，你可以自己下去实现这一过程。你可以到最后一部分查看我的实现结果。

### 模块

在工程中，一般会把分模块来实现工程，这样容易实现代码的松耦合。

举个例子，我这里有两个文件，add.py 和 main.py

    [add.py]
    # -*- coding: utf-8 -*-
    
    def add(a,b):
        return a+b

说明下以下的部分，开头的 \# -*- coding: utf-8 -*- 说明采用 utf-8 编码（关于 Py2 的编码问题我们之后会另做介绍，在这之前你默认这段需要加上，而且暂时不要在代码文件中加入中文字符，注释除外）

    [main.py]
    # -*- coding: utf-8 -*-
    
    import add # 这里我们导入了 add 文件，也就是模块
    
    print add.add(1,2) # 执行 add 模块下的 add 函数

进入命令行，执行"python main.py"，你会得到 3 的输出结果。
其中的 import 语句，就是导入 add.py ，编译器会先检查 add.py 的语法，然后你就可以在 main.py 文件下调用 add.py 里的变量和函数了。

实际上，import 的过程远比我讲的复杂得许多，而且 import 的一些技巧也比上面列得多得多（有些我也没掌握...），如果对此感兴趣的同学可以下去翻一翻[官方文档](https://docs.python.org/2/)，甚至可以去翻一翻[ Python 学习手册](http://al.lib.xidian.edu.cn/F/EEN95L5MMRGQP9I7CIQG7TY5PK6N5GQ623VKPBXRFA7JTLASHN-39624?func=find-b&find_code=WRD&request=python%E5%AD%A6%E4%B9%A0%E6%89%8B%E5%86%8C&local_base=XDU01&filter_code_1=WLN&filter_request_1=&filter_code_2=WYR&filter_request_2=&filter_code_3=WYR&filter_request_3=&filter_code_4=WFM&filter_request_4=&filter_code_5=WSL&filter_request_5=)。

关于模块，有绝对引入和相对引入两种方法。上文介绍的就是绝对引入。我把两种引入简略地介绍一下。

> \>\>\> import math # 绝对引入
> \>\>\> math.sqrt(5)

> \>\>\> from math import * # 相对引入,* 的意思是引入全部函数和数据
> \>\>\> sqrt(5)

> \>\>\> from math import sqrt # 只引入 sqrt 函数
> \>\>\> sqrt(5)

相对引入实际上是先通过进行了一次绝对引入的过程，然后再将目标函数或者变量导入命名空间中。所以相对引入本质上是和绝对引入的速度是差不多的，并不会快多少。在实际运用中，我不推荐使用过多的相对引用，尤其相对引用了两个不同的模块，因为这样会导致命名空间的混乱。示例：

我有两个模块，module1.py 和 module2.py：

    [module1.py]
    # -*- coding: utf-8 -*-
    __all__ = ['add'] # 对外申明只能引入 add 函数
    
    n = 1
    
    def add(a,b):
        return a + b + 1
> 
    
    [module2.py]
    # -*- coding: utf-8 -*-
    
    n = 1
    
    def add(a,b):
        return a + b - 1

在命令行交互中：

    >>> from module1 import *
    >>> add(1,2)
    4
    >>> n
    # 这里会报错，因为 module1 只允许引入 add 函数
    >>> from module2 import *
    >>> add(1,2)
    2
    >>> n
    1

这个示例里很清楚地展示了命名空间的混乱问题，同时还展示了如何限制引入的函数和变量。
其实还有一种写法可以尽可能的避免这种命名空间的混乱问题

    >>> from module1 import add as add1
    >>> from module2 import add as add2
    >>> add(1,2) # 报错，命名空间没有这个函数
    >>> add1(1,2) 
    4
    >>> add2(1,2)
    2

这是我们第一次接触 as 关键字。如上所述，import a as b 其实也同理。

模块就讲这么多，可参考[延伸阅读](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868200171577d6385bb5b4f4875bee9cbf0f0fa29c5000)。

### 命名空间和求值策略

这部分问题属于大众比较喜闻乐见的部分，尤其是传值问题，好多开发者都喜欢对此津津乐道。

实际上，这一块问题比起高阶函数来说更难解释开来，我会尽我所能简单地说明这一块问题。（维基上有简单易懂的解释...）

Python使用叫做命名空间的东西来记录变量的轨迹。命名空间是一个 字典（dictionary） ，它的键就是变量名，它的值就是那些变量的值。
（A namespace is a mapping from names to objects. Most namespaces are currently implemented as Python dictionaries.）
 
在一个 Python 程序中的任何一个地方，都存在几个可用的命名空间。

1. 每个函数都有着自已的命名空间，叫做局部命名空间，它记录了函数的变量，包括函数的参数和局部定义的变量。
2. 每个模块拥有它自已的命名空间，叫做全局命名空间，它记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。
3. 还有就是内置命名空间，任何模块均可访问它，它存放着内置的函数和异常。
 
假设有一行代码要使用变量 x 的值时，Python 会到所有可用的名字空间去查找变量，按照如下顺序：

1. 局部命名空间：特指当前函数或类的方法。如果函数定义了一个局部变量 x，或一个参数 x，Python 将使用它，然后停止搜索。
2. 全局命名空间：特指当前的模块。如果模块定义了一个名为 x 的变量，函数或类，Python 将使用它然后停止搜索。
3. 内置命名空间：对每个模块都是全局的。作为最后的尝试，Python 将假设 x 是内置函数或变量。
4. 如果 Python在这些名字空间找不到 x，它将放弃查找并引发一个 NameError 异常，如，NameError: name 'x' is not defined。

这里我们举一个简单地例子。

    >>> i = -1
    >>> def test():
    ...     i = 10
    ...
    >>> test()
    >>> i
    -1

可以发现，i 值并没有改变，可以发现函数的命名空间和主命名空间是分隔的。这就是全局变量和局部变量的区别。

要查看当前的全局命名空间和局部命名空间，最简单的方法就是使用 globals 和 locals 函数。

    >>> def test():
    ...     i = 10
    ...     x = 1
    ...     print locals()
    ...
    >>> test()
    {'i':10,'x':1}
    >>> x = 10
    >>> print globals()
    {'__builtins__': <module '__builtin__' (built-in)>, '__package__': None, 'x': 10, 'test': <function test at 0x1057f5c80>, '__name__': '__main__', '__doc__': None} # 其中 x 和 test 都在全局命名空间中。

其实，在函数中，也可以引入全局变量。

    >>> i = -1
    >>> def test():
    ...     global i
    ...     i = 10
    ...
    >>> test()
    >>> i
    10

如果不使用 global 关键字申明的话，test 命名空间就会建立一个 i 的局部命名空间。

global 一般在多线程编程上会用到，也可以作为函数的返回值来处理（至少那些只有一个返回值的编程语言是这么干的）。

关于命名空间的更多知识，可以简单阅读[这篇博客](http://www.cnblogs.com/windlaughing/archive/2013/05/26/3100362.html)。

接下来主要介绍津津乐道的传值问题，严格来说应该叫做[求值策略](https://zh.wikipedia.org/wiki/%E6%B1%82%E5%80%BC%E7%AD%96%E7%95%A5)

在这里我们先明确两个定义，传引用和传值。

传值的意思就是百分百复制拷贝对象的值到新的对象，而传引用则只是将该对象新建了一个新的命名。在 Python 中：

    >>> i = 0
    >>> a = i
    >>> id(i) == id(a) 
    # id 函数表示返回该对象所在内存位置
    # 可以视作不可显式调用的指针
    True

返回 True，说明 i 和 a 所指对象是一致的，这就是传引用。

    >>> i = 1
    >>> a = 1
    >>> id(i) == id(a)
    False

这就是传值（这里不要将 1 视作简单的数字 1，而是视作一个整数对象，其值是 1 ）。

实际上，Python 有专门的运算符给我们判断两个对象是否一致。

    >>> i = a = 1
    # 等价于
    # a = 1
    # i = a
    >>> i is a
    True
    >>> i = 2
    >>> i is a
    False

在 Python 中，其求值策略并不单一地遵循传引用或者传值，有时候会根据操作变化，比如：

    >>> i = a = 1
    >>> i is a
    True
    >>> i += 1 # i = i + 1
    >>>i is a
    False

有时候使用较小的字符串时，Python 会选择传引用；而对于较长字符串，Python 会选择传值。（这个冷知识对编程影响不大，权当好玩）

    >>> s1 = 'Hi'
    >>> s2 = 'Hi'
    >>> s1 is s2
    True
    >>> s1 = "this is just for testing"
    >>> s2 = "this is just for testing"
    >>> s1 is s2
    False

而传值传引用最关键的，就是列表等内置数据结构的拷贝。

    >>> a = [1,2,3,4]
    >>> b = a
    >>> b is a
    True
    >>> c = a[:]
    >>> c is a
    False

这点比较重要，有时候想要复制列表时，可以通过这个简单的方式。但这个方式仅适合于列表里面的数据元素是数字、字符的时候。

那如果列表里面嵌套了列表呢？

    >>> a = [[1,2,3],[4,5,6]]
    >>> b = a[:]
    >>> b is a
    False
    >>> a[0].append(10)
    >>> b
    >>> [[1,2,3,10],[4,5,6]]

其实，b = a[:] 的过程，等价于以下的代码：

    b = list()
    for i in a:
        b.append(i)
        
所以我们可以看到，对于像非数字字符串的复杂对象，切片操作只是传引用传到了新的列表之中，并不能有效地拷贝新的值。这个过程，我们称之为**浅拷贝**。

关于列表的传引用还有一个明显的示例：

    >>> v = [0,1,2]
    >>> v[1] = v
    >>> v
    [0,[...],2]
    >>> v[1]
    [0,[...],2]
    >>> v[1][1]
    [0,[...],2]

v[1] 像是被反复递归了一样，只不过没有边界。

我们可以发现，在简单对象上，Python 遵循传值策略，而复杂对象默认遵循传引用策略。其实，Python 有一个内置库，专门处理复杂对象的拷贝操作，即让复杂对象遵循传值策略。

    >>> import copy
    >>> i = [1,2,[3,4]]
    >>> a = copy.deepcopy(i)
    >>> a
    [1,2,[3,4]]
    >>> i[2].append(5)
    >>> i
    [1,2,[3,4,5]]
    >>> a
    [1,2,[3,4]]

这样，i 和 a 就彻底的分离了。

实际上，不可变对象和可变对象还有一个区别就是不可变对象修改其值时必定要重新安排内存地址，而可变对象则不一定重新安排内存地址，只需要重新调整内存所占的大小即可。

    >>> n = 1
    >>> id1 = id(n)
    >>> n += 2
    >>> id2 = id(n)
    >>> id1 == id2
    False
    >>> l = [1,2,3]
    >>> id1 = id(l)
    >>> l.append(4)
    >>> id2 = id(l)
    >>> id1 == id2
    True
    >>> l = [1,2,3,4]
    >>> id(l) == id1
    False
    
发现了吗，只要重新赋值(rebinding)的话，无论可变不可变，对象的内存位置都会改变，而对于可变对象，只要修改其值，其内存位置是不会改变的。

所以对于不可变对象的修改，尤其像数字，我们有时候会误认为数字其实是可变对象，它们的修改过程都是只有通过重新赋值才能达到的。严格来说，可变对象和不可变对象的区别不在于其值能否被改变，而是修改其值时其内存地址是否有必要改变。

以上讲的都是 Python 在对象方面的求值策略，下面我们结合一下我们命名空间和求值策略，来谈一谈在函数中容易犯的一些错误。

    >>> d = {}
    >>> n = 1
    >>> def test1(n):
    ...     return id(n)
    ... 
    >>> def test2(d,n):
    ...     d[1] = 1
    ...     n = 2
    ...
    >>> def test3():
    ...     d = {3:3}
    ...     n = 3
    ...
    >>> id(n) == test1(n)
    True # 函数是传引用赋值给参量
    >>> test2(d,n)
    >>> print d,n
    {1:1} 1 # d 已经改变，n 不会改变
    >>> test3()
    >>> print d,n
    {1:1} 2 # d 未改变，n 未改变
    
test1：不同于 C 语言的形参，对于 Python 来说，函数中的参数采用了传引用的策略。
test2,test3：在函数中，在没有使用 global 关键词的前提下，对全局变量进行更改，使用重新赋值是无法更改其值，而采用可变对象的方法是可以更改其值的。

你可能会突然感觉到，在函数中，不用 global 且去采用赋值的方法去改变对象值时，实际上，会在函数内部的命名空间建立一个局部变量，并且还会在函数命名空间内覆盖全局变量。例如：

    >>> d = [1,2,3]
    >>> def test():
    ...     d = [1,2] # 创建了局部变量 d
    ...     return id(d) # 函数结束时，局部变量 d 会被销毁
    ... 
    >>> id(d) == test()
    False
    
所以，在实际项目中，慎用可变对象。虽然 Python 自称自己比其它语言简单，但实际上，越简单的语言在这些逻辑处理上就越要小心。

### 综合

可能大家会在看命名空间和传值策略这一段上会有所疑惑，其实我自己在写的时候也翻看了许多资料，阅读了许多博客以确保我写的示例和语言至少不会犯错（可能会不怎么严谨）。

应该说，如果你只是想入门一门编程语言，你现在暂时不用纠结求值策略这个问题，你仅需知道慎用可变对象这一点就足够了。而对于已经学过其它语言的同学，可以去了解一下 C++ 的析构函数和 const 的作用，可能会对你了解 Python 的求值策略有所帮助。









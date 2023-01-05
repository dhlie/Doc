## 基本语法
* python中每一行是一条语句，每条语句以回车结束
* 一条语句太长需要换行时在行末加`\`
* python是缩进严格语言，代码中不要随便缩进
* 注释，单行注释用`#`， 多行注释用`'''`或`"""`

###### 变量&标识符
* python中使用变量不需要声明，直接赋值即可

        a = 'hello'
        println(a)

* python是动态类型语言,可以为变量赋任何类型的值,也可以任意修改变量的类型

        a = 'hello'
        a = 123

* 标识符可以用字母,数字,下划线,数字不能开头

## 数据类型

#### 数值 python中数值分三种:整数,浮点数,复数
整数都是int类型,长度没有限制,能一直大到占满可用内存,如果数值太大可以加`_`分隔

    a = 999999_99999_9999999_999999_9999
    b = 0b1010 # 二进制
    c = 0o123 # 八进制
    d = 0x7f # 十六进制
    
    整数类型的按位运算
    x | y       x 和 y 按位 或
    x ^ y       x 和 y 按位 异或
    x & y       x 和 y 按位 与
    x << n      x 左移 n 位
    x >> n      x 右移 n 位
    ~x          x 逐位取反

python中所有小数都是float类型
对浮点数进行运算时,可能会得到一个不精确的结果

    a = 0.1 + 0.2
    print(a) # 0.30000000000000004

### 字符串
字符串用`'`或`"`或`'''`,`"""`引起来
后面两个可以跨行使用,格式会保留,里面的`'` `"`不用转义
字符串可以用`+`进行拼接,不能和其它类型的值进行加法运算 `a = 'a' + 'b' + 'c'`

占位符  
%s 字符串占位符  
%d 整数占位符  
%f 浮点数占位符 

    a = 'hello %f' % 123.456          # 浮点占位符
    a = 'hello %d,%d' % (123, 456)    # 多个占位符
    a = 'hello %s' % a                # 可以用变量

格式化字符串,可以通过在字符串前面添加一个`f`/`F`创建格式化字符串
在格式化字符串中可以直接嵌入变量

    a = f'b = {b}, c = {c}'
    print(f'a = "{a}"')

格式说明符是可选的，写在表达式后面

    # 保留三位小数,占10个字符宽度右对齐, 字符不够填充'-'
    print(f'"{math.pi:->10.3f}."') # output:"-----3.142"
    # 在 ':' 后传递整数，为该字段设置最小字符宽度，常用于列对齐
    # ':'号后边带填充字符，只能是一个字符，不指定的话默认是空格填充
    # '<','^','>'分别是左对齐、居中、右对齐，后面带宽度
    print(f'{name:10} ==> {phone:10d}')
        Sjoerd     ==>       4127
        Jack       ==>       4098

如果你不希望前置了 \ 的字符转义成特殊字符，
    可以使用 **原始字符串** 方式，在引号前添加 r 即可:

    print('C:\tome\demo')  # here \t means table! # C:      ome\demo
    print(r'C:\tome\demo')  # note the r before the quote

相邻两个字符串会自动连接到一起 

    print('Py' 'thon' == 'Python') # True
    text = ('Put several strings within parentheses '
         'to have them joined together.')

字符串复制  
将字符串和数字相乘`*`,字符串会重复指定次数 `a = 'abc' * 3 # a = "abcabcabc"`

字符串支持 索引 （下标访问）。单字符没有专用的类型，就是长度为一的字符串  
索引还支持负数，用负数索引时，从右边开始计数  
注意，-0 和 0 一样，因此，负数索引从 -1 开始

    word = 'Python'
    word[0]  # P
    word[-1] # n

### 布尔值(bool)
布尔值有两个`True`和`False`
实际上也属于整型,True相当于1, False相当于0

    a = "bool True = %s"%True   # bool True = True
    a = "bool True = %d False = %d"%(True, False)   # bool True = 1 False = 0
    a = 12 + True               # 13


### None 空值

### 类型检查 type() 该函数用来检查值的类型

    print(type(123))    # <class 'int'>
    print(type(1.23))   # <class 'float'>
    print(type("123"))  # <class 'str'>
    print(type(True))   # <class 'bool'>
    print(type(None))   # <class 'NoneType'>
    c = type(123)
    print(type(c))      # <class 'type'>

### 对象(object)
python是面向对象的语言,一切皆对象
对象的结构:每个对象都要保存三种数据

* id (标识)
    > 用来标识对象的唯一性  
    > 在CPython中,id就是对象的内存地址  
    > 可以通过`id()`函数查看对象的id

* type (类型)
    > 类型用来标识当前对象所属的类型  
    > 类型决定了对象有哪些功能  
    > 通过`type()`函数查看对象的类型  
    > Python是一门强类型语言,对象一旦创建类型便不能修改

* value (值)
    > 值就是对象中存储的具体的数据  
    > 对象分两大类,可变对象 不可变对象  
    > 可变对象的值可以改变  
    > 不可变对象的值不能改变

变量是对象的别名,变量中存储的是对象的id,使用变量时实际上是通过id在查找对象

### 类型转换
类型转换是将对象的值转换成指定类型的对象,对原来对象没有影响
类型转换四个函数 `int()` `float()` `bool()` `str()`

    int() 将其他类型转换成整型,规则:

        布尔值:`True` -> 1    `False` -> 0
        浮点数:直接取整,省略小数部分
        字符串:合法的整数字符串,直接转换为对应的数字,不合法的会抛异常

    `float()` 和`int()`基本一致
    
    `bool()` 任何对象都可以转换成布尔值, 规则如下:

    逻辑值检测
        任何对象都可以进行逻辑值的检测，以便在 if 或 while 作为条件或是作为下文所述布尔运算的操作数来使用。

        一个对象在默认情况下均被视为真值，除非当该对象被调用时其所属类定义了 __bool__() 方法且返回 False 
            或是定义了 __len__() 方法且返回零。

        1 下面基本完整地列出了会被视为假值的内置对象:
        被定义为假值的常量: None 和 False。
        任何数值类型的零: 0, 0.0, 0j, Decimal(0), Fraction(0, 1)
        空的序列和多项集: '', (), [], {}, set(), range(0)

        产生布尔值结果的运算和内置函数总是返回 0 或 False 作为假值，1 或 True 作为真值，除非另行说明。 
        （重要例外：布尔运算 or 和 and 总是返回其中一个操作数。）


### 运算符

* 算数运算符

        `+` 数值相加,字符串拼接
        `-` 数值相减
        `*` 数值相乘, 字符串和数字相乘是重复字符串指定次数 `print('-'*20) # 打印分割线`
        `/` 数值相除,结果是浮点数
        `//` 整除,结果只保留整数部分, 对浮点数整除结果仍是浮点数 `a = 25.0 // 4 # 6.0`
        `**` 幂运算,求一个值的几次幂(0.5次幂是开平方)
        `%` 取模

* 赋值运算符
    `=` `+=` `-=` `*=` `/=` `%=` `//=` `**=`
    
        size = width, height = 640, 480 #连续赋值， size是元祖， width， height是变量

        := 可在表达式内部为变量赋值。 它被昵称为“海象运算符”因为它很像是 海象的眼睛和长牙。(赋完值后返回结果)

        if (n := len(a)) > 10:
            pass
        
        # Loop over fixed length blocks
        while (block := f.read(256)) != '':
            process(block)

* 关系运算符

        `>` `<` `>=` `<=` `==` `!=`
        `is` `is not`比较两个对象id
        `==` `!=` 比较的是对象的值
        对字符串进行比较时,比较的是字符串的Unicode码的大小,逐位进行比较

* 逻辑运算符

        `and` `or` `not` (and or会短路)
        对非布尔值进行`not`运算时会先转换成布尔值
        对非布尔值进行`and` `or`运算时,Python会将其当做布尔值运算,**最终会返回最后求值的操作数(包括类型)**

            a = 1 and 2     # 2
            a = 2 and 1     # 1
            a = 1 and 0     # 0
            a = 0 and 1     # 0
            a = 1 or 2      # 1
            a = 2 or 1      # 2
            a = 1 and 'a'   # 'a' 字符串类型
            a = 0 and 'dd'  # 0

* 条件运算符  
    语法:语句1 if 条件表达式 else 语句2
    条件表达式的值为`True`执行语句1, 否则执行语句2  
    `max = a if a > b else b`

* 运算符优先级 参考官方文档

## 语句

### if 语句
    语法:  if 条件表达式:
             代码块
          elif 条件表达式:
             代码块
          else:
             代码块
    代码块:一组代码,以缩进开始,直到代码恢复到之前的缩进级别时结束

### while 语句
    语法: while 条件表达式:
            代码块
         else: # 可选
            代码块

### `break` `continue` `pass`
    `pass` 占位语句,什么也不执行
        if a == 5:
            pass

## 序列
    有三种基本序列类型：list, tuple 和 range 对象

### 列表(list)
    使用一对方括号来表示空列表: []

    使用方括号，其中的项以逗号分隔: [a], [a, b, c]

    使用列表推导式: [x for x in iterable]

    使用类型的构造器: list() 或 list(iterable)

    my_list = []        # 创建一个空列表 type is  <class 'list'>
    my_list = [5]       # 创建一个长度为1的列表
    my_list = [1, 2, 3] # 创建一个长度为3的数组
    my_list = ['a', True, 10, [1,2,3], 3.2] # 列表中可以保存任意对象
    # 列表中元素是有序的 通过索引访问列表元素,索引从0开始
    print(my_list[0])   # 1
    print(my_list[-1])  # 3 索引可以是负数,从后往前的索引
    print(my_list[-2])  # 2

### 序列通用操作

|运算               |结果|
|:-                 |:-|
|x in s             |如果 s 中的某项等于 x 则结果为 True，否则为 False|
|x not in s         |如果 s 中的某项等于 x 则结果为 False，否则为 True|
|s + t              |s 与 t 相拼接|
|s * n or n * s     |相当于 s 与自身进行 n 次拼接
|s[i]               |s 的第 i 项，起始为 0|
|s[i:j]             |s 从 i 到 j 的切片, 切片类型和 s 类型一致|
|s[i:j:k]           |s 从 i 到 j 步长为 k 的切片|
|len(s)             |s 的长度|
|min(s)             |s 的最小项|
|max(s)             |s 的最大项|
|s.index(x[, i[, j]])|x 在 s 中首次出现项的索引号（索引号在 i 或其后且在 j 之前）|
|s.count(x)         |x 在 s 中出现的总次数|

    my_list = [1,2,3] * 2       # [1, 2, 3, 1, 2, 3]
    my_list = [1,2,3] + [4,5,6] # [1, 2, 3, 4, 5, 6]
    print(2 in my_list)         # True
    print(3 not in my_list)     # False
    print(len(my_list))         # 6 列表长度
    print(min(my_list))         # 1 列表最小值
    print(max(my_list))         # 6 列表最大值
    print(my_list.index(3))     # 2 元素第一次出现的位置,元素不在列表中会抛异常
    print(my_list.index(3, 0, len(my_list)))# 2 元素第一次出现的位置,参数是查找范围
    print(my_list.count(3))     # 1 元素出现的次数
    
    切片
    从序列中获取一个子序列, 语法:[起始:结束:步长]包含头不包含尾,步长可以省略,默认值为1
    返回一个新序列,不影响原来的序列
    步长不能为0 可以是负数,如果是负数,会从列表后向前取元素  
    切片索引的默认值很有用；省略开始索引时，默认值为 0，省略结束索引时，默认为到序列的结尾
    注意切片的开始总是被包括在结果中，而结束不被包括。这使得 s[:i] + s[i:] 总是等于 s

    my_list = [1,2,3,4,5,6,7,8,9]
    sub_list = my_list[1:3] # [2, 3] 包含头不包含尾
    sub_list = my_list[1:len(my_list)] # [2, 3, 4, 5, 6, 7, 8, 9]
    sub_list = my_list[1:]  # [2, 3, 4, 5, 6, 7, 8, 9]
    sub_list = my_list[:3]  # [1, 2, 3] 起始和结束为止的索引都可以不写
    sub_list = my_list[:]   # [1, 2, 3, 4, 5, 6, 7, 8, 9]
    sub_list = my_list[::2] # [1, 3, 5, 7, 9] 步长
    sub_list = my_list[::-1]# [9, 8, 7, 6, 5, 4, 3, 2, 1] 步长为负数
    sub_list = my_list[::-2]# [9, 7, 5, 3, 1]
    print(sub_list)

#### 修改列表
    my_list = [1,2,3,4,5,6]
    my_list[0] = 10         # [10, 2, 3, 4, 5, 6]
    del my_list[0]          # [2, 3, 4, 5, 6] 删除元素
    my_list[0:2] = [22,33]  # [22, 33, 4, 5, 6] 通过切片修改
    my_list[0:0] = [44,55]  # [44, 55, 22, 33, 4, 5, 6] 在最前面插入 通过切片修改
    del my_list[0:2]        # [22, 33, 4, 5, 6]
    del my_list[::2]        # [33, 5] 以上操作都支持步长

#### 可变序列方法

|运算                    |结果|
|:-                     |:-|
|s[i] = x               |将 s 的第 i 项替换为 x|
|s[i:j] = t             |将 s 从 i 到 j 的切片替换为可迭代对象 t 的内容|
|s[i:j:k] = t           |将 s[i:j:k] 的元素替换为 t 的元素,**元素个数必须一样**|
|del s[i:j]             |等同于 s[i:j] = []|
|del s[i:j:k]           |从列表中移除 s[i:j:k] 的元素|
|s.append(x)            |将 x 添加到序列的末尾 (等同于 s[len(s):len(s)] = [x])|
|s.insert(i, x)         |在由 i 给出的索引位置将 x 插入 s (等同于 s[i:i] = [x])|
|s.extend(t) 或 s += t  |用 t 的内容扩展 s (基本上等同于 s[len(s):len(s)] = t)|
|s.remove(x)            |删除 s 中第一个 s[i] 等于 x 的项目。|
|s.pop([i])             |提取在 i 位置上的项，并将其从 s 中移除|
|s.clear()              |从 s 中移除所有项 (等同于 del s[:])|
|s.copy()               |创建 s 的浅拷贝 (等同于 s[:])|
|s *= n                 |使用 s 的内容重复 n 次来对其进行更新|
|s.reverse()            |翻转

#### 遍历列表
    for循环语法:
        for 变量 in 序列 :
            代码块

    for item in s :
        print(item)

### range对象
    range 类型表示不可变的数字序列，通常用于在 for 循环中循环指定的次数。
    class range(stop)
    class range(start, stop[, step])
    range 构造器的参数必须为整数（可以是内置的 int 或任何实现了 __index__ 特殊方法的对象）。 
    如果省略 step 参数，其默认值为 1。 如果省略 start 参数，其默认值为 0，如果 step 为零则会引发 ValueError。
    如果 step 为正值，确定 range r 内容的公式为 r[i] = start + step*i 其中 i >= 0 且 r[i] < stop。

    如果 step 为负值，确定 range 内容的公式仍然为 r[i] = start + step*i，但限制条件改为 i >= 0 且 r[i] > stop.

    如果 r[0] 不符合值的限制条件，则该 range 对象为空。
     range 对象确实支持负索引，但是会将其解读为从正索引所确定的序列的末尾开始索引。

    >>> list(range(10))
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    >>> list(range(1, 11))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> list(range(0, 30, 5))
    [0, 5, 10, 15, 20, 25]
    >>> list(range(0, 10, 3))
    [0, 3, 6, 9]
    >>> list(range(0, -10, -1))
    [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
    >>> list(range(0))
    []
    >>> list(range(1, 0))
    []

    使用 == 和 != 检测 range 对象是否相等是将其作为序列来比较。 
    也就是说，如果两个 range 对象表示相同的值序列就认为它们是相等的。 
    （请注意比较结果相等的两个 range 对象可能会具有不同的 start, stop 和 step 属性，
    例如 range(0) == range(2, 1, 3) 而 range(0, 3, 2) == range(0, 4, 2)。）

### 元祖(tuple)
    元祖是一个不可变系列,操作方式基本上和列表是一致的
    使用一对圆括号来表示空元组: ()

    使用一个后缀的逗号来表示单元组: a, 或 (a,)

    使用以逗号分隔的多个项: a, b, c or (a, b, c)

    使用内置的 tuple(): tuple() 或 tuple(iterable)

    my_tuple = (1,2,3,4,5)  
    my_tuple = 1,2,3,4      # 当元祖不是空时,()可以省略
    # my_tuple[1] = 3       # TypeError: 'tuple' object does not support item assignment
    a,b,c,d = my_tuple      # 序列的解包,将序列中每一个元素都赋值给一个变量
    # a,b,c = my_tuple      # 个数不匹配 ValueError: too many values to unpack (expected 3)
    a,b,*c = my_tuple       # 可以在变量前面加一个*,这样变量将会获取序列中剩余的元素
    print(c)                # [3, 4]
    *a,b,c = my_tuple       # 可以在变量前面加一个*,这样变量将会获取序列中剩余的元素
    print(a)                # [1, 2]
    print(my_tuple)

    #利用序列解包交换两个变量的值
    a = 10
    b = 20
    print(a, b)
    a,b = b, a
    print(a, b)

### 可变对象
    对象是由id,type,value组成的,可变对象的值是可变的

## 字典(dict)
    字典存放的是key-value对,使用 {} 或 dict() 构造函数来创建字典
    key是不可变对象,不能重复,重复时会覆盖,value可以是任意对象
    字典会保留插入时的顺序
    两个字典的比较当且仅当它们具有相同的 (键, 值) 对时才会相等（不考虑顺序）。 排序比较 ('<', '<=', '>=', '>') 会引发 TypeError。

    m = {}
    a = {'one': 1, 'two': 2, 'three': 3}
    b = dict(one=1, two=2, three=3)
    c = dict({'three': 3, 'one': 1, 'two': 2})
    d = dict([('two', 2), ('one', 1), ('three', 3)])
    print(a == b == c == d) # True

    d[key]
        获取与key对应的value值,如果dict中没有key会引发KeyError
    list(d)
        返回字典 d 中使用的所有键的列表
    len(d)
        返回字典 d 中的项数
    d[key] = value
        将 d[key] 设为 value
    del d[key]
        将 d[key] 从 d 中移除。 如果映射中不存在 key 则会引发 KeyError。
    key in d
        如果 d 中存在键 key 则返回 True，否则返回 False。
    key not in d
        等价于 not key in d。
    iter(d)
        返回以字典的键为元素的迭代器。 这是 iter(d.keys()) 的快捷方式。
    clear()
        移除字典中的所有元素。
    copy()
        返回原字典的浅拷贝。
    get(key[, default])
        如果 key 存在于字典中则返回 key 的值，否则返回 default。 如果 default 未给出则默认为 None，因而此方法绝不会引发 KeyError。
    pop(key[, default])
        如果 key 存在于字典中则将其移除并返回其值，否则返回 default。 如果 default 未给出且 key 不存在于字典中，则会引发 KeyError。
    popitem()
        从字典中移除并返回一个 (键, 值) 对。 键值对会按 LIFO 的顺序被返回。
        popitem() 适用于对字典进行消耗性的迭代，这在集合算法中经常被使用。 如果字典为空，调用 popitem() 将引发 KeyError。
    items()
        返回由字典项 ((键, 值) 对) 组成的一个新视图。
        dict_items([('two', 2), ('one', 1), ('three', 3)])
    keys()
        返回由字典键组成的一个新视图。
        dict_keys(['two', 'one', 'three'])
    values()
        返回由字典值组成的一个新视图。
        dict_values([2, 1, 3])
    setdefault(key[, default])
        如果字典存在键 key ，返回它的值。如果不存在，插入值为 default 的键 key ，并返回 default 。 default 默认为 None。
    update([other])
        使用来自 other 的键/值对更新字典，覆盖原有的键。 返回 None。

    d = {'a':1, 'b':2, 'c':3}
    for k,v in d.items() :
        print(f'key = {k}, value = {v}')
    
    当在序列中循环时，用 enumerate() 函数可以将索引位置和其对应的值同时取出
    li = [1,2,3,4,5,]
    for i,v in enumerate(li):
        print(f'position = {i}, value = {v}')
        
    # Strategy:  Iterate over a copy 不要一边遍历一边修改，通常做法是创建副本
    for user, status in users.copy().items():
        if status == 'inactive':
            del users[user]

## 集合(set)
    集合中只能存储不可变对象
    存储的对象是无序的
    元素不能重复
    目前有两种内置集合类型，set 和 frozenset。 set 类型是可变的 
        --- 其内容可以使用 add() 和 remove() 这样的方法来改变。 
        由于是可变类型，它没有哈希值，且不能被用作字典的键或其他集合的元素。 
    frozenset 类型是不可变并且为 hashable 
        --- 其内容在被创建后不能再改变；因此它可以被用作字典的键或其他集合的元素。
    除了可以使用 set 构造器，非空的 set (不是 frozenset) 
    还可以通过将以逗号分隔的元素列表包含于花括号之内来创建，例如: {'jack', 'sjoerd'}。
    class set([iterable])
    class frozenset([iterable])

### 集合操作
    set forzenset都支持的操作:
    len(s)
        返回集合 s 中的元素数量（即 s 的基数）。
    x in s
        检测 x 是否为 s 中的成员。
    x not in s
        检测 x 是否非 s 中的成员。
    isdisjoint(other)
        如果集合中没有与 other 共有的元素则返回 True。 当且仅当两个集合的交集为空集合时，两者为不相交集合。
    issubset(other)
    set <= other
        检测是否集合中的每个元素都在 other 之中。
    set < other
        检测集合是否为 other 的真子集，即 set <= other and set != other。
    issuperset(other)
    set >= other
        检测是否 other 中的每个元素都在集合之中。
    set > other
        检测集合是否为 other 的真超集，即 set >= other and set != other。
    union(*others)
    set | other | ...
        返回一个新集合，其中包含来自原集合以及 others 指定的所有集合中的元素。
    intersection(*others)
    set & other & ...
        返回一个新集合，其中包含原集合以及 others 指定的所有集合中共有的元素。
    difference(*others)
    set - other - ...
        返回一个新集合，其中包含原集合中在 others 指定的其他集合中不存在的元素。
    symmetric_difference(other)
    set ^ other
        返回一个新集合，其中的元素或属于原集合或属于 other 指定的其他集合，但不能同时属于两者。
    copy()
        返回原集合的浅拷贝。

    请注意，非运算符版本的 union(), intersection(), difference()，
        以及 symmetric_difference(), issubset() 和 issuperset() 方法会接受任意可迭代对象作为参数。 
        相比之下，它们所对应的运算符版本则要求其参数为集合。 这就排除了容易出错的构造形式
        例如 set('abc') & 'cbs'，而推荐可读性更强的 set('abc').intersection('cbs')。

    set支持 forzenset不支持的操作:
    update(*others)
    set |= other | ...
        更新集合，添加来自 others 中的所有元素。
    intersection_update(*others)
    set &= other & ...
        更新集合，只保留其中在所有 others 中也存在的元素。
    difference_update(*others)
    set -= other | ...
        更新集合，移除其中也存在于 others 中的元素。
    symmetric_difference_update(other)
    set ^= other
        更新集合，只保留存在于集合的一方而非共同存在的元素。
    add(elem)
        将元素 elem 添加到集合中。
    remove(elem)
        从集合中移除元素 elem。 如果 elem 不存在于集合中则会引发 KeyError。
    discard(elem)
        如果元素 elem 存在于集合中则将其移除。
    pop()
        从集合中移除并返回任意一个元素。 如果集合为空则会引发 KeyError。
    clear()
        从集合中移除所有元素。

    请注意，非运算符版本的 update(), intersection_update(), 
        difference_update() 和 symmetric_difference_update()
        方法将接受任意可迭代对象作为参数。

## 函数
    def 函数名([形参1 = a, 形参2 = b, ...]) :
        代码块
    调用时实参可以是任意类型

    可以在形参前边加一个*,这样这个形参将会获取到所有实参,它将所有实参保存到一个元祖中

    带*的参数可以写在任意位置,调用的时候*参数后所有的参数都要用关键字参数

    '/,'前面的形参为仅限位置形参(可以避免形参名修改后影响调用代码), '*,'后面的形参必须以关键字形式传递

    形参 a 和 b 为仅限位置形参，c 或 d 可以是位置形参或关键字形参，而 e 或 f 要求为关键字形参:
    def f(a, b, /, c, d, *, e, f):
        print(a, b, c, d, e, f)

        def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
              -----------    ----------     ----------
                |             |                  |
                |        Positional or keyword   |
                |                                - Keyword only
                -- Positional only

    **形参 带两个*的形参,用来接收关键字参数

    返回值用return,可以返回任意类型
        return 语句返回函数的值。return 语句不带表达式参数时，返回 None。函数执行完毕退出也返回 None

    函数定义在当前符号表中把函数名与函数对象关联在一起。函数名可以赋值给变量

    重要警告： 默认值只计算一次。默认值为列表、字典或类实例等可变对象时，会产生与该规则不同的结果。
        def f(a, L=[]):
            L.append(a)
            return L

        print(f(1)) # [1]
        print(f(2)) # [1, 2]
        print(f(3)) # [1, 2, 3]

    文档字符串(doc str) 定义函数时可以在函数内部编写文档字符串,可以使用help()查看

    作用域:全局作用域,函数作用域
        所有函数以外的区域都是全局作用域
        在全局作用域定义的变量都是全局变量,全局变量可以在程序的任意位置被访问

        在函数作用域中定义的变量都是局部变量,只能在函数内部访问

        在函数作用域使用全局变量时用global
            global a # 使用全局变量a,如果未定义则定义全局变量
        nonlocal 语句表明特定变量生存于外层作用域中并且应当在其中被重新绑定。
        def scope_test():
            def do_local():
                spam = "local spam"
        
            def do_nonlocal():
                nonlocal spam
                spam = "nonlocal spam"
        
            def do_global():
                global spam
                spam = "global spam"
        
            spam = "test spam"
            do_local()
            print("After local assignment:", spam)
            do_nonlocal()
            print("After nonlocal assignment:", spam)
            do_global()
            print("After global assignment:", spam)
        
        scope_test()
        print("In global scope:", spam)
        
        #代码输出：
        After local assignment: test spam
        After nonlocal assignment: nonlocal spam
        After global assignment: nonlocal spam
        In global scope: global spam

    命名空间:命名空间实际上就是一个字典,专门用来存储变量
        locals()用来获取当前作用域的命名空间
        globals()获取全局命名空间
        拿到命名空间字典后可以修改,访问里面的值
        
    函数的 执行 会引入一个用于函数局部变量的新符号表。 更确切地说，
        函数中所有的变量赋值都将存储在局部符号表中；而变量引用会
        首先在局部符号表中查找，然后是外层函数的局部符号表，
        再然后是全局符号表，最后是内置名称的符号表。 
        因此，全局变量和外层函数的变量不能在函数内部直接
        赋值（除非是在 global 语句中定义的全局变量)

    高阶函数:接收函数作为参数,或者将函数作为返回值的函数是高阶函数

    匿名函数:lambda函数表达式,专门用来创建一些简单地函数,
        他是函数创建的又一种方式
        语法: lambda 参数列表 : 返回值

    闭包:将函数作为返回值返回,也是一种高阶函数,这种高阶函数叫做闭包
        通过闭包可以创建一些只有当前函数能访问的变量
        可以将一些私有的数据藏到闭包中

    装饰器:
        def say_hello() :
            print('hello')

        def sum(a, b) :
            return a + b

        def begin_end(func) :

            def deco(*args, **kwargs) :
                print('deco start------')
                result = func(*args, **kwargs)
                print('deco end--------')
                return result
            
            return deco
        deco_say_hello = begin_end(say_hello)
        deco_say_hello()
        deco_sum = begin_end(sum)
        print(deco_sum(3,5))

-----

    # 形参可以设置默认值
    def fn(a = 1, b = 2, c = 3) :
        print(f'a = {a}, b = {b}, c = {c}')

    fn(1,2,3)               # 位置参数
    fn(c = 3, a = 2, b = 1) # 关键字参数
    fn(1, c = 3)            # 位置参数和关键字参数混用
    位置参数和关键字参数混用时关键字参数后面的参数都必须是关键字参数, 也就是位置参数在前,关键字参数在后
    def deco(*args, **kwargs) 这种写法可以接收函数调用时的所有实参

    def sum(a, *b) :
        print('a = ', a, 'b = ', b)
    sum(1, 2, 3)            # a =  1 b =  (2, 3)
    sum(1)                  # a =  1 b =  ()

    # *参数不是必须放在最后,它后面的所有参数都必须以关键字参数形式传递
    def sum(*a, b) :
        print('a = ', a, 'b = ', b)
    sum(1,2,3,b=4)          # a =  (1, 2, 3) b =  4

    # 这里必须以关键字形式传参
    def sum(*,a, b) :
        print('a + b = ', a + b)
    sum(a = 1, b = 2)       # 这里必须以关键系形式传参

    # **形参可以接收关键字参数,它会将这些参数保存在一个字典中
    def sum(**a) :
        print('a = ', a)
    sum(a = 1, b = 2, c = 3) # a =  {'a': 1, 'b': 2, 'c': 3}

    # 参数解包
    # 传递序列参数时可以加一个*,这样会自动将序列中的元素依次作为参数传递
    # 对字典解包用**
    t = (10, 20, 30)
    d = {'a':10, 'b':20, 'c':30}
    def sum(a, b, c) :
        print(f'a = {a}, b = {b}, c = {c}')
    sum(*t)         # a = 10, b = 20, c = 30
    sum(**d)        # a = 10, b = 20, c = 30

    #形参类型
    def sum(a:int, b:float, c:bool) :   # 可以给形参加类型,传参时类型可以不匹配
        print('a = ', a, 'b = ', b, 'c = ', c)
    sum(1,2,3) # 类型不匹配  a =  1 b =  2 c =  3

--------------------------

## 对象(Object)
    用class关键字定义类
    class 类名([父类]) :
        公共属性
        方法
    object 是所有类的父类
    isinstance() 检查一个对象是不是类的实例
    __method__() 这种方法叫魔术方法/特殊方法,不要尝试去调用魔术方法
        会在特殊时期自动调用

    __属性,该属性是隐藏属性,外部无法访问,其实就是改了个名字(_类名__属性)
        依然可以通过修改后的名字访问
    一般用单_开头标识私有属性,只是一种规范,外部依然可以访问修改

    p = Person()的运行流程
        1.创建一个变量
        2.在内存中创建一个新对象
        3.调用__init__(self)方法
        4.将对象id赋值给变量

    property装饰器
    class Dog() :
        def __init__(self, name, age, gender, height) :
            self._name = name

        # getter 方法
        @property
        def name(self) :
            return self._name

        # setter方法
        @name.setter
        def name(self, name) :
            self._name = name

        d1 = Dog('xiaoqiang', 2, '公', 100)
        d1.name = 'lisi'
        print(d1.name) # lisi

    方法重写:子类中有和父类同名方法时子类会覆盖父类的方法
        当调用一个对象的方法时会优先从当前对象中找,
        如果找不到回去当前对象的父类中找,
        如果还找不到,则去父类的父类中找,以此类推
        super() 获取父类对象
        同名方法都会覆盖,python中没有方法重载,方法和属性也不能重名,同名也会覆盖

    继承:python支持多重继承
        多重继承时,子类同时拥有所有父类中的方法
        方法调用时会按继承顺序查找,找到了就调用
        开发中不建议使用多继承

    类名.__bases__ 这个属性可以用来获取当前类的所有父类
        print(Dog.__bases__) # (<class '__main__.Animal'>,)

    多态:

    类属性/实例属性:
        直接在类中定义的属性叫类属性,类属性只能通过类对象修改
            无法通过实例对象修改
            class Dog :
                name = ''   # 类属性
            Dog.name = 'ddd'
            d.name = 'aaa'  # 无法通过对象修改类属性 会在对象中新增实例属性
            print(Dog.name) # ddd
            print(d.name)   # aaa

    实例方法:在类中定义,以self为第一个参数的方法都是实例方法
        实例方法通过实例和类调用
            通过实例调用时会自动将当前调用对象作为self传入
            通过类调用时,需要手动传入self参数

            d = Dog()
            d.run()
            Dog.run(d)

    类方法:在类内部使用@classmethod 来修饰的方法属于类方法
        类方法第一个参数是cls,也会被自动传递,cls就是当前类对象
        和实例方法的区别:第一个参数不同;类方法可以通过类调用,也可以通过实例调用
        
        @classmethod
        def cm(cls) :
            print('classmethod cls:', cls, cls.age)

    静态方法:在类中使用@staticmethod 修饰的方法属于静态方法
        静态方法不需要默认参数,通过类和实例调用
        本质上是一个和当前类无关的方法,只是保存在这个类中

        @staticmethod
        def sm() :
            print('staticmethod')
            
    # 特殊方法
    object.__new__(cls[, ...])
        调用以创建一个 cls 类的新实例。
        典型的实现会附带适宜的参数使用 super().__new__(cls[, ...])，
            通过超类的 __new__() 方法来创建一个类的新实例，
            然后根据需要修改新创建的实例再将其返回。
        如果返回了一个新实例，__init__()方法会被调用，否则不会被调用
    object.__init__(self[, ...])
        在实例 (通过 __new__()) 被创建之后，返回调用者之前调用。
    object.__del__(self)
        在实例将被销毁时调用。
        del x 并不直接调用 x.__del__() --- 前者会将 x 的
            引用计数减一，而后者仅会在 x 的引用计数变为零时被调用。
    object.__repr__(self)
        由 repr() 内置函数调用以输出一个对象的“官方”字符串表示。
            如果可能，这应类似一个有效的 Python 表达式，能被用来
            重建具有相同取值的对象（只要有适当的环境）。
            如果这不可能，则应返回形式如 <...some useful description...> 的字符串
    object.__str__(self)
        通过 str(object) 以及内置函数 format() 和 print() 
            调用以生成一个对象的“非正式”或格式良好的字符串表示。
            返回值必须为一个 字符串 对象。
    object.__bytes__(self)
        通过 bytes 调用以生成一个对象的字节串表示。这应该返回一个 bytes 对象。
    object.__format__(self, format_spec)
        通过 format() 内置函数、扩展、格式化字符串字面值 的求
            值以及 str.format() 方法调用以生成一个对象的“格式化”字符串表示。 
    object.__lt__(self, other)
    object.__le__(self, other)
    object.__eq__(self, other)
    object.__ne__(self, other)
    object.__gt__(self, other)
    object.__ge__(self, other)
        < <= == != > >=运算符
    object.__bool__(self)
        调用此方法以实现真值检测以及内置的 bool() 操作；应该返回 False 或 True
        如果未定义此方法，则会查找并调用 __len__() 并在其返回非零值时视对象的逻辑值为真。
        如果一个类既未定义 __len__() 也未定义 __bool__() 则视其所有实例的逻辑值为真。
     

------------

## 模块(module)
    python中一个.py文件就是一个模块,模块名就是文件名
    模块可以包含可执行的语句以及函数定义。这些语句用于初始化模块。
        它们仅在模块 第一次 在 import 语句中被导入时才执行。
    每个模块都有它自己的私有符号表，该表用作模块中定义的所有函数的全局符号表。
        因此，模块的作者可以在模块内使用全局变量，而不必担心与用户的全局变量发生意外冲突
    在一个模块中引入外部模块,格式为:
        import 模块名 [as 别名]
        from 包[.子包] import 模块名
        可以写在程序任意位置,但一般情况下都写在文件开头

    每个模块内部都有一个 __name__ 属性,通过这个属性可以获取到模块的名字
    主模块的名字为 __main__,程序只有一个主模块
        if __name__=='__main__':  # 推荐使用这种方式调试代码，
            # 只有执行当前模块的人才会执行以下代码，
            # 如果是别人调用该模块，以下的代码是不会被执行的！
            foo()

        import test_module as test

        print(__name__)         # __main__
        print(test.__name__)    # test_module

    包:包也是一个模块,普通模块就是一个py文件,包是一个带有__init__.py文件的文件夹
        包中必须要有一个 __init__.py文件,否则就是一个普通目录
        import语句会执行__init__.py文件

        packA
        |-- __init__.py
        |-- math.py
        |-- packB
            |-- __init__.py
            |-- log.py

        # 多级包的引入
        from packA.packB import log

    从包中导入 *
        如果一个包的 __init__.py 代码定义了一个名为 __all__ 的列表，它会被视为在遇到 from package import * 时应该导入的模块名列表

            __all__ = ["echo", "surround", "reverse"]
        如果未定义,from sound.effects import * 语句不会导入包中的模块
        

    __pycache__ 是python的缓存目录.
        py文件在执行前,需要被解析器先转换为机器码,然后再执行
        为了提高性能,python会在编译过一次以后,将机器码保存到缓存文件中

    为了实现开箱即用的思想,python提供了一系列标准库

    sys模块,它里面提供了一些变量和函数,我们可以获取到python解析器的信息
        或者通过函数操作解析器

        sys.argv 获取代码执行时,命令行中所包含的参数
        sys.modules 获取当前程序中所有模块
        sys.path 他是一个模块搜索路径列表,类似java 中的classpath
            使用import语句时候，Python解释器通过sys.path的路径搜索。
        sys.platform 运行平台
        
            |系统           |平台值|
            |:-             |:-|
            |AIX            |'aix'|
            |Linux          |'linux'|
            |Windows        |'win32'|
            |Windows/Cygwin |'cygwin'|
            |macOS          |'darwin'|

        sys.exit() 退出程序函数

    pprint模块,有一个pprint()方法,用来对打印的数据做简单的格式化

    os 模块对操作系统进行访问
        os.environ 系统环境变量
        os.system() 用来执行平台命令
        os.listdir()
        os.getcwd() 获取当前工作目录
        os.chdir() cd命令
        os.mkdir()
        os.rmdir()
        os.remove() 删除文件
        os.rename() 重命名文件

## 异常
    异常处理语法
        try:
            代码块(可能出错的代码)
        [except 异常类型1 as 别名 :
            代码块
        except 异常类型2 :
            代码块
        except: # 不跟错误类型或 跟Exception类型 时会捕获所有异常
            代码块(出错后要执行的代码)]
        [else:
            代码块(未出错时执行的代码)]
        [finally:
            代码块]

    抛异常用 raise 异常类型
        # 自定义异常 Exception是所有异常的父类
        class SelfError(Exception) :
            pass

        # 抛异常
        def ex() :
            raise SelfError('ex error')

    
## 文件操作
    file = open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
        |模式   |意义|
        |:-     |:-|
        |'r'    |只读(默认)|
        |'w'    |只写,不能读,文件不存在会创建,打开时会清空文件|
        |'x'    |排他性创建并写入,如果文件已存在则失败|
        |'a'    |追加写入,文件不存在会创建文件|
        |'b'    |二进制模式|
        |'t'    |文本模式(默认)|
        |'+'    |打开用于更新(读取与写入)|

    file.close() 关闭文件
    file.read(size=-1, /) 读取size个字符(文本模式)或字节(二进制模式),如果size是负数或者省略,一直读到文件结束
    file.readline() 读取一行,返回字符串
    file.readlines() 读取所有行,返回一个list,里面每个元素是一行的字符串
    file.tell() 返回当前位置
    file.seek(cookie, whence=0, /) 改变流的位置,whence:
        0 从流开始出偏移
        1 从当前位置偏移,偏移量可以是负数
        2 从流结尾处偏移,偏移量通常是负数

    with 语句 as 语句
        代码块
    执行过程：
        1.对上下文表达式 (在 with_item 中给出的表达式) 求值以获得一个上下文管理器。
        2.载入上下文管理器的 __exit__() 以便后续使用。
        3.发起调用上下文管理器的 __enter__() 方法。
        4.如果 with 语句中包含一个目标，来自 __enter__() 的返回值将被赋值给它。
        5.执行语句体。
        6.发起调用上下文管理器的 __exit__() 方法。 

    # with 语句结束会自动关闭文件
    with open('hello.txt') as file_obj :
        r = file_obj.read()
        print(r)

    try :
        with open('hello.txt') as file :
            buf = 1024
            while True :
                content = file.read(buf)
                if not content :
                    break
                print(content, end = '')
    except :
        print('error')

    # 拷贝文件
    try:
        oldp = '/Users/duanhl/Downloads/bb.pdf'
        newp = 'aa.pdf'
        with open(oldp, 'rb') as file :
            with open(newp, 'wb') as new_file :
                buf = 1024 * 4
                while True :
                    c = file.read(buf)
                    if not c :
                        break
                    new_file.write(c)
    except Exception as e :
        raise e

## 迭代器

    for element in [1, 2, 3]:
        print(element)
    for element in (1, 2, 3):
        print(element)
    for key in {'one':1, 'two':2}:
        print(key)
    for char in "123":
        print(char)
    for line in open("myfile.txt"):
        print(line, end='')

    在幕后，for 语句会调用容器对象中的 iter()。 该函数返回一个定义了 __next__() 方法的
        迭代器对象，该方法将逐一访问容器中的元素。 当元素用尽时，__next__() 将
        引发 StopIteration 异常来通知终止 for 循环

    给你的类添加迭代器行为很容易。 定义一个 __iter__() 方法来返回
        一个带有 __next__() 方法的对象。 如果类已定义了 __next__()，
        则 __iter__() 可以简单地返回 self:

        class Msg :

            def __init__(self, data) :
                self._data = data
                self._len = len(data)
                self._index = 0

            def __iter__(self) :
                return self

            def __next__(self) :
                if self._index == self._len :
                    raise StopIteration
                result = self._data[self._index]
                self._index += 1
                return result

        m = Msg('hello')
        for i in m :
            print(i)
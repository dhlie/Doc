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
整数都是int类型,大小没有限制,不会溢出,如果数值太大可以加`_`分隔
```
a = 999999_99999_9999999_999999_9999
b = 0b1010 # 二进制
c = 0o123 # 八进制
d = 0x7f # 十六进制
```

python中所有小数都是float类型
对浮点数进行运算时,可能会得到一个不精确的结果
```
    a = 0.1 + 0.2
    print(a) # 0.30000000000000004
```

### 字符串
字符串用`'`或`"`或`'''`,`"""`引起来
后面两个可以跨行使用,格式会保留,里面的`'` `"`不用转义
字符串可以用`+`进行拼接,不能和其它类型的值进行加法运算 `a = 'a' + 'b' + 'c'`
占位符
%s 字符串占位符
%d 整数占位符
%f 浮点数占位符 
```
    a = 'hello %f'%123.456          # 浮点占位符
    a = 'hello %d,%d'%(123, 456)    # 多个占位符
    a = 'hello %s'%a                # 可以用变量
```

格式化字符串,可以通过在字符串前面添加一个`f`/`F`创建格式化字符串
在格式化字符串中可以直接嵌入变量
```
    a = f'b = {b}, c = {c}'
    print(f'a = "{a}"')
```

字符串复制
将字符串和数字相乘`*`,字符串会重复指定次数 `a = 'abc' * 3 # a = "abcabcabc"`

### 布尔值(bool)
布尔值有两个`True`和`False`
实际上也属于整型,True相当于1, False相当于0
```
    a = "bool True = %s"%True   # bool True = True
    a = "bool True = %d False = %d"%(True, False)   # bool True = 1 False = 0
    a = 12 + True # 13
```

### None 空值

### 类型检查 type() 该函数用来检查值的类型
```
    print(type(123))    # <class 'int'>
    print(type(1.23))   # <class 'float'>
    print(type("123"))  # <class 'str'>
    print(type(True))   # <class 'bool'>
    print(type(None))   # <class 'NoneType'>
    c = type(123)
    print(type(c))      # <class 'type'>
```

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
        >> 可变对象的值可以改变
        >> 不可变对象的值不能改变

变量是对象的别名,变量中存储的是对象的id,使用变量时实际上是通过id在查找对象

### 类型转换
类型转换是将对象的值转换成指定类型的对象,对原来对象没有影响
类型转换四个函数 `int()` `float()` `bool` `str()`

    int() 将其他类型转换成整型,规则:

        布尔值:`True` -> 1    `False` -> 0
        浮点数:直接取整,省略小数部分
        字符串:合法的整数字符串,直接转换为对应的数字,不合法的会抛异常

    `float()` 和`int()`基本一致
    `bool()` 任何对象都可以转换成布尔值, 规则:
        对于所有标识空性的对象都会转换成False,其余的转换为True
        哪些表示的空性:0,None,''等

### 运算符

* 算数运算符
    `+` 数值相加,字符串拼接
    `-` 数值相减
    `*` 数值相乘, 字符串和数字相乘是重复字符串指定次数 `print('-'*20) # 打印分割线`
    `/` 数值相除,结果是浮点数
    `//` 整除,结果只保留整数部分, **对浮点数整除结果仍是浮点数** `a = 25.0 // 4 # 6.0`
    `**` 幂运算,求一个值的几次幂(0.5次幂是开平方)
    `%` 取模

* 赋值运算符
    `=` `+=` `-=` `*=` `/=` `%=` `//=` `**=`

* 关系运算符
    `>` `<` `>=` `<=` `==` `!=`
    `is` `is not`比较两个对象id
    `==` `!=` 比较的是对象的值,不是id
    对字符串进行比较时,比较的是字符串的Unicode码的大小,逐位进行比较

* 逻辑运算符
    `and` `or` `not` (会短路)
    对非布尔值进行`not`运算时会先转换成布尔值
    对非布尔值进行`and` `or`运算时,Python会将其当做布尔值运算,**最终会返回最后求值的原值(包括类型)**

        a = 1 and 2 # 2
        a = 2 and 1 # 1
        a = 1 and 0 # 0
        a = 0 and 1 # 0
        a = 1 or 2 # 1
        a = 2 or 1 # 2
        a = 1 and 'a' # 'a' 字符串类型
        a = 0 and 'dd' # 0

* 条件运算符
    语法:语句1 if 条件表达式 else 语句2
    条件表达式的值为`True`执行语句1, 否则执行语句2
    `max = a if a > b else b`

* 运算符优先级 参考官方文档

## 语句

### if 语句
    语法:  if 条件表达式 :
             代码块
          elif 条件表达式 :
             代码块
          else :
             代码块
    代码块:一组代码,以缩进开始,直到代码恢复到之前的缩进级别时结束

### while 语句
    语法: while 条件表达式 :
            代码块
         else : # 可选
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
    my_list = ['a', True, 10, [1,2,3], 3.2] # 列表中可以保存任意对象
    my_list= [1, 2, 3]  # 创建一个长度为3的数组
    # 列表中元素是有序的 通过索引访问列表元素,索引从0开始
    print(my_list[0])   # 1
    print(my_list[-1])  # 3 索引可以是负数,从后往前的索引
    print(my_list[-2])  # 2

### 序列通用操作

|运算|结果|
|:-|:-|
|x in s             |如果 s 中的某项等于 x 则结果为 True，否则为 False|
|x not in s         |如果 s 中的某项等于 x 则结果为 False，否则为 True|
|s + t              |s 与 t 相拼接|
|s * n or n * s     |相当于 s 与自身进行 n 次拼接
|s[i]               |s 的第 i 项，起始为 0|
|s[i:j]             |s 从 i 到 j 的切片|
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
    从列表中获取一个子列表, 语法:[起始:结束:步长]包含头不包含尾,步长可以省略,默认值为1
    返回一个新列表,不影响原来的列表
    步长不能为0 可以是负数,如果是负数,会从列表后向前取元素

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
    items()
        返回由字典项 ((键, 值) 对) 组成的一个新视图。
        dict_items([('two', 2), ('one', 1), ('three', 3)])
    keys()
        返回由字典键组成的一个新视图。
        dict_keys(['two', 'one', 'three'])
    values()
        返回由字典值组成的一个新视图。
        dict_values([2, 1, 3])
    pop(key[, default])
        如果 key 存在于字典中则将其移除并返回其值，否则返回 default。 如果 default 未给出且 key 不存在于字典中，则会引发 KeyError。
    popitem()
        从字典中移除并返回一个 (键, 值) 对。 键值对会按 LIFO 的顺序被返回。
        popitem() 适用于对字典进行消耗性的迭代，这在集合算法中经常被使用。 如果字典为空，调用 popitem() 将引发 KeyError。
    setdefault(key[, default])
        如果字典存在键 key ，返回它的值。如果不存在，插入值为 default 的键 key ，并返回 default 。 default 默认为 None。
    update([other])
        使用来自 other 的键/值对更新字典，覆盖原有的键。 返回 None。

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
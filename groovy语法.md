# 1. Groovy Language Specification

## 1.1. 语法

### 1.1.1. 注释
    单行注释
    // a standalone single line comment

    多行注释
    /* a standalone multiline comment
        spanning two lines */

    文档注释
    /**
    * A Class description
    */
    class Person {
    
    }

### 1.1.2. 引号标识符

引号标识符出现在逗点表达式的逗点之后，例如，`person.name`可以写成`person."name"`或`person.'name'`。当标识符中有java非法字符而在groovy中是合法的时候这种写法很有用。

    def map = [:]

    map."an identifier with a space and double quotes" = "ALLOWED"
    map.'with-dash-signs-and-single-quotes' = "ALLOWED"

    assert map."an identifier with a space and double quotes" == "ALLOWED"
    assert map.'with-dash-signs-and-single-quotes' == "ALLOWED"

groovy中所有的字符常量表示形式都可以出现在逗点表达式之后

    map.'single quote'
    map."double quote"
    map.'''triple single quote'''
    map."""triple double quote"""
    map./slashy string/
    map.$/dollar slashy string/$

### 1.1.3. 字符串

在groovy中可以实例化`java.lang.String`字符串，也可以实例化`GString(groovy.lang.GString)`，又叫内差值字符串。   
groovy中所有的字符串都可以使用 **+** 号进行拼接

单引号字符串，java String类型，不支持内差值

    'a single quoted string'

双引号字符串  
如果没有内差值表达式，是`java.lang.String`类型，否则是`groovy.lang.GString`类型

    "a double quoted string"


三单引号字符串，可以直接写成多行，而不用像Java那样分成几部分拼接起来。   
java String类型，不支持内差值

    '''a triple single
     quoted 
     string'''

    //这样写第一个字符和最后一个字符是换行符
    def startingAndEndingWithANewline = '''
    line one
    line two
    line three
    '''

    //可以用‘\'转义换行符
    def strippedFirstNewline = '''\
    line one
    line two
    line three\
    '''

三双引号字符串  
和双引号字符串类似，唯一不同的是，可以直接写成多行

    def name = 'Groovy'
    def template = """
    Dear Mr ${name},
    Dave
    """

斜线字符串  
优势是只有`/`需要转义，其它字符不需要转义。可以写多行，支持内插值表达式

    def escapeSlash = /The character \/ is a forward slash/

$/字符串  
这种字符串转义字符是`$`,支持多行和内差值表达式

| String syntax | Interpolated | Multiline | Escape character |
| :-: | :-: | :-: | :-: |
| `'…​'` | |  | `\` |
| `'''…​'''` | | √ | `\` |
| `“…​”` | √ |  | `\` |
| `“““…​“““` | √ | √ | `\` |
| `/…​/` | √ | √ | `\` |
| `$/…​/$` | √ | √ | `$` |

转义字符
>同java，略

字符串内差值   
除了单引号和三单引号字符串外，其它字符串内可以插入任何groovy表达式  
The placeholder expressions are surrounded by ${} or prefixed with $ for dotted expressions.  
当这个GStirng被传递到一个需要 `String`参数的方法时，会对表达式求值然后调用`toString()`方法
>当一个方法需要`java.lang.String`参数，但传递的是`groovy.lang.GString`时，会自动调用`GString的toString()`方法

    def name = 'Guillaume' // a plain string
    def greeting = "Hello ${name}"

    assert greeting.toString() == 'Hello Guillaume'

    //任何表达式都是有效的，会先对表达式求值，然后调用toString()
    def sum = "The sum of 2 and 3 equals ${2 + 3}"
    assert sum.toString() == 'The sum of 2 and 3 equals 5'

不仅表达式可以放置在`${}`中，语句也可以。语句的结果是`null`,如果放置几条语句，最后一条应该返回一个有意义的值，例如

    "The sum of 1 and 2 is equal to ${def a = 1; def b = 2; a + b}"

除了`${}`外，也可以用`$逗点表达式`，这种只支持a.b, a.b.c的形式，不能带`()`,`{}`,`算数运算符`等

    def person = [name: 'Guillaume', age: 36]
    assert "$person.name is $person.age years old" == 'Guillaume is 36 years old'

    def number = 3.14
    println "$number.toString()" //错误写法，抛异常

    assert '${name}' == "\${name}"

内插闭包表达式  
当placeholder中有一个`->`时`${ -> }`,表达式是一个闭包表达式。

    //无参闭包
    def sParameterLessClosure = "1 + 2 == ${-> 3}" 
    assert sParameterLessClosure == '1 + 2 == 3'

    //闭包有一个java.io.StringWriter参数，可以使用<<操作符向其添加内容
    def sOneParamClosure = "1 + 2 == ${ w -> w << 3}" 
    assert sOneParamClosure == '1 + 2 == 3'

**闭包表达式和普通表达式相比有一个有点：懒加载**

    def number = 1 
    def eagerGString = "value == ${number}"
    def lazyGString = "value == ${ -> number }"

    assert eagerGString == "value == 1" 
    assert lazyGString ==  "value == 1" 

    number = 2 
    assert eagerGString == "value == 1" 
    assert lazyGString ==  "value == 2" 

`GString`和`String`的`hashcode`   
GString的hashcode不是固定的，取决于内差值。即使它所代表的字符串和String相等，它们的hashcode也不相等。

    assert "one: ${1}".hashCode() != "one: 1".hashCode()

因为GString和String有不同的hashcode，GString不能作为Map的key

    def key = "a"
    def m = ["${key}": "letter ${key}"]     

    assert m["a"] == null  

字符  
和java不同，groovy没有明确的字符字面值。可以显示的把groovy字符串声明为字符，有三种方法：

    char c1 = 'A' 
    assert c1 instanceof Character

    def c2 = 'B' as char 
    assert c2 instanceof Character

    def c3 = (char)'C' 
    assert c3 instanceof Character

### 1.1.4. 数字

#### 整数字面值
类型：`byte` `char` `short` `int` `long` `java.lang.BigInteger`   
可以使用具体类型声明整数

    // primitive types
    byte  b = 1
    char  c = 2
    short s = 3
    int   i = 4
    long  l = 5

    // infinite precision
    BigInteger bi =  6

如果使用`def`声明，具体类型会根据数据大小而改变

    def a = 1
    assert a instanceof Integer

    // Integer.MAX_VALUE
    def b = 2147483647
    assert b instanceof Integer

    // Integer.MAX_VALUE + 1
    def c = 2147483648
    assert c instanceof Long

    // Long.MAX_VALUE
    def d = 9223372036854775807
    assert d instanceof Long

    // Long.MAX_VALUE + 1
    def e = 9223372036854775808
    assert e instanceof BigInteger

二进制字面值，以`0b`作为前缀。  
八进制字面值，以`0`作为前缀  
十六进制字面值，以`0x`作为前缀

    int xInt = 0b10101111
    assert xInt == 175

    short xShort = 0b11001001
    assert xShort == 201 as short

    byte xByte = 0b11
    assert xByte == 3 as byte

    long xLong = 0b101101101101
    assert xLong == 2925l

#### 小数字面值
类型：`float` `double` `java.lang.BigDecimal`

为了方便阅读，字面值可以随意加`_`来分组

    long creditCardNumber = 1234_5678_9012_3456L
    long socialSecurityNumbers = 999_99_9999L
    double monetaryAmount = 12_345_132.12
    long hexBytes = 0xFF_EC_DE_5E
    long hexWords = 0xFFEC_DE5E
    long maxLong = 0x7fff_ffff_ffff_ffffL
    long alsoMaxLong = 9_223_372_036_854_775_807L
    long bytes = 0b11010010_01101001_10010100_10010010

### 1.1.5. 布尔值
同Java，`true`, `false`两个值

### 1.1.6. Lists
Groovy使用`[]`来定义集合，里面的值用`,`隔开。Groovy没有定义自己的集合类，使用的是Java的`java.util.List`，默认使用的是`java.util.ArrayList`。也可以使用`as`操作符制定具体的集合，还可以使用具体的实现来定义集合。

    def arrayList = [1, 2, 3]
    assert arrayList instanceof java.util.ArrayList

    def linkedList = [2, 3, 4] as LinkedList    
    assert linkedList instanceof java.util.LinkedList

    LinkedList otherLinked = [3, 4, 5]          
    assert otherLinked instanceof java.util.LinkedList

    def letters = ['a', 'b', 'c', 'd']

    assert letters[0] == 'a'     
    assert letters[1] == 'b'

    assert letters[-1] == 'd' //下表为负数时从尾部偏移    
    assert letters[-2] == 'c'

    letters[2] = 'C'             
    assert letters[2] == 'C'

    letters << 'e'  //使用<<操作符向集合添加元素      
    assert letters[ 4] == 'e'
    assert letters[-1] == 'e'


    //一次获取两个元素，返回包含两个元素的新集合 
    assert letters[1, 3] == ['b', 'd'] 
    //使用rang获取一定范围内的元素
    assert letters[2..4] == ['C', 'd', 'e']  

多维list

    def multi = [[0, 1], [2, 3]]     
    assert multi[1][0] == 2     

### 1.1.7. 数组
和声明集合语法类似，但是要制定具体的数组类型

    String[] arrStr = ['Ananas', 'Banana', 'Kiwi']  

    assert arrStr instanceof String[] 

    def numArr = [1, 2, 3] as int[]      

    assert numArr instanceof int[]

### 1.1.8. Maps
    //定义一个map
    def colors = [red:'#FF0000', green:'#00FF00', blue:'#0000FF']   

    assert colors['red'] == '#FF0000'    
    assert colors.green  == '#00FF00'    

    colors['pink'] = '#FF00FF'           
    colors.yellow  = '#FFFF00'           

    assert colors.pink == '#FF00FF'
    assert colors['yellow'] == '#FFFF00'

    assert colors instanceof java.util.LinkedHashMap

也可以用其它类型的数据作为key

    def numbers = [1: 'one', 2: 'two']
    assert numbers[1] == 'one'

变量作为key

    def key = 'name'
    def person = [key: 'Guillaume']      

    assert !person.containsKey('name')   
    assert person.containsKey('key')

    //要把变量值作为key，需要用()
    person = [(key): 'Guillaume']        

    assert person.containsKey('name')    
    assert !person.containsKey('key')    

## 1.2. 操作符
### 算数操作符
    +   -    *   /   %   **（幂）
    assert  2 ** 3 == 8

### 赋值运算符
    +=   -=   *=   /=   %=   **=

### 关系运算符
    >   >=   <   <=   ==   !=

### 逻辑运算符
    &&   ||   ！

### 位操作符
    &   |   ^   ~

### 三元操作符
    ? :
    displayName = user.name ? user.name : 'Anonymous'  
    //三元运算符简写 
    displayName = user.name ?: 'Anonymous'

### 对象操作符
#### 安全导航操作符(Safe navigation operator)
    ?. //避免空指针异常，返回null
    def person = null    
    def name = person?.name                   
    assert name == null

#### 字段直接获取操作符(Direct field access operator)
    class User {
        public final String name                 
        User(String name) { this.name = name}
        String getName() { "Name: $name" }       
    }
    def user = new User('Bob')
    //user.name调用getName()方法
    assert user.name == 'Name: Bob'
    //user.@name 直接访问name属性
    assert user.@name == 'Bob'

#### 方法指针操作符
方法指针的类型是`groovy.lang.Closure`,可以用在任何`Closure`出现的地方

    def str = 'example of method reference'  
    def fun = str.&toUpperCase                 
    def upper = fun()                          
    assert upper == str.toUpperCase() 

    //支持重载
    def doSomething(String str) { str.toUpperCase() }    
    def doSomething(Integer x) { 2*x }        
    def reference = this.&doSomething         
    assert reference('foo') == 'FOO'    
    assert reference(123)   == 246

### 正则表达式操作符
#### Pattern operator
The pattern operator (`~`) provides a simple way to create a java.util.regex.Pattern instance:

    def p = ~/foo/
    assert p instanceof Pattern

#### Find operator
Alternatively(二选一) to building a pattern, you can directly use the find operator `=~` to build a `java.util.regex.Matcher` instance:

    def text = "some text to match"
    def m = text =~ /match/          
    assert m instanceof Matcher                
    if (!m.find()) {                          
        throw new RuntimeException("Oops, text not found!")
    }

#### Match operator
The match operator (`==~`) is a slight variation of the find operator, that does not return a Matcher but a boolean and requires a strict match of the input string:

    m = text ==~ /match/                    
    assert m instanceof Boolean     
    if (m) {                
        throw new RuntimeException("Should not reach that point!")
    }

### 其它操作符
#### Spread operator
The Spread Operator (`*.`) is used to invoke an action on all items of an aggregate object. It is equivalent to calling the action on each item and collecting the result into a list:

    class Car {
        String make
        String model
    }
    def cars = [
        new Car(make: 'Peugeot', model: '508'),
        new Car(make: 'Renault', model: 'Clio')]       
    def makes = cars*.make                                
    assert makes == ['Peugeot', 'Renault']

    //类型安全，可以用在任何实现了Iterable接口的对象上
    cars = [new Car(make: 'Peugeot', model: '508'),null, new Car(make: 'Renault'，model: 'Clio')]
    assert cars*.make == ['Peugeot', null, 'Renault']     
    assert null*.make == null

Spreading method arguments

    int function(int x, int y, int z) {
        x*y+z
    }
    def args = [4,5,6]
    assert function(*args) == 26
    args = [4]
    assert function(*args,5,6) == 26 //和普通参数混用

Spread list elements

    def items = [4,5]                      
    def list = [1,2,3,*items,6]            
    assert list == [1,2,3,4,5,6] 

Spread map elements

    def m1 = [c:3, d:4]                   
    def map = [a:1, b:2, *:m1]            
    assert map == [a:1, b:2, c:3, d:4]    

#### 范围操作符
Ranges的实现是轻量级的，至存储了上下界，只要是`Comparable `并且有`next()` `previous()`方法，都可以创建rang

    def range = 0..5                 
    assert (0..5).collect() == [0, 1, 2, 3, 4, 5]       
    assert (0..<5).collect() == [0, 1, 2, 3, 4 
    assert (0..5) instanceof List              
    assert (0..5).size() == 6

### 比较操作符
The spaceship operator (`<=>`) delegates to the compareTo method:

    assert (1 <=> 1) == 0
    assert (1 <=> 2) == -1
    assert (2 <=> 1) == 1
    assert ('a' <=> 'z') == -1

### 下标操作符
下标操作符`[]`是`getAt` `putAt`的简写.

    def list = [0,1,2,3,4]
    assert list[2] == 2   
    list[2] = 4    
    assert list[0..2] == [0,1,4]  
    list[0..2] = [6,6,6]   
    assert list == [6,6,6,3,4]

    class User {
        Long id
        String name
        def getAt(int i) {                                             
            switch (i) {
                case 0: return id
                case 1: return name
            }
            throw new IllegalArgumentException("No such element $i")
        }
        void putAt(int i, def value) {                                 
            switch (i) {
                case 0: id = value; return
                case 1: name = value; return
            }
            throw new IllegalArgumentException("No such element $i")
        }
    }
    def user = new User(id: 1, name: 'Alex')                           
    assert user[0] == 1                                                
    assert user[1] == 'Alex'                                           
    user[1] = 'Bob'                                                    
    assert user.name == 'Bob'

#### 成员操作符
`in`等价于调用`isCase`方法，如果是`List`，等价于`contains`方法

    def list = ['Grace','Rob','Emmy']
    assert ('Emmy' in list) 

#### ==运算符
在Groovy中`==`运算符会调用`equals`方法，如果进比较引用可以使用`is`

    def list1 = ['Groovy 1.8','Groovy 2.0','Groovy 2.3']        
    def list2 = ['Groovy 1.8','Groovy 2.0','Groovy 2.3']        
    assert list1 == list2                                       
    assert !list1.is(list2)  

#### 强转操作符 as
    Integer x = 123
    String s = (String) x //ClassCastException
    String s1 = x as String  

当把一种类型转换成另一种类型时，除非两种类型相同，否则会生成一个新对象。转换实际上调用的 **`asType`** 方法

    class Identifiable {
        String name
    }
    class User {
        Long id
        String name
        def asType(Class target) {                                              
            if (target == Identifiable) {
                return new Identifiable(name: name)
            }
            throw new ClassCastException("User cannot be coerced into $target")
        }
    }
    def u = new User(name: 'Xavier')      
    def p = u as Identifiable           
    assert p instanceof Identifiable    
    assert !(p instanceof User)

#### call操作符
`()`操作符是`call`方法的简写。

    class MyCallable {
        int call(int x) {           
            2*x
        }
    }

    def mc = new MyCallable()
    assert mc.call(2) == 4          
    assert mc(2) == 4 

#### 操作符重载
    class Bucket {
        int size

        Bucket(int size) { this.size = size }

        Bucket plus(Bucket other) {   
            return new Bucket(this.size + other.size)
        }
    }

    def b1 = new Bucket(4)
    def b2 = new Bucket(11)
    assert (b1 + b2).size == 15

操作符重载表
| Operator | Method | Operator | Method |
| :-: | :-: | :-: | :-: |
| `+` |a.plus(b)|`a[b]`|a.getAt(b)|
| `-` |a.minus(b)|`a[b] = c`|a.putAt(b, c)|
| `*` |a.multiply(b)|`a in b`|b.isCase(a)|
| `/` |a.div(b)|`<<`|a.leftShift(b)|
| `%` |a.mod(b)|`>>`|a.rightShift(b)|
| `**` |a.power(b)|`>>>`|a.rightShiftUnsigned(b)|
| `|` |a.or(b)|`++`|a.next()|
| `&` |a.and(b)|`--`|a.previous()|
| `^` |a.xor(b)|`+a`|a.positive()|
| `as` |a.asType(b)|`-a`|a.negative()|
| `a()` |a.call()|`~a`|a.bitwiseNegate()|

## 1.3. Program structure
### 1.3.1. 包名，Imports
    同Java
    package com.yoursite
    import groovy.xml.MarkupBuilder

    //groovy默认引入的包
    import java.lang.*
    import java.util.*
    import java.io.*
    import java.net.*
    import groovy.lang.*
    import groovy.util.*
    import java.math.BigInteger
    import java.math.BigDecimal

    //别名
    import java.util.Date
    import java.sql.Date as SQLDate

### 1.3.2. Scripts vs classes
    //类
    class Main {                                    
        static void main(String... args) {          
            println 'Groovy world!'                 
        }
    }

    //script 
    println 'Groovy world!'

>一个script会被编译成一个class。Groovy编译器会把执行语句复制到run方法里，script中定义的方法也会复制到生成的class中

    println 'Hello'   
    int power(int n) { 2**n }    
    println "2^6==${power(6)}"  

    //上面的脚本生成的class
    import org.codehaus.groovy.runtime.InvokerHelper
    class Main extends Script {
        int power(int n) { 2** n}                   
        def run() {
            println 'Hello'                         
            println "2^6==${power(6)}"              
        }
        static void main(String[] args) {
            InvokerHelper.runScript(Main, args)
        }
    }

script中变量的定义可以不需要类型

    int x = 1
    int y = 2
    assert x+y == 3

    x = 1
    y = 2
    assert x+y == 3

上面个两种写法是不同的：  
第一种是局部变量，会定义在生成的`run`方法中，其它方法不可见  
第二种：if the variable is undeclared, it goes into the script binding. The binding is visible from the methods, and is especially important if you use a script to interact with an application and need to share data between the script and the application. Readers might refer to the integration guide for more information.

>If you want a variable to become a field of the class without going into the Binding, you can use the @Field annotation.

## 1.4. Object orientation
### 1.4.1. 类型
#### Primitive types
Groovy支持和Java一样的原始类型。但Groovy中一切都是对象，原始类型会自动包装，使用对应的包装类型

|Primitive type|Wrapper class|
|:-:|:-:|
|boolean|Boolean|
|char|Character|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|

    class Foo {
        static int i
    }

    assert Foo.class.getDeclaredField('i').type == int.class
    assert Foo.i.class != int.class && Foo.i.class == Integer.class

#### Class

Groovy中class和Java中的class类似，主要有以下几个不同点：  
* `public`字段会自动变成属性，这样可以不用写`getter` `setter`方法，减少冗余代码
* 方法和属性默认都是`public`的
* 类名可以不和文件名相同
* 一个文件可以包含一个或多个`class`，如果没有`class`，会被当成`script`

#### Interface
    interface Greeter {    
        void greet(String name)         
    }

    class DefaultGreeter {
        void greet(String name) { println "Hello" }
    }

    //使用as操作符可以在运行时让一个对象实现某个接口
    greeter = new DefaultGreeter()  
    assert !(coerced instanceof Greeter)        
    coerced = greeter as Greeter      
    assert coerced instanceof Greeter

#### Constructors
    class PersonConstructor {
        String name
        Integer age

        PersonConstructor(name, age) {          
            this.name = name
            this.age = age
        }
    }

    //使用构造方法的三中形式
    def person1 = new PersonConstructor('Marie', 1)  
    def person2 = ['Marie', 2] as PersonConstructor  
    PersonConstructor person3 = ['Marie', 3]

**如果没有声明构造方法，可以传递`map`形式的参数来实例化对象。这种形式比传统的构造方法更加灵活**

    class PersonWOConstructor {    
        String name
        Integer age
    }

    def person4 = new PersonWOConstructor()                      
    def person5 = new PersonWOConstructor(name: 'Marie')         
    def person6 = new PersonWOConstructor(age: 1)                
    def person7 = new PersonWOConstructor(name: 'Marie', age: 2)

#### Methods
一个方法要定义一个返回类型或者使用`def`指定返回类型为untyped,方法参数类型可以不指定。  
在Groovy中，方法总是有返回值的，如果没有`return`语句，会返回最后一个表达式的值

    def someMethod() { 'method called' }   
    String anotherMethod() { 'another method called' }   
    def thirdMethod(param1) { "$param1 passed" }     
    static String fourthMethod(String param1) { "$param1 passed" } 
Named arguments

    //命名参数要用map来接收，方法内通过map.key获取参数值
    def foo(Map args) { "${args.name}: ${args.age}" }
    foo(name: 'Marie', age: 1)

Default arguments

    def foo(String par1, Integer par2 = 1) { [name: par1, age: par2] }
    assert foo('Marie').age == 1

Varargs

    def foo(Object... args) { args.length }
    assert foo() == 0
    assert foo(1) == 1
    assert foo(1, 2) == 2

>Groovy allows T[] as a alternative notation to T…​. That means any method with an array as last parameter is seen by Groovy as a method that can take a variable number of arguments.

    //Groovy把可变参数当成数组，如果参数是数组也可当成可变参数来用
    def foo(Object[] args) { args.length }
    assert foo() == 0
    assert foo(1) == 1
    assert foo(1, 2) == 2

可变参数方法重载，会选择最匹配的方法

    def foo(Object... args) { 1 }
    def foo(Object x) { 2 }
    assert foo() == 1
    assert foo(1) == 2
    assert foo(1, 2) == 1

#### Fields and properties
Fields 同Java，不同的是类型可以省略，定义方法：  
* 一个必选的访问权限(`pubic/private/protected`)
* 可选的类型
* 一个名字

```
    class BadPractice {
        private mapping //省略类型不是一个好的写法，应该使用强类型
    }
```

Properties  
属性是一个`private`字段和`getter/setter`的组合,定义方法：  
* 缺省的访问权限(没有`public/private/protected`)
* 可选的类型
* 一个名字

```
    class Person {
        String name //creates a backing private String name field, a getName and a setName method                           
        int age   //creates a backing private int age field, a getAge and a setAge method                              
    }
```

通过名字获取属性实际上是访问的`getter/setter`方法，在类的内部是直接访问(这是为了防止栈溢出)。

    class Person {
        String name
        void name(String name) {
            this.name = "Wonder$name" //直接访问      
        }
    }
    def p = new Person()
    p.name = 'Marge'            //setter方法                    
    assert p.name == 'Marge'    //getter方法  

可以使用隐含的`properties`字段列出一个class的所有属性

    class Person {
        String name
        int age
    }
    def p = new Person()
    assert p.properties.keySet().containsAll(['name','age'])

只要有相关的`getter/setter`方法，就可以用属性名访问，即使类中没有定义该属性

    class PseudoProperties {
        // a pseudo property "name"
        void setName(String name) {}
        String getName() {}

        // a pseudo read-only property "age"
        int getAge() { 42 }

        // a pseudo write-only property "groovy"
        void setGroovy(boolean groovy) {  }
    }
    def p = new PseudoProperties()
    p.name = 'Foo'                      
    assert p.age == 42                  
    p.groovy = true

### 1.4.2. Traits
略

## 1.5. 闭包(Closures)
闭包是开放，匿名的代码块，可以有参数，返回值，赋值给一个变量。闭包和方法的区别是闭包总是有返回值 

### 1.5.1. 语法 

闭包定义语法： `[closureParameters -> ] statements }`  
`[closureParameters->]`是可选的逗号分隔的参数列表。有参数时`->`是必须的，不能省略。`statements`是0，1或多个groovy语句。

    { item++ }        //无参闭包 

    { -> item++ }     //无参闭包   

    { println it }    //默认参数           

    { it -> println it }      //声明参数   

    { name -> println name }  //声明参数     

    { String x, int y ->                                
        println "hey ${x} the value is ${y}"  //多个参数
    }

    { reader ->                                         
        def line = reader.readLine()        //多条语句
        line.trim()
    }
一个闭包是类`groovy.lang.Closure`的一个实例.可以赋值给变量

    def listener = { e -> println "Clicked on $e.source" }  
    assert listener instanceof Closure

    Closure callback = { println 'Done!' }                      
    Closure<Boolean> isTextFile = {//可以使用泛型指定闭包的返回值
        File it -> it.name.endsWith('.txt')                     
    }

闭包可以向方法一样调用,也可以调用闭包的`call()`方法:

    def isOdd = { int i -> i%2 != 0 }                           
    assert isOdd(3) == true                                     
    assert isOdd.call(2) == false                          

    def isEven = { it%2 == 0 }                                  
    assert isEven(3) == false                                   
    assert isEven.call(2) == true

### 1.5.2. 参数
#### 正常参数
正常参数和方法参数一样 :可选的类型,名字,可选的默认值

    def closureWithTwoArgsAndOptionalTypes = { a,int b -> a+b }
    assert closureWithTwoArgsAndOptionalTypes(1,2) == 3

    def closureWithTwoArgAndDefaultValue = { int a, int b=2 -> a+b }
    assert closureWithTwoArgAndDefaultValue(1) == 3

#### 默认参数
当一个闭包没有声明参数,并且没有使用`->`,闭包会有一个默认参数`it`  
没有声明参数,但是用了`->`,声明闭包是无参闭包,调用时不能传参.  

    def magicNumber = { -> 42 }

    // this call will fail because the closure doesn't accept any argument
    magicNumber(11)

#### 可变参数
闭包可以声明可变参数 

    def concat1 = { String... args -> args.join('') } 
    assert concat1('abc','def') == 'abcdef'  
    def concat2 = { String[] args -> args.join('') }    
    assert concat2('abc', 'def') == 'abcdef'

### 1.5.3. 代理策略
#### this
闭包中`this`指的是**离闭包最近**的**enclosing class**的对象  

    class EnclosedInInnerClass {
        class Inner {
            Closure cl = { this }                               
        }
        void run() {
            def inner = new Inner()
            assert inner.cl() == inner                          
        }
    }
    class NestedClosures {
        void run() {
            def nestedClosures = {
                def cl = { this }                               
                cl()
            }
            assert nestedClosures() == this                     
        }
    }

#### owner
闭包的`owner`指的是离闭包最近的enclosing对象,可能是闭包或class对象   

    class EnclosedInInnerClass {
        class Inner {
            Closure cl = { owner }                               
        }
        void run() {
            def inner = new Inner()
            assert inner.cl() == inner                           
        }
    }
    class NestedClosures {
        void run() {
            def nestedClosures = {
                def cl = { owner }                               
                cl()
            }
            assert nestedClosures() == nestedClosures            
        }
    }

#### delegate
不像`this owner`,`delegate`是用户定义的对象,默认设置成`owner` 

    class Enclosing {
        void run() {
            def cl = { getDelegate() }  
            def cl2 = { delegate }     
            assert cl() == cl2()    
            assert cl() == this    
            def enclosed = {
                { -> delegate }.call()    
            }
            assert enclosed() == enclosed    
        }
    }

`delegate`可以动态设置成任何对象  
 
    class Person {
        String name
    }
    class Thing {
        String name
    }

    def p = new Person(name: 'Norman')
    def t = new Thing(name: 'Teapot')

    def upperCasedName = { delegate.name.toUpperCase() }

    upperCasedName.delegate = p
    assert upperCasedName() == 'NORMAN'
    upperCasedName.delegate = t
    assert upperCasedName() == 'TEAPOT'

#### 代理策略  

- Closure.OWNER_FIRST is the default strategy. If a property/method exists on the owner, then it will be called on the owner. If not, then the delegate is used.

- Closure.DELEGATE_FIRST reverses the logic: the delegate is used first, then the owner

- Closure.OWNER_ONLY will only resolve the property/method lookup on the owner: the delegate will be ignored.

- Closure.DELEGATE_ONLY will only resolve the property/method lookup on the delegate: the owner will be ignored.

- Closure.TO_SELF can be used by developers who need advanced meta-programming techniques and wish to implement a custom resolution strategy: the resolution will not be made on the owner or the delegate but only on the closure class itself. It makes only sense to use this if you implement your own subclass of Closure.   

        class Person {
            String name
            def pretty = { "My name is $name" }             
            String toString() {
                pretty()
            }
        }
        class Thing {
            String name                                     
        }

        def p = new Person(name: 'Sarah')
        def t = new Thing(name: 'Teapot')

        assert p.toString() == 'My name is Sarah' 
        p.pretty.delegate = t      
        assert p.toString() == 'My name is Sarah'//owner中有会优先用owner中的属性

        //改变代理策略
        p.pretty.resolveStrategy = Closure.DELEGATE_FIRST
        assert p.toString() == 'My name is Teapot'

#### Closures in GStrings  
    def x = 1   //x是一个对象,并不是原始类型
    def gs = "x = ${x}"  //GString创建时引用了对象x,值为1
    assert gs == 'x = 1'

    x = 2
    assert gs == 'x = 1'  //x变成了新对象,GString仍然引用的旧值

    //因为Groovy中一切都是对象
    class Person {
        String name
        String toString() { name }          
    }
    def sam = new Person(name:'Sam')        
    def lucy = new Person(name:'Lucy')      
    def p = sam                             
    def gs = "Name: ${p}"                   
    assert gs == 'Name: Sam'                
    p = lucy                                
    assert gs == 'Name: Sam'                
    sam.name = 'Lucy'                       
    assert gs == 'Name: Lucy'

    //使用闭包,懒加载求值
    class Person {
        String name
        String toString() { name }
    }
    def sam = new Person(name:'Sam')
    def lucy = new Person(name:'Lucy')
    def p = sam
    // Create a GString with lazy evaluation of "p"
    def gs = "Name: ${-> p}"
    assert gs == 'Name: Sam'
    p = lucy
    assert gs == 'Name: Lucy'

## 1.6. Semantics
### 1.6.1. Statements
#### 变量定义
定义变量时可以使用具体类型,或者使用`def`不指定具体类型  
多个变量可以同时赋值  

    def (a, b, c) = [10, 20, 'foo']
    assert a == 10 && b == 20 && c == 'foo'

    //可以指明类型
    def (int i, String j) = [10, 'foo']
    assert i == 10 && j == 'foo'
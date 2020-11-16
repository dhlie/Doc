### 包
目录与包的结构无需匹配：源代码可以在文件系统的任意位置。（java中不允许）

### 基本类型

在 Kotlin 中，所有东西都是对象，在这个意义上讲我们可以在任何变量上调用成员函数与属性。

Kotlin 中使用的基本类型：数字、字符、布尔值、数组与字符串

##### 数字

###### 整数

| 类型   | 大小（比特数） | 最小值                     | 最大值                    |
| :----- | -------------- | -------------------------- | ------------------------- |
| Byte   | 8              | -128                       | 127                       |
| Short  | 16             | -32768                     | 32767                     |
| Int    | 32             | -2,147,483,648             | 2,147,483,647             |
| Long   | 64             | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| UByte  | 8              | 0                          | 255                       |
| UShort | 16             | 0                          | 65535                     |
| UInt   | 32             | 0                          | 2^32 - 1                  |
| ULong  | 64             | 0                          | 2^64 - 1                  |

所有以未超出 `Int` 最大值的整型值初始化的变量都会推断为 `Int` 类型。如果初始值超过了其最大值，那么推断为 `Long` 类型。 如需显式指定 `Long` 型值，请在该值后追加 `L` 后缀。

```
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

###### 浮点数

| 类型 | 大小（比特数） | 十进制位数 |
| -------- | ------------------ | -------- |
| Float    | 32                 | 6-7 |
| Double   | 64                 | 15-16 |

对于以小数初始化的变量，编译器会推断为 `Double` 类型。 如需将一个值显式指定为 `Float` 类型，请添加 `f` 或 `F` 后缀。 如果这样的值包含多于 6～7 位十进制数，那么会将其舍入。

```
val pi = 3.14 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float，实际值为 2.7182817
```

请注意，与一些其他语言不同，Kotlin 中的数字没有隐式拓宽转换。 例如，具有 `Double` 参数的函数只能对 `Double` 值调用，而不能对 `Float`、 `Int` 或者其他数字值调用。如需将数值转换为不同的类型，请使用显示转换。

数字字面值：

```
123		十进制
123L	Long 类型
0x7f	十六进制
0b010	二进制 **不支持八进制**
123.1	Double
1.2f 1.2F Float
val creditCardNumber = 1234_5678_9012_3456L	 可以加下划线方便阅读
123u 123U 无符号数
```

数字都是  `Number` 的子类，不同类型可以互相转换

```
public abstract class Number {
    public abstract fun toDouble(): Double

    public abstract fun toFloat(): Float

    public abstract fun toLong(): Long

    public abstract fun toInt(): Int

    public abstract fun toChar(): Char

    public abstract fun toShort(): Short

    public abstract fun toByte(): Byte
}
```

位运算

对于位运算，没有特殊字符来表示，而只可用中缀方式调用具名函数

这是完整的位运算列表（只用于 `Int` 与 `Long`）：

- `shl(bits)` – 有符号左移
- `shr(bits)` – 有符号右移
- `ushr(bits)` – 无符号右移
- `and(bits)` – 位**与**
- `or(bits)` – 位**或**
- `xor(bits)` – 位**异或**
- `inv()` – 位非



##### 字符

字符用 `Char` 类型表示，字面值用单引号括起来: `'1'`。它们不能直接当作数字

特殊字符可以用反斜杠转义。 支持这几个转义序列：`\t`、 `\b`、`\n`、`\r`、`\'`、`\"`、`\\` 与 `\$`。 编码其他字符要用 Unicode 转义序列语法：`'\uFF00'`。

```
fun decimalDigitValue(c: Char): Int {
    if (c !in '0'..'9')
        throw IllegalArgumentException("Out of range")
    return c.toInt() - '0'.toInt() // 显式转换为数字
}
```

###### 布尔

布尔用 `Boolean` 类型表示，它有两个值：*true* 与 *false*。

内置的布尔运算有：

- `||` – 短路逻辑或
- `&&` – 短路逻辑与
- `!` - 逻辑非

###### 数组

数组在 Kotlin 中使用 `Array` 类来表示，它定义了 `get` 与 `set` 函数（按照运算符重载约定这会转变为 `[]`）以及 `size` 属性，以及一些其他有用的成员函数：

```
public class Array<T> {
    public inline constructor(size: Int, init: (Int) -> T)

    public operator fun get(index: Int): T

    public operator fun set(index: Int, value: T): Unit

    public val size: Int

    public operator fun iterator(): Iterator<T>
}
```

原生类型数组

`ByteArray` `CharArray` `ShortArray` `IntArray` `LongArray` `FloatArray` `DoubleArray` `BooleanArray` `UByteArray` `UShort|Int|LongArray`

生成的数组为 `byte[]`  `char[]`  `short[]` `int[]`  `long[]`  `float[]` `double[]` `boolean[]` 不会像 `Array` 的泛型一样自动装箱

##### 字符串

字符串用 `String` 类型表示。字符串是不可变的。 字符串的元素——字符可以使用索引运算符访问: `s[i]`。 可以用 *for* 循环迭代字符串:

```
for (c in str) {
    println(c)
}
```

可以用 `+` 操作符连接字符串, ***任何类型都可以和字符串相 +， 只要表达式中的第一个元素是字符串***

```
data class Pointer(val x: Int, var y: Int)
var str = "adad " + Pointer(1, 2)		//adad Pointer(x=1, y=2)
```

字符串字面值

Kotlin 有两种类型的字符串字面值: 转义字符串可以有转义字符， 以及原始字符串可以包含换行以及任意文本

```
val s = "Hello, world!\n"	//转义字符串
val text = """						//原始字符串
    for (c in "foo")
        print(c)
"""
```

字符串模板

模板表达式以美元符（`$`）开头，由一个简单的名字构成，或者用花括号括起来的任意表达式：原始字符串与转义字符串内部都支持模板



#### 控制流 if、when、for、while

###### If 表达式

在 Kotlin 中，*if* 是一个表达式，即它会返回一个值。 因此就不需要三元运算符（条件 ? 然后 : 否则），因为普通的 *if* 就能胜任这个角色。

###### When 表达式

*when* 表达式取代了类 C 语言的 switch 语句

```
when (x) {
    1 -> print("x == 1")
    2,3,4 -> print("x > 2")
    x is Boolean -> print("x is Boolean")		//可以用任意表达式（而不只是常量）作为分支条件
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}
```

*when* 将它的参数与所有的分支条件顺序比较，直到某个分支满足条件, 可以用任意表达式（而不只是常量）作为分支条件

*when* 也可以用来取代 *if*-*else* *if*链。 如果不提供参数，所有的分支条件都是简单的布尔表达式，而当一个分支的条件为真时则执行该分支：

```
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is even.")
}
```

###### For 循环

*for* 循环可以对任何提供迭代器（iterator）的对象进行遍历, 语法：

```
for (item in collection) print(item)
```

*for* 可以循环遍历任何提供了迭代器的对象。即：

- 有一个成员函数或者扩展函数 `iterator()` ，它的返回类型
  - 有一个成员函数或者扩展函数 `next()`，并且
  - 有一个成员函数或者扩展函数 `hasNext()` 返回 `Boolean`。

这三个函数都需要标记为 `operator`。

如需在数字区间上迭代，请使用区间表达式:

```
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
```

如果你想要通过索引遍历一个数组或者一个 list，你可以这么做：

```
for (i in array.indices) {
    println(array[i])
}
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

###### While 循环

*while* 与 *do*..*while* 照常使用, 在循环中 Kotlin 支持传统的 *break* 与 *continue* 操作符



#### 返回和跳转

###### Break 与 Continue 标签

在 Kotlin 中任何表达式都可以用标签（*label*）来标记。 标签的格式为标识符后跟 `@` 符号，例如：

```
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}
```

###### 返回到标签

return 会返回到最直接包围它的函数（不包含 lambda 表达式）

```
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // 非局部直接返回到 foo() 的调用者
        print(it)
    }
    println("this point is unreachable")
}
```

这个 *return* 表达式从最直接包围它的函数即 `foo` 中返回。 （注意，这种非局部的返回只支持传给[内联函数](https://www.kotlincn.net/docs/reference/inline-functions.html)的 lambda 表达式。） 如果我们需要从 lambda 表达式中返回，我们必须给它加标签并用以限制 *return*。

```
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // 局部返回到该 lambda 表达式的调用者，即 forEach 循环
        print(it)
    }
    print(" done with explicit label")
}
```

现在，它只会从 lambda 表达式中返回。通常情况下使用隐式标签更方便。 该标签与接受该 lambda 的函数同名。

```
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // 局部返回到该 lambda 表达式的调用者，即 forEach 循环
        print(it)
    }
    print(" done with implicit label")
}
```

或者，我们用一个匿名函数替代 lambda 表达式。 匿名函数内部的 *return* 语句将从该匿名函数自身返回

```
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return  // 局部返回到匿名函数的调用者，即 forEach 循环
        print(value)
    })
    print(" done with anonymous function")
}
```



#### 类与继承

##### 类

类声明由类名、类头（指定其类型参数、主构造函数等）以及由花括号包围的类体构成。类头与类体都是可选的； 如果一个类没有类体，可以省略花括号。

```
class Empty
```

###### 构造函数

一个类可以有一个**主构造函数**以及一个或多个**次构造函数**。主构造函数是类头的一部分

```
class Person constructor(firstName: String) { /*……*/ }
//如果主构造函数没有任何注解或者可见性修饰符，可以省略 constructor 关键字
class Person(firstName: String) { /*……*/ } 
//constructor 不能省略
class Customer public @Inject constructor(name: String) { /*……*/ } 
```

主构造函数不能包含任何的代码。初始化的代码可以放到以 *init* 关键字作为前缀的**初始化块（initializer blocks）**中, 初始化块可以有多个，初始化块和字段会按顺序执行初始化

```
    init {
        println("init111")
        //println(s)//报错，s 未初始化
    }

    var s = "filed s".also(::println)

    init {
        println("init222")
    }
```

主构造的参数会作为类的属性，提供 get/set 方法（var 属性）或 get 方法（val 属性）

次构造函数参数不会作为类属性

###### 次构造函数

如果类有一个主构造函数，每个次构造函数需要委托给主构造函数， 可以直接委托或者通过别的次构造函数间接委托。委托到同一个类的另一个构造函数用 **this** 关键字即可：

```
class View(val context: String) {

    constructor(context: String, attr: String): this(context, attr, 0) {}

    constructor(context: String, attr: String, theme: Int): this(context) {}

}
```

***初始化块中的代码和字段初始化代码实际上会成为主构造函数的一部分***。委托给主构造函数会作为次构造函数的第一条语句，因此所有初始化块与属性初始化器中的代码都会在次构造函数体之前执行。***即使该类没有主构造函数，这种委托仍会隐式发生，并且仍会执行初始化块***

如果一个非抽象类没有声明任何（主或次）构造函数，它会有一个生成的不带参数的主构造函数。构造函数的可见性是 public。如果你不希望你的类有一个公有构造函数，你需要声明一个带有非默认可见性的空的主构造函数：

```
class DontCreateMe private constructor () { /*……*/ }
```

要创建一个类的实例，我们就像普通函数一样调用构造函数, Kotlin 并没有 *new* 关键字

##### 继承

在 Kotlin 中所有类都有一个共同的超类 `Any`

`Any` 有三个方法：`equals()`、 `hashCode()` 与 `toString()`

默认情况下，Kotlin 类是最终（final）的：它们不能被继承。 要使一个类可继承，请用 `open` 关键字标记它。

```
open class Base // 该类开放继承
```

如果派生类有一个主构造函数，其基类必须就地初始化。

如果派生类没有主构造函数，那么每个次构造函数必须使用 *super* 关键字初始化其基类型，或委托给另一个构造函数做到这一点

```
class TextView(text: String) : View("context", "attr") {//必须调基类构造函数
    constructor(context: String, attr: String): this("123")//必须委托给主构造函数
}

class MyView : View {
    constructor(ctx: Context) : super(ctx)
}
```

###### 覆盖方法

Kotlin 对于可覆盖的成员（我们称之为*开放*）以及覆盖后的成员需要显式修饰符：

```
open class Shape {
    open fun draw() { /*……*/ }
    fun fill() { /*……*/ }
}

class Circle() : Shape() {
    override fun draw() { /*……*/ }
}
```

将 *open* 修饰符添加到 final 类（即没有 *open* 的类）的成员上不起作用。

标记为 *override* 的成员本身是开放的，可以在子类中覆盖。如果你想禁止再次覆盖，使用 *final* 关键字：

```
open class Rectangle() : Shape() {
    final override fun draw() { /*……*/ }
}
```

###### 覆盖属性

属性覆盖与方法覆盖类似; 在超类中声明然后在派生类中重新声明的属性必须以 *override* 开头，并且它们必须具有兼容的类型。

用一个 `var` 属性覆盖一个 `val` 属性，但反之则不行。 这是允许的，因为一个 `val` 属性本质上声明了一个 `get` 方法， 而将其覆盖为 `var` 只是在子类中额外声明一个 `set` 方法。

可以在主构造函数中使用 *override* 关键字作为属性声明的一部分。

```
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // 总是有 4 个顶点

class Polygon : Shape {
    override var vertexCount: Int = 0  // 以后可以设置为任何数
}
```

###### 派生类初始化顺序

在构造派生类的新实例的过程中，第一步完成其基类的初始化（在之前只有对基类构造函数参数的求值）

先对基类构造函数参数求职 ---> 基类初始化 ---> 派生类初始化

这意味着，基类构造函数执行时，派生类中声明或覆盖的属性都还没有初始化。如果在基类初始化逻辑中（直接或通过另一个覆盖的 *open* 成员的实现间接）使用了任何一个这种属性，那么都可能导致不正确的行为或运行时故障

###### 调用超类实现

派生类中的代码可以使用 *super* 关键字调用其超类的函数与属性访问器的实现：

```
class FilledRectangle: Rectangle() {
    override fun draw() {
        super.draw()
        println("Filling the rectangle")
    }
    val borderColor: String get() = "black"
    
    inner class Filler {
        fun fill() { /* …… */ }
        fun drawAndFill() {
            super@FilledRectangle.draw() // 调用 Rectangle 的 draw() 实现
            fill()
            println("Drawn a filled rectangle with color ${super@FilledRectangle.borderColor}") // 使用 Rectangle 所实现的 borderColor 的 get()
        }
    }
```

###### 覆盖规则

如果一个类从它的直接超类继承相同成员的多个实现， 它必须覆盖这个成员并提供其自己的实现（也许用继承来的其中之一）。 为了表示采用从哪个超类型继承的实现，我们使用由尖括号中超类型名限定的 *super*，如 `super<Base>`：

```
open class Rectangle {
    open fun draw() { /* …… */ }
}

interface Polygon {
    fun draw() { /* …… */ } // 接口成员默认就是“open”的
}

class Square() : Rectangle(), Polygon {
    // 编译器要求覆盖 draw()：
    override fun draw() {
        super<Rectangle>.draw() // 调用 Rectangle.draw()
        super<Polygon>.draw() // 调用 Polygon.draw()
    }
}
```

##### 抽象类

不需要用 `open` 标注一个抽象类或者函数——因为这不言而喻

可以用一个抽象成员覆盖一个非抽象的开放成员

```
open class Polygon {
    open fun draw() {}
}

abstract class Rectangle : Polygon() {
    abstract override fun draw()
}
```

##### 伴生对象


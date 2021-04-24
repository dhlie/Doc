### 包

目录与包的结构无需匹配：源代码可以在文件系统的任意位置。（java中不允许）

### 可见性修饰符

在 Kotlin 中有这四个可见性修饰符：`private`、 `protected`、 `internal` 和 `public`。 如果没有显式指定修饰符的话，默认可见性是 `public`。

getter 总是与属性有着相同的可见性. 局部变量、函数和类不能有可见性修饰符。

可见性修饰符 `internal` 意味着该成员只在相同模块内可见。更具体地说， 一个模块是编译在一起的一套 Kotlin 文件：

- 一个 IntelliJ IDEA 模块；
- 一个 Maven 项目；
- 一个 Gradle 源集（例外是 `test` 源集可以访问 `main` 的 internal 声明）；
- 一次 `<kotlinc>` Ant 任务执行所编译的一套文件。

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

```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

###### 浮点数

| 类型   | 大小（比特数） | 十进制位数 |
| ------ | -------------- | ---------- |
| Float  | 32             | 6-7        |
| Double | 64             | 15-16      |

对于以小数初始化的变量，编译器会推断为 `Double` 类型。 如需将一个值显式指定为 `Float` 类型，请添加 `f` 或 `F` 后缀。 如果这样的值包含多于 6～7 位十进制数，那么会将其舍入。

```kotlin
val pi = 3.14 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float，实际值为 2.7182817
```

请注意，与一些其他语言不同，Kotlin 中的数字没有隐式拓宽转换。 例如，具有 `Double` 参数的函数只能对 `Double` 值调用，而不能对 `Float`、 `Int` 或者其他数字值调用。如需将数值转换为不同的类型，请使用显示转换。

数字字面值：

```kotlin
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

```kotlin
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

```kotlin
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

```kotlin
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

```kotlin
for (c in str) {
    println(c)
}
```

可以用 `+` 操作符连接字符串, ***任何类型都可以和字符串相 +， 只要表达式中的第一个元素是字符串***

```kotlin
data class Pointer(val x: Int, var y: Int)
var str = "adad " + Pointer(1, 2)		//adad Pointer(x=1, y=2)
```

字符串字面值

Kotlin 有两种类型的字符串字面值: 转义字符串可以有转义字符， 以及原始字符串可以包含换行以及任意文本

```kotlin
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

```kotlin
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

```kotlin
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is even.")
}
```

###### For 循环

*for* 循环可以对任何提供迭代器（iterator）的对象进行遍历, 语法：

```kotlin
for (item in collection) print(item)
```

*for* 可以循环遍历任何提供了迭代器的对象。即：

- 有一个成员函数或者扩展函数 `iterator()` ，它的返回类型
  - 有一个成员函数或者扩展函数 `next()`，并且
  - 有一个成员函数或者扩展函数 `hasNext()` 返回 `Boolean`。

这三个函数都需要标记为 `operator`。

如需在数字区间上迭代，请使用区间表达式:

```kotlin
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
```

如果你想要通过索引遍历一个数组或者一个 list，你可以这么做：

```kotlin
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

```kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}
```

###### 返回到标签

return 会返回到最直接包围它的函数（不包含 lambda 表达式）

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // 非局部直接返回到 foo() 的调用者
        print(it)
    }
    println("this point is unreachable")
}
```

这个 *return* 表达式从最直接包围它的函数即 `foo` 中返回。 （注意，这种非局部的返回只支持传给[内联函数](https://www.kotlincn.net/docs/reference/inline-functions.html)的 lambda 表达式。） 如果我们需要从 lambda 表达式中返回，我们必须给它加标签并用以限制 *return*。

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // 局部返回到该 lambda 表达式的调用者，即 forEach 循环
        print(it)
    }
    print(" done with explicit label")
}
```

现在，它只会从 lambda 表达式中返回。通常情况下使用隐式标签更方便。 该标签与接受该 lambda 的函数同名。

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // 局部返回到该 lambda 表达式的调用者，即 forEach 循环
        print(it)
    }
    print(" done with implicit label")
}
```

或者，我们用一个匿名函数替代 lambda 表达式。 匿名函数内部的 *return* 语句将从该匿名函数自身返回

```kotlin
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

```kotlin
class Empty
```

###### 构造函数

一个类可以有一个**主构造函数**以及一个或多个**次构造函数**。主构造函数是类头的一部分

```kotlin
class Person constructor(firstName: String) { /*……*/ }
//如果主构造函数没有任何注解或者可见性修饰符，可以省略 constructor 关键字
class Person(firstName: String) { /*……*/ } 
//constructor 不能省略
class Customer public @Inject constructor(name: String) { /*……*/ } 
```

主构造函数不能包含任何的代码。初始化的代码可以放到以 *init* 关键字作为前缀的**初始化块（initializer blocks）**中, 初始化块可以有多个，初始化块和字段会按顺序执行初始化

```kotlin
    init {
        println("init111")
        //println(s)//报错，s 未初始化
    }

    var s = "filed s".also(::println)

    init {
        println("init222")
    }
```

主构造的参数可以在初始化块中使用。它们也可以在类体内声明的属性初始化器中使用:

主构造的参数如果有`var/val`修饰符会作为类的属性，提供 get/set 方法（var 属性）或 get 方法（val 属性）

次构造函数参数不能带`var/val`修饰符, 不会作为类属性

###### 次构造函数

如果类有一个主构造函数，每个次构造函数需要委托给主构造函数， 可以直接委托或者通过别的次构造函数间接委托。委托到同一个类的另一个构造函数用 **this** 关键字即可：

```kotlin
class View(val context: String) {

    constructor(context: String, attr: String): this(context, attr, 0) {}

    constructor(context: String, attr: String, theme: Int): this(context) {}

}
```

***初始化块中的代码和字段初始化代码实际上会成为主构造函数的一部分***。委托给主构造函数会作为次构造函数的第一条语句，因此所有初始化块与属性初始化器中的代码都会在次构造函数体之前执行。***即使该类没有主构造函数，这种委托仍会隐式发生，并且仍会执行初始化块***

如果一个非抽象类没有声明任何（主或次）构造函数，它会有一个生成的不带参数的主构造函数。构造函数的可见性是 public。如果你不希望你的类有一个公有构造函数，你需要声明一个带有非默认可见性的空的主构造函数：

```kotlin
class DontCreateMe private constructor () { /*……*/ }
```

要创建一个类的实例，我们就像普通函数一样调用构造函数, Kotlin 并没有 *new* 关键字

##### 继承

在 Kotlin 中所有类都有一个共同的超类 `Any`

`Any` 有三个方法：`equals()`、 `hashCode()` 与 `toString()`

默认情况下，Kotlin 类是最终（final）的：它们不能被继承。 要使一个类可继承，请用 `open` 关键字标记它。

```kotlin
open class Base // 该类开放继承
```

如果派生类有一个主构造函数，其基类必须就地初始化。

如果派生类没有主构造函数，那么每个次构造函数必须使用 *super* 关键字初始化其基类型，或委托给另一个构造函数做到这一点

```kotlin
class TextView(text: String) : View("context", "attr") {//必须调基类构造函数
    constructor(context: String, attr: String): this("123")//必须委托给主构造函数
}

class MyView : View {
    constructor(ctx: Context) : super(ctx)
}
```

###### 覆盖方法

Kotlin 对于可覆盖的成员（我们称之为*开放*）以及覆盖后的成员需要显式修饰符：

```kotlin
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

```kotlin
open class Rectangle() : Shape() {
    final override fun draw() { /*……*/ }
}
```

###### 覆盖属性

属性覆盖与方法覆盖类似; 在超类中声明然后在派生类中重新声明的属性必须以 *override* 开头，并且它们必须具有兼容的类型。

用一个 `var` 属性覆盖一个 `val` 属性，但反之则不行。 这是允许的，因为一个 `val` 属性本质上声明了一个 `get` 方法， 而将其覆盖为 `var` 只是在子类中额外声明一个 `set` 方法。

可以在主构造函数中使用 *override* 关键字作为属性声明的一部分。

```kotlin
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

```kotlin
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

```kotlin
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

```kotlin
open class Polygon {
    open fun draw() {}
}

abstract class Rectangle : Polygon() {
    abstract override fun draw()
}
```

##### 伴生对象

#### 属性与字段

声明一个属性的完整语法是：

```kotlin
// 可变属性
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
// 只读属性
val <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
```

其初始器（initializer）、getter 和 setter 都是可选的。属性类型如果可以从初始器 （或者从其 getter 返回值，如下文所示）中推断出来，也可以省略。

```kotlin
var allByDefault: Int? // 错误：需要显式初始化器，隐含默认 getter 和 setter
var initialized = 1 // 类型 Int、默认 getter 和 setter

val simple: Int? // 类型 Int、默认 getter、必须在构造函数中初始化
val inferredType = 1 // 类型 Int 、默认 getter
```

自定义 getter setter

```kotlin
var flag: Int = 100
	get() {
		println("getter")
		return field
	}
	set(value) {
		println("setter")
		field = value
	}
```

如果你需要改变一个访问器的可见性或者对其注解，但是不需要改变默认的实现， 你可以定义访问器而不定义其实现:

```kotlin
var setterVisibility: String = "abc"
    private set // 此 setter 是私有的并且有默认实现

var setterWithAnnotation: Any? = null
    @Inject set // 用 Inject 注解此 setter
```

###### 幕后字段

在 Kotlin 类中不能直接声明字段。然而，当一个属性需要一个幕后字段时，Kotlin 会自动提供。这个幕后字段可以使用`field`标识符在访问器中引用：

```kotlin
var counter = 0 // 注意：这个初始器直接为幕后字段赋值
    set(value) {
        if (value >= 0) field = value
    }
```

`field` 标识符只能用在属性的访问器内。

如果属性至少一个访问器使用默认实现，或者自定义访问器通过 `field` 引用幕后字段，将会为该属性生成一个幕后字段。

例如，下面的情况下， 就没有幕后字段：

```kotlin
val isEmpty: Boolean
    get() = this.size == 0
```

**有幕后字段的属性转换成Java代码一定有一个对应的Java变量**

###### 幕后属性

有时候有这种需求，我们希望一个属性：**对外表现为只读，对内表现为可读可写**，我们将这个属性成为**幕后属性**。：

```kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if (_table == null) {
            _table = HashMap() // 类型参数已推断出
        }
        return _table ?: throw AssertionError("Set to null by another thread")
    }
```

将`_table`属性声明为`private`,因此外部是不能访问的，内部可以访问，外部访问通过`table`属性，而`table`属性的值取决于`_table`，这里`_table`就是幕后属性。

###### 编译期常量

如果只读属性的值在编译期是已知的，那么可以使用 *const* 修饰符将其标记为*编译期常量*。 这种属性需要满足以下要求：

- 位于顶层或者是 *object* 声明 或 *companion object*的一个成员
- 以 `String` 或原生类型值初始化
- 没有自定义 getter

###### 延迟初始化属性与变量

一般地，属性声明为非空类型必须在构造函数中初始化。 然而，这经常不方便。例如：属性可以通过依赖注入来初始化， 或者在单元测试的 setup 方法中初始化。 这种情况下，你不能在构造函数内提供一个非空初始器。 但你仍然想在类体中引用该属性时避免空检测。

为处理这种情况，你可以用 `lateinit` 修饰符标记该属性, 该属性或变量必须为非空类型，并且不能是原生类型。

在初始化前访问一个 `lateinit` 属性会抛出一个特定异常，该异常明确标识该属性被访问及它没有初始化的事实。

检测一个 lateinit var 是否已初始化

```kotlin
if (foo::bar.isInitialized) {
    println(foo.bar)
}
```

此检测仅对可词法级访问的属性可用，即声明位于同一个类型内、位于其中一个外围类型中或者位于相同文件的顶层的属性。

#### 接口

Kotlin 的接口可以既包含抽象方法也包含非抽象方法。**接口无法保存状态**。它可以有属性但必须声明为抽象或提供访问器实现。接口中声明的属性不能有幕后字段（backing field）

##### 函数式（SAM）接口

只有一个抽象方法的接口称为*函数式接口*或 *SAM（单一抽象方法）*接口。函数式接口可以有多个非抽象成员，但只能有一个抽象成员。

```kotlin
fun interface KRunnable {
    val name: String get() = "A"
    fun invoke()
    fun test() {
        println("test")
    }
}
```

使用 lambda 表达式可以替代手动创建实现函数式接口的类。通过 SAM 转换， Kotlin 可以将其签名与接口的单个抽象方法的签名匹配的任何 lambda 表达式转换为实现该接口的类的实例。

```kotlin
var implOld = object: KRunnable {
        override fun invoke() {
            println("invoke")
        }
    }
var impl = KRunnable { println("invoke") }
```



#### 扩展

Kotlin 能够扩展一个类的新功能而无需继承该类或者使用像装饰者这样的设计模式。 这通过叫做 *扩展* 的特殊声明完成。 例如，你可以为一个你不能修改的、来自第三方库中的类编写一个新的函数。 这个新增的函数就像那个原始类本来就有的函数一样，可以用普通的方法调用。 这种机制称为 *扩展函数* 。此外，也有 *扩展属性* ， 允许你为一个已经存在的类添加新的属性。

##### 扩展函数

声明一个扩展函数，我们需要用一个 *接收者类型* 也就是被扩展的类型来作为他的前缀。

```kotlin
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // “this”对应该列表
    this[index1] = this[index2]
    this[index2] = tmp
}

val list = mutableListOf(1, 2, 3)
list.swap(0, 2) // “swap()”内部的“this”会保存“list”的值
```

##### 扩展是静态解析的

扩展不能真正的修改他们所扩展的类。通过定义一个扩展，你并没有在一个类中插入新成员， 仅仅是可以通过该类型的变量用点表达式去调用这个新函数。

我们想强调的是扩展函数是静态分发的。 这意味着调用的扩展函数是由函数调用所在的表达式的类型来决定的， 而不是由表达式运行时求值结果决定的。例如：

```kotlin
open class Shape

class Rectangle: Shape()

fun Shape.getName() = "Shape"

fun Rectangle.getName() = "Rectangle"

fun printClassName(s: Shape) {
    println(s.getName())
}    

printClassName(Rectangle())
//输出 "Shape"，因为调用的扩展函数只取决于参数 s 的声明类型，该类型是 Shape 类。
```

如果成员函数和扩展函数签名一样,会调用成员函数,即成员函数不会被扩展函数覆盖,可以重载

可以为可空的接收者类型定义扩展。这样的扩展可以在对象变量上调用， 即使其值为 null，并且可以在函数体内检测 `this == null`

```kotlin
fun Any?.toString(): String {
    if (this == null) return "null"
    // 空检测之后，“this”会自动转换为非空类型，所以下面的 toString()
    // 解析为 Any 类的成员函数
    return toString()
}
```

##### 扩展属性

```kotlin
val <T> List<T>.lastIndex: Int
    get() = size - 1
```

由于扩展没有实际的将成员插入类中，因此对扩展属性来说[幕后字段](https://www.kotlincn.net/docs/reference/properties.html#幕后字段)是无效的。这就是为什么**扩展属性不能有初始化器**。他们的行为只能由显式提供的 getters/setters 定义。

##### 伴生对象的扩展

```kotlin
class MyClass {
    companion object { }  // 将被称为 "Companion"
}

fun MyClass.Companion.printCompanion() { println("companion") }

fun main() {
    MyClass.printCompanion()
}
```

##### 扩展声明为成员

在一个类内部你可以为另一个类声明扩展。在这样的扩展只能用在该类内部，该扩展函数内部有多个 *隐式接收者* : *分发接收者*, *扩展接收者*

对于分发接收者与扩展接收者的成员名字冲突的情况，扩展接收者优先。要引用分发接收者的成员你可以使用 [限定的 `this` 语法](https://www.kotlincn.net/docs/reference/this-expressions.html#限定的-this)。

```kotlin
fun interface KRunnable {
    fun invoke(s: String)

    fun String.aaa() {
        println(toString()) //优先调用 String.toString 方法
        println(this@KRunnable.toString()) //使用 this 调用分发接收者方法
    }
}

fun KRunnable.extFun() {
    println("extFun")
}

fun main() {
    var impl = KRunnable { println("invoke") }
    impl.extFun()
    "str".aaa()// 错误，该扩展函数在 KRunnable 外不可用
}
```

扩展函数也可以被覆盖

```kotlin
open class Base { }

class Derived : Base() { }

open class BaseCaller {
    open fun Base.printFunctionInfo() {
        println("Base extension function in BaseCaller")
    }

    open fun Derived.printFunctionInfo() {
        println("Derived extension function in BaseCaller")
    }

    fun call(b: Base) {
        b.printFunctionInfo()   // 调用扩展函数
    }
}

class DerivedCaller: BaseCaller() {
    override fun Base.printFunctionInfo() {
        println("Base extension function in DerivedCaller")
    }

    override fun Derived.printFunctionInfo() {
        println("Derived extension function in DerivedCaller")
    }
}

fun main() {
    BaseCaller().call(Base())   // “Base extension function in BaseCaller”
    DerivedCaller().call(Base())  // “Base extension function in DerivedCaller”——分发接收者虚拟解析
    DerivedCaller().call(Derived())  // “Base extension function in DerivedCaller”——扩展接收者静态解析
}
```



有时候，我们需要创建一个对某个类做了轻微改动的类的对象，而不用为之显式声明新的子类。 Kotlin 用*对象表达式*和*对象声明*处理这种情况。

#### 对象表达式

要创建一个继承自某个（或某些）类型的匿名类的对象，我们会这么写：

```kotlin
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { /*……*/ }

    override fun mouseEntered(e: MouseEvent) { /*……*/ }
})
```

任何时候，如果我们只需要“一个对象而已”，并不需要特殊超类型，那么我们可以简单地写：

```kotlin
fun foo() {
    val adHoc = object {
        var x: Int = 0
        var y: Int = 0
    }
    print(adHoc.x + adHoc.y)
}
```

#### 对象声明

单例模式在一些场景中很有用， 而 Kotlin（继 Scala 之后）使单例声明变得很容易：

```kotlin
object DataProviderManager {
    fun registerDataProvider(provider: DataProvider) {
        // ……
    }

    val allDataProviders: Collection<DataProvider>
        get() = // ……
}
```

这称为*对象声明*。并且它总是在 *object* 关键字后跟一个名称。 就像变量声明一样，对象声明不是一个表达式，不能用在赋值语句的右边。

对象声明的初始化过程是线程安全的并且在首次访问时进行。

#### 伴生对象

类内部的对象声明可以用 *companion* 关键字标记：

```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

val instance = MyClass.create()
```

可以省略伴生对象的名称，在这种情况下将使用名称 `Companion`：

```kotlin
class MyClass {
    companion object { }
}

val x = MyClass.Companion
```

其自身所用的类的名称（不是另一个名称的限定符）可用作对该类的伴生对象 （无论是否具名）的引用：

```kotlin
class MyClass1 {
    companion object Named { }
}

val x = MyClass1

class MyClass2 {
    companion object { }
}

val y = MyClass2
```

请注意，即使伴生对象的成员看起来像其他语言的静态成员，在运行时他们仍然是真实对象的实例成员，而且，例如还可以实现接口：

```kotlin
interface Factory<T> {
    fun create(): T
}

class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}

val f: Factory<MyClass> = MyClass
```

#### 数据类

```kotlin
data class User(val name: String, val age: Int)
```

编译器自动从**主构造函数中声明的所有属性**导出以下成员：

- `equals()`/`hashCode()` 对；
- `toString()` 格式是 `"User(name=John, age=42)"`；
- [`componentN()` 函数](https://www.kotlincn.net/docs/reference/multi-declarations.html) 按声明顺序对应于所有属性；
- `copy()` 函数（见下文）。

为了确保生成的代码的一致性以及有意义的行为，数据类必须满足以下要求：

- 主构造函数需要至少有一个参数；
- 主构造函数的所有参数需要标记为 `val` 或 `var`；
- 数据类不能是抽象、开放、密封或者内部的；
- （在1.1之前）数据类只能实现接口。

此外，成员生成遵循关于成员继承的这些规则：

- 如果在数据类体中有显式实现 `equals()`、 `hashCode()` 或者 `toString()`，或者这些函数在父类中有 *final* 实现，那么不会生成这些函数，而会使用现有函数；
- 如果超类型具有 *open* 的 `componentN()` 函数并且返回兼容的类型， 那么会为数据类生成相应的函数，并覆盖超类的实现。如果超类型的这些函数由于签名不兼容或者是 final 而导致无法覆盖，那么会报错；
- 从一个已具 `copy(……)` 函数且签名匹配的类型派生一个数据类在 Kotlin 1.2 中已弃用，并且在 Kotlin 1.3 中已禁用。
- 不允许为 `componentN()` 以及 `copy()` 函数提供显式实现。

在 JVM 中，如果生成的类需要含有一个无参的构造函数，则所有的属性必须指定默认值。

对于那些自动生成的函数，编译器只使用在主构造函数内部定义的属性。如需在生成的实现中排除一个属性，请将其声明在类体中：

```kotlin
data class Person(val name: String) {
    var age: Int = 0
}
```

在 `toString()`、 `equals()`、 `hashCode()` 以及 `copy()` 的实现中只会用到 `name` 属性，并且只有一个 component 函数 `component1()`。虽然两个 `Person` 对象可以有不同的年龄，但它们会视为相等。

###### 复制

在很多情况下，我们需要复制一个对象改变它的一些属性，但其余部分保持不变。 `copy()` 函数就是为此而生成。对于上文的 `User` 类，其实现会类似下面这样：

```kotlin
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```

这让我们可以写：

```kotlin
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

###### 数据类与解构声明

为数据类生成的 *Component 函数* 使它们可在[解构声明](https://www.kotlincn.net/docs/reference/multi-declarations.html)中使用：

```kotlin
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age") // 输出 "Jane, 35 years of age"
```

#### 密封类

密封类用来表示受限的类继承结构：当一个值为有限几种的类型、而不能有任何其他类型时。在某种意义上，他们是枚举类的扩展：枚举类型的值集合也是受限的，但每个枚举常量只存在一个实例，而密封类的一个子类可以有可包含状态的多个实例。

要声明一个密封类，需要在类名前面添加 `sealed` 修饰符。虽然密封类也可以有子类，但是所有子类都必须在与密封类自身相同的文件中声明。

```kotlin
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```

一个密封类是自身[抽象的](https://www.kotlincn.net/docs/reference/classes.html#抽象类)，它不能直接实例化并可以有抽象（*abstract*）成员。

使用密封类的关键好处在于使用 [`when` 表达式](https://www.kotlincn.net/docs/reference/control-flow.html#when-表达式) 的时候，如果能够验证语句覆盖了所有情况，就不需要为该语句再添加一个 `else` 子句了。当然，这只有当你用 `when` 作为表达式（使用结果）而不是作为语句时才有用。

```kotlin
fun eval(expr: Expr): Double = when(expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
    // 不再需要 `else` 子句，因为我们已经覆盖了所有的情况
}
```

#### 嵌套类

```kotlin
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

嵌套类是 java 中的 静态内部类

#### 内部类

```kotlin
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}
```

内部类是 java 中的非静态内部类, 会带有一个对外部类的对象的引用

#### 匿名内部类

使用[对象表达式](https://www.kotlincn.net/docs/reference/object-declarations.html#对象表达式)创建匿名内部类实例：

```kotlin
window.addMouseListener(object : MouseAdapter() {

    override fun mouseClicked(e: MouseEvent) { …… }

    override fun mouseEntered(e: MouseEvent) { …… }
})
```

*注*：对于 JVM 平台, 如果对象是函数式 Java 接口（即具有单个抽象方法的 Java 接口）的实例， 你可以使用带接口类型前缀的lambda表达式创建它：

```kotlin
val listener = ActionListener { println("clicked") }
```

#### 枚举类

枚举类的最基本的用法是实现类型安全的枚举：

```kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```

每个枚举常量都是一个对象。枚举常量用逗号分隔。

因为每一个枚举都是枚举类的实例，所以他们可以是这样初始化过的：

```kotlin
enum class Color(val rgb: Int) {
        RED(0xFF0000),
        GREEN(0x00FF00),
        BLUE(0x0000FF)
}
```

#### 泛型

与 Java 类似，Kotlin 中的类也可以有类型参数：

```kotlin
class Box<T>(t: T) {
    var value = t
}
```

一般来说，要创建这样类的实例，我们需要提供类型参数：

```kotlin
val box: Box<Int> = Box<Int>(1)
```

但是如果类型参数可以推断出来，例如从构造函数的参数或者从其他途径，允许省略类型参数：

```kotlin
val box = Box(1) // 1 具有类型 Int，所以编译器知道我们说的是 Box<Int>。
```

#### 类型别名

类型别名为现有类型提供替代名称。 如果类型名称太长，你可以另外引入较短的名称，并使用新的名称替代原类型名。

它有助于缩短较长的泛型类型。 例如，通常缩减集合类型是很有吸引力的：

```kotlin
typealias NodeSet = Set<Network.Node>

typealias FileTable<K> = MutableMap<K, MutableList<File>>
```

你可以为函数类型提供另外的别名：

```kotlin
typealias MyHandler = (Int, String, Any) -> Unit

typealias Predicate<T> = (T) -> Boolean
```

#### 内联类

内联类仅在 Kotlin 1.3 之后版本可用，目前处于 [Alpha](https://www.kotlincn.net/docs/reference/evolution/components-stability.html) 版。

#### 委托

##### 委托属性

语法是： `val/var <属性名>: <类型> by <表达式>`。在 *by* 后面的表达式是该 *委托*， 因为属性对应的 `get()`（与 `set()`）会被委托给它的 `getValue()` 与 `setValue()` 方法。 属性的委托不必实现任何的接口，但是需要提供一个 `getValue()` 函数（与 `setValue()`——对于 *var* 属性）。 例如:

```kotlin
import kotlin.reflect.KProperty

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }
 
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name}' in $thisRef.")
    }
}

class Example {
    var p: String by Delegate()
}
```

##### 标准委托

Kotlin 标准库为几种有用的委托提供了工厂方法。

###### 延迟属性 Lazy

[`lazy()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/lazy.html) 是接受一个 lambda 并返回一个 `Lazy <T>` 实例的函数，返回的实例可以作为实现延迟属性的委托： 第一次调用 `get()` 会执行已传递给 `lazy()` 的 lambda 表达式并记录结果， 后续调用 `get()` 只是返回记录的结果。

```kotlin
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}
// lazy 没有 set 函数, 不能作为 var 属性的委托
```

###### 可观察属性 Observable

[`Delegates.observable()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.properties/-delegates/observable.html) 接受两个参数：初始值与修改时处理程序（handler）。 每当我们给属性赋值时会调用该处理程序（在赋值*后*执行）。它有三个参数：被赋值的属性、旧值与新值

```kotlin
var name: String by Delegates.observable("<no name>") {
        prop, old, new ->
        println("$old -> $new")
}
```

如果你想截获赋值并“否决”它们，那么使用 [`vetoable()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.properties/-delegates/vetoable.html) 取代 `observable()`。 在属性被赋新值生效*之前*会调用传递给 `vetoable` 的处理程序。

###### 委托给另一个属性

从 Kotlin 1.4 开始，一个属性可以把它的 getter 与 setter 委托给另一个属性。这种委托对于顶层和类的属性（成员和扩展）都可用

为将一个属性委托给另一个属性，应在委托名称中使用恰当的 `::` 限定符，例如，`this::delegate` 或 `MyClass::delegate`。

```kotlin
class MyClass(var memberInt: Int, val anotherClassInstance: ClassWithDelegate) {
    var delegatedToMember: Int by this::memberInt
    var delegatedToTopLevel: Int by ::topLevelInt
    
    val delegatedToAnotherClass: Int by anotherClassInstance::anotherClassInt
}
var MyClass.extDelegated: Int by ::topLevelInt
```

这是很有用的，例如，当想要以一种向后兼容的方式重命名一个属性的时候：引入一个新的属性、 使用 `@Deprecated` 注解来注解旧的属性、并委托其实现。

```kotlin
class MyClass {
   var newName: Int = 0
   @Deprecated("Use 'newName' instead", ReplaceWith("newName"))
   var oldName: Int by this::newName
}
```

###### 将属性储存在映射中

一个常见的用例是在一个映射（map）里存储属性的值。 这经常出现在像解析 JSON 或者做其他“动态”事情的应用中。 在这种情况下，你可以使用映射实例自身作为委托来实现委托属性。

```kotlin
class User(val map: Map<String, Any?>) {
    val name: String by map
    val age: Int     by map
}
```

在这个例子中，构造函数接受一个映射参数：

```kotlin
val user = User(mapOf(
    "name" to "John Doe",
    "age"  to 25
))
```

委托属性会从这个映射中取值（通过字符串键——属性的名称）：

```kotlin
println(user.name) // Prints "John Doe"
println(user.age)  // Prints 25
```

Target platform: JVMRunning on kotlin v. 1.4.10

这也适用于 *var* 属性，如果把只读的 `Map` 换成 `MutableMap` 的话：

```kotlin
class MutableUser(val map: MutableMap<String, Any?>) {
    var name: String by map
    var age: Int     by map
}
```

###### 局部委托属性

你可以将局部变量声明为委托属性。 例如，你可以使一个局部变量惰性初始化：

```kotlin
fun example(computeFoo: () -> Foo) {
    val memoizedFoo by lazy(computeFoo)

    if (someCondition && memoizedFoo.isValid()) {
        memoizedFoo.doSomething()
    }
}
```

`memoizedFoo` 变量只会在第一次访问时计算。 如果 `someCondition` 失败，那么该变量根本不会计算。

###### 提供委托

通过定义 `provideDelegate` 操作符，可以扩展创建属性实现所委托对象的逻辑。 如果 `by` 右侧所使用的对象将 `provideDelegate` 定义为成员或扩展函数， 那么会调用该函数来创建属性委托实例。例如，如果要在绑定之前检测属性名称，可以这样写：

```kotlin
class ResourceDelegate<T> : ReadOnlyProperty<MyUI, T> {
    override fun getValue(thisRef: MyUI, property: KProperty<*>): T { ... }
}
    
class ResourceLoader<T>(id: ResourceID<T>) {
    operator fun provideDelegate(
            thisRef: MyUI,
            prop: KProperty<*>
    ): ReadOnlyProperty<MyUI, T> {
        checkProperty(thisRef, prop.name)
        // 创建委托
        return ResourceDelegate()
    }

    private fun checkProperty(thisRef: MyUI, name: String) { …… }
}

class MyUI {
    fun <T> bindResource(id: ResourceID<T>): ResourceLoader<T> { …… }

    val image by bindResource(ResourceID.image_id)
    val text by bindResource(ResourceID.text_id)
}
```

#### 协程

[runBlocking](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/run-blocking.html) 与 [coroutineScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/coroutine-scope.html) 可能看起来很类似，因为它们都会等待其协程体以及所有子协程结束。 主要区别在于，[runBlocking](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/run-blocking.html) 方法会*阻塞*当前线程来等待， 而 [coroutineScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/coroutine-scope.html) 只是挂起，会释放底层线程用于其他用途。 由于存在这点差异，[runBlocking](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/run-blocking.html) 是常规函数，而 [coroutineScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/coroutine-scope.html) 是挂起函数。

```kotlin
fun main() = runBlocking { // this: CoroutineScope

    coroutineScope { // 创建一个协程作用域
        launch {
            delay(1000L)
            println("444")
        }
        delay(500L)
        println("111")
    }

    //coroutineScope 所有子协程执行完毕才会执行到这里
    launch {
        delay(1000L)
        println("222")
    }

    println("333")
}
//output:  111 444 333 222
```

##### 取消

`launch` 返回 `Job`, `async` 返回 `Deferred`, 都有 `cancel()` `cancelAndJoin` 方法

取消是协作的, 一段协程代码必须协作才能被取消. 代码中要用`isActive`判断协程是否被取消了, 不然不会自动取消的, 挂起函数都是 *可被取消的* 。它们检查协程的取消， 并在取消时抛出 [CancellationException](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-cancellation-exception/index.html)。

###### 在 `finally` 中释放资源

协程取消后在`finally` 块中调用挂起函数的行为都会抛出 [CancellationException](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-cancellation-exception/index.html)，后面的代码不会被执行, 如果确实要调用挂起函数, 可以放在 withContext(NonCancellable) {……} 中执行

```kotlin
fun main() = runBlocking { // this: CoroutineScope

    val job = launch {
        try {
            repeat(1000) { i ->
                delay(500L)
            }
        } finally {
            println("111")
            delay(100)
            println("222")
        }
    }
    delay(1300L) // 延迟一段时间
    job.cancelAndJoin() // 取消该作业并且等待它结束
    println("main: Now I can quit.")
}
//不会打印 222
```

##### 超时

```kotlin
withTimeout(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
}
```

[withTimeout](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/with-timeout.html) 抛出了 `TimeoutCancellationException`，它是 [CancellationException](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-cancellation-exception/index.html) 的子类。 我们调用`cancel`方法没有在控制台上看到堆栈跟踪信息的打印。这是因为在被取消的协程中 `CancellationException` 被认为是协程执行结束的正常原因

[withTimeoutOrNull](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/with-timeout-or-null.html) 通过返回 `null` 来进行超时操作，从而替代抛出一个异常

```kotlin
val result = withTimeoutOrNull(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
    "Done" // 在它运行得到结果之前取消它
}
println("Result is $result")
```

##### 组合挂起函数

```kotlin
fun main() = runBlocking { // this: CoroutineScope

    val time = measureTimeMillis {
        val a = doSomethingUsefulOne()//挂起函数
        val t = doSomethingUsefulTwo()//挂起函数
    }
    println("time:$time")

}//两个函数顺序执行
```

```kotlin
fun main() = runBlocking { // this: CoroutineScope

    val time = measureTimeMillis {
        val one = async { doSomethingUsefulOne() }
        val two = async { doSomethingUsefulTwo() }
        println("The answer is ${one.await() + two.await()}")
    }
    println("Completed in $time ms")

}//两个函数并发执行
```

[async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 就类似于 [launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html)。它启动了一个单独的协程，与其它所有的协程一起并发的工作。不同之处在于 `launch` 返回一个 [Job](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/index.html) 并且不附带任何结果值，而 `async` 返回一个 [Deferred](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/index.html) —— 一个轻量级的非阻塞 future, 你可以使用 `.await()` 在一个延期的值上得到它的最终结果，  `Deferred` 继承自 `Job`，所以如果需要的话，你可以取消它。

###### 惰性启动的 async

可选的，[async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 可以通过将 `start` 参数设置为 [CoroutineStart.LAZY](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-start/-l-a-z-y.html) 而变为惰性的。 在这个模式下，只有结果通过 [await](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/await.html) 获取的时候协程才会启动，或者在 `Job` 的 [start](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/start.html) 函数调用的时候。

```kotlin
val time = measureTimeMillis {
    val one = async(start = CoroutineStart.LAZY) { doSomethingUsefulOne() }
    val two = async(start = CoroutineStart.LAZY) { doSomethingUsefulTwo() }
    // 执行一些计算
    one.start() // 启动第一个
    two.start() // 启动第二个
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```

注意，如果我们只是在 `println` 中调用 [await](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/await.html)，而没有在单独的协程中调用 [start](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/start.html)，这将会导致顺序行为，直到 [await](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/await.html) 启动该协程 执行并等待至它结束，这并不是惰性的预期用例。

###### 异常取消

如果其中一个子协程（即 `two`）失败，第一个 `async` 以及等待中的父协程都会被取消

```kotlin
fun main() = runBlocking<Unit> {
    try {
        failedConcurrentSum()
    } catch(e: ArithmeticException) {
        println("Computation failed with ArithmeticException")
    }
}

suspend fun failedConcurrentSum(): Int = coroutineScope {
    val one = async<Int> {
        try {
            delay(Long.MAX_VALUE) // 模拟一个长时间的运算
            42
        } finally {
            println("First child was cancelled")
        }
    }
    val two = async<Int> {
        println("Second child throws an exception")
        throw ArithmeticException()
    }
    one.await() + two.await()
}//output: 111 222 333
```

##### 协程上下文与调度器

协程上下文包含一个 *协程调度器* （参见 [CoroutineDispatcher](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-dispatcher/index.html)）它确定了相关的协程在***哪个***线程或***哪些***线程上执行。协程调度器可以将协程限制在一个特定的线程执行，或将它分派到一个线程池，亦或是让它不受限地运行。

所有的协程构建器诸如 [launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html) 和 [async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 接收一个可选的 [CoroutineContext](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.coroutines/-coroutine-context/) 参数，它可以被用来显式的为一个新协程或其它上下文元素指定一个调度器。

```kotlin
launch { // 运行在父协程的上下文中
    println("main runBlocking      : I'm working in thread ${Thread.currentThread().name}")
}
launch(newSingleThreadContext("MyOwnThread")) { // 将使它获得一个新的线程,使用完要 close
    println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
}
```

当调用 `launch { …… }` 时不传参数，它从启动了它的 [CoroutineScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-scope/index.html) 中承袭了上下文（以及调度器）。

当协程在 [GlobalScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-global-scope/index.html) 中启动时，使用的是由 [Dispatchers.Default](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-dispatchers/-default.html) 代表的默认调度器。 默认调度器使用共享的后台线程池。 所以 `launch(Dispatchers.Default) { …… }` 与 `GlobalScope.launch { …… }` 使用相同的调度器。

[newSingleThreadContext](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/new-single-thread-context.html) 为协程的运行启动了一个线程。 一个专用的线程是一种非常昂贵的资源。 在真实的应用程序中两者都必须被释放，当不再需要的时候，使用 [close](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-executor-coroutine-dispatcher/close.html) 函数，或存储在一个顶层变量中使它在整个应用程序中被重用。

###### 子协程

当一个协程被其它协程在 [CoroutineScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-scope/index.html) 中启动的时候， 它将通过 [CoroutineScope.coroutineContext](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-scope/coroutine-context.html) 来承袭上下文，并且这个新协程的 [Job](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/index.html) 将会成为父协程作业的 *子* 作业。当一个父协程被取消的时候，所有它的子协程也会被递归的取消。

然而，当使用 [GlobalScope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-global-scope/index.html) 来启动一个协程时，则新协程的作业没有父作业。 因此它与这个启动的作用域无关且独立运作。

###### 父协程的职责

一个父协程总是等待所有的子协程执行结束

```kotlin
// 启动一个协程来处理某种传入请求（request）
val request = launch {
    repeat(3) { i -> // 启动少量的子作业
        launch  {
            delay((i + 1) * 200L) // 延迟 200 毫秒、400 毫秒、600 毫秒的时间
            println("Coroutine $i is done")
        }
    }
    println("request: I'm done and I don't explicitly join my children that are still active")
}
request.join() // 等待请求的完成，包括其所有子协程
println("Now processing of the request is complete")
```

```
request: I'm done and I don't explicitly join my children that are still active
Coroutine 0 is done
Coroutine 1 is done
Coroutine 2 is done
Now processing of the request is complete
```

###### 命名协程以用于调试

```kotlin
val v1 = async(CoroutineName("v1coroutine")) {
    delay(500)
    log("Computing v1")
    252
}
```

###### 组合上下文中的元素

有时我们需要在协程上下文中定义多个元素。我们可以使用 `+` 操作符来实现。 比如说，我们可以显式指定一个调度器来启动协程并且同时显式指定一个命名：

```kotlin
launch(Dispatchers.Default + CoroutineName("test")) {
    println("I'm working in thread ${Thread.currentThread().name}")
}
```

###### 线程局部数据

有时，能够将一些线程局部数据传递到协程与协程之间是很方便的。 然而，由于它们不受任何特定线程的约束，如果手动完成，可能会导致出现样板代码。

[`ThreadLocal`](https://docs.oracle.com/javase/8/docs/api/java/lang/ThreadLocal.html)， [asContextElement](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/java.lang.-thread-local/as-context-element.html) 扩展函数在这里会充当救兵。它创建了额外的上下文元素， 且保留给定 `ThreadLocal` 的值，并在每次协程切换其上下文时恢复它。

```kotlin
val threadLocal = ThreadLocal<String?>() // 声明线程局部变量

fun main() = runBlocking<Unit> {
    threadLocal.set("main")
    println("111: ${Thread.currentThread().name}, value: '${threadLocal.get()}'")
    val job = launch(Dispatchers.Default + threadLocal.asContextElement(value = "launch")) {
        println("222: ${Thread.currentThread().name}, value: '${threadLocal.get()}'")
        yield()
        println("333: ${Thread.currentThread().name}, value: '${threadLocal.get()}'")
    }
    job.join()
    println("444: ${Thread.currentThread().name},  value: '${threadLocal.get()}'")
}
output:
111: main, value: 'main'
222: DefaultDispatcher-worker-1, value: 'launch'
333: DefaultDispatcher-worker-2, value: 'launch'
444: main,  value: 'main'
```

##### 异常处理

###### 异常的传播

协程构建器有两种形式：自动传播异常（[launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html) 与 [actor](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/actor.html)）或向用户暴露异常（[async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 与 [produce](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/produce.html)）。 当这些构建器用于创建一个*根*协程时，即该协程不是另一个协程的*子*协程， 前者这类构建器将异常视为**未捕获**异常，类似 Java 的 `Thread.uncaughtExceptionHandler`， 而后者则依赖用户来最终消费异常，例如通过 [await](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/await.html) 或 [receive](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-receive-channel/receive.html)（[produce](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/produce.html) 与 [receive](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-receive-channel/receive.html) 的相关内容包含于[通道](https://github.com/Kotlin/kotlinx.coroutines/blob/master/docs/channels.md)章节）。

```kotlin
fun main() = runBlocking {
    val job = GlobalScope.launch { // launch 根协程
        println("Throwing exception from launch")
        throw IndexOutOfBoundsException() // 我们将在控制台打印 Thread.defaultUncaughtExceptionHandler
    }
    job.join()
    println("Joined failed job")
    val deferred = GlobalScope.async { // async 根协程
        println("Throwing exception from async")
        throw ArithmeticException() // 没有打印任何东西，依赖用户去调用等待
    }
    try {
        deferred.await()
        println("Unreached")
    } catch (e: ArithmeticException) {
        println("Caught ArithmeticException")
    }
}
```



###### CoroutineExceptionHandler

将**未捕获**异常打印到控制台的默认行为是可自定义的。 *根*协程中的 [CoroutineExceptionHandler](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-exception-handler/index.html) 上下文元素可以被用于这个根协程通用的 `catch` 块.你无法从 `CoroutineExceptionHandler` 的异常中恢复。当调用处理者的时候，协程已经完成并带有相应的异常。

`CoroutineExceptionHandler` 仅在**未捕获**的异常上调用 — 没有以其他任何方式处理的异常。 特别是，所有*子*协程委托它们的父协程处理它们的异常，然后它们也委托给其父协程，以此类推直到根协程， 因此永远不会使用子协程的 `CoroutineExceptionHandler`。 除此之外，[async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 构建器始终会捕获所有异常并将其表示在结果 [Deferred](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/index.html) 对象中， 因此它的 `CoroutineExceptionHandler` 也无效。

```kotlin
val handler = CoroutineExceptionHandler { _, exception -> 
        println("CoroutineExceptionHandler got $exception") 
    }
    val job = GlobalScope.launch(handler) { // 根协程，运行在 GlobalScope 中
        throw AssertionError()
    }
    val deferred = GlobalScope.async(handler) { // 同样是根协程，但使用 async 代替了 launch
        throw ArithmeticException() // 没有打印任何东西，依赖用户去调用 deferred.await()
    }
    joinAll(job, deferred)   
```

###### 取消与异常

取消与异常紧密相关。协程内部使用 `CancellationException` 来进行取消，这个异常会被所有的处理者忽略

当一个协程使用 [Job.cancel](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/cancel.html) 取消的时候，它会被终止，但是它不会取消它的父协程。

如果一个协程遇到了 `CancellationException` 以外的异常，它将使用该异常取消它的父协程。

当父协程的所有子协程都结束后，原始的异常才会被父协程处理， 见下面这个例子。

```kotlin
fun main() = runBlocking {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("CoroutineExceptionHandler got $exception")
    }
    val job = GlobalScope.launch(handler) {
        launch { // 第一个子协程
            try {
                delay(Long.MAX_VALUE)
            } finally {
                withContext(NonCancellable) {
                    println("111")
                    delay(100)
                    println("222")
                }
            }
        }
        launch { // 第二个子协程
            delay(10)
            println("333")
            throw ArithmeticException()
        }
    }
    job.join()
}
output:
333
111
222
CoroutineExceptionHandler got java.lang.ArithmeticException
```

##### 共享的可变状态与并发

```kotlin
@Volatile // 在 Kotlin 中 `volatile` 是一个注解
var counter = 0

fun main() = runBlocking {
    withContext(Dispatchers.Default) {
        coroutineScope {
            repeat(100) {
                launch {
                    repeat(1000) { counter++ }
                }
            }
        }
    }
    println("Counter = $counter")
}//结果不会是 100_000, 加了 volatile 也无济于事
time:8
Counter = 55712
```

解决方法1

```
val counter = AtomicInteger()

fun main() = runBlocking {
    withContext(Dispatchers.Default) {
        massiveRun {
            counter.incrementAndGet()
        }
    }
    println("Counter = $counter")
}
time:8
Counter = 100000
```

解决方法二:以细粒度限制线程, 让特定线程操作变量

```kotlin
val counterContext = newSingleThreadContext("CounterContext")
var counter = 0

fun main() = runBlocking {
    withContext(Dispatchers.Default) {
        coroutineScope { // 协程的作用域
            val time = measureTimeMillis {
                repeat(100) {
                    launch {
                        repeat(1000) {
                            withContext(counterContext) {
                                counter++
                            }
                        }
                    }
                }
            }
            println("time:$time")
        }
    }
    println("Counter = $counter")
}
time:13
Counter = 100000
```

解决方法三:互斥  协程中的 [Mutex](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.sync/-mutex/index.html) 。它具有 [lock](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.sync/-mutex/lock.html) 和 [unlock](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.sync/-mutex/unlock.html) 方法， 可以隔离关键的部分。关键的区别在于 `Mutex.lock()` 是一个挂起函数，它不会阻塞线程。

还有 [withLock](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.sync/with-lock.html) 扩展函数，可以方便的替代常用的 `mutex.lock(); try { …… } finally { mutex.unlock() }` 模式：

```kotlin
val mutex = Mutex()
var counter = 0

fun main() = runBlocking {
    withContext(Dispatchers.Default) {
        coroutineScope { // 协程的作用域
            val time = measureTimeMillis {
                repeat(100) {
                    launch {
                        repeat(1000) {
                            mutex.withLock {
                                counter++
                            }
                        }
                    }
                }
            }
            println("time:$time")
        }
    }
    println("Counter = $counter")
}
time:7
Counter = 100000
```























TODO use 函数
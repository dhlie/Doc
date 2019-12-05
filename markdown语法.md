# Markdown语法

## 标题

 在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶


    # 这是一级标题
    ## 这是二级标题
    ### 这是三级标题
    #### 这是四级标题
    ##### 这是五级标题
    ###### 这是六级标题


 ## 字体

*斜体* `*斜体*`
**加粗** `**加粗**`
***斜体加粗*** `***斜体加粗***`
~~删除线~~ `~~删除线~~`

## 区块引用 Blockquotes

Markdown 标记区块引用是使用类似 email 中用 > 的引用方式。：

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
>> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.


引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：

> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");

## 分割线
三个或者三个以上的 - 或者 * 都可以。

---

## 代码区块

要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以，前提是代码区块前要有一个空行，例如，下面的输入：
    这是一个普通段落：

    public static void main()

也可以用三个（`）把代码区块包括起来：
```
public static void main()
```
如果要标记一小段行内代码，你可以用反引号把它包起来（`），例如：  

Use the `printf()` function.

## 列表

无序列表使用`-`,`+`,`*`作为列表标记
有序列表则使用数字接着一个英文句点(你可以完全不用在意数字的正确性).
~~~
    *   Red
    *   Green
    *   Blue

    等同于：
    +   Red
    +   Green
    +   Blue

    也等同于：
    -   Red
    -   Green
    -   Blue
~~~

1. 第一
4. 第二
6. 第三

~~~
1. 第一
4. 第二
6. 第三
~~~

## 链接

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

如果你是要链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.

## 图片

行内式的图片语法看起来像是：

    ![Alt text](/path/to/img.jpg)

    ![Alt text](/path/to/img.jpg "Optional title")

## 自动链接

Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用方括号包起来， Markdown 就会自动把它转成链接。一般网址的链接文字就和链接地址一样，例如：

    <http://example.com/>

<http://example.com/>

## 反斜杠

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 <em> 标签），你可以在星号的前面加上反斜杠：

\*literal asterisks\*

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号

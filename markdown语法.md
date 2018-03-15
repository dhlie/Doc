# Markdown语法

## 段落和换行

一个 Markdown 段落是由一个或多个连续的文本行组成，它的前后要有一个以上的空行（空行的定义是显示上看起来像是空的，便会被视为空行。比方说，若某一行只包含空格和制表符，则该行也会被视为空行）。普通段落不该用空格或制表符来缩进。

「由一个或多个连续的文本行组成」这句话其实暗示了 Markdown 允许段落内的强迫换行（插入换行符），这个特性和其他大部分的 text-to-HTML 格式不一样（包括 Movable Type 的「Convert Line Breaks」选项），其它的格式会把每个换行符都转成 `<br />` 标签

如果你确实想要依赖 Markdown 来插入 `<br />` 标签的话，在插入处先按入两个以上的空格然后回车。

## 标题

 在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶

## 区块引用 Blockquotes

Markdown 标记区块引用是使用类似 email 中用 > 的引用方式。如果你还熟悉在 email 信件中的引言部分，你就知道怎么在 Markdown 文件中建立一个区块引用，那会看起来像是你自己先断好行，然后在每行的最前面加上 > ：

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

    > This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
    > consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
    > Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
    > 
    > Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
    > id sem consectetuer libero luctus adipiscing.

Markdown 也允许你偷懒只在整个段落的第一行最前面加上 > ：

    > This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
    consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
    Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

    > Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
    id sem consectetuer libero luctus adipiscing.

区块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 > ：

    > This is the first level of quoting.
    >
    > > This is nested blockquote.
    >
    > Back to the first level.

引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：

    > ## 这是一个标题。
    > 
    > 1.   这是第一行列表项。
    > 2.   这是第二行列表项。
    > 
    > 给出一些例子代码：
    > 
    >     return shell_exec("echo $input | $markdown_script");
任何像样的文本编辑器都能轻松地建立 email 型的引用。例如在 BBEdit 中，你可以选取文字后然后从选单中选择增加引用阶层。

## 列表

Markdown 支持有序列表和无序列表。

无序列表使用星号、加号或是减号作为列表标记,有序列表则使用数字接着一个英文句点(你可以完全不用在意数字的正确性).

列表项目标记通常是放在最左边，但是其实也可以缩进，最多 3 个空格，项目标记后面则一定要接着至少一个空格或制表符。

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

列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符：

    1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

        Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

    2.  Suspendisse id sem consectetuer libero luctus adipiscing.

如果要在列表项目内放进引用，那 > 就需要缩进：

    *   A list item with a blockquote:

        > This is a blockquote
        > inside a list item.

如果要放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符：

    *   一列表项包含一个列表区块：

            <代码写在这>

行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠。

    1986\. What a great season.

## 代码区块

要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以，例如，下面的输入：

    这是一个普通段落：

        这是一个代码区块。

如果要标记一小段行内代码，你可以用反引号把它包起来（`），例如：

    Use the `printf()` function.

## 分隔线

你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：

    * * *

    ***

    *****

    - - -

    ---------------------------------------

## 链接

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

如果你是要链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.

## 强调

Markdown 使用星号（*）和底线（_）作为标记强调字词的符号，被 * 或 _ 包围的字词会被转成用 `<em>` 标签包围，用两个 * 或 _ 包起来的话，则会被转成 `<strong>`，例如：

    *single asterisks*

    _single underscores_

    **double asterisks**

    __double underscores__


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

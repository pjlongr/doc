
# 宏语言指南

> Source Insight 提供了一种类似 C 的解释型宏语言，可用于编写命令脚本、插入特殊格式的文本和自动执行编辑操作。
>
> 宏保存在扩展名为 .em 的文本文件中。宏源文件可以添加到您的项目中，也可以添加到项目符号路径上的任何项目中，也可以添加到基本项目中。一旦宏文件成为项目的一部分，文件中的宏函数将作为用户级命令在"Key Assignments" 和 “Menu Assign­ments” 对话框中提供。
>
> 有一个内置宏函数库，用于访问 Source Insight 对象，例如文件缓冲区或窗口。您可以从自己的宏函数调用这些内置函数。

- 数据来源于官网

    [Source Insight 4.0](https://www.sourceinsight.com/doc/v4/userguide/index.html)

- 基本语法

    >  标识符名称用于标识变量和函数。

    - 标识符名称不区分大小写。例如，Open 和 open 是相同的。
    - 标识符名称必须以字母 A 到 Z 或 a 到 z 或下划线 （_） 开头，后跟零个或多个字母、下划线和数字。例如 isOpen、MyThing2 和 open_file。
    - 标识符名称不能包含标点符号，例如 @、% 或 dot（.）。

- 缩进和空白

    > 空格表示制表符或空格字符。空格通常被忽略，除非它用于分隔标识符和运算符。完全空白的行将被忽略。您可以使用任何您喜欢的缩进样式，因为 Source Insight 会忽略它。

- 注释 

    支持两种类型的注释

    - 注释以 /* 开头，以 */ 结尾。注释可以跨越任意数量的行。
    - 单行注释以 // 开头，并延伸到行尾。

- 字符串

    > 字符串必须用双引号括起来。不使用单引号。

- 语句末尾的分号

    > 语句末尾不需要分号，但允许使用分号并忽略这些分号。

## 宏函数

宏函数以类似于 C 函数的格式声明。空格将被忽略，并且函数和变量名称不区分大小写。宏函数以<font color=red>macro</font> 和 <font color=red> function</font>关键字开头，后跟函数名称。

宏函数可以有参数，并且可以调用其他宏。可以使用“return n”从宏返回值，其中 n 是返回值。

- 用户级宏与函数
    - 可以分配给按键或菜单的用户级命令。它们必须使用 macro 关键字声明，并且没有函数参数。命令的名称与宏函数名称相同。
    - 用作子例程的函数，可以从其他宏函数调用这些子例程。这些应该用 <font color=red> function</font>关键字声明。
    - 用于挂接到事件的事件函数。这些是用 event 关键字声明的

## 宏范围和引用

> 所有宏函数都位于同一个全局命名空间中。在任何打开的文件中声明的、存储在项目中的宏或项目符号路径上的项目中的所有宏都在范围内。也就是说，您可以具有对宏的正向引用。在调用它们之前，您不必声明它们。
>
> Source Insight 使用其符号查找引擎在执行宏和用户调用宏命令时解析对宏函数的引用。因此，符号查找规则适用于宏名称绑定
>
> 您还可以在编辑时使用各种符号查找技术来定位宏函数

## 语句

- 宏由各个语句组成。由两个或多个语句组成的复合语句用大括号 <font color=red>{ }</font> 括起来。 
- 语句可以是下列语句之一：
    - 宏语言语句元素，如 if 或 while 语句。这些语句将在后面介绍。
    - 对用户定义的宏函数的调用。您可以在宏源文件中定义和保存宏函数。
    - 对内部宏函数（如 GetCurrentBuf）的调用。Source Insight 提供了许多内置函数。它们将在后面的部分中介绍。
    - 对 Source Insight 用户命令的调用，例如 Line_Up 或 Copy。Source Insight 中的所有用户级命令都可用于宏语言。若要调用用户命令，请单独使用命令的名称，不带参数。不使用括号。如果命令名称中包含嵌入的空格，则必须将空格替换为下划线 （_） 字符。例如，若要调用“全选”命令，该语句应类似于 Select_All。
- 您可以使用 <font color=red>stop </font>语句停止执行。

> 如果从宏运行带有对话框的 Source Insight 命令，则会出现该对话框。
>
> 函数和变量名称不区分大小写。
>
> 语句语法通常与 C 或 Java 中的语法相同，只是不需要分号，并且会忽略分号。

- 语句元素

    | Statement             | Description                                                 |
    | --------------------- | ----------------------------------------------------------- |
    | break                 | 退出 while 循环                                             |
    | if (cond)… else       | 测试条件。                                                  |
    | continue              | 在顶部继续 while 循环。                                     |
    | global                | Declares a global variable.                                 |
    | return n <or> return; | 从函数返回 n。如果 return 后面跟着分号，则返回 nil 字符串。 |
    | stop                  | 停止执行宏。                                                |
    | strictvars true/false | 打开或关闭严格变量声明模式。                                |
    | var                   | 声明函数局部变量。                                          |
    | while (cond)          | cond 为 true 时循环。                                       |

## 变量

> 变量的定义方式是第一次给它们赋值。使用默认设置时，无需在使用变量之前声明变量。但是，您可以使用 var 语句显式声明变量。此外，可以使用 strictvars 语句要求变量声明。

您可以通过首次为其赋值来创建变量。例如：

```c
i = 0 // 创建值为 0 的新变量 i
```


如果 strictvars 模式处于打开状态，则上述语句将导致错误，因为该变量在声明之前就被使用了。

可以使用 var 语句声明变量。例如：

```c
var i // 声明新变量 i
```


变量名称不区分大小写。变量名称必须以字母或下划线开头，而不是以数字开头。

大多数时候，变量的作用类似于字符串。没有像 C 中那样的类型，也不需要声明变量。这使得使用变量变得非常容易。无需进行转换或强制转换。此外，无需字符串内存管理。

### 声明变量

> var 语句声明一个局部变量，并将其值设置为空字符串 （nil）。

```C
var variable_name
var a, b, c    // declares three variables
```

> 局部变量不能在其函数之外访问。
>
> 如果 strictvars 模式处于关闭状态（默认），则不必声明变量，但这样做有几个好处：
>
> 语法格式在显示中用于引用它。 
>
> 变量初始化为空字符串值 nil，因此在未初始化的情况下使用时，它们不会与文本值混淆。

### strictvars 模式

> strictvars 语句打开或关闭 strictvars 模式。如果 strictvars 模式处于打开状态，则必须在使用变量之前声明变量。默认情况下，strictvars 模式处于关闭状态。

strictvars 语句语法为：

```C
strictvars <value>
```

其中 true 或 “<value>1” 启用模式，或 false 或 “0” 禁用模式。

例如：

```C
strictvars true
i = 0 // 错误：变量 i 尚未声明

```

### 变量初始化

变量只需为其赋值即可初始化（除非启用了 strictvars 模式）。将变量初始化为空字符串可能很有用。为此使用了一个名为 nil 的特殊常量。

```C
S = nil    // s is set to the empty string
S = ""     // same thing
```

### 全局变量

> 使用 var 语句声明的变量或通过赋值创建的变量被视为包含该语句的函数的局部变量。不能在定义局部变量的函数之外访问局部变量。函数返回后，局部变量将不复存在。但是，全局变量具有全局范围而不是局部范围。这意味着您可以从多个函数访问它，并且其值在函数返回或整个宏停止运行后存在。
>
> 定义全局变量后，只要 Source Insight 会话持续，其值就会保留。也就是说，它们可以包含宏或事件函数调用之间的信息。当您退出 Source Insight 时，它们会丢失。

global 语句用于声明全局变量。例如：

```C
global a
```

### 变量值

如果标识符是已定义变量的名称，则生成变量值。否则，标识符名称将用作文本字符串。例如：

```c
s = abc  // same as s = "abc" if abc is not defined

abc = Hello
s = abc     // same as s = "Hello" (if Hello is not defined!)
s = "abc"   // s equals "abc"

```

### 扩展字符串中的变量

>  您可以使用特殊的 @ 字符将变量插入到另一个字符串中。当变量名称出现在文本字符串中，并且变量名称被 @ 字符括起来时，Source Insight 会将@variable@替换为变量值。

例如：

```C
s = "Hey, @username@, don't break the build again!"
```

> 此示例将 @username@ 替换为字符串中变量 username 的值。
>
> 您可以使用反斜杠 "\" 或同时使用两个 "@" 字符来转义 @ 字符。例如：

```C
s = "Mail info@@company.com for information."
```

### 值运算

> 即使变量表示为字符串，也可以对它们执行算术运算）。

```C
s = "1"
x = s + 2       // x now contains the string "3"
y = 2 * x + 5   // x now contains "11"
```

### 字符串索引

> 您可以索引变量，就好像它们是字符数组一样。指数从零开始。例如：

```C
s = "abc"
x = s[0]    // x now contains the string "a"
```

### 记录变量

> 虽然不支持类等数据结构，但 Source Insight 支持记录变量。记录变量实际上是“name=value”对的分隔列表。记录变量中没有预定义的字段顺序，在使用它们之前不必声明它们。
>
> 记录变量的使用方式与 C 语言中使用“结构”的方式相同，记录字段使用 .格式通过点 （.） 运算符引用<recordvar><fieldname>。
>
> 例如，这将从符号查找记录变量中读取符号文件位置的名称

```C
Filename = slr.file       // get file field of slr
LineNumber = slr.lnFirst  // get lnFirst field of slr
userinfo.name = myname; // set "name" field of userinfo

userinfo = nil   // make a new empty record
userinfo.name = "Jeff"   // begin adding fields
```

### 记录变量存储

> 记录变量仅存储为字符串。每个字段都存储为“fieldname=value”对，用分号分隔。
>
> 例如

```C
name="Joe Smith";age="34";experience="guru"
```

### 数组

> Source Insight 宏语言不支持数组变量。但是，文件缓冲区可以用作动态数组。缓冲区中的每一行都可以表示一个数组元素。此外，记录变量可以存储在每一行中，为您提供记录数组。
>
> 下一节将介绍缓冲区函数。一些有用的函数是用于创建和销毁缓冲区的 NewBuf 和 CloseBuf;以及缓冲区行函数：GetBufLine、PutBufLine、InsBufLine、AppendBufLine、DelBufLine 和 GetBufLineCount。还可以调用 NewWnd 将数组缓冲区放在窗口中，以便查看数组内容。
>
> 此示例创建记录的缓冲区数组。

```C
hbuf = NewBuf()
rec = "name=\"Joe Smith\";age=\"34\";experience=\"guru\""
AppendBufLine(hbuf, rec)
rec = "name=\"Mary X"\";age=\"31\";experience=\"intern\""
AppendBufLine(hbuf, rec)
// hbuf now has 2 records in it
...
rec = GetBufLine(hbuf, 0) // retrieve element at index 0
PutBufLine(hbuf, 1, rec)  // store element at index 1
CloseBuf(hbuf)
```

## 特殊常量

> 在宏运行时，始终定义某些常量值。与所有其他标识符一样，常量名称不区分大小写。

| Constant | Value                          |
| -------- | ------------------------------ |
| True     | "1"                            |
| False    | "0"                            |
| Nil      | "" – the empty string.         |
| hNil     | "0" – an invalid handle value. |
| invalid  | "-1" – an invalid index value. |

## 运算符

> 大多数二进制运算符与 C 中的运算符相同，运算符优先级与 C 相同。您还可以使用括号对表达式进行分组。

| Operator    | Meaning                                             |
| ----------- | --------------------------------------------------- |
| + and -     | add and subtract                                    |
| * and /     | multiply and divide                                 |
| !           | Invert or "Not". E.g. !True equals  False.          |
| ++i and i++ | pre and post increment                              |
| --i and i-- | pre and post decrement                              |
| \|\|        | logical OR operation                                |
| &&          | logical AND operation                               |
| !=          | logical NOT EQUAL operation                         |
| ==          | logical EQUAL operation                             |
| <           | less than                                           |
| >           | greater than                                        |
| <=          | less than or equal to                               |
| >=          | greater than or equal to                            |
| #           | 字符串连接 - eg: "dog" # "cat"                      |
| "@var@"     | 变量扩展 - 在带引号的字符串内用于扩展字符串中的变量 |

## 条件和循环：if-else 和 while

> Source Insight 宏语言支持用于条件测试和循环的 if 和 while 语句。

### if 语句

> if 语句用于测试条件并基于该条件执行一条或多条语句。if 语句的语法如下：

```C
if (condition)
   statement    // condition is true
    
if (condition)
   {
   statement1
   statement2
   }

if (condition)
   statement1    // condition is true
else
   statement2    // condition is false

```

### while语句

> while 语句在条件为 true 时循环。while 语句的语法如下

```C
while (condition)
   {
   statement1
   statement2
   }

```

### Break 和 Continue

> break 语句用于退出 while 循环。如果嵌套了多个 while 循环，则 break 语句会从最里面的循环中分离出来。

```C
while (condition)
   {
   if (should_exit)
       break        // exit the while loop
   …
   }
```

> continue 语句在循环的顶部再次继续，就在计算条件表达式之前。

```C
while (condition)
   {
   if (skip_rest_loop)
       continue         // continue at the top of the while loop
   …
   }
```

### 条件判断

> Source Insight 每次都会评估整个条件表达式。表达式不会短路。这与计算 C 条件表达式的方式有一个重要区别。在 C 中，可以部分计算表达式。请考虑以下条件表达式：

```C
if (hbuf != hNil)
    if (GetBufLineCount(hbuf) > x)
```

## 命名约定

> 本宏指南中描述的变量和函数参数使用以下约定命名。

| Name            | Meaning                                                |
| --------------- | ------------------------------------------------------ |
| s and sz        | 字符串                                                 |
| ch              | 单字符串。如果字符串中有多个字符，则仅使用第一个字符。 |
| ich             | 字符串中的字符或行中的字符的从零开始的索引             |
| ichFirst        | 字符范围中的第一个索引                                 |
| ichLast         | 字符范围中的最后一个索引（含）                         |
| ichLim          | 限制指数 - 超过一个范围内的最后一个指数                |
| cch             | 字符数                                                 |
| fThing          | “f”表示标志或布尔值。fThing 表示“事物”是 True。        |
| TRUE            | 非零值，例如“1”                                        |
| FALSE           | 零值，即“0”                                            |
| ln              | 从零开始的行号                                         |
| lnFirst         | 范围中的第一行编号                                     |
| lnLast          | 范围中的最后一行编号（含）                             |
| lnLim           | limit - 超过范围中最后一行编号的 1 个                  |
| hbuf            | 文件缓冲区的句柄                                       |
| hwnd            | 窗口的句柄                                             |
| hprj            | 项目的句柄                                             |
| hsyml           | 符号列表的句柄                                         |
| Any other names | 常规字符串变量                                         |

## 结构体说明

> 结构类似于 C 数据结构。记录变量实际上是“name=value”对的分隔列表。记录变量的使用方式与在 C 语言中使用“结构”的方式相同
>
> 记录变量由一些内部宏函数返回，因为它是返回多个值的便捷方法。 
>
> 本节介绍 Source Insight 中的内部宏函数使用的记录结构。

### Bookmark结构

> 书签指向文件中的位置。Bookmark 记录包含以下字段：

| Field        | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| Name         | 书签名称。                                                   |
| File         | 书签位置的文件名。                                           |
| ln           | 行号位置。                                                   |
| ich          | 行上的字符索引（从零开始）。                                 |
| Symbol       | 书签锚定到的附近符号定义。这通常是封闭函数或类。             |
| lnOffset     | 与符号的行号偏移量（距离）。                                 |
| SymbolType   | 符号的类型。                                                 |
| FtpSiteName  | 如果书签指向 FTP 主机上托管的文件，则这是 FTP 面板中使用的“站点名称”。如果书签指向本地文件，则此字段为空。 |
| FtpRemoteDir | 如果书签指向 FTP 主机上托管的文件，则这是主机上的远程目录名称。如果书签指向本地文件，则此字段为空。 |

### Bufprop 结构

> Bufprop结构包含文件缓冲区属性。它由 GetBufProps 返回。Bufprop 记录包含以下字段：

| Field           | Description                                       |
| --------------- | ------------------------------------------------- |
| Name            | 缓冲区名称（即文件名）                            |
| fNew            | 如果 buffer 是新的、未保存的缓冲区，则为 True。   |
| fDirty          | 如果缓冲区在保存或打开后已被编辑，则为 True。     |
| fReadOnly       | 如果缓冲区是只读的，则为 True。                   |
| fClip           | 如果缓冲区是出现在“剪辑窗口”中的剪辑，则为 True。 |
| fMacro          | 如果缓冲区是宏文件，则为 True。                   |
| fRunningMacro   | 如果缓冲区是当前正在运行的宏文件，则为 True。     |
| fCaptureResults | 如果缓冲区包含捕获的自定义命令输出，则为 True。   |
| fSearchResults  | 如果缓冲区包含搜索结果，则为 True。               |
| fProtected      | 如果缓冲区受到保护并保留供内部使用，则为 True。   |
| lnCount         | 缓冲区的行数。                                    |
| Language        | 缓冲区的编程语言。语言由文件的文档类型决定。      |
| DocumentType    | 缓冲区的文档类型。                                |

### DIM结构

> DIM 记录描述水平和垂直像素尺寸。

| Field | Description            |
| ----- | ---------------------- |
| Cxp   | X 像素计数（水平大小） |
| Cyp   | Y 像素计数（垂直大小） |

### Link 结构

> 链接记录描述文件中由源链接指向的位置。链接记录包含以下字段：

| Field | Description                                                  |
| ----- | ------------------------------------------------------------ |
| File  | 文件名                                                       |
| ln    | 行号 - 仅当文件未更改时才有效。如果在文件中插入或删除行，则行号将关闭。但是，您可以再次调用 GetSourceLink 以获取更新的行号。 |

## ProgEnvInfo 结构

> ProgEnvInfo 记录包含有关运行 Source Insight 的环境的信息。 

| Field                   | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| ProgramDir              | Source Insight 程序目录，用于存储程序文件。                  |
| TempDir                 | 临时文件目录。                                               |
| BackupDir               | 备份目录，Source Insight 用于存储您保存的文件的备份。        |
| ClipDir                 | 保存剪辑的目录。                                             |
| ProjectDirectoryFile    | 项目目录文件的名称。项目目录文件包含已打开的所有项目的列表。 |
| ConfigurationFile       | 当前处于活动状态的单个配置文件的名称。如果主配置文件对单独的部分使用单独的文件，则此条目将为空。 |
| MasterConfigurationFile | 主配置文件的名称。                                           |
| ShellCommand            | 命令 shell 的名称。命令 shell 取决于您运行的操作系统版本。   |
| UserName                | 注册用户的名称。                                             |
| UserOrganization        | 注册用户的组织。                                             |
| SerialNumber            | 已注册的许可证序列号                                         |

## ProgInfo 结构

> ProgInfo 记录描述正在运行的 Source Insight 版本。它包含以下字段：

| Field             | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| ProgramName       | 程序的名称（即“Source Insight”）                             |
| versionMajor      | 主版本号。如果版本号为 1.02.0003，则主要版本为 1。           |
| versionMinor      | 次要版本号。如果版本号为 1.02.0003，则主要版本为 2。         |
| versionBuild      | 版本的内部版本号。如果版本号为 1.02.0003，则内部版本号为 3。 |
| CopyrightMsg      | Source Dynamics 版权消息。                                   |
| fTrialVersion     | 如果程序是试用版，则为 True。                                |
| fBetaVersion      | 如果程序是 Beta 版本，则为 True。                            |
| ExeFileName       | 可执行文件的名称。                                           |
| cchLineMax        | 源文件行中允许的最大字符数。                                 |
| cchPathMax        | 路径名中支持的最大字符数。                                   |
| cchSymbolMax      | 符号全名中允许的最大字符数。                                 |
| cchCmdMax         | 命令、自定义命令或宏命令名称中允许的最大字符数。             |
| cchBookmarkMax    | 书签名称中允许的最大字符数。                                 |
| cchInputMax       | 对话框文本输入字段中允许的最大字符数。                       |
| cchMacroStringMax | 宏字符串变量中允许的最大字符数。                             |
| lnMax             | 源文件中支持的最大行数。                                     |
| integerMax        | 支持的最大整数值。                                           |
| integerMin        | 支持的最小整数值（负数）。                                   |

## Rect 结构

> Rect 记录描述矩形的坐标。Rect 记录包含以下字段

| Field  | Description        |
| ------ | ------------------ |
| Left   | 矩形的左像素坐标   |
| Top    | 矩形的顶部像素坐标 |
| Right  | 矩形的右像素坐标   |
| Bottom | 矩形的底部像素坐标 |

## Selection 结构

> “选择”记录描述窗口中文本选择的状态。选择记录包含以下字段：

| Field                                              | Description                                                  |
| -------------------------------------------------- | ------------------------------------------------------------ |
| lnFirst                                            | 第一行编号                                                   |
| ichFirst                                           | lnFirst 行上第一个字符的索引                                 |
| lnLast                                             | 最后一行编号                                                 |
| ichLim                                             | lnLast 中给定的行上最后一个字符的限制索引（超过最后一个字符） |
| fExtended                                          | 如果选择扩展为包含多个字符，则为 TRUE。如果所选内容是简单的插入点，则为 FALSE。这与以下表达式相同：（sel.fRect \|\| sel.lnFirst ！= sel.lnLast \|\| sel.ichFirst ！= sel.ichLim） |
| fRect                                              | 如果选择是矩形（块样式），则为 TRUE。如果所选内容是字符的线性范围，则为 FALSE。 |
| The following fields only apply if fRect is  TRUE: |                                                              |
| xLeft                                              | 矩形在窗口坐标中的左像素位置。                               |
| xRight                                             | 矩形右边缘在窗口坐标中的像素位置。                           |

## Symbol 结构

> 符号记录描述符号声明。它指定符号的位置和类型。它用于唯一描述项目中或打开的文件缓冲区中的符号。
>
> 符号记录由多个函数返回，符号记录用作多个函数的输入。符号记录包含以下字段：

| Field    | Description                                                  |
| -------- | ------------------------------------------------------------ |
| Symbol   | 完整的符号名称。符号名称实际上是一个路径。每个符号名称都划分为路径组件，这些组件由点 （.） 字符分隔。例如，符号名称可能是“myclass.member1”。在此示例中，“member1”包含在“myclass”中。 |
| Type     | 符号的类型（例如“函数”、“类”等）                             |
| Project  | 找到符号的项目的完整路径                                     |
| File     | 找到符号的文件的完整路径                                     |
| lnFirst  | 符号声明的第一行编号                                         |
| lnLim    | 符号声明的限制行号                                           |
| lnName   | 符号名称出现在声明中的行号。                                 |
| ichName  | 声明中 lnName 行中符号名称的字符索引。                       |
| Instance | 文件中符号的实例编号路径。 例如，符号的第一个匹配项是实例 0，第二个是实例 1，依此类推。 |

## SYSTIME 结构

> SYSTIME 记录描述系统时间。它由 GetSysTime 函数返回。

| Field        | Description                                         |
| ------------ | --------------------------------------------------- |
| time         | 字符串格式的一天中的时间。                          |
| date         | 以字符串形式表示的星期几、日、月和年。              |
| Year         | current year.                                       |
| Month        | current month number. January is 1.                 |
| DayOfWeek    | day of week number. Sunday is 0, Monday is 1,  etc. |
| Day          | day of month.                                       |
| Hour         | current hour.                                       |
| Minute       | current minute.                                     |
| Second       | current second                                      |
| Milliseconds | current milliseconds                                |

## 内部宏函数

> Source Insight 提供了许多内置的内部宏函数。有用于操作字符串、文件缓冲区、窗口、项目和符号信息的函数。
>
> 调用内部宏函数的语法与调用用户定义的宏函数的语法相同。例如：

```c
hbuf = GetCurrentBuf()   // call GetCurrentBuf function
```

## 字符串函数

> 提供了字符串函数以允许字符串操作。您不必担心字符串的内存管理，也不必担心声明缓冲区来保存字符串。

### AsciiFromChar (ch)

> 返回给定字符 ch 的 ASCII 值。如果 ch 是包含多个字符的字符串，则仅返回第一个字符。

### cat (a, b)

> 将字符串 a 和 b 连接在一起并返回结果。
>
> 或者，您可以在表达式中使用中缀字符串连接运算符 （#）。例如：

```C
full_name = first_name # " " # last_name
```

### CharFromAscii (ascii_code)

> 返回一个字符串，该字符串包含与 ascii_code 中的 ASCII 代码相对应的单个字符。

### islower  (ch)

> 如果给定字符 ch 为小写，则返回 TRUE。如果 ch 是包含多个字符的字符串，则仅测试第一个字符。

### IsNumber  (s)

> 如果字符串 s 包含数字字符串，则返回 TRUE。在算术表达式中使用非数字字符串时会导致运行时错误。

### isupper  (ch)

> 如果给定字符 ch 为大写，则返回 TRUE。如果 ch 是包含多个字符的字符串，则仅测试第一个字符。

### strlen (s)

> 返回字符串的长度。

### strmid (s,  ichFirst, ichLim)

> 返回从 ichFirst 到 （但不包括 ichLim）范围内 s 的中间字符串。即 s[ichFirst] 到 s[ichLim - 1]。如果 ichFirst 等于 ichLim，则指定一个空字符串范围。

### strtrunc (s,  cch)

> 返回截断为 cch 字符数的字符串。

### tolower (s)

> 返回给定字符串的小写版本。

### toupper (s)

> 返回给定字符串的大写版本。

## 用户输入和输出功能

> 用户输入和输出函数允许您从用户那里获取输入，或在消息窗口中显示输出。

### Ask  (prompt_string)

> 使用显示字符串 prompt_string的消息框窗口提示用户。“询问”消息框具有“确定”按钮和“取消”按钮。单击“取消”按钮可停止宏。

### AssignKeyToCmd(key_value, cmd_name) 

> 将key_value分配给由 cmd_name 命名的命令。随后，当用户按下key_value时，将调用该命令。
>
> key_value 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_value转换为字符。
>
> cmd_name 是命令的字符串名称。 
>
> 例：

```C
key = GetKey();
AssignKeyToCmd(key, "Open Project");
```

### Beep ()

> 发出一声哔哔声。

### CharFromKey  (key_code)

> 返回与给定键代码关联的字符。如果key_code不是常规字符键代码，则返回零。
>
> key_code 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_code转换为字符。

### CmdFromKey(key_value) 

> 返回当前映射到 key_value 的 Source Insight 命令的字符串名称。返回的命令是用户按key_value时调用的命令的名称。
>
> key_value 是由 GetKey 和 KeyFromChar 返回的数字键盘值。 
>
> 可以使用 CharFromKey 将key_value转换为字符。
>
> 例：

```C
key = GetKey();
cmd_name = CmdFromKey(key);
msg("That key will invoke the @cmd@ command.");
```

### EndMsg ()

> 删除由 StartMsg 启动的消息框。

### FuncFromKey  (key_code)

> 从功能键代码中返回功能键编号（F1 - F12 为 1 - 12）。如果 key_code 不是功能键代码，则返回零。
>
> key_code 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_code转换为字符。

### GetChar ()

> 等待用户按下某个键并返回一个字符。

### GetKey ()

> 等待用户按下某个键并返回键代码。密钥代码是 Source Insight 与每个密钥关联的特殊数值。可以使用 CharFromKey 函数将键代码映射到字符。

### GetSysTime(fLocalTime) 

> 返回一个 SYSTIME 记录，其中包含时间和日期的字符串表示形式。 请参见：SYSTIME 结构。
>
> 如果 fLocalTime 不为零，则返回本地时间，否则返回系统时间（以协调世界时 （UTC） 表示）。

### IsAltKeyDown (key_code)

> 如果 ALT 键关闭 key_code，则返回 TRUE。键代码包含 CTRL 和 ALT 键状态。
>
> key_code 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_code转换为字符。

### IsCtrlKeyDown (key_code)

> 如果 Ctrl 键按下 key_code，则返回 TRUE。键代码包含 CTRL 和 ALT 键状态。
>
> key_code 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_code转换为字符。

### IsFuncKey  (key_code)

> 如果 key_code 是功能键，则返回 TRUE，如果不是，则返回 FALSE。
>
> key_code 是由 GetKey 和 KeyFromChar 返回的数字键盘值。可以使用 CharFromKey 将key_code转换为字符。

### KeyFromChar(char, fCtrl, fShift, fAlt)

> 返回一个键值，给定一个字符和修饰符键状态。键值是 GetKey 返回的数字键盘值。可以使用 CharFromKey 将键值转换为字符。
>
> 输入：
>
> char - 击键的字符部分。它不区分大小写。
>
> fCtrl - 如果包含 CTRL 键，则为非零。 
>
> fShift - 如果包含 Shift 键，则为非零。 
>
> fAlt - 如果包含 ALT 键，则为非零。
>
> char 参数可以有一些特殊值：

| Char Value                                              | Meaning                                          |
| ------------------------------------------------------- | ------------------------------------------------ |
| a-z                                                     | Simple alpha characters                          |
| Fx                                                      | Function Key number x; e.g. F10                  |
| Nx                                                      | Numeric keypad character x; e.g. N+ for "+"  key |
| Up, Down, Left, Right                                   | Arrow keys                                       |
| Page Up, Page Down Insert, Delete Home, End, Tab, Enter | Other special keys                               |

 Examples of Key  Assignments:

```C
// assign Ctrl+C to Page Down command:
key = KeyFromChar("c", 1, 0, 0);  // Ctrl+C 
AssignKeyToCmd(key, "Page Down");
 

// assign F9 to Close Project command:
key = KeyFromChar("F9", 0, 0, 1);  // Alt+F9 
AssignKeyToCmd(key, "Close Project");

```

Examples of input  functions:

```
// input a keypress and decode it
key = GetKey()
if (IsFuncKey(key))
   Msg cat("Function key = ", FuncFromKey(key))
if (IsAltKeyDown(key))
   Msg "Alt key down"
if (IsCtrlKeyDown(key))
   Msg "Ctrl key down"
ch = CharFromKey(key)
if (Ascii(ch) == 13)
   Msg "You pressed Enter!"
else if (toupper(ch) == 'S')
   Search_Files
...
```

### Msg (s)

> 显示显示字符串 s 的消息窗口。消息框中有一个“取消”按钮，用户可以单击该按钮来停止宏。

### StartMsg  (s)

> 显示显示字符串 s 的消息窗口。消息框中有一个“取消”按钮，用户可以单击该按钮来停止宏。返回后，消息窗口将保持打开状态。

## 缓冲区列表函数

>缓冲区列表是文件缓冲区句柄的集合。Source Insight 应用程序中只有一个缓冲区列表。它包含所有开源文件的文件缓冲区句柄。可以使用缓冲区列表函数枚举所有文件缓冲区。

### BufListCount ()

> 此函数返回缓冲区列表中的缓冲区数。使用 BufListItem 访问特定索引位置的缓冲区句柄。

### BufListItem  (index)

> 此函数返回给定索引处的缓冲区句柄。缓冲区列表的大小由 BufListCount 返回。索引值从零开始，一直持续到比 BufListCount 返回的值少 1 的索引值。
>
> 此示例枚举所有打开的缓冲区句柄：

```C
cbuf = BufListCount()
ibuf = 0
while (ibuf < cbuf)
   {
   hbuf = BufListItem(ibuf)
    // ... do something with buffer hbuf 
   ibuf = ibuf + 1
   }
```

## 文件缓冲区函数

> 文件缓冲区函数用于创建和操作文件缓冲区及其中的文本。文件缓冲区是文本文件的加载图像。文件缓冲区由用户编辑，然后使用“保存”命令保存回磁盘。
>
> 许多文件缓冲区函数都使用缓冲区句柄 （hbuf）。这些是用于打开文件缓冲区的句柄。hbuf 通常是一个小整数值。hbuf 值为 hNil（零）表示错误。

### AppendBufLine (hbuf, s)

> Appends a new line of text s to the file buffer hbuf.

### ClearBuf  (hbuf)

> 清空缓冲区 hbuf，使其不包含任何行。

### CloseBuf  (hbuf)

> Closes a file buffer. Hbuf is the buffer handle.

### CopyBufLine  (hbuf, ln)

> 将行 ln 从文件缓冲区 hbuf 复制到剪贴板。

### DelBufLine  (hbuf, ln)

> 从文件缓冲区 hbuf 中删除行 ln。

### GetBufHandle (filename)

> 返回名称为 filename 的打开文件缓冲区的句柄。此函数搜索所有打开的文件缓冲区，以查找与给定 filename 参数匹配的文件缓冲区。GetBufHandle 如果找不到具有给定文件名的缓冲区，则返回 hNil。

### GetBufLine  (hbuf, ln)

> 返回给定文件缓冲区 hbuf 中行 ln 的文本。

### GetBufLineCount (hbuf)

> 返回文件缓冲区中的文本行数。Hbuf 是文件缓冲区句柄。

### GetBufLineLength (hbuf, ln)

> 返回给定文件缓冲区 hbuf 中 ln 行的字符数。

### GetBufLnCur  (hbuf)

> 返回文件缓冲区 hbuf 中用户所选内容的当前行号。如果给定的文件缓冲区尚未显示在源文件窗口中，则会生成宏错误。

### GetBufName  (hbuf)

> 返回与文件缓冲区关联的文件的名称。Hbuf 是文件缓冲区句柄。

### GetBufProps  (hbuf)

> 返回一个 Bufprop 记录，其中包含给定缓冲区的属性。 请参阅：Bufprop 结构。

### GetBufSelText (hbuf)

> 以字符串形式返回文件缓冲区 hbuf 中的选定字符。最多返回一行文本。这对于获取单词选择的文本很有用。如果给定的文件缓冲区尚未显示在源文件窗口中，则会生成宏错误。

### GetCurrentBuf ()

> 返回当前缓冲区的句柄。当前缓冲区是显示在最前面的源文件窗口中的文件缓冲区。如果没有当前缓冲区（即没有开源文件窗口），则返回 hNil。

### InsBufLine  (hbuf, ln, s)

> 在文件缓冲区 hbuf 中为行号 ln 插入一行新的文本 s。

### IsBufDirty  (hbuf)

> 如果缓冲区被修改过但还没有保存，则返回 True。被修改过的缓冲区指的是自从它被打开或保存以来被编辑过的缓冲区。被修改过的缓冲区包含了还没有保存的更改。

### IsBufRW  (hbuf)

> 如果给定的缓冲区是可读写的，则返回 True。如果缓冲区是只读的，则返回 False。

### MakeBufClip  (hbuf, fClip)

> 如果 fClip 为 True，则将文件缓冲区 hbuf 转换为剪贴板缓冲区。之后，该缓冲区将在剪贴板窗口中显示为常规剪贴板。如果 fClip 为 False，则将缓冲区转换为常规的非剪贴板文件缓冲区。剪贴板缓冲区在 Source Insight 退出时会自动保存到 Source Insight 程序目录的 Clips 子目录中。

### NewBuf  (name)

> 创建一个新的空文件缓冲区，并返回文件缓冲区的句柄（hbuf）。新缓冲区的名称由 name 参数指定。如果由于错误而无法创建缓冲区，则 New 返回 hNil

### OpenBuf  (filename)

> 在文件缓冲区中打开名为 filename 的文件，并返回文件缓冲区的句柄。如果无法打开文件，OpenBuf 将返回 hNil。

### OpenMiscFile (filename)

> 打开名为 filename 的文件。所采取的操作取决于打开的文件类型。例如，打开带有 .CF3 扩展将加载一个新的配置文件。打开项目文件 （.PR 扩展）将打开该项目。OpenMiscFile 如果成功，则返回 TRUE，否则返回 FALSE。

### PasteBufLine (hbuf, ln)

> 将 clipboad 内容粘贴到文件缓冲区 hbuf 中 ln 行的前面。

### PrintBuf  (hbuf, fUseDialogBox)

> 在打印机上打印给定的文件缓冲区。    如果 fUseDialogBox 为 True，则首先显示“打印”对话框。否则，在默认打印机上打印文件时无需用户交互。

### PutBufLine  (hbuf, ln, s)

> 将文件缓冲区 hbuf 中行号 ln 的文本行替换为 s 中的字符串。

### RenameBuf  (hbuf, szNewName)

> 将给定的缓冲区重命名为 szNewName。磁盘上的文件也会重命名。如果缓冲区是打开项目的成员，则在项目中重命名该文件。

### SaveBuf  (hbuf)

> 将文件缓冲区保存到磁盘。Hbuf 是缓冲区句柄。

### SaveBufAs  (hbuf, filename)

> 使用不同的文件名保存文件缓冲区。Hbuf 是缓冲区句柄。Filename 是新文件的名称。

### SetBufDirty  (hbuf, fDirty)

> 将给定缓冲区的脏状态设置为 fDirty。脏缓冲区是自打开或保存以来已编辑的缓冲区。脏缓冲区包含尚未保存的更改。
>
> 当用户关闭脏缓冲区时，Source Insight 会提示保存文件。您可以使用此函数取消弄脏缓冲区，以便系统不会提示用户保存它。

### SetBufIns  (hbuf, ln, ich)

> 将光标位置插入点设置为文件缓冲区 hbuf 中字符索引 ich 处的行号 ln。如果给定的文件缓冲区尚未显示在源文件窗口中，则会生成宏错误。

### SetBufSelText (hbuf, s)

> 将文件缓冲区 hbuf 中当前选定的字符替换为字符串 s。这对于替换单词选择的文本很有用。如果给定的文件缓冲区尚未显示在源文件窗口中，则会生成宏错误。
>
> 这是将新文本插入缓冲区的最简单方法。例如，以下代码将“新文本”插入到当前缓冲区的当前插入位置：

```C
hbuf = GetCurrentBuf()
SetBufSelText(hbuf, "new text")
```

### SetCurrentBuf (hbuf)

> 将活动文件缓冲区设置为句柄为 hbuf 的缓冲区。当前缓冲区是显示在最前面的源文件窗口中的文件缓冲区。 

## 环境和过程函数

> 环境和进程函数允许您获取和设置注册表和环境值;以及运行内部和外部命令

### GetEnv  (env_name)

> 返回 env_name 中给定的环境变量的值。如果环境变量不存在，则返回空字符串。

### GetReg  (reg_key_name)

> 返回与名为 reg_key_name 的注册表项关联的值。键值存储在键路径下：HKEY_CURRENT_USER/Software/Source Dynamics/Source Insight/4.0。如果键不存在，则返回空字符串。
>
> SetReg 和 GetReg 函数提供了一种在会话之间存储自己的 Source Insight 相关信息的方法。

### IsCmdEnabled (cmd_name)

> 如果当前启用了 cmd_name 中指定的命令，则返回 TRUE。如果 Source Insight 由于程序的状态而无法运行该命令，则不会启用该命令。例如，如果没有开源文件窗口，则不会启用“保存”命令。

### PutEnv  (env_name, value)

> 将名为 env_name 的环境变量设置为 value。

### RunCmd  (cmd_name)

> 运行 cmd_name 指定的命令。这允许您运行特殊命令，即自定义命令，这些命令是使用“自定义命令”命令定义的。

### RunCmdLine  (sCmdLine, sWorkingDirectory, fWait)

> 在 sCmdLine 中生成给定的命令行字符串。如果成功，则返回非零，如果错误，则返回零。
>
> 如果 sWorkingDirectory 不是 Nil，则使用工作目录。如果 sWorkingDirectory 为 Nil，则使用当前项目主目录 i。
>
> 如果 fWait 不为零，则在进程完成之前，函数不会返回。否则，它会立即返回。如果该进程是另一个 Windows 应用程序，则无论 fWait 如何，它都会立即返回。

### SetReg  (reg_key_name, value)

> 设置与名为 reg_key_name 的注册表项关联的值。键值存储在键路径下：HKEY_CURRENT_USER/Software/Source Dynamics/Source Insight/4.0。如果密钥尚不存在，则创建该密钥。 
>
> SetReg 和 GetReg 函数提供了一种在会话之间存储自己的 Source Insight 相关信息的方法。

### ShellExecute (sVerb, sFile, sExtraParams,  sWorkingDirectory, windowstate) 

> 对给定文件执行“ShellExecute”函数。这使您可以告诉 Windows shell 对指定文件执行操作。
>
> ShellExecute 的好处是，您不需要知道注册了哪个特定应用程序来处理特定类型的文件。有关技术背景信息，请参阅 Windows Shell API 文档中的“ShellExecute”函数。

ShellExecute 参数

| Parameter    | Meaning                                                      |
| ------------ | ------------------------------------------------------------ |
| sVerb        | A single word that specifies the action to be taken.  See the table below for possible values. |
| sFile        | The filespec parameter can be any valid path. Use  double quotes around complex path names with embedded spaces. It can also be the  name of an exe­cutable file. |
| sExtraParams | Optional parameters:  It specifies the parameters to  be passed to the application that ultimately runs . The format is determined by  the verb that is to be invoked, and the application that runs. |
| sWorkingDir  | The working directory when the command runs. If empty,  then the project home directory is used. |
| windowstate  | An integer that specifies the size and state of the  win­dow that opens. Valid values are: 1 = normal, 2 = mini­mized, 3 =  maximized. |

### sVerb Values

The sVerb parameter is a single word string that specifies  the action to be taken by Shell Execute.

| sVerb Value       | Meaning                                                      |
| ----------------- | ------------------------------------------------------------ |
| edit              | 打开文件的编辑器。                                           |
| explore           | 该函数浏览指定的文件夹。                                     |
| open              | 该函数将打开指定的文件。该文件可以是可执行文件，也可以是文档文件。它也可以是一个文件夹。 |
| print             | 该函数打印指定的文档文件。如果 filespec 不是文档文件，则该函数将失败。 |
| properties        | 显示文件或文件夹的属性。                                     |
| find              | 启动“开始”菜单上的“查找文件”应用程序。                       |
| "" (empty string) | 将此参数传送到 ShellExecute。                                |

Examples:

```C
//To browse a web site: 
ShellExecute("open", "http://www.somedomain.com", "", "", 1)

//To open a document file:
ShellExecute("open", "somefile.doc", "", "", 1)

//To explore your Windows documents file folder: 
ShellExecute("explore", "C :\Documents and Settings", "", "", 1)

//To launch Internet Explorer: 
ShellExecute("", "iexplore", "", "", 1)
    
//To preview a file in Internet Explorer: 
ShellExecute("", "iexplore somefile.htm", "", "", 1)
    
//To search for files in the current project folder: 
ShellExecute("find",  filespec, "", "", 1)

```

## 窗口列表函数

### WndListCount ()

> 此函数返回窗口列表中的窗口数。使用 WndListItem 访问列表中特定索引处的窗口句柄。

### WndListItem  (index)

> 此函数返回给定索引处的窗口句柄。窗口列表的大小由 WndListCount 返回。索引值从零开始，一直持续到比 WndListCount 返回的值少 1 为止。
>
> 此示例枚举所有打开的窗口句柄：

```C
cwnd = WndListCount()
iwnd = 0
while (iwnd < cwnd)
   {
   hwnd = WndListItem(iwnd)
   // … do something with window hwnd 
   iwnd = iwnd + 1
   }
```

# Window Functions

> 窗口函数允许操作源文件窗口。文件缓冲区显示在源窗口中。hwnd 通常是一个小整数值。hwnd 为 hNil 表示错误。
>
> 这些函数使用窗口句柄 （hwnd） 参数。这些是开源文件窗口的宏级句柄。请注意，宏 hwnd 在概念上类似，但与 Microsoft Windows API 中的窗口句柄 HWND 并不完全相同。
>
> 每个源窗口中都有一个选择，用于描述选择的字符。有关详细信息，请参阅 GetWndSel （hwnd）。
>
> 在某些函数中，窗口句柄 （hwnd） 还可以表示系统级窗口（如应用程序窗口）的句柄。系统级窗口不包含文件缓冲区或选择。GetApplicationWnd 函数返回 Source Insight 应用程序窗口的句柄。

### CloseWnd  (hwnd)

> 关闭窗口 hwnd。关闭窗口不会关闭窗口中显示的文件缓冲区。

### GetApplicationWnd ()

> 将窗口句柄返回到 Source Insight 应用程序窗口。 
>
> 这与系统级窗口句柄不同。它是 Source Insight 宏级句柄值。
>
> 返回的句柄可以传递给不采用文件缓冲区或选择的函数，例如 GetWndDim 和 IsWndMax。

### GetCurrentWnd ()

> 返回活动的最前面的源文件窗口的句柄，如果没有打开的窗口，则返回 hNil。

### GetNextWnd  (hwnd)

> 返回窗口 Z 顺序中 hwnd 之后的下一个窗口句柄。这通常是上一个处于活动状态的窗口。如果没有其他窗口，GetNextWnd 将返回 hNil。
>
> 例如，如果 hwnd 是最顶部的窗口，则 GetNextWnd（hwnd） 将返回下一个窗口。请注意，如果使用 SetCurrentWnd 设置最前面的活动窗口，则对 GetNextWnd 的后续调用将受到影响。

### GetWndBuf  (hwnd)

> 返回窗口 hwnd 中显示的文件缓冲区的句柄。 

### GetWndClientRect (hwnd)

> 返回一个 Rect 记录，该记录包含给定窗口的客户端矩形。坐标在窗口的局部坐标系中给出。客户端矩形不包括窗口的框架或其他非工作区。 请参见：Rect  Record。

### GetWndDim  (hwnd)

> 返回 DIM 记录，该记录描述给定窗口 hwnd 的像素尺寸。 请参见：DIM 记录。
>
> 返回的水平尺寸仅是窗口文本区域的宽度。它不包括左边距或附加到源窗口的符号窗口。

### GetWndHandle (hbuf)

> 返回最前面窗口的窗口句柄，该窗口显示 hbuf 指定的文件缓冲区。如果文件缓冲区不在窗口中，GetWndHandle 将返回 hNil。 
>
> 由于文件缓冲区可能出现在多个窗口中，因此 GetWndHandle 按从前到后的顺序搜索所有窗口。因此，如果指定的文件缓冲区是活动窗口中的当前缓冲区，则始终返回该窗口的句柄。

### GetWndHorizScroll (hwnd)

> 返回窗口 hwnd 的水平滚动状态。水平滚动状态是滚动的像素数。

### GetWndLineCount (hwnd)

> 以行为单位返回窗口 hwnd 的垂直大小。这是窗口中可能可见的最大行数。如果文件缓冲区未填满整个窗口，GetWndLineCount 仍将返回最大行数。

### GetWndLineWidth (hwnd, ln, cch)

> 返回给定窗口中指定文本行的宽度。

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ln        | The line number that contains the text to be  mea­sured. If ln is out of range, then –1 is returned. |
| cch       | The count of characters to measure on the line. If cch  is set to –1, then the whole line length is  measured. |

> 此函数允许您测量给定窗口中字符的宽度。由于每个窗口使用的字体由文件的文档类型决定，因此文本的宽度可能因窗口而异。语法格式也会影响文本的宽度。
>
> 此函数可以与 ScrollWndHoriz 一起使用，以滚动窗口以显示特定字符。

例子
要在第 100 行处找到整条线的宽度：

```
dim = GetWndLineWidth(hwnd, 100, -1)
Msg ("Line 100 is " # dim.cxp # " pixels wide.")
```

### GetWndParent (hwnd) 

> 返回窗口的父窗口的句柄。如果没有父级，则返回 hNil。

### GetWndRect  (hwnd)

> 返回一个 Rect 记录，其中包含给定窗口的屏幕矩形坐标。矩形包括窗口框架和非工作区。 See: Rect 

### GetWndSel (hwnd)

> 返回 hwnd 指定的窗口的选择状态。选择状态在选择记录中返回。 请参见： Selection Record . 。

### GetWndSelIchFirst (hwnd)

> 返回窗口 hwnd 中所选内容中第一个字符的索引。

### GetWndSelIchLim (hwnd)

> 返回窗口 hwnd 中所选内容中最后一个字符之后的索引。

### GetWndSelLnFirst (hwnd)

> 返回窗口 hwnd 中所选内容的第一行号。GetWndSelLnLast (hwnd)

> 返回窗口 hwnd 中所选内容的最后一行号。

### GetWndVertScroll (hwnd)

> 返回窗口 hwnd 的垂直滚动状态。垂直滚动状态是显示在窗口顶部的行号。

### IchFromXpos (hwnd, ln, xp)

> 返回给定窗口中行号 （ln） 上给定像素 x 位置 （xp） 的字符索引。字符索引是指定行上字符的从零开始的索引。在调用此函数时，该行实际上不必显示在窗口中。 请参见：XposFromIch （hwnd， ln， ich）。

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ln        | 包含要测量的文本的行号。如果 ln 超出范围，则返回 –1。        |
| xp        | x 位置，相对于整个窗口的左边缘。如果 xp 超过行的宽度，则返回该行的字符总数。 |

### IsWndMax  (hwnd)

> 如果窗口 hwnd 当前最大化，则返回 TRUE。

### IsWndMin  (hwnd)

> 如果窗口 hwnd 当前已最小化，则返回 TRUE。

### IsWndRestored (hwnd)

> 如果窗口 hwnd 当前未最大化且未最小化，则返回 TRUE。

### LineFromYpos (hwnd, ypos)

> 返回位于给定窗口中给定 y 像素位置的行号。y 位置相对于窗口的上边缘。如果行号在窗口中不可见，则返回 -1。请参阅：YposFromLine （hwnd， ln）。

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ypos      | 相对于窗口顶部的 y 像素位置。Ypos 值在窗口顶部为零，在窗口底部为正值。如果 ypos 不在窗口内，则返回 -1。 |

### MaximizeWnd  (hwnd)

> Maximizes (or "zooms") the window specified by hwnd.

### MinimizeWnd  (hwnd)

> Minimizes (or "iconizes") the window specified by  hwnd.

### NewWnd  (hbuf)

> 创建一个新窗口，并在窗口中显示文件缓冲区 hbuf。NewWnd 返回窗口句柄，如果出现错误，则返回 hNil。

### ScrollWndHoriz (hwnd, pixel_count)

> 将窗口水平滚动 hwnd 以pixel_count为单位。 
>
> 如果 pixel_count 小于零，则滚动在行中向后滚动（屏幕内容向右滚动）。
>
> 如果 pixel_count 大于零，则滚动在行中向前滚动（屏幕内容向左滚动）。

### ScrollWndToLine (hwnd, ln)

> 滚动窗口 hwnd 以在窗口顶部显示行号 ln。

### ScrollWndVert (hwnd, line_count)

> 按给定的 line_count 量垂直滚动窗口 hwnd。 
>
> 如果 line_count 小于零，则在文件中向后滚动（屏幕内容向下滚动）。
>
> 如果 line_count 大于零，则滚动在文件中向前滚动（屏幕内容向上滚动）。

### SetCurrentWnd (hwnd)

> 设置最前面的活动窗口。Hwnd 是要激活的窗口句柄。

### SetWndRect  (hwnd, left, top, right, bottom) 

> 设置给定窗口的新位置。Z 顺序不受影响。给定的坐标位于窗口父窗口的局部像素坐标系中。如果窗口是应用程序窗口，则坐标系是全局屏幕像素坐标系。

### SetWndSel  (hwnd, selection_record)

> 将 hwnd 指定的窗口的选择状态设置为 selection_record 中给出的选择记录。请参阅：GetWndSel (hwnd)..请参阅 Selection Record . 。

### ToggleWndMax (hwnd)

> 在最大化大小和恢复大小之间切换窗口 hwnd。

### XposFromIch (hwnd, ln, ich)

> 返回给定窗口中行号 （ln） 上给定字符位置 （ich） 的像素 x 位置编号。x 位置相对于整个窗口的左边缘。在调用此函数时，该行实际上不必显示在窗口中。 请参阅：IchFromXpos （hwnd， ln， xp）。

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ln        | 包含要测量的文本的行号。如果 ln 超出范围，则返回 –1。        |
| ich       | 字符索引，它是指定行上字符的从零开始的索引。如果 ich 超过行上的字符数，则 |

### YposFromLine (hwnd, ln)

> 返回给定窗口中给定行号 （ln） 的像素 y 位置。y 位置相对于窗口的上边缘。如果行号在窗口中不可见，则返回 -1

 [See:  LineFromYpos (hwnd, ypos).](LineFromYpos_hwnd_ypos.htm#XREF_90609_LineFromYpos_hwnd)

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ln        | 包含要测量的文本的行号。如果 ln 超出文件的范围，或者在窗口中不可见，则返回 –1。 |

### YdimFromLine (hwnd,  ln)

> 返回给定窗口中给定行号 （ln） 的像素高度尺寸。如果行号在窗口中不可见，则返回 -1

 [See:  YposFromLine (hwnd, ln).](YposFromLine_hwnd_ln.htm#XREF_75245_YposFromLine_hwnd)

Inputs:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| hwnd      | The window.                                                  |
| ln        | 包含要测量的文本的行号。如果 ln 超出文件的范围，或者在窗口中不可见，则返回 –1。 |

## 书签功能

> 所有书签都保存在一个书签列表中。可以使用书签函数枚举所有书签，以及添加和删除书签。书签列表将保留在工作区文件中。

### BookmarksAdd (name, filename, ln, ich)

> 添加新书签。新书签名称为 name。  书签位置位于文件文件名中字符索引 ich 的行号 ln 处。
>
> 如果成功，则返回 True，如果错误，则返回 False。

### BookmarksCount ()

> 此函数返回书签列表中的书签数。使用 BookmarksItem 访问列表中特定索引处的书签。

### BookmarksDelete (name)

> 删除名为 name 的书名。 

### BookmarksItem (index)

> 此函数返回给定索引处的书签。书签列表的大小由 BookmarksCount 返回。索引值从零开始，一直持续到比 BookmarksCount 返回的值少 1 的索引值。
>
> 此示例枚举所有书签：

```c
cmark = BookmarksCount()
imark = 0
while (imark < cmark)
   {
   bookmark = BookmarksItem(imark)
   // … do something with bookmark 
   imark = imark + 1
   }
```

### BookmarksLookupLine (filename, ln)

> 在给定位置搜索书签。文件位于文件名中，行号为 ln。
>
> 返回 Bookmark 记录，如果未找到书签，则返回 nil。

### BookmarksLookupName (name)

> 搜索名为 name 的书签。 
>
> 返回 Bookmark 记录，如果未找到书签，则返回 nil

## 符号列表函数

> 符号列表是符号记录的从零开始的索引集合。 请参见：Symbol Record
>
> 某些符号访问函数返回符号列表句柄。
>
> 符号列表是分配的资源，在完成访问后，应使用 SymListFree 释放这些资源。Source Insight 会在宏终止时自动清理动态分配的资源。但是，如果分配了许多句柄而未释放未使用的句柄，则 Source Insight 可能会耗尽资源。

### SymListCount ()

> 此函数返回符号列表中的符号数。使用 SymListItem 访问列表中从零开始的特定索引处的符号记录。

### SymListFree  (hsyml)

> 此函数解除分配给定的符号列表。 

### SymListInsert (hsyml, isymBefore,  symbolNew)

> 此函数将符号记录插入到符号列表 hsyml 中。符号插入到 isymBefore 之前。如果 isymBefore 为 –1，则符号将追加到列表的末尾。符号记录在 symbolNew 中给出

### SymListItem  (hsyml, isym)

> 此函数返回符号列表 hsyml 中从零开始的索引 isym 处的符号记录。符号列表的大小由 SymListCount 返回。 请参见：Symbol Record
>
> 索引值从零开始，一直持续到比 SymListCount 返回的值少 1 的索引值。
>
> 此示例枚举符号列表中的所有符号：

### SymListNew  ()

> 分配一个新的空符号列表。返回新的符号列表句柄，如果错误则返回 hNil。完成符号列表后，应调用 SymListFree。

### SymListRemove (hsyml, isym)

> 从符号列表 hsyml 中删除元素。删除 isym 处的符号元素。

## 符号函数

> 符号函数允许您访问 Source Insight 的符号查找引擎。Source Insight 在符号数据库中维护有关项目的符号信息。这些符号函数利用符号数据库和 Source Insight 的内置语言解析器来查找源文件中的符号。
>
> 您可能需要查看“项目”一章中的“符号和项目”部分，以了解 Source Insight 如何维护符号信息以及查找规则是什么。

- Symbol Record

    > 符号记录描述符号声明。它指定符号的位置和类型。它用于唯一描述项目中或打开的文件缓冲区中的符号。
    >
    > 符号记录由多个函数返回，符号记录用作多个函数的输入。 请参见:Symbol Record。

### GetBufSymCount(hbuf)

> 返回缓冲区 hbuf 中声明的符号数。如果未声明任何符号，或者无法处理文件，或者文件的文档类型未指定语言分析器，则返回零。

### GetBufSymLocation(hbuf, isym)

> 返回缓冲区 hbuf 中由 isym 索引的符号的符号记录。每个已分析的文件缓冲区都维护其中定义的符号的索引。索引按符号名称排序。符号索引值从零开始，一直到 GetBufSymCount 返回的计数减去 1。此函数将符号索引 （isym） 映射到符号符号记录。

### GetBufSymName(hbuf, isym)

> 返回缓冲区 hbuf 中由 isym 编制索引的符号的名称。每个已分析的文件缓冲区都维护其中定义的符号的索引。索引按符号名称排序。符号索引值从零开始，一直到 GetBufSymCount 返回的计数减去 1。此函数将符号索引 （isym） 映射到符号名称。
>
> 此示例循环访问所有文件缓冲区符号：

```c
isymMax = GetBufSymCount (hbuf)
isym = 0
while (isym < isymMax)
   {
   symname = GetBufSymName (hbuf, isym)
   ...
   isym = isym + 1
   }
```

### GetCurSymbol ()

> 返回当前所选内容所在的符号的名称。当前选择是活动窗口中的选择（或光标位置）。如果未找到任何符号，GetCurSymbol 将返回一个空字符串。

### GetSymbolLine (symbol_name)

> 返回名为 symbol_name 的符号的行号。  如果使用相同的名称定义了多个符号，则用户将能够选择适当的符号。

### GetSymbolLocation (symbol_name)

> 返回在 symbol_name 中指定的符号名称的位置。该位置在 Symbol 记录中返回。如果未找到符号，则返回空字符串。 请参见：符号记录。
>
> 此函数执行查找操作的方式与使用“跳转到定义”命令时 Source Insight 查找符号的方式相同。如果在当前项目或任何打开的文件中找不到该符号，则还会搜索项目符号路径上的所有项目。如果找到多个声明用于symbol_name，则会向用户显示一个多定义列表供您选择。
>
> 还可以调用 GetSymbolLocationEx 来更好地控制查找操作的执行方式，并查找同一符号名称的多个定义。
>
> 本示例查找符号的定义，并显示其源文件和行号。

```c
want to locate?")
loc = GetSymbolLocation(symbol)
if (loc == "")
   Msg (symbol # " was not found")
else
   Msg (symbol # " was found in " # loc.file # 
         " at line " # loc.lnFirst)

```

### GetSymbolLocationEx (symbol_name, output_buffer,  fMatchCase, LocateFiles, fLocateSymbols)

> 查找 symbol_name 中指定的符号的所有声明。每个声明位置都作为一行追加到给定的缓冲区output_buffer作为符号记录。  如果为 fMatchCase，则符号名称大小写必须完全匹配。如果为 fLocateFiles，则位于项目或项目符号路径中的文件名。不必指定文件扩展名。如果为 fLocateSymbols，则位于符号定义。fLocateFiles 和 fLocateSymbols 都可以设置为 True。
>
> 请参见：符号记录。
>
> 由于记录变量可以表示为字符串，因此 Symbol 记录可以写为缓冲区中的一行文本。若要从输出缓冲区读取符号记录，请使用 GetBufLine 函数返回整行文本;在本例中为 Symbol 记录。有关 Symbol 记录的说明，请参阅 GetSymbolLocation。
>
> GetSymbolLocationEx 返回匹配声明的数目，如果未找到任何声明，则返回零。
>
> 与 GetSymbolLocation 函数不同，此函数将查找在 symbol_name 上匹配的多个声明。可以使用此函数通过扫描输出缓冲区的每一行来枚举每个位置。
>
> 此示例查找一个符号，并枚举找到的每个声明：

```c
symbol = Ask("What symbol do you want to locate?")
hbuf = NewBuf("output")
count = GetSymbolLocationEx(symbol, hbuf, 1, 1, 1)
ln = 0
while (ln < count)
   {
   loc = GetBufLine(hbuf, ln)
   msg (loc.file # " at line " # loc.lnFirst)
   ln = ln + 1
   }
CloseBuf(hbuf)

```

### GetSymbolFromCursor (hbuf, ln, ich)

> 返回出现在给定光标位置的符号名称的符号记录。hbuf 是缓冲区句柄。ln 是行号。ich 是该行上从零开始的字符索引。
>
> 其工作方式与“跳转到定义”命令类似，只不过它返回的是“符号”记录，而不是跳转

### GetSymbolLocationFromLn (hbuf, ln)

> 返回缓冲区 hbuf 中行号 ln 处存在的符号的符号记录。ln 处的符号是其声明包含给定行号 ln 的符号

### JumpToLocation (symbol_record)

> 跳转到symbol_record中给出的位置。这将打开符号记录中的文件，并将光标移动到其中定义的符号。其工作方式与“跳转到定义”命令相同。
>
> Symbol 记录由 GetSymbolLocation 函数返回。

### JumpToSymbolDef (symbol_name)

> 跳转到名为 symbol_name 的符号的定义。这将打开一个文件，并将光标移动到其中定义的符号。其工作方式与“跳转到定义”命令相同。

### SymbolChildren (symbol)

> 返回包含给定符号的子级的新符号列表句柄。符号的子项是在符号主体中声明的符号。例如，班级的子级是班级成员。
>
> symbol 包含 Symbol 记录。 请参见：符号记录。
>
> 应调用 SymListFree 以释放 SymbolChildren 返回的符号列表句柄。
>
> 可以使用符号列表函数访问此函数返回的符号列表。
>
> 例
> 此示例查找符号的定义并显示其子项：

```c
symbolname = Ask("What symbol do you want to locate?")
symbol = GetSymbolLocation(symbolname)
if (symbol == nil)
   Msg (symbolname # " was not found")
else
   {
   hsyml = SymbolChildren(symbol)
   cchild = SymListCount(hsyml)
   ichild = 0
   while (ichild < cchild)
      {
      childsym = SymListItem(hsyml, ichild)
      Msg (childsym.symbol # " was found in " 
         # childsym.file # " at line " # childsym.lnFirst)
      ichild = ichild + 1
      }
   SymListFree(hsyml)
   }
```

### SymbolContainerName (symbol)

> 返回符号名称的容器组件。
>
> symbol 包含 Symbol 记录。 请参见：符号记录。
>
> 每个符号名称都划分为路径组件，这些组件由点 （.） 字符分隔。例如，符号名称可能是“myclass.member1”。在此示例中，“member1”包含在“myclass”中。

### SymbolDeclaredType (symbol)

> 返回给定符号的声明类型的 Symbol 记录。
>
> symbol 包含 Symbol 记录。 请参见：符号记录。

### SymbolLeafName (symbol)

> 返回“leaf”，即符号名称最右边的组成部分。
>
> symbol 包含 Symbol 记录。 请参见：符号记录。
>
> 每个符号名称都划分为路径组件，这些组件由点 （.） 字符分隔。例如，符号名称可能是“myclass.member1”。在此示例中，“member1”包含在“myclass”中。

### SymbolParent (symbol)

> 返回给定符号的父级的 Symbol 记录。符号的父级是包含该符号的符号。
>
> symbol 包含 Symbol 记录。

### SymbolRootContainer (symbol)

> 返回符号名称的根或最左边的组件。
>
> symbol 包含 Symbol 记录。 请参见：符号记录。
>
> 每个符号名称都划分为路径组件，这些组件由点 （.） 字符分隔。例如，符号名称可能是“myclass.member1”。在此示例中，“member1”包含在“myclass”中。

### SymbolStructureType (symbol)

> 返回给定符号的结构类型的 Symbol 记录。结构类型是符号的结构或类类型，可以通过 typedefs 间接引用。

## 搜索函数

> 这些函数搜索对单词和模式的引用。

### GetSourceLink (hbufSource, lnSource)

> 返回链接记录中源链接的目标。源缓冲区是 hbufSource，源行号是 lnSource。如果给定行不包含源链接，则返回一个空字符串。 请参见： Link Record 。
>
> 目标链接指向某个文件中某个行号的位置。此源链接信息链接两个任意位置。例如，“搜索结果”缓冲区包含与搜索模式匹配的每一行的源链接。

### LoadSearchPattern(pattern, fMatchCase, fRegExp,  fWholeWordsOnly)

> 加载用于“搜索”、“向前搜索”和“向后搜索”命令的搜索模式。
>
> 搜索模式字符串在模式中给出。 
>
> 如果为 fMatchCase，则搜索区分大小写。
>
> 如果为 fRegExpr，则模式包含正则表达式。否则，模式是一个简单的字符串。
>
> 如果 fWholeWordsOnly，则只有整个单词会导致匹配。

### ReplaceInBuf(hbuf, oldPattern, newPattern,  lnStart, lnLim, fMatchCase, fRegExp, fWholeWordsOnly, fConfirm)

> 在给定缓冲区中执行搜索和替换操作。
>
> 搜索模式字符串在 oldPattern 中给出。 
>
> 替换模式字符串在 newPattern 中给出。
>
> 行范围由 lnSart 到 lnLim 指定。更换仅在 lnStart 到 lnLim - 1 的线路上进行。
>
> 如果为 fMatchCase，则搜索区分大小写。
>
> 如果为 fRegExpr，则模式包含正则表达式。否则，模式是一个简单的字符串。
>
> 如果 fWholeWordsOnly，则只有整个单词会导致匹配。
>
> 如果为 fConfirm，则在每次替换之前都会提示用户。

### SearchForRefs (hbuf, word, fTouchFiles)

> 在整个项目中搜索对单词字符串的引用。包含单词的每一行都附加到缓冲区 hbuf 中。如果 fTouchFiles 为 TRUE，则每个包含单词的文件都会将其上次修改的时间戳设置为当前时间。
>
> 此函数类似于“查找引用”命令。Word 可以包含多个单词，但如果是单个单词，则此函数要快得多。 
>
> 此示例创建一个新的搜索结果文件并搜索引用。

```
macro LookupRefs (symbol)
{
   hbuf = NewBuf("Results") // create output buffer
   if (hbuf == 0)
      stop
   SearchForRefs(hbuf, symbol, 0)
   SetCurrentBuf(hbuf) // put buffer in a window
}
```

### SearchInBuf  (hbuf, pattern, lnStart, ichStart, fMatchCase, fRegExp, fWholeWordsOnly)

> 在缓冲区 hbuf 中搜索模式。搜索从行号 lnStart 和字符索引 ichFirst 开始。SearchInBuf 返回跨越匹配文本的 Sel 记录。如果未找到任何内容，则返回一个空字符串。有关 Sel 记录的说明，请参阅 GetWndSel。
>
> 如果为 fMatchCase，则搜索区分大小写。
>
> 如果为 fRegExpr，则模式包含正则表达式。否则，模式是一个简单的字符串。
>
> 如果 fWholeWordsOnly，则只有整个单词会导致匹配。

### SetSourceLink (hbufSource, lnSource,  target_file, lnTarget)

> 创建新的源链接。链接源缓冲区为 hbufSource。链接源行号为 lnSource。链接目标文件在 target_file 中以路径字符串的形式给出。链路目标行号为 lnTarget。
>
> 如果成功，则返回 True，否则返回 False。Target_file不必存在。操作不会仅仅因为target_file不存在而失败。此外，target_file不需要打开。
>
> 为了获得一致的结果，target_file应包含文件的完全限定路径名。但是，您可以将一个简单的文件规范传递给此函数，它将根据当前项目中包含的文件和项目符号路径展开target_file。
>
> 当源缓冲区关闭或删除源行时，源链接将被销毁。

## 项目函数

> 项目功能允许您打开和关闭项目，并获取项目信息。

### AddConditionVariable(hprj, szName, szValue) 

> 添加一个新的条件分析变量，用于在分析代码时计算条件语句（如 #if）。
>
> Hprj 是项目的句柄。如果 hprj 为 hNil，则新变量将添加到全局条件列表中。
>
> 变量的名称在 szName 中给出，值在 szValue 中给出
>
> 有两个条件列表：全局列表和特定于项目的列表。打开项目时，这两个列表将合并，特定于项目的列表优先于全局列表中的条目。

### AddFileToProj(hprj, filename)

> 将给定的文件名添加到项目 hprj。

### CloseProj  (hprj)

> Closes the project hprj.

### DeleteConditionVariable(hprj, szName) 

> 删除用于在分析代码时计算条件语句（如 #if）的新条件分析变量。
>
> Hprj 是项目的句柄。如果 hprj 为 hNil，则从全局条件列表中删除该变量。
>
> 变量的名称在 szName 中给出。
>
> 有两个条件列表：全局列表和特定于项目的列表。打开项目时，这两个列表将合并，特定于项目的列表优先于全局列表中的条目。

### DeleteProj  (proj_name)

> 删除proj_name中指定的项目。如果该项目当前处于打开状态，则系统会询问用户是否要先关闭它。如果用户未关闭项目，则不会删除该项目。

### EmptyProj  ()

> 通过从项目中删除所有文件来清空项目。实际文件本身不受影响。如果成功，则返回 True，如果错误，则返回 False。

### GetCurrentProj ()

> 返回当前打开的项目的句柄 （hprj）。Source Insight 只允许用户一次打开一个项目;但是，从宏语言来看，可以打开多个项目。

### GetProjDir  (hprj)

> 返回项目 hprj 的源目录路径。

### GetProjFileCount (hprj)

> 返回添加到项目 hprj 的文件数。

### GetProjFileName (hprj, ifile)

> 返回与项目 hprj 中的索引 ifile 关联的项目文件的名称。 
>
> 每个项目都有一个项目文件索引，按文件名排序。GetProjFileName 将索引映射到文件名。文件索引值从零开始，一直到 GetProjFileCount 返回的计数。
>
> 此示例通过所有项目文件进行交互：

```c
ifileMax = GetProjFileCount (hprj)
ifile = 0
while (ifile < ifileMax)
   {
   filename = GetProjFileName (hprj, ifile)
   ..
   ifile = ifile + 1
   }
```

### GetProjName  (hprj)

> 返回项目 hprj 的名称。该名称包含项目文件的完整路径。

### GetProjSymCount (hprj)

> 返回项目 hprj 中的符号数。

### GetProjSymLocation (hprj, isym)

> 返回与项目 hprj 中的索引 isym 关联的符号的符号记录中的符号位置信息。 请参见：Symbol Record。
>
> 每个项目都有一个符号索引，按符号名称排序。GetProjSymLocation 将索引映射到符号符号记录。符号索引值从零开始，一直到 GetProjSymCount 返回的计数。
>
> 可以调用 JumpToLocation 以移动到 GetProjSymLocation 返回的 Symbol 记录。
>
> 有关符号记录的详细信息，请参阅 GetSymbolLocation。

### GetProjSymName (hprj, isym)

> 返回与项目 hprj 中的索引 isym 关联的符号的名称。 
>
> 每个项目都有一个符号索引，按符号名称排序。GetProjSymName 将索引映射到符号名称。符号索引值从零开始，一直到 GetProjSymCount 返回的计数减去 1。
>
> 此示例通过所有项目符号进行交互：

```c
isymMax = GetProjSymCount (hprj)
isym = 0
while (isym < isymMax)
   {
   symname = GetProjSymName (hprj, isym)
   ..
   isym = isym + 1
   }
```

### NewProj  (proj_name)

> 创建一个新项目并返回项目句柄 （hprj），如果出现错误，则返回 hNil。

### OpenProj  (proj_name)

> 打开名为 proj_name 的项目，并返回项目句柄 （hprj），如果出现错误，则返回 hNil。

### RemoveFileFromProj(hprj, filename)

> 从项目 hprj 中删除给定的文件名。磁盘上的文件不会被更改或删除。

### SyncProj  (hprj)

> 同步项目 hprj。检查项目中的所有文件是否存在外部更改，并且 Source Insight 的符号数据库会针对已更改的文件进行增量更新。

### SyncProjEx(hprj, fAddNewFiles, fForceAll,  fSupressWarnings)

> 同步项目 hprj。检查项目中的所有文件是否存在外部更改，并且 Source Insight 的符号数据库会针对已更改的文件进行增量更新。 
>
> 如果 fAddNewFiles 为 True，则会自动将新文件添加到项目中。仅添加与“文档选项”命令中定义的文档类型匹配的文件名。
>
> 如果为 fForceAll，则项目中的每个文件都会重新同步，而不考虑其时间戳。
>
> 如果为 fSuppressWarnings，则 Source Insight 在打开文件时出错时不会发出警告。

## 其他宏函数

> 这些函数不能完全归入其他类别，但很有用。

### DumpMacroState (hbufOutput)

> 此函数将描述正在运行的宏的当前状态的文本追加到缓冲区 hbufOutput。宏状态由所有变量的值和执行堆栈组成。此函数在调试宏时很有用。

### GetProgramEnvironmentInfo ()

> 返回 ProgEnvInfo 记录，其中包含有关运行 Source Insight 的环境的信息

### GetProgramInfo ()

> 返回一个 ProgInfo 结构，其中包含有关 Source Insight 的信息。

## 有关宏的其他信息

## Debugging

> Source Insight 不包含宏的调试器。但是，由于宏是解释的，因此可以通过在代码中的关键点使用“Msg”函数来输出字符串和变量值，从而轻松弄清楚发生了什么。 请参阅：Msg （s）。
>
> 若要开始在当前光标位置执行宏语句，请使用 Run Macro.只需将插入点放在要开始运行的行上，然后调用“运行宏”命令即可。
>
> 可以通过调用 DumpMacroState 函数来转储正在运行的宏的执行堆栈和变量状态

### Persistence

> 全局变量在运行之间保留，但在会话之间不保留。在运行或会话之间不保留局部变量。但是，可以通过将值存储在文件中或写入和读取注册表项来保留值。

## No Self-Modifying Macros

> 确保宏在运行时不会自行修改。Source Insight 将中止任何尝试编辑包含正在运行的宏的文件的宏。

## Sample Macros

> Source Insight 中包含一个名为“utils.em”的宏文件。此文件包含一些有用的函数，您可能需要查看它以查看一些示例。

# 宏事件处理程序

> 本章介绍用 Source Insight 宏语言编写的事件处理程序函数。事件处理程序是在特定事件发生时调用的函数。本章假定您熟悉宏语言规则和语法。

## 宏事件使用

> 可以将事件处理程序用于许多事情，但有一些想法是：
>
> 监视和记录活动。例如，您可以将操作记录到日志文件中，然后使用该信息来分析项目的哪些部分已被编辑。
>
> 将另一个程序或文件与 Source Insight 中的操作同步。
>
> 更改 Source Insight 的工作方式。
>
> 在保存文件之前对文件进行后处理。
>
> 执行新文件的预处理。

## 向 Source Insight 添加事件处理程序

> 事件处理程序存储在宏源文件中。也就是说，它们具有 .em 扩展名。您可以在同一文件中混合使用事件函数和宏函数。编写事件处理程序后，应将其添加到当前项目中。如果您希望在不考虑项目的情况下处理事件，则可以将其添加到基本项目中。如果 Source Insight 找不到给定的事件处理程序，则忽略该事件处理程序。Source Insight 在您的项目、项目符号路径和 Base 项目中进行搜索。
>
> 请务必记住，必须将 .em 文件添加到项目中，否则 Source Insight 将不会调用该文件中的事件处理程序。这是为了防止事件处理程序仅通过打开包含事件函数的 .em 文件而意外运行。

## Enabling Event Handlers

> 在使用事件处理程序之前，必须启用它们。出于安全原因，默认情况下它们处于禁用状态。若要启用事件处理程序，请在“首选项”>选择“选项”，然后单击“常规”选项卡。然后，选中“启用事件处理程序”框。启用事件处理程序后，该选项将被保存，因此您不必再次执行此操作。
>
> 还有一个名为“启用事件处理程序”的用户级命令，可以分配给菜单或击键。

## Editing Event Handler Files

> Source Insight 将忽略任何已修改和未保存的文件中的事件处理程序。因此，如果您正在编辑事件处理程序源文件，则 Source Insight 不会在您编辑处理程序时尝试执行该处理程序！完成编辑后，保存文件。保存文件后，Source Insight 将再次执行处理程序。

## Errors in Event Handlers

> 如果事件处理程序导致语法错误或运行时错误，则在 Source Insight 会话的其余部分禁用所有事件处理程序。您将看到“宏错误”警告消息。若要再次启用事件处理程序，必须重新启动 Source Insight。

## Synchronous Vs. Asynchronous Events

> 某些事件处理程序在事件发生时立即调用。这些事件称为“同步”事件。例如，DocumentNew。一旦用户创建新文档（文件缓冲区），就会调用它。
>
> 但是，某些事件在事件发生后不久被调用，通常是在短暂的空闲时间之后。这些事件称为“异步”事件。它们是异步的，因为如果在事件发生的确切时间调用用户编写的宏，则会破坏 Source Insight 的稳定性。

## Helpful Tips for Event Handlers

> 最好将所有事件处理程序放在一个文件中，或者放在名称类似于“event-something.em”的少量文件中。这样，您可以轻松地从项目中删除这些文件，以有效地关闭处理程序。
>
> 全局变量可用于添加计数器和维护事件之间的状态。

# Application Events

> 应用程序事件将应用于整个 Source Insight 应用程序。

### event AppStart()

> 在 Source Insight 应用程序加载并初始化后调用。当前项目和工作区会话已加载。

### event  AppShutdown()

> 在 Source Insight 应用程序退出之前调用。

### event  AppCommand(sCommand)

> 在给定的用户级命令执行后立即调用。

# Document Events

> 文档事件适用于打开、关闭、保存或修改文件缓冲区的时间。

### event  DocumentNew(sFile)

> 在创建给定文件缓冲区后立即调用。文件名为 sFile。

### event  DocumentOpen(sFile)

> 在打开文件缓冲区后立即调用。文件名为 sFile。

### event  DocumentClose(sFile)

> 在文件缓冲区关闭后立即调用。文件名为 sFile。

### event  DocumentSave(sFile)

> 在保存文件缓冲区之前调用。文件名为 sFile。此时，您可以在保存文件缓冲区之前对其进行编辑。如果要在保存文件后执行某些操作，则可以使用 DocumentSaveComplete 事件。

### event DocumentSaveComplete(sFile)

> 在保存文件缓冲区后立即调用。文件名为 sFile。如果要在保存文件之前获得控制权，则可以使用 DocumentSave 事件。

### event  DocumentChanged(sFile)

> 在用户编辑文件缓冲区时调用。文件名为 sFile。此事件是异步处理的。也就是说，它不会在用户键入时调用。它是在闲置片刻后调用的。这允许您在此事件处理程序中编辑文件。注意 由于此函数是异步调用的，因此 sFile 文件可能无法打开，因此需要测试 GetBufHandle（sFile） 的返回值以确保它不是 hNil。

### event  DocumentSelectionChanged(sFile)

> 当用户选择文本或移动当前文件中的光标时调用。文件名为 sFile。此事件是异步处理的。也就是说，当用户移动光标时，不会调用它。它是在闲置片刻后调用的。注意 由于此函数是异步调用的，因此 sFile 文件可能无法打开，因此需要测试 GetBufHandle（sFile） 的返回值以确保它不是 hNil。

## Project Events

> 项目事件适用于打开和关闭 Source Insight 项目。

### event  ProjectOpen(sProject)

> 在项目打开后调用。 

### event  ProjectClose(sProject)

> 在项目关闭之前调用。

# Statusbar Events

> 当状态栏文本更改时，将发生状态栏事件。

### event  StatusbarUpdate(sMessage)

> 在状态栏的内容更改时调用。此事件是异步处理的。也就是说，它不会在状态栏更改的确切时刻调用。它是在闲置片刻后调用的。这允许您在此事件处理程序中编辑文件。
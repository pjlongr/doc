<a name="tZPLY"></a>
# Makefile 有什么
Makefile 里主要包含了五个东西：显式规则、隐晦规则、变量定义、文件指示和注释。<br />PS：<br />在 Makefile 中的命令，必须要以[Tab]键开始
<a name="Kkros"></a>
## 显式规则
显式规则说明了，如何生成一个或多的的目标文件。这是由 Makefile 的书写者明显指<br />出，要生成的文件，文件的依赖文件，生成的命令。
<a name="WgRNR"></a>
## 隐晦规则
由于我们的 make 有自动推导的功能，所以隐晦的规则可以让我们比较粗糙地简略地书<br />写 Makefile，这是由 make 所支持的。
<a name="WH2w1"></a>
## 变量定义
在 Makefile 中我们要定义一系列的变量，变量一般都是字符串，这个有点你 C 语言中<br />的宏，当 Makefile 被执行时，其中的变量都会被扩展到相应的引用位置上。
<a name="gXFh9"></a>
## 文件指示
其包括了三个部分，一个是在一个 Makefile 中引用另一个 Makefile，就像 C 语言中的<br />include 一样；另一个是指根据某些情况指定 Makefile 中的有效部分，就像 C 语言中的预<br />编译#if 一样；还有就是定义一个多行的命令。有关这一部分的内容，我会在后续的部分中<br />讲述。
<a name="ZtneD"></a>
## 注释
Makefile 中只有行注释，和 UNIX 的 Shell 脚本一样，其注释是用“#”字符，这个就<br />像 C/C++中的“//”一样。如果你要在你的 Makefile 中使用“#”字符，可以用反斜框进行<br />转义，如：“\#”。
<a name="nXkPi"></a>
# Makefile 的文件名
大多数的 make 都支持“makefile”和“Makefile”这两种默认文件名。
<a name="B63yc"></a>
# 引用其它的 Makefile
在 Makefile 使用 include 关键字可以把别的 Makefile 包含进来。<br />include 的语法是： <br />include <filename> <br />filename 可以是当前操作系统 Shell 的文件模式（可以保含路径和通配符） 在 include<br />前面可以有一些空字符，但是绝不能是[Tab]键开始。include 和<filename>可以用一个或<br />多个空格隔开。举个例子，你有这样几个 Makefile：a.mk、b.mk、c.mk，还有一个文件叫<br />foo.make，以及一个变量$(bar)，其包含了 e.mk 和 f.mk，那么，下面的语句： <br />include foo.make *.mk $(bar) <br />等价于： <br />include foo.make a.mk b.mk c.mk e.mk f.mk<br />make 命令开始时，会把找寻 include 所指出的其它 Makefile，并把其内容安置在当前<br />的位。<br />。如果文件都没有指定绝对路径或是相对路径的话，<br />make 会在当前目录下首先寻找，如果当前目录下没有找到，那么，make 还会在下面的几个<br />目录下找： <br /> <br />1、如果 make 执行时，有“-I”或“--include-dir”参数，那么 make 就会在这个参数 <br />所指定的目录下去寻找。 <br />2、如果目录<prefix>/include（一般是：/usr/local/bin 或/usr/include）存在的话，<br />make 也会去找。如果有文件没有找到的话，make 会生成一条警告信息，但不会马上出现致<br />命错误。它会继续载入其它的文件，一旦完成 makefile 的读取，make 会再重试这些没有找<br />到，或是不能读取的文件，如果还是不行，make 才会出现一条致命信息。如果你想让 make<br />不理那些无法读取的文件，而继续执行，你可以在 include 前加一个减号“-”。 <br />如： -include <filename> <br />其表示，无论 include 过程中出现什么错误，都不要报错继续执行。和其它版本 make<br />兼 容的相关命令是 sinclude，其作用和这一个是一样的。
<a name="fIyHX"></a>
# make 的工作方式 
GNU 的 make 工作时的执行步骤入下：（想来其它的 make 也是类似） <br />1、读入所有的 Makefile。 <br />2、读入被 include 的其它 Makefile。 <br />3、初始化文件中的变量。 <br />4、推导隐晦规则，并分析所有规则。 <br />5、为所有的目标文件创建依赖关系链。 <br />6、根据依赖关系，决定哪些目标要重新生成。 <br />7、执行生成命令。 <br /> <br />1-5 步为第一个阶段，6-7 为第二个阶段。第一个阶段中，如果定义的变量被使用了，那么，<br />make 会把其展开在使用的位置。但 make 并不会完全马上展开，make 使用的是拖延战术，如<br />果变量出现在依赖关系的规则中，那么仅当这条依赖被决定要使用了，变量才会在其内部展<br />开。
<a name="Au0GR"></a>
# 书写规则
规则包含两个部分，一个是依赖关系，一个是生成目标的方法。 <br />在 Makefile 中，规则的顺序是很重要的，因为，Makefile 中只应该有一个最终目标，<br />其它的目标都是被这个目标所连带出来的，所以一定要让 make 知道你的最终目标是什么。<br />一般来说，定义在 Makefile 中的目标可能会有很多，但是第一条规则中的目标将被确立为<br />最终的目标。如果第一条规则中的目标有很多个，那么，第一个目标会成为最终的目标。make<br />所完成的也就是这个目标。
<a name="zzDx0"></a>
## 规则举例 
```makefile
foo.o : foo.c defs.h # foo 模块 
    cc -c -g foo.c
```
	看到这个例子，各位应该不是很陌生了，前面也已说过，foo.o 是我们的目标，foo.c<br />和 defs.h 是目标所依赖的源文件，而只有一个命令“cc -c -g foo.c”（以 Tab 键开头）。<br />这个规则告诉我们两件事： <br /> <br />1、文件的依赖关系，foo.o 依赖于 foo.c 和 defs.h 的文件，如果 foo.c 和 defs.h 的<br />文件日期要比 foo.o 文件日期要新，或是 foo.o 不存在，那么依赖关系发生。 <br />2、如果生成（或更新）foo.o 文件。也就是那个 cc 命令，其说明了，如何生成 foo.o<br />这个文件。（当然 foo.c 文件 include 了 defs.h 文件） 
<a name="n17sZ"></a>
## 规则的语法
```makefile

targets : prerequisites 
    command 
    ... 
或是这样： 
targets : prerequisites ; command 
    command 
    ... 
```
	targets 是文件名，以空格分开，可以使用通配符。一般来说，我们的目标基本上是一<br />个文件，但也有可能是多个文件。 <br />command 是命令行，如果其不与“target:prerequisites”在一行，那么，必须以[Tab键]开头，如果和 prerequisites 在一行，那么可以用分号做为分隔。（见上） prerequisites也就是目标所依赖的文件（或依赖目标）。如果其中的某个文件要比目标文件要新，那么，目标就被认为是“过时的”，被认为是需要重生成的。这个在前面已经讲过了。 <br />如果命令太长，你可以使用反斜框（‘\’）作为换行符。make 对一行上有多少个字符没有限制。规则告诉 make 两件事，文件的依赖关系和如何成成目标文件。 <br />一般来说，make 会以 UNIX 的标准 Shell，也就是/bin/sh 来执行命令。
<a name="Ct98Q"></a>
## 在规则中使用通配符
make 支持三各通配符：“*”，“?”和“[...]”。<br />通配符代替了你一系列的文件，如“*.c”表示所以后缀为 c 的文件。一个需要我们注意的是，如果我们的文件名中有通配符，如：“*”，那么可以用转义字符“\”，如“\*”来表示真实的“*”字符，而不是任意长度的字符串。
<a name="ubt1K"></a>
# 文件搜寻
当 make 需要去找寻文件的依赖关系时，你可以在文件前加上路径，但最好的方法是把一个路径告诉 make，让 make 在自动去找。 <br />Makefile 文件中的特殊变量“VPATH”就是完成这个功能的，如果没有指明这个变量，make 只会在当前的目录中去找寻依赖文件和目标文件。如果定义了这个变量，那么，make就会在当当前目录找不到的情况下，到所指定的目录中去找寻文件了。
```makefile
VPATH = src:../headers 
```
上面的的定义指定两个目录，“src”和“../headers”，make 会按照这个顺序进行搜<br />索。目录由“冒号”分隔。（当然，当前目录永远是最高优先搜索的地方） <br />另一个设置文件搜索路径的方法是使用 make 的“vpath”关键字（注意，它是全小写<br />的），这不是变量，这是一个 make 的关键字，这和上面提到的那个 VPATH 变量很类似，但是<br />它更为灵活。它可以指定不同的文件在不同的搜索目录中。这是一个很灵活的功能。它的使<br />用方法有三种： <br />1、vpath <pattern> <directories> <br />为符合模式<pattern>的文件指定搜索目录<directories>。 <br />2、vpath <pattern> <br />清除符合模式<pattern>的文件的搜索目录。 <br />3、vpath <br />清除所有已被设置好了的文件搜索目录。 

vapth 使用方法中的<pattern>需要包含“%”字符。“%”的意思是匹配零或若干字符，<br />例如，“%.h”表示所有以“.h”结尾的文件。<我们可以连续地使用 vpath 语句，以指定不同搜索策略。如果连续的 vpath 语句中出现了相同的<pattern>，或是被重复了的<pattern>，那么，make 会按照 vpath 语句的先后顺序来执行搜索。如： <br />vpath %.c foo <br />vpath % blish <br />vpath %.c bar <br />其表示“.c”结尾的文件，先在“foo”目录，然后是“blish”，最后是“bar”目录。 <br />vpath %.c foo:bar <br />vpath % blish <br />而上面的语句则表示“.c”结尾的文件，先在“foo”目录，然后是“bar”目录，最后<br />才是“blish”目录。
<a name="r9lu6"></a>
# 伪目标
“伪目标”并不是一个文件，只是一个标签，由于“伪目标”不是文件，所以 make 无法生成它的依赖关系和决定它是否要执行。我们只有通过显示地指明这个“目标”才能让其生效。当然，“伪目标”的取名不能和文件名重名，不然其就失去了“伪目标”的意义了。 <br />为了避免和文件重名的这种情况，我们可以使用一个特殊的标记“.PHONY”来显示地指明一个目标是“伪目标”，向 make 说明，不管是否有这个文件，这个目标就是“伪目标”。 
```makefile
.PHONY : clean
```
<a name="PIWKn"></a>
# 多目标
Makefile 的规则中的目标可以不止一个，其支持多目标，有可能我们的多个目标同时依赖于一个文件，并且其生成的命令大体类似。于是我们就能把其合并起来。当然，多个目标的生成规则的执行命令是同一个，这可能会可我们带来麻烦，不过好在我们的可以使用一个自动化变量“$@”（关于自动化变量，将在后面讲述），这个变量表示着目前规则中所有的目标的集合，这样说可能很抽象，还是看一个例子吧。
```makefile
bigoutput littleoutput : text.g 
generate text.g -$(subst output,,$@) > $@ 
 
上述规则等价于： 
 
bigoutput : text.g 
generate text.g -big > bigoutput 
littleoutput : text.g 
generate text.g -little > littleoutput
```
其中，-$(subst output,,$@)中的“$”表示执行一个 Makefile 的函数，函数名为 subst，后面的为参数。关于函数，将在后面讲述。这里的这个函数是截取字符串的意思，“$@”表示目标的集合，就像一个数组，“$@”依次取出目标，并执于命令。
<a name="wnZgc"></a>
# 静态模式 
静态模式可以更加容易地定义多目标的规则，可以让我们的规则变得更加的有弹性和灵活.
```makefile
objects = foo.o bar.o 

#指明了我们的目标从$object 中获取
all: $(objects) 
 
$(objects): %.o: %.c 
$(CC) -c $(CFLAGS) $< -o $@
```
 上面的例子中，指明了我们的目标从$object 中获取，“%.o”表明要所有以“.o”结尾的目标，也就是“foo.o bar.o”，也就是变量$object 集合的模式，而依赖模式“%.c”则取模式“%.o”的“%”，也就是“foo bar”，并为其加下“.c”的后缀，于是，我们的依赖目标就是“foo.c bar.c”。而命令中的“$<”和“$@”是自动化变量，“$<”表示所有的依赖目标集（也就是“foo.c bar.c”），“$@”表示目标集（也就是“foo.o bar.o”）。<br />于是，上面的规则展开后等价于下面的规则：
```makefile
foo.o : foo.c 
$(CC) -c $(CFLAGS) foo.c -o foo.o 
bar.o : bar.c 
$(CC) -c $(CFLAGS) bar.c -o bar.o
```
<a name="Dpryc"></a>
# 自动生成依赖性
大多数的C/C++编译器都支持一个“-M”的选项，即自动找寻源文件中包含的头文件，并生成一个依<br />赖关系。例如，如果我们执行下面的命令：
```makefile
cc -M main.c 
 
其输出是： 
main.o : main.c defs.h
```
	于是由编译器自动生成的依赖关系，这样一来，你就不必再手动书写若干文件的依赖关系，而由编译器自动生成了。需要提醒一句的是，如果你使用 GNU 的 C/C++编译器，你得用“-MM”参数，不然，“-M”参数会把一些标准库的头文件也包含进来。
<a name="hv9WL"></a>
# 书写命令
每条命令的开头必须以[Tab]键开头，除非，命令是紧跟在依赖规则后面的分号后的。在命令行之间中的空格或是空行会被忽略，但是如果该空格或空行是以 Tab 键开头的，那么 make 会认为其是一个空命令.
<a name="kQHGE"></a>
## 显示命令
make 会把其要执行的命令行在命令执行前输出到屏幕上。当我们用“@”字符在命令行前，那么，这个命令将不被 make 显示出来，最具代表性的例子是，我们用这个功能来像屏幕显示一些信息。<br />make 参数“-n”或“--just-print”，那么其只是显示命令，但不会执行命令，这个功能很有利于我们调试我们的 Makefile.<br />make 参数“-s”或“--slient”则是全面禁止命令的显示。 
<a name="ezEHf"></a>
## 命令执行
当依赖目标新于目标时，也就是当规则的目标需要被更新时，make 会一条一条的执行其后的命令。需要注意的是，如果你要让上一条命令的结果应用在下一条命令时，你应该使用分号分隔这两条命令。比如你的第一条命令是 cd 命令，你希望第二条命令得在 cd 之后的基础上运行，那么你就不能把这两条命令写在两行上，而应该把这两条命令写在一行上，用分号分隔。如：
```makefile
exec: 
cd /home/hchen; pwd 
```
<a name="ykRhh"></a>
# 命令出错 
每当命令运行完后，make 会检测每个命令的返回码，如果命令返回成功，那么 make 会执行下一条命令，当规则中所有的命令成功返回后，这个规则就算是成功完成了。如果一个规则中的某个命令出错了（命令退出码非零），那么 make 就会终止执行当前规则，这将有可能终止所有规则的执行。<br />为了做到这一点，忽略命令的出错，我们可以在 Makefile 的命令行前加一个减号“-”（在 Tab 键之后），标记为不管命令出不出错都认为是成功的。如：
```makefile
clean: 
-rm -f *.o
```
还有一个全局的办法是，给 make 加上“-i”或是“--ignore-errors”参数，那么，Makefile 中所有命令都会忽略错误。<br />make 的参数的是“-k”或是“--keep-going”，这个参数的意思是，如果某规则中的命令出错了，那么就终目该规则的执行，但继续执行其它规则.
<a name="A492m"></a>
# 嵌套执行 make
我们有一个子目录叫 subdir，这个目录下有个 Makefile 文件，来指明了这个目录下文件的编译规则。那么我们总控的 Makefile 可以这样书写:
```makefile
subsystem: 
cd subdir && $(MAKE) 
 
其等价于： 
 
subsystem: 
$(MAKE) -C subdir
```
如果你要传递所有的变量，那么，只要一个 export 就行了。后面什么也不用跟，表示传递所有的变量。<br />当你使用“-C”参数来指定 make 下层 Makefile 时，“-w”会被自动打开的。如果参数中有“-s”（“--slient”）或是“--no-print-directory”，那么，“-w”总是失效的。
<a name="QqVM0"></a>
# 定义命令包
如果 Makefile 中出现一些相同命令序列，那么我们可以为这些相同的命令序列定义一个变量。定义这种命令序列的语法以“define”开始，以“endef”结束，如：
```makefile
define run-yacc 
yacc $(firstword $^) 
mv y.tab.c $@ 
endef
```
这里，“run-yacc”是这个命令包的名字，其不要和 Makefile 中的变量重名。在“define”和“endef”中的两行就是命令序列。这个命令包中的第一个命令是运行 Yacc程序，因为 Yacc 程序总是生成“y.tab.c”的文件，所以第二行的命令就是把这个文件改改名字。还是把这个命令包放到一个示例中来看看吧。 
```makefile
foo.c : foo.y 
$(run-yacc)
```
我们可以看见，要使用这个命令包，我们就好像使用变量一样。在这个命令包的使用中，命令包“run-yacc”中的“$^”就是“foo.y”，“$@”就是“foo.c”（有关这种以“$”开头的特殊变量，我们会在后面介绍），make 在执行命令包时，命令包中的每个命令会被依次独立执行。 

<a name="J70mp"></a>
# 使用变量
变量的命名字可以包含字符、数字，下划线（可以是数字开头），但不应该含有“:”、“#”、“=”或是空字符（空格、回车等）。变量是大小写敏感的，“foo”、“Foo”和“FOO”是三个不同的变量名。<br />有一些变量是很奇怪字串，如“$<”、“$@”等，这些是自动化变量。
<a name="M177x"></a>
## 变量的基础
变量在声明时需要给予初值，而在使用时，需要给在变量名前加上“$”符号，但最好用小括号“（）”或是大括号“{}”把变量给包括起来。如果你要使用真实的“$”字符，那么你需要用“$$”来表示。
<a name="k3loX"></a>
## 变量中的变量
在定义变量的值时，我们可以使用其它变量来构造变量的值，在 Makefile 中有两种方式来在用变量定义变量的值。  <br />先看第一种方式，也就是简单的使用“=”号，在“=”左侧是变量，右侧是变量的值，右侧变量的值可以定义在文件的任何一处，也就是说，右侧中的变量不一定非要是已定义好的值，其也可以使用后面定义的值。如：
```makefile
foo = $(bar) 
bar = $(ugh) 
ugh = Huh? 
 
all: 
echo $(foo
```
我们可以使用 make 中的另一种用变量来定义变量的方法。这种方法使用的是“:=”操作符，如：
```makefile
x := foo 
y := $(x) bar 
x := later 
 
其等价于： 
 
y := foo bar 
x := later 
 
值得一提的是，这种方法，前面的变量不能使用后面的变量，只能使用前面已定义好了的变
量。如果是这样： 
 
y := $(x) bar 
x := foo 
 
那么，y 的值是“bar”，而不是“foo bar”。 
```
<a name="Ur3m7"></a>
## 变量高级用法
第一种是变量值的替换。我们可以替换变量中的共有的部分，其格式是“$(var:a=b)”或是“${var:a=b}”，其意思是，把变量“var”中所有以“a”字串“结尾”的“a”替换成“b”字串。这里的“结尾”意思是“空格”或是“结束符”。<br />第二种高级用法是——“把变量的值再当成变量”。
<a name="L3woY"></a>
## 追加变量值
使用“+=”操作符给变量追加值，如： 
```makefile
objects = main.o foo.o bar.o utils.o 
objects += another.o 
```
 	于是，我们的$(objects)值变成：“main.o foo.o bar.o utils.o another.o”（another.o被追加进去了）
<a name="UhMy6"></a>
## override 指示符
如果有变量是通常 make的命令行参数设置的，那么 Makefile中对这个变量的赋值会被忽略。<br />如果你想在 Makefile 中设置这类参数的值，那么，你可以使用“override”指示符。其语法是： <br />override <variable> = <value> <br />override <variable> := <value> <br />当然，你还可以追加： <br />override <variable> += <more text> <br />对于多行的变量定义，我们用 define 指示符，在 define 指示符前，也同样可以使用 ovveride指示符，如： <br />override define foo <br />bar <br />endef
<a name="c4rqJ"></a>
## 多行变量
设置变量值的方法是使用 define 关键字。使用 define 关键字设置变量的值可以有换行，这有利于定义一系列的命令（前面我们讲过“命令包”的技术就是利用这个关键字）。 <br />define 指示符后面跟的是变量的名字，而重起一行定义变量的值，定义是以 endef 关键字结束。其工作方式和“=”操作符一样。变量的值可以包含函数、命令、文字，或是其它变量。因为命令需要以[Tab]键开头，所以如果你用 define 定义的命令变量中没有以[Tab]键开头，那么 make 就不会把其认为是命令。 <br /> 下面的这个示例展示了 define 的用法：
```makefile
define two-lines 
echo foo 
echo $(bar) 
endef
```
<a name="K7lek"></a>
## 环境变量
make 运行时的系统环境变量可以在 make 开始运行时被载入到 Makefile 文件中，但是如果 Makefile 中已定义了这个变量，或是这个变量由 make 命令行带入，那么系统的环境变量的值将被覆盖。（如果 make 指定了“-e”参数，那么，系统环境变量将覆盖 Makefile 中定义的变量） <br /> 	因此，如果我们在环境变量中设置了“CFLAGS”环境变量，那么我们就可以在所有的Makefile 中使用这个变量了。这对于我们使用统一的编译参数有比较大的好处。如果Makefile 中定义了 CFLAGS，那么则会使用Makefile 中的这个变量，如果没有定义则使用系统环境变量的值，一个共性和个性的统一，很像“全局变量”和“局部变量”的特性。 
<a name="uygFq"></a>
## 目标变量
为某个目标设置局部变量，这种变量被称为“Target-specific Variable”，它可以和“全局变<br />量”同名，因为它的作用范围只在这条规则以及连带规则中，所以其值也只在作用范围内有效。而不会影响规则链以外的全局变量的值。 <br />其语法是：
```makefile
<target ...> : <variable-assignment> 
 <target ...> : overide <variable-assignment> 
```
<variable-assignment>可以是前面讲过的各种赋值表达式，如“=”、“:=”、“+=”或是“？=”。第二个语法是针对于 make 命令行带入的变量，或是系统环境变量。这个特性非常的有用，当我们设置了这样一个变量，这个变量会作用到由这个目标所引发的所有的规则中去。如：
```makefile
prog : CFLAGS = -g 
prog : prog.o foo.o bar.o 
$(CC) $(CFLAGS) prog.o foo.o bar.o 
 
prog.o : prog.c 
$(CC) $(CFLAGS) prog.c
foo.o : foo.c 
$(CC) $(CFLAGS) foo.c 
 
bar.o : bar.c 
$(CC) $(CFLAGS) bar.c
```
在这个示例中，不管全局的$(CFLAGS)的值是什么，在 prog 目标，以及其所引发的所有<br />规则中（prog.o foo.o bar.o 的规则），$(CFLAGS)的值都是“-g”
<a name="L52r5"></a>
## 模式变量
在 GNU 的 make 中，还支持模式变量（Pattern-specific Variable），通过上面的目标变量中，我们知道，变量可以定义在某个目标上。模式变量的好处就是，我们可以给定一种“模式”，可以把变量定义在符合这种模式的所有目标上。 <br /> 	我们知道，make 的“模式”一般是至少含有一个“%”的，所以，我们可以以如下方式给所有以[.o]结尾的目标定义目标变量：<br />%.o : CFLAGS = -O <br />同样，模式变量的语法和“目标变量”一样：<br /><pattern ...> : <variable-assignment> <br /><pattern ...> : override <variable-assignment> <br />override 同样是针对于系统环境传入的变量，或是 make 命令行指定的变量。
<a name="N0U69"></a>
# 使用条件判断
使用条件判断，可以让 make 根据运行时的不同情况选择不同的执行分支。条件表达式可以是比较变量的值，或是比较变量和常量的值。<br />ifeq:比较参数“arg1”和“arg2”的值是否相同.ifeq "<arg1>" "<arg2>"<br />ifneq：其比较参数“arg1”和“arg2”的值是否相同，如果不同，则为真。ifneq "<arg1>" "<arg2>"<br />ifdef：ifdef <variable-name> 如果变量 <variable-name>的值非空，那到表达式为真。否则，表达式为假。当然，<variable-name>同样可以是一个函数的返回值。注意，ifdef 只是测试一个变量是否有值，其并不会把变量扩展到当前位置。<br />ifndef：和“ifdef”是相反的意思。 在<conditional-directive>这一行上，多余的空格是被允许的，但是不能以[Tab]键做为开始（不然就被认为是命令）。而注释符“#”同样也是安全的。“else”和“endif”也一样，只要不是以[Tab]键开始就行了。
<a name="r0BqH"></a>
# 使用函数
<a name="BJV1Y"></a>
## 函数的调用语法
函数调用，很像变量的使用，也是以“$”来标识的，其语法如下： <br />$(<function> <arguments>) <br />或是 <br />${<function> <arguments>} <br />这里，<function>就是函数名，make 支持的函数不多。<arguments>是函数的参数，参数间以逗号“,”分隔，而函数名和参数之间以“空格”分隔。函数调用以“$”开头，以圆括号或花括号把函数名和参数括起。感觉很像一个变量，是不是？函数中的参数可以使用变量，为了风格的统一，函数和变量的括号最好一样，如使用“$(subst a,b,$(x))”这样的形式，而不是“$(subst a,b,${x})”的形式。因为统一会更清楚，也会减少一些不必要的麻烦。<br />示例：
```makefile
comma:= , 
empty:= 
space:= $(empty) $(empty) 
foo:= a b c 
bar:= $(subst $(space),$(comma),$(foo)) 
```
在这个示例中，$(comma)的值是一个逗号。$(space)使用了$(empty)定义了一个空格，$(foo)的值是“a b c”，$(bar)的定义用，调用了函数“subst”，这是一个替换函数，这个函数有三个参数，第一个参数是被替换字串，第二个参数是替换字串，第三个参数是替换操作作用的字串。这个函数也就是把$(foo)中的空格替换成逗号，所以$(bar)的值是“a,b,c”。 
<a name="pTo2Z"></a>
## 字符串处理函数 
<a name="NCE2P"></a>
### subst
$(subst <from>,<to>,<text>)  <br />名称：字符串替换函数——subst。 <br />功能：把字串<text>中的<from>字符串替换成<to>。 <br />返回：函数返回被替换过后的字符串。
```makefile
$(subst ee,EE,feet on the street)， 
 
把“feet on the street”中的“ee”替换成“EE”，返回结果是“fEEt on the 
strEEt”。
```
<a name="lnBoL"></a>
### patsubst
$(patsubst <pattern>,<replacement>,<text>) <br /> 	名称：模式字符串替换函数——patsubst。 <br />功能：查找<text>中的单词（单词以“空格”、“Tab”或“回车”“换行”分隔）是否符合模式<pattern>，如果匹配的话，则以<replacement>替换。这里，<pattern>可以包括通配符“%”，表示任意长度的字串。如果<replacement>中也包含“%”，那么，<replacement>中的这个“%”将是<pattern>中的那个“%”所代表的字串。（可以用“\”来转义，以“\%”来表示真实含义的“%”字符） <br />返回：函数返回被替换过后的字符串。 <br />示例： <br />$(patsubst %.c,%.o,x.c.c bar.c) <br />把字串“x.c.c bar.c”符合模式[%.c]的单词替换成[%.o]，返回结果是“x.c.o bar.o” 
<a name="G2Ocb"></a>
### strip
$(strip <string>) <br />名称：去空格函数——strip。 <br />功能：去掉<string>字串中开头和结尾的空字符。 <br />返回：返回被去掉空格的字符串值。<br />示例： <br />$(strip a b c ) <br />把字串“a b c ”去到开头和结尾的空格，结果是“a b c”。
<a name="ZMEKg"></a>
### findstring
$(findstring <find>,<in>) <br /> 	名称：查找字符串函数——findstring。 <br />功能：在字串<in>中查找<find>字串。 <br />返回：如果找到，那么返回<find>，否则返回空字符串。 <br />示例： <br />$(findstring a,a b c) <br />$(findstring a,b c) <br />第一个函数返回“a”字符串，第二个返回“”字符串（空字符串） 
<a name="hdH5H"></a>
### filter
$(filter <pattern...>,<text>) <br />名称：过滤函数——filter。 <br />功能：以<pattern>模式过滤<text>字符串中的单词，保留符合模式<pattern>的单词。可以有多个模式。 <br />返回：返回符合模式<pattern>的字串。 <br />示例： <br />sources := foo.c bar.c baz.s ugh.h <br />foo: $(sources) <br />cc $(filter %.c %.s,$(sources)) -o foo <br />$(filter %.c %.s,$(sources))返回的值是“foo.c bar.c baz.s”。 
<a name="dITdR"></a>
### filter-out
$(filter-out <pattern...>,<text>) <br />名称：反过滤函数——filter-out。 <br />功能：以<pattern>模式过滤<text>字符串中的单词，去除符合模式<pattern>的单词。可以有多个模式。 <br />返回：返回不符合模式<pattern>的字串。 <br />示例： <br />objects=main1.o foo.o main2.o bar.o <br />mains=main1.o main2.o <br />$(filter-out $(mains),$(objects)) 返回值是“foo.o bar.o”
<a name="PT7Hy"></a>
### sort
$(sort <list>) <br />名称：排序函数——sort。 <br />功能：给字符串<list>中的单词排序（升序）。 <br />返回：返回排序后的字符串。 <br />示例：$(sort foo bar lose)返回“bar foo lose” 。 <br />备注：sort 函数会去掉<list>中相同的单词。 
<a name="Dx3Ox"></a>
### word
$(word <n>,<text>) <br />名称：取单词函数——word。 <br />功能：取字符串<text>中第<n>个单词。（从一开始） <br />返回：返回字符串<text>中第<n>个单词。如果<n>比<text>中的单词数要大，那么返 回空字符串。 <br />示例：$(word 2, foo bar baz)返回值是“bar”。
<a name="Kjs2Z"></a>
### wordlist
$(wordlist <s>,<e>,<text>) <br />名称：取单词串函数——wordlist。 <br />功能：从字符串<text>中取从<s>开始到<e>的单词串。<s>和<e>是一个数字。 <br />返回：返回字符串<text>中从<s>到<e>的单词字串。如果<s>比<text>中的单词数要大，那么返回空字符串。如果<e>大于<text>的单词数，那么返回从<s>开始，到<text>结束的单词串。 <br />示例： $(wordlist 2, 3, foo bar baz)返回值是“bar baz”。
<a name="L9uIr"></a>
### words
$(words <text>) <br />名称：单词个数统计函数——words。 <br />功能：统计<text>中字符串中的单词个数。 <br />返回：返回<text>中的单词数。 <br />示例：$(words, foo bar baz)返回值是“3”。 <br />备注：如果我们要取<text>中最后的一个单词，我们可以这样：$(word $(words <text>),<text>)。
<a name="QUjNo"></a>
### firstword
$(firstword <text>) <br />名称：首单词函数——firstword。 <br />功能：取字符串<text>中的第一个单词。 <br />返回：返回字符串<text>的第一个单词。 <br />示例：$(firstword foo bar)返回值是“foo”。 <br />备注：这个函数可以用 word 函数来实现：$(word 1,<text>)。
<a name="xzGEu"></a>
## 文件名操作函数
<a name="qdPsK"></a>
### dir
$(dir <names...>) <br />名称：取目录函数——dir。 <br />功能：从文件名序列<names>中取出目录部分。目录部分是指最后一个反斜杠（“/”）之前的部分。如果没有反斜杠，那么返回“./”。 <br />返回：返回文件名序列<names>的目录部分。 <br />示例： $(dir src/foo.c hacks)返回值是“src/ ./”。
<a name="eVdo1"></a>
### notdir
$(notdir <names...>) <br />名称：取文件函数——notdir。 <br />功能：从文件名序列<names>中取出非目录部分。非目录部分是指最后一个反斜杠（“ /”）之后的部分。 <br />返回：返回文件名序列<names>的非目录部分。 <br />示例： $(notdir src/foo.c hacks)返回值是“foo.c hacks”。
<a name="mu19t"></a>
### suffix
$(suffix <names...>) <br />名称：取后缀函数——suffix。 <br />功能：从文件名序列<names>中取出各个文件名的后缀。 <br />返回：返回文件名序列<names>的后缀序列，如果文件没有后缀，则返回空字串。 <br />示例：$(suffix src/foo.c src-1.0/bar.c hacks)返回值是“.c .c”。 
<a name="BZgGe"></a>
### basename
$(basename <names...>) <br />名称：取前缀函数——basename。 <br />功能：从文件名序列<names>中取出各个文件名的前缀部分。 <br />返回：返回文件名序列<names>的前缀序列，如果文件没有前缀，则返回空字串。 <br />示例：$(basename src/foo.c src-1.0/bar.c hacks)返回值是“src/foo src-1.0/b ar hacks”。 
<a name="gKKU0"></a>
### addsuffix
$(addsuffix <suffix>,<names...>) <br />名称：加后缀函数——addsuffix。 <br />功能：把后缀<suffix>加到<names>中的每个单词后面。 <br />返回：返回加过后缀的文件名序列。 <br />示例：$(addsuffix .c,foo bar)返回值是“foo.c bar.c”。
<a name="aPziD"></a>
### addprefix
$(addprefix <prefix>,<names...>) <br />名称：加前缀函数——addprefix。 <br />功能：把前缀<prefix>加到<names>中的每个单词后面。 <br />返回：返回加过前缀的文件名序列。 <br />示例：$(addprefix src/,foo bar)返回值是“src/foo src/bar”。
<a name="V1HaW"></a>
### join
$(join <list1>,<list2>) <br />名称：连接函数——join。 <br />功能：把<list2>中的单词对应地加到<list1>的单词后面。如果<list1>的单词个数要比<list2>的多，那么，<list1>中的多出来的单词将保持原样。如果<list2>的单词个数要比<list1>多，那么，<list2>多出来的单词将被复制到<list2>中。 <br />返回：返回连接过后的字符串。 <br />示例：$(join aaa bbb , 111 222 333)返回值是“aaa111 bbb222 333”。
<a name="MdtA2"></a>
## foreach 函数 
$(foreach <var>,<list>,<text>) <br />名称：循环函数。<br />功能：把参数<list>中的单词逐一取出放到参数<var>所指定的变量中，然后再执行<text>所包含的表达式。每一次<text>会返回一个字符串，循环过程中，<text>的所返回的每个字符串会以空格分隔，最后当整个循环结束时，<text>所返回的每个字符串所组成的整个字符串（以空格分隔）将会是 foreach 函数的返回值。 <br /> 	所以，<var>最好是一个变量名，<list>可以是一个表达式，而<text>中一般会使用<var>这个参数来依次枚举<list>中的单词。<br />示例：<br />names := a b c d <br />files := $(foreach n,$(names),$(n).o) <br />上面的例子中，$(name)中的单词会被挨个取出，并存到变量“n”中，“$(n).o”每次根据“$(n)”计算出一个值，这些值以空格分隔，最后作为 foreach 函数的返回，所以，$(files)的值是“a.o b.o c.o d.o”。 <br /> 	注意，foreach 中的<var>参数是一个临时的局部变量，foreach 函数执行完后，参数<var>的变量将不在作用，其作用域只在 foreach 函数当中。
<a name="fYmIC"></a>
## if 函数
$(if <condition>,<then-part>) <br />或是 <br />$(if <condition>,<then-part>,<else-part>) <br />if 函数可以包含“else”部分，或是不含。即 if 函数的参数可以是两个，也可以是三个。<condition>参数是 if 的表达式，如果其返回的为非空字符串，那么这个表达式就相当于返回真，于是，<then-part>会被计算，否则<else-part>会被计算。 <br /> 	而 if 函数的返回值是，如果<condition>为真（非空字符串），那个<then-part>会是整个函数的返回值，如果<condition>为假（空字符串），那么<else-part>会是整个函数的返回值，此时如果<else-part>没有被定义，那么，整个函数返回空字串。 <br />所以，<then-part>和<else-part>只会有一个被计算。
<a name="I8pKU"></a>
## call 函数
call 函数是唯一一个可以用来创建新的参数化的函数。你可以写一个非常复杂的表达式，这个表达式中，你可以定义许多参数，然后你可以用 call 函数来向这个表达式传递参数。其语法是： <br /> $(call <expression>,<parm1>,<parm2>,<parm3>...) <br />当 make 执行这个函数时，<expression>参数中的变量，如$(1)，$(2)，$(3)等，会被参数<parm1>，<parm2>，<parm3>依次取代。而<expression>的返回值就是 call 函数的返回值。例如： <br />reverse = $(1) $(2) <br />foo = $(call reverse,a,b) <br />那么，foo 的值就是“a b”。当然，参数的次序是可以自定义的，不一定是顺序的，<br />如： <br />reverse = $(2) $(1) <br />foo = $(call reverse,a,b) <br />此时的 foo 的值就是“b a”。
<a name="cHBya"></a>
## origin 函数
origin 函数不像其它的函数，他并不操作变量的值，他只是告诉你你的这个变量是哪里来的？其语法是： <br />$(origin <variable>) <br />注意，<variable>是变量的名字，不应该是引用。所以你最好不要在<variable>中使用“$”字符。Origin 函数会以其返回值来告诉你这个变量的“出生情况”，下面，是 origin函数的返回值:<br />undefined：如果<variable>从来没有定义过，origin 函数返回这个值“undefined”。<br />default：如果<variable>是一个默认的定义，比如“CC”这个变量，这种变量我们将在后面 讲述。environment” 如果<variable>是一个环境变量，并且当 Makefile 被执行时，“-e”参数没有被打开。<br />file：如果<variable>这个变量被定义在 Makefile 中。<br />command line：如果<variable>是被 override 指示符重新定义的。<br />automatic： 如果<variable>是一个命令运行中的自动化变量
<a name="uPRwQ"></a>
## shell 函数
它的参数应该就是操作系统 Shell 的命令。<br />它和反引号“`”是相同的功能。这就是说，shell 函数把执行操作系统命令后的输出作为函数返回。于是，我们可以用操作系统命令以及字符串处理命令 awk，sed 等等命令来生成一个变量，如： <br />contents := $(shell cat foo) <br />files := $(shell echo *.c) <br />注意，这个函数会新生成一个 Shell 程序来执行命令
<a name="RaXJJ"></a>
## 控制 make 的函数
<a name="lq5j0"></a>
### error
$(error <text ...>) <br />产生一个致命的错误，<text ...>是错误信息。注意，error 函数不会在一被使用就会产生错误信息，所以如果你把其定义在某个变量中，并在后续的脚本中使用这个变量，那么也是可以的。例如： <br />示例一： <br />ifdef ERROR_001 <br />$(error error is $(ERROR_001)) <br />endif <br /> <br />示例二： <br />ERR = $(error found an error!) <br />.PHONY: err <br />err: ; $(ERR) 

示例一会在变量 ERROR_001 定义了后执行时产生 error 调用，而示例二则在目录 err被执行时才发生 error 调用。 
<a name="xA4Cu"></a>
### warning
$(warning <text ...>) <br />这个函数很像 error 函数，只是它并不会让 make 退出，只是输出一段警告信息，而 make 继续执行。
<a name="zqfAY"></a>
# make 的运行
<a name="B7gLu"></a>
## make 的退出码
make 命令执行后有三个退出码： <br />0 - 表示成功执行。 <br />1 - 如果 make 运行时出现任何错误，其返回 1。 <br />2 - 如果你使用了 make 的“-q”选项，并且 make 使得一些目标不需要更新，那么返回 2。
<a name="xH373"></a>
## 指定 Makefile
可以给 make 命令指定一个特殊名字的 Makefile。要达到这个功能，我们要使用 make 的“-f”或是“--file”参数（“--makefile”参数也行）。例如，我们有个makefile 的名字是“hchen.mk”，那么，我们可以这样来让 make 来执行这个文件：  <br />make –f hchen.mk
<a name="jer3M"></a>
## 指定目标
任何在 makefile 中的目标都可以被指定成终极目标，但是除了以“-”打头，或是包含了“=”的目标，因为有这些字符的目标，会被解析成命令行参数或是变量。甚至没有被我们明确写出来的目标也可以成为 make 的终极目标，也就是说，只要 make 可以找到其隐含规则推导规则，那么这个隐含目标同样可以被指定成终极目标。 <br /> 	有一个 make 的环境变量叫“MAKECMDGOALS”，这个变量中会存放你所指定的终极目标的列表，如果在命令行上，你没有指定目标，那么，这个变量是空值。这个变量可以让你使用在一些比较特殊的情形下。比如下面的例子： <br />sources = foo.c bar.c <br />ifneq ( $(MAKECMDGOALS),clean) <br />include $(sources:.c=.d) <br />endif <br />基于上面的这个例子，只要我们输入的命令不是“make clean”，那么 makefile 会自动包含“foo.d”和“bar.d”这两个 makefile。 <br />all：这个伪目标是所有目标的目标，其功能一般是编译所有的目标。<br />clean：这个伪目标功能是删除所有被 make 创建的文件<br />install：这个伪目标功能是安装已编译好的程序，其实就是把目标执行文件拷贝到指定的目标中去。<br />print：这个伪目标的功能是例出改变过的源文件<br />tar：这个伪目标功能是把源程序打包备份。也就是一个 tar 文件。<br />dist：这个伪目标功能是创建一个压缩文件，一般是把 tar 文件压成 Z 文件。或是 gz 文件。 <br />TAGS：这个伪目标功能是更新所有的目标，以备完整地重编译使用。 
<a name="W83cn"></a>
## 检查规则 
“-n” <br />“--just-print” <br />“--dry-run” <br />“--recon” <br />不执行参数，这些参数只是打印命令，不管目标是否更新，把规则和连带规则下的命令打印出来，但不执行，这些参数对于我们调试 makefile 很有用处。 <br /> <br />“-t” <br />“--touch” <br />这个参数的意思就是把目标文件的时间更新，但不更改目标文件。也就是说，make 假装编译目标，但不是真正的编译目标，只是把目标变成已编译过的状态。 <br /> <br />“-q” <br />“--question” <br />这个参数的行为是找目标的意思，也就是说，如果目标存在，那么其什么也不会输出，当然也不会执行编译，如果目标不存在，其会打印出一条出错信息。 <br /> <br />“-W <file>” <br />“--what-if=<file>” <br />“--assume-new=<file>” <br />“--new-file=<file>” <br />这个参数需要指定一个文件。一般是是源文件（或依赖文件），Make 会根据规则推导来运行依赖于这个文件的命令，一般来说，可以和“-n”参数一同使用，来查看这个依赖文件所发生的规则命令。 

另外一个很有意思的用法是结合“-p”和“-v”来输出 makefile 被执行时的信息
<a name="lnZ3j"></a>
## make 的参数
| 参数 | 描述 |
| --- | --- |
| -b<br />-m | 这两个参数的作用是忽略和其它版本 make 的兼容性。 |
| -B<br />--always-make | 认为所有的目标都需要更新（重编译）。 |
| -C <dir><br />--directory=<dir> | 指定读取 makefile 的目录。如果有多个“-C”参数，make 的解释是后面的路径以前面的作为相对路径，并以最后的目录作为被指定目录。如：“make –C ~hchen/test –C prog”等价于“make –C ~hchen/test/prog”。 |
| —debug[=<options>] | 输出 make 的调试信息。它有几种不同的级别可供选择，如果没有参数，那就是输出最简单<br />的调试信息。下面是<options>的取值： <br />a —— 也就是 all，输出所有的调试信息。（会非常的多） <br />b —— 也就是 basic，只输出简单的调试信息。即输出不需要重编译的目标。 <br />v —— 也就是 verbose，在 b 选项的级别之上。输出的信息包括哪个 makefile 被解析，不<br />需要被重编译的依赖文件（或是依赖目标）等。 <br />i —— 也就是 implicit，输出所以的隐含规则。 <br />j —— 也就是 jobs，输出执行规则中命令的详细信息，如命令的 PID、返回码等。 <br />m —— 也就是 makefile，输出 make 读取 makefile，更新 makefile，执行 makefile 的信<br />息。  |
| -d | 相当于“--debug=a”。 |
| -e<br />--environment-overrides | 指明环境变量的值覆盖 makefile 中定义的变量的值 |
| -f=<file><br />--file=<file><br />--makefile=<file> | 指定需要执行的 makefile。 |
| -h<br />--help | 显示帮助信息 |
| -i<br />--ignore-errors | 在执行时忽略所有的错误 |
| -I <dir><br />--include-dir=<dir> | 指定一个被包含 makefile 的搜索目标。可以使用多个“-I”参数来指定多个目录。 |
| -j [<jobsnum>]<br />--jobs[=<jobsnum>] | 指同时运行命令的个数。如果没有这个参数，make 运行命令时能运行多少就运行多少。如果有一个以上的“-j”参数，那么仅最后一个“-j”才是有效的。（注意这个参数在 MS-DOS中是无用的） |
| -k<br />--keep-going | 出错也不停止运行。如果生成一个目标失败了，那么依赖于其上的目标就不会被执行了 |
| -l <load><br />--load-average[=<load]<br />—max-load[=<load>] | 指定 make 运行命令的负载 |
| -n<br />--just-print<br /> --dry-run  <br /> --recon   |  仅输出执行过程中的命令序列，但并不执行。   |
|  -o <file><br />--old-file=<file><br />-assume-old=<file> |  不重新生成的指定的，即使这个目标的依赖文件新于它  |
| -p<br />-print-data-base |  输出 makefile 中的所有数据，包括所有的规则和变量。这个参数会让一个简单的 makefile 都会输出一堆信息。如果你只是想输出信息而不想执行 makefile，你可以使用“make -qp” 命令。如果你想查看执行 makefile 前的预设变量和规则，你可以使用“make –p –f /dev/null”。这个参数输出的信息会包含着你的 makefile 文件的文件名和行号，所以，用 这个参数来调试你的 makefile 会是很有用的，特别是当你的环境变量很复杂的时候。   |
| -q<br />--question |  不运行命令，也不输出。仅仅是检查所指定的目标是否需要更新。如果是 0 则说明要更新， 如果是 2 则说明有错误发生。   |
| -r<br />--no-builtin-rules |  禁止 make 使用任何隐含规则。   |
| -R<br />--no-builtin-variabes |  禁止 make 使用任何作用于变量上的隐含规则。   |
| -s<br />--silent<br />--quiet |  在命令运行时不输出命令的输出。   |
| -S<br />--no-keep-going<br />--stop |  取消“-k”选项的作用。因为有些时候，make 的选项是从环境变量“MAKEFLAGS”中继承下 来的。所以你可以在命令行中使用这个参数来让环境变量中的“-k”选项失效。   |
| -t<br />--touch |  相当于 UNIX 的 touch 命令，只是把目标的修改日期变成最新的，也就是阻止生成目标的命 令运行   |
| -v<br />--version |  输出 make 程序的版本、版权等关于 make 的信息   |
| -w<br />--print-directory |  输出运行 makefile 之前和之后的信息。这个参数对于跟踪嵌套式调用 make 时很有用。   |
| --no-print-directory |  禁止“-w”选项。   |
| -W <file><br />--what-if=<file><br />--new-file=<file><br />--assume-file=<file> |  假定目标需要更新，如果和“-n”选项使用，那么这个参数会输出该目标更新时的运 行动作。如果没有“-n”那么就像运行 UNIX 的“touch”命令一样，使得的修改时间 为当前时间。  |
| --warn-undefined-variables |  只要 make 发现有未定义的变量，那么就输出警告信息。   |

<a name="Z9hbR"></a>
# 隐含规则
<a name="Os1iq"></a>
##  隐含规则一览  
<a name="G2mfu"></a>
###  编译 C 程序的隐含规则  
 “.o”的目标的依赖目标会自动推导为“.c”，并且其生成命令是“$(CC) –c $(CPPFLAGS) $(CFLAGS)” 
<a name="Nk0Z0"></a>
###  编译 C++程序的隐含规则  
 “.o”的目标的依赖目标会自动推导为“.cc”或是“.C”，并且其生成命令 是“$(CXX) –c $(CPPFLAGS) $(CFLAGS)”。（建议使用“.cc”作为 C++源文件的后缀，而 不是“.C”） 
<a name="L4ktF"></a>
###  编译 Pascal 程序的隐含规则  
 “.o”的目标的依赖目标会自动推导为“.p”，并且其生成命令是“$(PC) –c $ (PFLAGS)”。
<a name="opfnX"></a>
###  编译 Fortran/Ratfor 程序的隐含规则  
 “.o”的目标的依赖目标会自动推导为“.r”或“.F”或“.f”，并且其 生成命令是: “.f” “$(FC) –c $(FFLAGS)” “.F” “$(FC) –c $(FFLAGS) $(CPPFLAGS)” “.f” “$(FC) –c $(FFLAGS) $(RFLAGS)” 

<a name="Lw5mE"></a>
###  预处理 Fortran/Ratfor 程序的隐含规则  
 “.f”的目标的依赖目标会自动推导为“.r”或“.F”。这个规则只是转换 Ratfor 或有预处理的 Fortran 程序到一个标准的 Fortran 程序。其使用的命令是： “.F” “$(FC) –F $(CPPFLAGS) $(FFLAGS)” “.r” “$(FC) –F $(FFLAGS) $(RFLAGS)” 
<a name="tRMiV"></a>
###  编译 Modula-2 程序的隐含规则  
 “.sym”的目标的依赖目标会自动推导为“.def”，并且其生成命令是：“$(M2C)$(M2FLAGS) $(DEFFLAGS)” 。 “” 的 目 标 的 依 赖 目 标 会 自 动 推 导 为 “.mod”， 并且其生成命令是：“$(M2C) $(M2FLAGS) $(MODFLAGS)”。 
<a name="QBjcL"></a>
###  汇编和汇编预处理的隐含规则  
 “.o” 的目标的依赖目标会自动推导为“.s”，默认使用编译品“as”，并且 其生成命令是：“$(AS) $(ASFLAGS)”。“.s” 的目标的依赖目标会自动推导为 “.S”，默认使用 C 预编译器“cpp”，并且其生成命令是：“$(AS) $(ASFLAGS)”。 
<a name="p9N4t"></a>
###  链接 Object 文件的隐含规则  
 “”目标依赖于“.o”，通过运行 C 的编译器来运行链接程序生成（一般是 “ld”），其生成命令是：“$(CC) $(LDFLAGS) .o $(LOADLIBES) $(LDLIBS)”。这个规 则对于只有一个源文件的工程有效，同时也对多个 Object 文件（由不同的源文件生成）的 也有效。例如如下规则： x : y.o z.o 并且“x.c”、“y.c”和“z.c”都存在时，隐含规则将执行如下命令： cc -c x.c -o x.o cc -c y.c -o y.o cc -c z.c -o z.o cc x.o y.o z.o -o x rm -f x.o rm -f y.o rm -f z.o 
<a name="q0cz8"></a>
###  Yacc C 程序时的隐含规则  
 “.c”的依赖文件被自动推导为“n.y”（Yacc 生成的文件），其生成命令是： “$(YACC) $(YFALGS)”。（“Yacc”是一个语法分析器，关于其细节请查看相关资料 
<a name="rguY8"></a>
###  Lex C 程序时的隐含规则  
  “.c”的依赖文件被自动推导为“n.l”（Lex 生成的文件），其生成命令是：“$(LEX) $(LFALGS)”。（关于“Lex”的细节请查看相关资料） 
<a name="CbapS"></a>
###  Lex Ratfor 程序时的隐含规则  
 “.r”的依赖文件被自动推导为“n.l”（Lex 生成的文件），其生成命令是：“$(LEX) $(LFALGS)”。 
<a name="ndA0n"></a>
###  从 C 程序、Yacc 文件或 Lex 文件创建 Lint 库的隐含规 则  
 “.ln” （lint 生成的文件）的依赖文件被自动推导为“n.c”，其生成命令是：“$(LINT) $(LINTFALGS) $(CPPFLAGS) -i”。对于“.y”和“.l”也是同样的规则。 
<a name="rdURj"></a>
##  隐含规则使用的变量  
<a name="kzbY4"></a>
###  关于命令的变量  
 AR 函数库打包程序。默认命令是“ar”。<br /> AS 汇编语言编译程序。默认命令是“as”。  <br /> CC C 语言编译程序。默认命令是“cc”。<br /> CXX C++语言编译程序。默认命令是“g++”。 <br />CO 从 RCS 文件中扩展文件程序。默认命令是“co”。 <br />CPP C 程序的预处理器（输出是标准输出设备）。默认命令是“$(CC) –E”。<br /> FC Fortran 和 Ratfor 的编译器和预处理程序。默认命令是“f77”。<br /> GET 从 SCCS 文件中扩展文件的程序。默认命令是“get”。<br /> LEX Lex 方法分析器程序（针对于 C 或 Ratfor）。默认命令是“lex”。 <br />PC Pascal 语言编译程序。默认命令是“pc”。 YACC Yacc 文法分析器（针对于 C 程序）。默认命令是“yacc”。 <br />YACCR Yacc 文法分析器（针对于 Ratfor 程序）。默认命令是“yacc –r”。<br /> MAKEINFO 转换 Texinfo 源文件（.texi）到 Info 文件程序。默认命令是“makeinfo”。<br /> TEX 从 TeX 源文件创建 TeX DVI 文件的程序。默认命令是“tex”。<br /> TEXI2DVI 从 Texinfo 源文件创建军 TeX DVI 文件的程序。默认命令是“texi2dvi”。 <br />WEAVE 转换 Web 到 TeX 的程序。默认命令是“weave”。 <br />CWEAVE 转换 C Web 到 TeX 的程序。默认命令是“cweave”。 <br />TANGLE 转换 Web 到 Pascal 语言的程序。默认命令是“tangle”。 <br />CTANGLE 转换 C Web 到 C。默认命令是“ctangle”。 <br />RM 删除文件命令。默认命令是“rm –f”。  
<a name="tSFiW"></a>
###  关于命令参数的变量  
 下面的这些变量都是相关上面的命令的参数。如果没有指明其默认值，那么其默认值都 是空。<br />ARFLAGS 函数库打包程序 AR 命令的参数。默认值是“rv”。 <br />ASFLAGS 汇编语言编译器参数。（当明显地调用“.s”或“.S”文件时）。 <br />CFLAGS C 语言编译器参数。 <br />CXXFLAGS C++语言编译器参数。 <br />COFLAGS RCS 命令参数。 <br />CPPFLAGS C 预处理器参数。（ C 和 Fortran 编译器也会用到）。 <br />FFLAGS Fortran 语言编译器参数。 <br />GFLAGS SCCS “get”程序参数。<br />LDFLAGS 链接器参数。（如：“ld”） <br />LFLAGS Lex 文法分析器参数。 <br />PFLAGS Pascal 语言编译器参数。 <br />RFLAGS Ratfor 程序的 Fortran 编译器参数。 <br />YFLAGS Yacc 文法分析器参数  
<a name="jA9Rf"></a>
##  定义模式规则    
<a name="OwopG"></a>
###  模式规则介绍  
 模式规则中，至少在规则的目标定义中要包含"%"，否则，就是一般的规则。目标中的 "%"定义表示对文件名的匹配，"%"表示长度任意的非空字符串。例如："%.c"表示以".c"结 尾的文件名（文件名的长度至少为 3），而"s.%.c"则表示以"s."开头，".c"结尾的文件名（文 件名的长度至少为 5）。 <br />如果"%"定义在目标中，那么，目标中的"%"的值决定了依赖目标中的"%"的值，也就是  说，目标中的模式的"%"决定了依赖目标中"%"的样子。例如有一个模式规则如下： %.o : %.c ;  其含义是，指出了怎么从所有的[.c]文件生成相应的[.o]文件的规则。如果要生成的目标 是"a.o b.o"，那么"%c"就是"a.c b.c"。 <br />一旦依赖目标中的"%"模式被确定，那么，make 会被要求去匹配当前目录下所有的文件名， 一旦找到，make 就会规则下的命令，所以，在模式规则中，目标可能会是多个的，如果有 模式匹配出多个目标，make 就会产生所有的模式目标，此时，make 关心的是依赖的文件名 和生成目标的命令这两件事  
<a name="V9Yfs"></a>
###  模式规则示例  
 下面这个例子表示了,把所有的[.c]文件都编译成[.o]文件.<br /> %.o : %.c <br />$(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@ <br />其中，"$@"表示所有的目标的挨个值，"$<"表示了所有依赖目标的挨个值。这些奇怪的 变量我们叫"自动化变量"，后面会详细讲述。 下面的这个例子中有两个目标是模式的：<br /> %.tab.c %.tab.h: %.y <br />bison -d $<<br /> 这条规则告诉 make 把所有的 [.y]文件都以 "bison -d .y"执行，然后生成 ".tab.c"和".tab.h"文件。（其中，""表示一个任意字符串）。如果我们的执行程 序 "foo" 依 赖 于 文 件 "parse.tab.o" 和 "scan.o" ， 并 且 文 件 "scan.o" 依 赖 于 文 件 "parse.tab.h"，如果"parse.y"文件被更新了，那么根据上述的规则，"bison -d parse.y" 就会被执行一次，于是，"parse.tab.o"和"scan.o"的依赖文件就齐了。（假设，"parse.tab.o" 由"parse.tab.c"生成，和"scan.o"由"scan.c"生成，而"foo"由"parse.tab.o"和"scan.o" 链接生成，而且 foo 和其[.o]文件的依赖关系也写好，那么，所有的目标都会得到满足） 
<a name="l82Wj"></a>
###  自动化变量  
 下面是所有的自动化变量及其说明： <br />$@ <br />表示规则中的目标文件集。在模式规则中，如果有多个目标，那么，"$@"就是匹配于 目标中模式定义的集合。<br /> $%<br /> 仅当目标是函数库文件中，表示规则中的目标成员名。例如，如果一个目标是"foo.a (bar.o)"，那么，"$%"就是"bar.o"，"$@"就是"foo.a"。如果目标不是函数库文件（Unix 下是[.a]，Windows 下是[.lib]），那么，其值为空。 <br />$< <br />依赖目标中的第一个目标名字。如果依赖目标是以模式（即"%"）定义的，那么"$<"将 是符合模式的一系列的文件集。注意，其是一个一个取出来的。 $? 所有比目标新的依赖目标的集合。以空格分隔。 <br />$^<br /> 所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的，那个这个变量 会去除重复的依赖目标，只保留一份。 $+ 这个变量很像"$^"，也是所有依赖目标的集合。只是它不去除重复的依赖目标。<br /> $* <br />这个变量表示目标模式中"%"及其之前的部分。如果目标是"dir/a.foo.b"，并且目标的 模式是"a.%.b"，那么，"$*"的值就是"dir/a.foo"。这个变量对于构造有关联的文件名是比 较有较。如果目标中没有模式的定义，那么"$*"也就不能被推导出，但是，如果目标文件的 后缀是 make 所识别的，那么"$*"就是除了后缀的那一部分。例如：如果目标是"foo.c"，因 为".c"是 make 所能识别的后缀名，所以，"$*"的值就是"foo"。这个特性是 GNU make 的，  很有可能不兼容于其它版本的 make，所以，你应该尽量避免使用"$*"，除非是在隐含规则 或是静态模式中。如果目标中的后缀是 make 所不能识别的，那么"$*"就是空值。 <br />当你希望只对更新过的依赖文件进行操作时，"$?"在显式规则中很有用，例如，假设有 一个函数库文件叫"lib"，其由其它几个 object 文件更新。那么把 object 文件打包的比较 有效率的 Makefile 规则是：<br /> lib : foo.o bar.o lose.o win.o ar r lib $?<br /> 在上述所列出来的自动量变量中。四个变量（$@、$<、$%、$*）在扩展时只会有一个文 件，而另三个的值是一个文件列表。这七个自动化变量还可以取得文件的目录名或是在当前 目录下的符合模式的文件名，只需要搭配上"D"或"F"字样。这是 GNU make 中老版本的特性， 在新版本中，我们使用函数"dir"或"notdir"就可以做到了。"D"的含义就是 Directory，就 是目录，"F"的含义就是 File，就是文件。 <br />下面是对于上面的七个变量分别加上"D"或是"F"的含义： <br />$(@D) <br />表示"$@"的目录部分（不以斜杠作为结尾），如果"$@"值是"dir/foo.o"，那么"$(@D)" 就是"dir"，而如果"$@"中没有包含斜杠的话，其值就是"."（当前目录）。 <br />$(@F)<br /> 表示"$@"的文件部分，如果"$@"值是"dir/foo.o"，那么"$(@F)"就是"foo.o"，"$(@F)" 相当于函数"$(notdir $@)"。<br /> "$(*D)"<br /> "$(*F)" <br />和上面所述的同理，也是取文件的目录部分和文件部分。对于上面的那个例子，"$(*D)" 返回"dir"，而"$(*F)"返回"foo" <br />"$(%D)" <br />"$(%F)" <br />分别表示了函数包文件成员的目录部分和文件部分。这对于形同"archive(member)"形 式的目标中的"member"中包含了不同的目录很有用。 <br />$(<D)<br />$(<F)<br /> 分别表示依赖文件的目录部分和文件部分。  <br />$(~D)<br />$(~F)<br /> 分别表示所有依赖文件的目录部分和文件部分。（无相同的）  <br /> "$(+D)"<br /> "$(+F)" <br />分别表示所有依赖文件的目录部分和文件部分。（可以有相同的） <br />"$(?D)" <br />"$(?F)" <br />分别表示被更新的依赖文件的目录部分和文件部分。 <br />最后想提醒一下的是，对于"$<"，为了避免产生不必要的麻烦，我们最好给$后面的那 个特定字符都加上圆括号，比如，"$(<)"就要比"$<"要好一些。  

<a name="zFD4I"></a>
# 关键字
<a name="z6eUE"></a>
# make 参数
<a name="mBxzZ"></a>
# 自动化变量


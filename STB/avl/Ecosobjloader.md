<a name="TlesM"></a>
# Ecos 网站
[https://doc.ecoscentric.com/](https://doc.ecoscentric.com/)<br />[http://ecos.sourceware.org/docs-3.0/](http://ecos.sourceware.org/docs-3.0/)
<a name="sbfhT"></a>
# eCos 支持动态模块加载(**Object Loader**)
<a name="EURe9"></a>
## 概要
#include <cyg/objloader/objload.h>       <br />void *cyg_ldr_open(cyg_ldr_open_stream *open_stream, CYG_ADDRWORD data);<br />void cyg_ldr_close(void *handle);<br />char *cyg_ldr_error(void);<br />void *cyg_ldr_find_symbol(void *handle, char *symbol);
<a name="fz97n"></a>
## 描述
注意：<br />Object Loader 包目前不支持所有处理器架构。<br />Object Loader 包支持将可执行模块动态加载到 eCos 系统中。 模块可以从各种来源加载到内存中，链接到正在运行的系统和调用的入口点以执行模块的代码。 当不再需要该模块时，它可能会被卸载并将内存重新用于其他目的或其他模块。<br />该系统最接近地模仿 Linux 内核模块机制，而不是 Windows DLL 或 Unix 共享对象。因此，它有许多限制：

- 仅支持用 C 编写的模块。对象加载器目前不支持调用静态构造函数和析构函数、C++异常、RTTI和C++运行时系统的其他部分。
- 自动符号解析仅适用于从模块到主可执行文件的引用.不支持模块之间的引用，也不支持将主可执行文件中的未解析符号解析为模块符号。
- 加载的模块需要使用与主系统相同或相似的配置构建。
- 加载的模块应该使用相同的配置或兼容的编译器标志作为主系统。有一个重要的例外。包括 MIPS 和 Nios II 在内的一些体系结构实现了一个全局指针寄存器。小的全局变量被放置在一个最大为 64K 的内存区域中。 gp 寄存器指向该内存区域，允许使用一条指令直接访问变量，而不是使用两条或更多条指令来访问。此技术不能用于动态加载的模块。因此，必须使用编译器标志（通常是 -G0）来禁止使用 gp 相对寻址。
<a name="s3ENG"></a>
## 创建可加载模块（Creating Loadable Modules）
模块可以只是编译器生成的目标文件。 在包含 $(INSTALL_DIR)/include/pkgconf/ecos.mak 定义文件的 Makefile 中，构建 module.o 的条目可能是：
```makefile
module.o: module.c
        $(ECOS_COMMAND_PREFIX)gcc -c -I$(INSTALL_DIR)/include $(ECOS_GLOBAL_CFLAGS) -o $@ $<
        $(ECOS_COMMAND_PREFIX)strip -g $@
```
	编译行生成一个 .o 文件。 -I 选项允许从 eCos 安装中获取包含。 命令前缀和全局标志由 eCos 构建过程存储在 ecos.mak 文件中。 如果编译标志包括 -g 或其他一些调试选项，那么为了节省内存和加载时间，将完成的模块通过 strip 以将文件内容限制为仅可加载的 ELF 部分是有用的。<br />通过使用链接器执行部分链接的能力，可以从多个目标文件中创建一个模块：
```makefile
module.o : file1.o file2.o file3.o
        $(ECOS_COMMAND_PREFIX)gcc $(subst --gc-sections,-r,$(ECOS_GLOBAL_LDFLAGS)) -L$(PREFIX)/lib \
            -Tmodule.ld -o $@ $^
        $(ECOS_COMMAND_PREFIX)strip -g $@
```
	module.ld 链接描述文件由 Object Loader 包定义，并被复制到安装 lib 目录。 它应该在组合多个文件时使用，或者在单个对象文件中使用 HAL 表等高级功能时使用。<br />如果模块使用 float、double、long long 和一些 long 算术运算，那么在加载之前它应该与 libgcc 部分链接。 这可以通过以下 makefile 片段来完成：
```makefile
# Single source file module…
module.o: module.c
        $(ECOS_COMMAND_PREFIX)gcc -c -I$(INSTALL_DIR)/include $(ECOS_GLOBAL_CFLAGS) -o $@.tmp $<
        $(ECOS_COMMAND_PREFIX)gcc $(subst --gc-sections,-r,$(ECOS_GLOBAL_LDFLAGS)) \
            -L`dirname \`$(ECOS_COMMAND_PREFIX)gcc $(ECOS_GLOBAL_CFLAGS) \
            -print-libgcc-file-name\`` -L$(PREFIX)/lib -Tmodule.ld -o $@ $@.tmp -lgcc
        $(ECOS_COMMAND_PREFIX)strip -g $@

# Combine multiple object files…
module.o : file1.o file2.o file3.o
        $(ECOS_COMMAND_PREFIX)gcc $(subst --gc-sections,-r,$(ECOS_GLOBAL_LDFLAGS)) \
            -L`dirname \`$(ECOS_COMMAND_PREFIX)gcc $(ECOS_GLOBAL_CFLAGS) \
            -print-libgcc-file-name\`` -L$(PREFIX)/lib -Tmodule.ld -o $@ $^ -lgcc
        $(ECOS_COMMAND_PREFIX)strip -g $@
```
<a name="hW66H"></a>
## 目标特定注意事项(Target Specific Considerations)
对于特定的目标架构，有一些特殊的考虑：

- 为 Thumb 编译的模块可以加载到为 ARM32 或 Thumb 编译的目标中。 使用对象加载器的 eCos 的 Thumb 构建应该具有“-mlong-calls”编译器选项集。 如果要加载拇指模块（对象加载器模块自动执行此操作），则 ARM32 构建应该启用拇指互通。 Thumb 模块应使用“-mthumb -mthumb-interwork -mlong-calls”编译器选项进行编译。 但是，一些后来的 ARM 变体不需要“-mthumb-interwork”选项，因为这在架构中是隐含的。 对于这样的目标，不需要给出这个选项。
- 如果要加载的模块将占用与程序其余部分不同的地址空间区域，则为 ARM、Thumb 或 Thumb 2 编译的模块可能需要“-mlong-calls”编译器选项。 导致这种情况出现的最常见情况是，如果主程序从 FLASH 存储器运行，但模块加载到 RAM 中。 如果有疑问，请使用该选项，因为它始终是安全的，唯一的缺点是代码大小和函数调用的运行时执行惩罚。
- 为 MIPS16 指令集编译的模块可以加载到 MIPS 目标中，只要处理器支持该指令集。 要编译和链接这样的模块，“-mips16”编译器选项必须替换“-mips32”以及“-fwritable-strings”。
- 为 NIOS II 处理器编译的模块必须使用“-G0”编译器选项进行编译。 这确保了加载的模块不会假设小初始化数据（“.sdata”）或小零初始化数据（“.sbss”）相对于它加载的地址的可访问性。
<a name="YqvgE"></a>
## 加载模块(Loading Modules)
函数 cyg_ldr_open() 用于将模块加载到内存中。 它需要两个参数。 第一个参数定义一个模块加载器，而第二个参数是一个通用数据项，其值取决于加载器。 如果加载成功，则将返回一个非 NULL 句柄。 失败时将返回 NULL 指针。<br />如果加载过程中出现错误，则函数 cyg_ldr_error() 将返回一个字符串，描述最后发生的错误。 请注意，这不是线程安全的，因为所有加载操作只记录了一个最后一个错误。<br />目前实现了以下加载器：<br />**CYG_LDR_FILESYSTEM**<br />此加载程序使用 FILEIO 操作从文件系统中的命名文件中读取 ELF 文件。 例如，要从文件“/lib/modules/module.o”中读取模块：
```makefile
mod_handle = cyg_ldr_open( CYG_LDR_FILESYSTEM,(CYG_ADDRWORD)"/lib/modules/module.o");
```
	如果包含 CYGPKG_IO_FILEIO 包，则默认包含此加载程序，但可以通过禁用 CYGPKG_OBJLOADER_LOADER_FS 将其省略。<br />**CYG_LDR_MEMORY**<br />此加载程序使用内存访问原语从任何可寻址内存（如 ROM、FLASH 或 RAM）读取 ELF 文件。 例如从位置 module_base 读取模块：
```makefile
mod_handle = cyg_ldr_open( CYG_LDR_MEMORY,(CYG_ADDRWORD)&module_base );
```
	默认情况下包含此加载程序，但可以通过禁用 CYGPKG_OBJLOADER_LOADER_MEM 将其省略。<br />加载器 CYG_LDR_FTP、CYG_LDR_TFTP、CYG_LDR_HTTP 和 CYG_LDR_FLASH 已定义，但目前尚未实现。
<a name="yXFgB"></a>
## 卸载模块(Unloading Modules)
可以通过调用 cyg_ldr_close() 卸载模块，将 cyg_ldr_open() 返回的句柄传递给它。 这将导致加载程序占用的内存被释放。 任何指向模块代码或数据的指针都将变为无效，不应使用。
<a name="wB2Ic"></a>
## 引用模块符号(Referencing Module Symbols)
加载模块时，将加载一个符号表，其中列出了它定义的所有外部符号。 函数 cyg_ldr_find_symbol() 搜索该表并返回一个指向由符号定义的位置的指针。 例如，要创建从模块中的函数运行的线程：
```c
cyg_thread_entry_t *thread_entry;

thread_entry = cyg_ldr_find_symbol( handle, "thread_entry");

cyg_thread_create(THREAD_PRIORITY,
                  thread_entry,
                  0,
                  "Module Thread",
                  (void *)thread_stack,
                  THREAD_STACK_SIZE,
                  &thread_handle,
                  &thread_object);
```
函数和变量都可以通过这种方式访问。<br />没有将主 eCos 应用程序或其他模块中的悬空引用解析为新加载模块中的符号的机制。 主 eCos 应用程序必须在链接时解析所有引用。 但是，可以通过使用函数指针来模拟动态分辨率的效果。 例如，定义一个指向初始虚拟函数的全局函数指针：
```c
typedef int module_fn_t(int a, int b);

module_fn_t dummy_fn;

module_fn_t *module_fn = dummy_fn;

int dummy_fn( int a, int b )
{
    return -1;
}
```
	加载模块时，函数指针可以指向模块内的函数，并在卸载时指向虚拟函数：
```c
void *mod_handle;

void load_module(void)
{
    mod_handle = cyg_ldr_open( CYG_LDR_FILESYSTEM, (CYG_ADDRWORD)"/lib/modules/module.o");

    module_fn = cyg_ldr_find_symbol( mod_handle, "module_fn");
}

void unload_module(void)
{
    cyg_ldr_close( mod_handle );

    module_fn = dummy_fn;
}
```
	甚至可以通过结合 dummy_fn 和 load_module 来实现一种需求加载形式：
```c
int dummy_fn( int a, int b )
{
    mod_handle = cyg_ldr_open( CYG_LDR_FILESYSTEM, (CYG_ADDRWORD)"/lib/modules/module.o");

    module_fn = cyg_ldr_find_symbol( mod_handle, "module_fn");

    return module_fn( a, b );
}
```
<a name="J3LWv"></a>
## 模块打开和关闭功能(Module Open and Close Functions)
加载模块时，对象加载器将查找名为“module_open”的符号，如果找到，将使用以下原型调用它：
```c
void module_open( void );
```
	类似地，当调用 cyg_ldr_close() 时，对象加载器将查找名为“module_close”的符号并调用它，具有相同的原型。
<a name="AwM6p"></a>
## 外部参考(External References)
加载模块时，Object Loader 包执行它需要的任何重定位并解析它包含的任何未解析的符号引用。 只有当所有这些都可以完成时，加载才会成功。 对于要解析的这些符号，主 eCos 可执行文件必须包含一个定义要解析的符号的符号表。 普通的 eCos 可执行文件不包含这样的符号表，因为它会占用不合理的大量内存。 也没有机制可以说服链接器将可加载符号表包含到可执行文件中。 因此，应用程序有必要明确定义将符号名称映射到地址的符号表。<br />loader 提供一个空表； 然后，用户可以定义任何可加载模块所需的附加条目。 为了将表的大小保持在最小，用户可以有选择地只包含那些预期被加载程序用来解析所有引用的函数。 objload.h 中定义了几个用于定义表条目的宏：<br />**CYG_LDR_TABLE_FUN( name )**<br />此宏为具有给定名称的函数定义表条目。<br />**CYG_LDR_TABLE_VAR( name )**<br />此宏为具有给定名称的数据变量定义表条目。<br />**CYG_LDR_TABLE_ENTRY( entry_name, symbol_name, address )**<br />这是一个低级宏，它允许控制符号表条目的所有方面。 entry_name 参数定义表条目对象名称（C 语言要求，因为不允许匿名对象）。 symbol_name 参数是一个字符串，给出了加载器将匹配的符号。 address 参数给出了这个符号将解析到的内存位置。<br />objload.h 文件包含许多宏，这些宏将函数组收集在一起，以方便地包含内核、C 库和 FILEIO 功能块。 其中包括：
```c
CYG_LDR_TABLE_KAPI_ALARM()
CYG_LDR_TABLE_KAPI_CLOCK()
CYG_LDR_TABLE_KAPI_COND()
CYG_LDR_TABLE_KAPI_COUNTER()
CYG_LDR_TABLE_KAPI_EXCEPTIONS()
CYG_LDR_TABLE_KAPI_FLAG()
CYG_LDR_TABLE_KAPI_INTERRUPTS()
CYG_LDR_TABLE_KAPI_MBOX()
CYG_LDR_TABLE_KAPI_MEMPOOL_FIX()
CYG_LDR_TABLE_KAPI_MEMPOOL_VAR()
CYG_LDR_TABLE_KAPI_MUTEX()
CYG_LDR_TABLE_KAPI_SCHEDULER()
CYG_LDR_TABLE_KAPI_SEMAPHORE()
CYG_LDR_TABLE_KAPI_THREAD()
CYG_LDR_TABLE_STRING()
CYG_LDR_TABLE_STDIO()
CYG_LDR_TABLE_INFRA_DIAG()
CYG_LDR_TABLE_FILEIO()
CYG_LDR_TABLE_NET()
```

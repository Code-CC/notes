# 操作系统思考

### 编译

1. 编译语言和解释语言

  * 编译语言

      程序被翻译成机器语言，之后由硬件执行。

  * 解释语言

      程序被软件解释器读取并执行。

2. 静态类型和动态类型

  * 动态类型

    无需定义变量类型，直到运行时才直到变量类型，解释语言通常支持动态类型。

  * 静态类型

    需定义变量类型，编译语言通常限制为静态类型。

    优点：
    
    * 编译时检查，可以更快找到错误。
    
    * 节省空间
        
          动态语言，变量的名称在程序运行时存储在内存中，并且它们通常可由程序访问。
          编译语言，变量的名称只存在编译时，而不是运行时。
          编译器为每个变量选择一个位置，并记录这些位置作为所编译程序的一部分。变量的位置被称为“地址”。在运行期间，每个变量的值都存储在它的地址处，但变量的名称完全不会存储。

3. 编译过程

  * 预处理

    C是包含"预处理指令"的几种语言之一，它生效于编译之前。例如，`#include` 指令使其他文件的源代码插入到指令所在的位置

  * 解析

      编译器读取源代码，并构建程序的内部表示，称为"抽象语法树(AST)"。这一阶段的错误检查通常为语法错误。

  * 静态检查

      编译器会检查变量和值得类型是否正确，函数调用是否带有正确数量和类型的参数，以及其他。这一阶段的错误检测通常为一些"静态语义"的错误

  * 代码生成

      编译器读取程序的内部表示，并生成机器码或字节码

  * 链接

      如果程序使用了定义在库中的值或函数，编译器需要找到合适的库并包含所需要的代码。

  * 优化

      在这个过程的几个时间点上，编译器可以修改程序来生成运行更快或占用更少空间的代码。

4. 目标代码

  编译后的程序，目标代码并不是可执行代码，但是它可以链接到可执行文件中。

5. 汇编代码

  编译后的程序，它通常为机器代码的可读形式。

6. 预处理

7. 理解错误

***

### 进程

1. 抽象和虚拟化

  * 抽象

      抽象是复杂事物的简单表示。

  * 虚拟化

      一类非常重要的抽象就是虚拟化，它是创建可取的幻象的过程。

2. 隔离

  工程最重要的原则之一就是隔离(lsolation)。

  操作系统最重要的目标之一，就是将每个进程和其他进程隔离，使程序员不必考虑每个可能的交互情况。提供这种隔离的软件对象叫做进程(Porcess)。

  进程是表示运行中程序的软件对象。通常一个对象包含数据，并且提供用于操作数据的方法。

  进程正是包含以下数据的对象：

  * 程序文本，通常是机器语言的指令序列。
  * 程序相关的数据，包括静态数据（编译时分配）和动态数据，后者包括运行时的栈和堆。
  * 任何等等中的IO状态。
  * 程序的硬件状态，包括存储在寄存器中的数据，状态信息，以及程序计数器，它表示当前执行了哪个指令。

  操作系统提供了隔离进程的基本功能：

  * 多任务：大多数操作系统有能力在几乎任何时候中断一个进程，保存它的硬件状态，并且在以后恢复它。
  * 虚拟内存：大多数操作系统会创建幻象，每个进程看似拥有独立内存片并且孤立于其他进程。
  * 设备抽象：运行于同一台计算机的进程共享磁盘、网络接口、显卡和其他硬件。

***

### 虚拟内存
***

### 虚拟内存

1. 简明信息理论

  比特是二进制的数字，也是信息单位。 n个比特可以表示`2 ** b`个值，一个字节是8个比特，所以它可以存储256个值。

2. 内存(Memory)和存储器(Storage)

  当进程处于运行期间，它的多数数据都放在内存中。内存中的数据容易丢失。单位为GiB代表"gibibyte"，相当于`2 ** 30`字节

  如果进程会读写文件，这些文件通常放在存储器中。存储器的数据可用于长时间存储。单位为GB代表"gigabyte"，相当于`10 ** 30`字节

3. 地址空间

  内存中的每个字节都由一个"物理地址"整数所指定，物理地址的集合叫做物理"地址空间"。范围通常为0到N-1，N是内存大小。

  操作系统提供"虚拟内存"，程序处理虚拟地址，范围为0到M-1，M是有效虚拟地址的大小。虚拟地址空间的大小取决于操作系统和硬件。

  * 32位系统

    虚拟地址是32位的，虚拟地址空间的大小是`2 ** 32`个字节，或者4GiB

  * 64位系统

    虚拟地址是64位的，虚拟地址空间的大小是`2 ** 64`个字节，或者是`4 * 1024 ** 6`个字节，16EiB

  当一个程序读写内存中的值时，它使用虚拟地址。硬件在操作系统的帮助下，在访问主存之前将物理地址翻译成虚拟地址。翻译过程在进程层级上完成，所以两个进程访问相同的虚拟地址，他们所映射的物理地址可能不同。
  
  虚拟内存是操作系统隔离进程的一种重要途径。

4. 内存段

  一个运行中进程的数据组织为4个段：

  * text段：包含程序文本，即程序所组成的机器语言指令。靠近内存"底部"，即接近0的地址
  * static段：包含由编译器所分配的变量，包含全局变量和使用`static`声明的局部变量。通常刚好在`text`段上面
  * stack段：包含运行时栈，它由栈帧组成。每个帧包含函数参数、本地变量以及其他。靠近内存顶部，即接近虚拟地址空间的最大地址。在扩张过程中，它向低地址的方向增长。
  * heap段：包含运行时分配的内存块，通常通过调用C标准库函数`malloc`来分配。通常在`static`段的上面，在扩张过程中，它向高地址的方向增长。


5. 静态局部变量

  栈上的局部变量有时称为"自动变量"，它们当函数创建时自动被分配，并且当函数返回时自动被释放。

  C中有另一种局部变量，叫做"静态变量"，它分配在static段上。它在程序启动时初始化，并且在函数调用之间保存它的值。

6. 地址翻译

  虚拟地址(VA)翻译成物理地址(GA)

  大多数处理器提供了内存管理单元(MMU)，位于CPU和主存之间。MMU在VA和PA之间执行快速的翻译。

  * 当程序读写变量时，CPU会得到VA。
  * MMU将VA分成两部分，称为页码和偏移。"页"是一个内存块，页的大小取决于操作系统和硬件，通常为1~4KiB
  * MMU在"页表"里查找页码，然后获取相应的物理页码。之后将物理页码和偏移组合得到PA。
  * PA传递给主存，用于读写指定地址。

***

### 文件和文件系统

“文件系统”将每个文件的名称映射到它的内容。是一种键值对的数据库。
“文件”是一组字节序列。

文件名通常是字符串，并且通常是分层的。这个字符串指定了顶级目录的路径，通过一系列子目录，到达特定的文件。

文件是基于字节的，而持久化存储器是基于块的。操作系统将C标准库中基于字节的文件操作翻译成基于块的存储设备操作。

1. 文件的读取和写入过程：

* 读取

  * 程序使用文件名寻找顶级目录，子目录以及n级目录
  * 找到名为xxx的文件，并且"打开"它以便读取。实际上是创建了一个数据结构料表示将要读取的文件。数据结构还跟踪了文件读取了多少字节，称为“文件位置”。
  * 操作系统检查下个字节是否已经在内存中。如果是的话，读取下一个字节，向前移动文件位置，并返回结果。
  * 如果不在内存中，操作系统产生IO请求来获取下一个块。
  * IO操作完成时，新的数据快回存储在内存中。
  * 当进程关闭文件时，操作系统完成或取消任何等待中的操作，移除内存中的数据，并且释放`OpenFileTableEntry`

* 写入

  * 程序使用文件名寻找文件。如果文件不存在，就会创建新的文件，并向父目录添加条目
  * 操作系统创建`OpenFileTableEntry`，表示这个文件已打开等待写入，并将文件位置设置为0.
  * 程序尝试写入文件的第一个字节。如果文件存在，操作系统需要将第一个块加载到内存中。否则它会在内存中分配新的块，并且在磁盘上请求新的块。
  * 在内存中的块被修改后，可能不会立即复制回磁盘。通常，写到文件中的数据是“被缓冲的”，意思是它存储在内存中，只在至少有一个块需要写入时才写回磁盘。
  * 文件关闭时，任何缓冲的数据都会写到磁盘，并且`OpenFileTableEntry`会被释放。

C标准库提供了文件系统的抽象，将文件名称映射到字节流。这个抽象建立在实际以块组织的存储设备之上。

2. 磁盘性能

操作系统和硬件提供了一些特性用于弥补主存和持久化存储器的性能间隔。

* 块的传输
* 预取
* 缓冲

3. 磁盘元数据

块可以放在磁盘上的任意位置，使用各种数据结构来跟踪这些块。

在UNIX文件系统中，这些数据结构叫做`inode`，它代表“索引节点”(index node)。也叫做“元数据”(数据的数据)

4. 块的分配

操作系统既要跟踪哪些块属于哪个文件，也需要跟踪哪些块可供使用。

块分配系统的目标：

* 速度：块的分配和释放应该很快。
* 最小的空间开销
* 最少的碎片
* 最大的连续性

***

### 内存管理

动态内存分配函数：

* `malloc`， 接受表示字节单位的大小的整数，返回指向新分配的、(至少)为指定大小的内存块的指针。如果不满足，返回`NULL`指针
* `calloc`， 和`malloc`一样，还会清空新分配的空间。
* `free`， 它接受指向之前分配的内存块的指针，并会释放它
* `realloc`，接受指向之前分配的内存块的指针，和一个新的大小

1. 内存错误

    * 访问任何没有分配的内存块
    * 释放某个内存块之后再访问它
    * 释放一个没有分配的内存块
    * 释放多次相同的内存块
    * 使用没有分配或者已经释放的内存块调用`realloc`

2. 内存泄漏

    分配了一块内存，并且没有释放它，导致内存总量无限增长

3. 实现

***

### 缓存

1. 程序如何运行

操作系统创建新的进程来运行程序，之后"加载器"将代码从存储器复制到主存中，并且通过调用`main`来启动程序。

在程序运行时，大部分数据存储在主存中，一些数据存储在寄存器中，它是CPU上的小型存储单元，包括：

* 程序计数器(PC)，含有程序下一条指令(在内存中)的地址
* 指令寄存器(IR)，含有当前执行的指令的机器码。
* 栈指针(SP)，含有当前函数栈帧的指针，其中包含函数参数和局部变量。
* 程序当前使用的存放数据的通用寄存器。
* 状态寄存器，含有当前计算的信息。

在程序运行时，CPU执行下列步骤，叫做"指令周期"：

* 取指(Fetch)：从内存中获取下一条指令，存储在指令寄存器中。
* 译码(Decode)：控制单元将指令译码，并向CPU的其他部分发送信号。
* 执行(Execute)：收到来自控制单元的信号后会执行合适的计算。

2. 缓存性能

"缓存"是CPU上小型、快速的存储空间。

当CPU从内存中读取数据时，它将一份副本存到缓存中。如果再次读取相同的数据，CPU就直接读取缓存。

3. 存储器层次结构

 设备 | 访问时间 | 通常大小 |
 --- | --- | --- |
 寄存器| 0.5 ns | 256 B |
 缓存 | 1 ns | 2 MiB |
 DRAM | 10 ns | 4 GiB |
 SSD | 10 us | 100 GiB |
 HDD | 5 ms | 500 GiB |

4. 缓存策略

5. 页面调度

操作系统可以将页面在存储器和内存之间移动。这种机制叫做"页面调度"。

工作流程:

* 进程A调用`malloc`来分配页面。如果堆中没有所请求大小的空闲空间，`malloc`会调用`sbrk`向操作系统请求更多内存。
* 如果物理内存中有空闲页，操作系统会将其加载到进程A的页表，创建新的虚拟内存范围。
* 如果没有空闲页面，调度系统会选择一个属于进程B的"牺牲页面"。它将页面内容从内存复制到磁盘，之后修改进程B的页表来表示这个页面"被换出"
* 一旦进程B的数据被写入，页面会重新分配给进程A。为了防止进程A读取进程B的数据，页面应被清空。
* 此时`sbrk`的调用可以返回，向`malloc`提供堆区额外的空间。之后`malloc`分配所请求的内存并返回。进程A可以继续执行。
* 当进程A执行完毕，或中断后，调度器可能会让进程B继续执行。当它访问到被换出的页面时，内存管理器单元注意到这个页面是"无效"的，并且会触发中断。
* 当操作系统处理中断时，它会看到页面被换出了，于是它将页面从磁盘传送到内存。
* 一旦页面被换入之后，进程B可以继续执行。

页面调度可以极大提升物理内存的利用水平，允许更多进程在更少的空间内执行。原因：

* 大多数进程不会用完所分配内存。这些页面被换出而不会引发任何问题。
* 如果程序泄漏了内存，它可能会丢掉所分配的空间。通过将这些页面换出，可以有效填补泄漏。
* 当进程闲置时，这些进程可以被换出
* 当多进程运行同一个程序时，进程可以共享相同的`text`段，避免在物理内存中保留过多副本。

***

### 多任务

CPU包含多个核心，也就是说可以运行多个进程。并且，每个核心都具有"多任务"的能力。也就是说它可以从一个进程快速切换到另一个进程，创造出同时运行许多进程的幻象。

在操作系统中，多任务由内核实现。其本质就是处理中断。“中断”是一个事件，它会停止通常的指令周期，并且使执行流跳到称为“中断处理器”的特殊代码区域内。

当一个设备向CPU发送信号时，会发生硬件中断。软件中断由运行中的程序所产生。

当程序需要访问硬件设备时，会进行“系统调用”，它就像函数调用，除了并非跳到函数的起始位置，而是执行一条特殊的指令来触发中断，使执行流跳到内核中。内核读取系统调用的参数，执行所请求的操作，之后使被中断进程恢复运行。

1. 硬件状态

* 当中断发生时，硬件将程序计数器保存到一个特殊的寄存器中，并且跳到合适的中断处理器。
* 中断处理器将程序计数器和位寄存器，以及任何打算使用的数据寄存器的内容储存到内存中。
* 中断处理器运行处理中断所需的代码。
* 之后它复原所保存寄存器的内容。最后，复原被中断进程的程序计数器，这会跳回到被中断的进程。

2. 上下文切换

中断处理器非常快，因为它们不需要保存整个硬件状态。它们只需要保存打算使用的寄存器。

但是当中断发生时，内核并不总会恢复被中断的进程。它可以选择切换到其它进程，这种机制叫做“上下文切换”。

3. 进程的生命周期

当进程被创建时，操作系统会为进程分配包含进程信息的数据结构，称为“进程控制块”（PCB）。在其它方面，PCB跟踪进程的状态，这包括：

* 运行（Running），如果进程正在运行于某个核心上。
* 就绪（Ready），如果进程可以但没有运行，通常由于就绪进程数量大于内核的数量。
* 阻塞（Blocked），如果进程由于正在等待未来的事件，例如网络通信或磁盘读取，而不能运行。
* 终止（Done）：如果进程运行完毕，但是带有没有读取的退出状态信息。

下面是一些可导致进程状态转换的事件：

* 一个进程在运行中的程序执行类似于fork的系统调用时诞生。在系统调用的末尾，新的进程通常就绪。之后调度器可能恢复原有的进程（“父进程”），或者启动新的进程（“子进程”）。
* 当一个进程由调度器启动或恢复时，它的状态从就绪变为运行。
* 当一个进程被中断，并且调度器没有选择使它恢复，它的状态从运行变成就绪。
* 如果一个进程执行不能立即完成的系统调用，例如磁盘请求，它会变为阻塞，并且调度器会选择另一个进程。
* 当类似于磁盘请求的操作完成时，会产生中断。中断处理器弄清楚哪个进程正在等待请求，并将它的状态从阻塞变为就绪。
* 当一个进程调用exit时，中断处理器在PCB中储存退出代码，并将进程的状态变为终止。

4. 调度

大多数情况下，只有一小部分进程是就绪或者运行的。当中断发生时，调度器会决定那个进程应启动或恢复。

大多数调度器使用一些基于优先级的调度形式，其中每个进程都有可以调上或调下的优先级。当调度器运行时，它会选择最高优先级的就绪进程。

下面是决定进程优先级的一些因素：

* 具有较高优先级的进程通常运行较快。
* 如果一个进程在时间片结束之前发出请求并被阻塞，就可能是IO密集型程序或交互型程序，优先级应该升高。
* 如果一个进程在整个时间片中都运行，就可能是长时间运行的计算密集型程序，优先级应该降低。
* 如果一个任务长时间被阻塞，之后变为就绪，它应该提升为最高优先级，便于响应所等待的东西。
* 如果进程A在等待进程B的过程中被阻塞，例如，如果它们由管道连接，进程B的优先级应升高。
* 系统调用nice允许进程降低（但不能升高）自己的优先级，并允许程序员向调度器传递显式的信息。

5. 实时调度

调度满足截止期限的任务叫做“实时调度”。对于一些应用，类似于Linux的通用操作系统可以被修改来处理实时调度。这些修改可能包括：
* 为控制任务的优先级提供更丰富的API。
* 修改调度器来确保最高优先级的进程在固定时间内运行。
* 重新组织中断处理器来保证最大完成时间。
* 修改锁和其它同步机制（下一章会讲到），允许高优先级的任务预先占用低优先级的任务。
* 选择保证最大完成时间的动态内存分配实现。
    
***


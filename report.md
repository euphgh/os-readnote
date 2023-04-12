# 第二次读书笔记——保护模式内存管理

## 内存管理概览
在理解三个地址概念之前，首先需要了解**线性地址空间**和**段**的概念，
+ **线性地址空间**：CPU能表示的所以地址空间。RISC中对应的概念是**虚地址空间**
+ **段**：将线性地址空间分成**小块的**，**受保护的**空间。RISC中<u>不存在</u>对应的概念。

三种地址的概念和转换如下图所示：
+ 逻辑地址(Logical Address)：段选择子:偏移量，段选择子是段的唯一标识符，还提供了描述符表到称为描述符的索引。
每个段都有一个段描述符，指定段的大小、访问权限和特权级别、读写执行类型以及段的基址。
+ 线性地址(Linear Address)：段基址+偏移量，即RISC中的虚地址
+ 物理地址(Physical Address)：经页表转换的地址，即实际存在的内存，而非CPU能区分的地址空间。
如果不开启分页功能，线性地址即物理地址。
![segment and paging](./image/seg-page.png)

## 分段机制
* Basic Flat Model：程序可以可以访问连续的、未分段的地址空间，相当于没有分段机制。
要使用IA-32架构实现基本的扁平内存模型，必须至少创建两个段描述符，一个用于引用代码段，另一个用于引用数据段。
但两个段描述符具有相同的基址值0、相同的上限4GB。即是访问了不存在的物理地址，分段机制就不会因为超出内存引用的限制而产生异常。
* Protected Flat Model：保护平面模型在基本平面模型的基础上，增加了只能引用内存地址范围的限制。
任何超出范围内存访问会导致一个通用保护异常(#GP)。此模型提供了针对某些类型的程序错误的最低级别硬件保护。
可以为这个受保护的平面模型添加更多的复杂性，以提供更多的保护。
* Multi-Segment Model：在保护平面模型的基础上提供了，共享和私有的检查，特权级别的检查、读写执行的检查。
一般会定义多个段描述符，而不是上面两种模式的少数个，如下图所示：
![Multi-Segment](./image/multi-seg.png)

## 逻辑地址和线性地址的转换
* 段选择子 Segment Selectors
* 段寄存器 Segment Registers，如何加载
* 段描述子 Segment Descriptors，掌握其结构
* 逻辑地址与线性地址的转换

## 描述符的分类
* 数据段描述符 Data segment Descriptor
* 代码段描述符 Code segment Descriptor
* 局部描述符表描述符 Local descriptor-table (LDT) segment descriptor
* 任务状态段描述符 Task-state segment (TSS) descriptor
* 调用门描述符 Call-gate descriptor
* 中断门描述符 Interrupt-gate descriptor
* 陷阱门描述符 Trap-gate descriptor
* 任务门描述符 Task-gate descriptor

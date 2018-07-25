# Zircon 内核概念

## 介绍

Zircon管理着大量不同类型的对象。这些对象可以通过C++实现的Dispatcher接口的类的系统调用来直接访问。这些对象定义在[kernel/object](../kernel/object)。
他们中很多是自包含的高级对象，也有一些是对低级lk原语的封装。

## [系统调用](syscall.md)

用户空间代码通过系统调用与内核对象相交互，而且几乎全部是通过句柄来实现的。在用户空间，句柄用一个32位的整数（type zx_handle_t）来表示，
当系统调用被执行时，内核检查这个句柄的参数是否指向这个调用进程的句柄表中存在的实际句柄。内核也会检查这个句柄是否类型正确（将一个线程句柄传给
一个需要事件句柄的系统调用将会导致错误），并且这个被请求的操作的句柄也被要求有正确的权限。

从访问角度来看，系统调用分为三大类：

1. 调用没有限制，这类调用非常少，例如[*zx_clock_get()*](syscalls/clock_get.md)和[*zx_nanosleep()*](syscalls/nanosleep.md)或许可以被任意线程调用。
2. 调用的第一个参数是一个句柄，表示要进行操作的对象，这类调用很多，例如[*zx_channel_write()*](syscalls/channel_write.md)和
[*zx_port_queue()*](syscalls/port_queue.md)。
3. 调用会创建一个新的对象，但并不会作为句柄使用，例如[*zx_event_create()*](syscalls/event_create.md)和
[*zx_channel_create()*](syscalls/channel_create.md)。通过调用的进程作业来访问（和限制）他们。

libzircon.so提供了所有的系统调用，这是Zircon内核提供个用户空间的一个“虚拟”共享库，或者叫[*虚拟动态共享对象*或vDSO](vdso.md)。
他们被表示为 *zx_noun_verb()* 或 *zx_noun_verb_direct-object()* 格式的C ELF ABI函数。

系统调用定义在[syscalls.abigen](../system/public/zircon/syscalls.abigen)文件中,并且用[abigen](../system/host/abigen)工具
写入到include文件、libzircon和内核的libsyscalls中。

## [句柄](handles.md) 和 [权限](rights.md)

对象可以有多个句柄（在一个或多个进程中）指向他们。

对于大多数对象，当最后一个指向他的句柄关闭后，他将被销毁，或者将其放入不可撤销的终止状态。

句柄可以从一个进程移动到另一个通过将他们写到通道(使用[*zx_channel_write()*](syscalls/channel_write.md)),或者
使用[*zx_process_start()*](syscalls/process_start.md)将句柄作为新进程中第一个线程的参数传递给他。

这些行为会受到句柄、句柄所引用的的对象的权限约束，同一对象的两个句柄也可能有不同权限。

[*zx_handle_duplicate()*](syscalls/handle_duplicate.md)和
[*zx_handle_replace()*](syscalls/handle_replace.md)系统调用可能会获取传入对象额外的句柄，也可以削弱他的权限。
[*zx_handle_close()*](syscalls/handle_close.md)关闭一个句柄，如果这个句柄是某个对象引用的最后一个句柄，那么这个
对象将被释放销毁。[*zx_handle_close_many()*](syscalls/handle_close_many.md)类似的的关闭一个句柄数组。

## 内核对象ID

内核中每一个对象都有一个“内核对象id”或者简称“koid”。这是一个64位的无符号整数，用来唯一的标识一个内核对象在系统运行的生命周期中。这就意味着koid们绝不会被重用。

有两个特殊的koid值：

*ZX_KOID_INVALID*的值为0，用来表示“null“。

*ZX_KOID_KERNEL*只有一个内核，他有自己的koid。

## 运行代码： 作业、进程、线程

线程代表了在其所属进程的地址空间内的执行（CPU寄存器、栈等）。进程属于作业，进程定义了许多资源的限制条件。
作业属于父作业，直到内核在启动时创建的根作业---[`userboot`, 第一个开始执行的用户空间进程](userboot.md)

如果没有作业的句柄，进程内的线程是没有办法创建其他进程或作业。

[加载进程](program_loading.md)由内核层上的用户空间设施、协议所提供。

更多参考：[process_create](syscalls/process_create.md)，
[process_start](syscalls/process_start.md)，
[thread_create](syscalls/thread_create.md)，
和 [thread_start](syscalls/thread_start.md)。

## 消息传递：套接字和通道

套接字和通道都是支持双向传递的IPC对象。创建一个套接字或通道将返回两个句柄，两个句柄分别指向两个对象的末端。

套接字是面向流设计的，数据可以一次多字节的写入或读出。短写（如果套接字缓冲区满）和短读（如果请求超出缓冲区大小的数据）都可能发生。

通道是面向数据包的，消息的最大长度为64K（可以设置，通常会设置的更小），支持1024个句柄连接到消息上（同样可以更改，同一通常会设置的更小）。但通道不支持短读和短写，不能自适应消息大小。

当句柄写入到通道，他们将被在发送进程中移除。当一条句柄消息在通道中被读取后，这个句柄将会添加到接受进程。在这两个操作之间，句柄继续存在（要保证他们引用的对象存在），除非句柄写入方向的通道关闭，这时，将丢弃传往末端的消息，并关闭所有的句柄。

更多参考：[channel_create](syscalls/channel_create.md)，
[channel_read](syscalls/channel_read.md)，
[channel_write](syscalls/channel_write.md)，
[channel_call](syscalls/channel_call.md)，
[socket_create](syscalls/socket_create.md)，
[socket_read](syscalls/socket_read.md)，
和 [socket_write](syscalls/socket_write.md)。

## 对象和信号

对象可以有32种信号（zx_signals_t 类型，由ZX_*_SIGNAL_*定义），信号相当于是一系列当前状态的信息，例如：通道和套接字可能有READABLE或WRITABLE状态，进程和线程可能有TERMINATED状态，等等。

线程可以在一个或多个对象上等待信号，让他变为活跃状态。

参考[signals](signals.md)获取更多信息。

## 等待：等待一个、等待多个、端口

线程可以使用[*zx_object_wait_one()*](syscalls/object_wait_one.md)来等待一个信号变为激活状态在一个信号句柄上，或用[*zx_object_wait_many()*](syscalls/object_wait_many.md)
来等待多个信号在多个句柄上。这两个系统调用都允许设置超时，之后即使没有信号要处理也可以返回。

如果一个线程在大量的句柄上等待，使用端口的效率将会更高，端口是一个对象，其他声明了信号的对象可以绑定到他，端口接收一个包含等待信息的数据包。

更多参考：[port_create](syscalls/port_create.md)，
[port_queue](syscalls/port_queue.md)，
[port_wait](syscalls/port_wait.md)，
[port_cancel](syscalls/port_cancel.md)。


## 事件，事件对

事件是最简单的对象，除了活动信号集之外没有任何其他状态。

事件对是可以互相发送信号的事件。事件对一个有用的使用场所是当事件的一方离开（他的所有句柄都被关闭），
事件对的另一方将会被设置为PEER_CLOSED信号。

更多参考：[event_create](syscalls/event_create.md),
和[eventpair_create](syscalls/eventpair_create.md).


##共享内存：虚拟内存对象（VMOs）

虚拟内存对象相当于物理内存页的一个集合，或操控页面的“潜力”（根据需要创建/填充）。

[*zx_vmar_map()*](syscalls/vmar_map.md)系统调用将VMO影射到进程的地址空间，或用
[*zx_vmar_unmap()*](syscalls/vmar_unmap.md)取消映射。页面的权限可以使用[*zx_vmar_protect()*](syscalls/vmar_protect.md)调整。

VMOs也可以直接的使用[*zx_vmo_read()*](syscalls/vmo_read.md)和[*zx_vmo_write()*](syscalls/vmo_write.md)读写。因此，对于诸如“创建VMO，将数据写入其中，并将其交给另一个进程使用”的
一次性操作，可以避免将他们映射到地址空间的成本。

## 地址空间管理

虚拟内存地址区（VMARs）提供了管理进程地址空间的抽象。在进程创建时，会传递给创建者一个根VMARs。
这个句柄是整个地址空间VMAR的引用，整个地址空间可以通过[*zx_vmar_map()*](syscalls/vmar_map.md)和
[*zx_vmar_allocate()*](syscalls/vmar_allocate.md)接口切分。
[*zx_vmar_allocate()*](syscalls/vmar_allocate.md)可以生成新的VMARs（被叫做子分区），这个分区可以是部分地址空间的组合。

更多参考：[vmar_map](syscalls/vmar_map.md)，
[vmar_allocate](syscalls/vmar_allocate.md)，
[vmar_protect](syscalls/vmar_protect.md)，
[vmar_unmap](syscalls/vmar_unmap.md)，
[vmar_destroy](syscalls/vmar_destroy.md)。


## 互斥体（Futexes）

互斥体是与用户空间原子操作一起使用的内核原语，用来实现高效的同步原语。例如：Mutex仅在竞争中才会做系统调用，通常来说，只有标准库实现者才会对此有兴趣，Zircon的libc和libc++提供了C11、C++、和pthread API版本的Mutex、条件变量等，以Futexe的形式实现。

更多参考: [futex_wait](syscalls/futex_wait.md)，
[futex_wake](syscalls/futex_wake.md)，
[futex_requeue](syscalls/futex_requeue.md)。
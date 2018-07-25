# Zircon 系统调用

## 句柄
+ [handle_close](syscalls/handle_close.md) - 关闭句柄
+ [handle_close_many](syscalls/handle_close_many.md) - 关闭多个句柄
+ [handle_duplicate](syscalls/handle_duplicate.md) - 创建一个拷贝句柄 (降低权限是可选项)
+ [handle_replace](syscalls/handle_replace.md) - 创建新句柄 (降低权限是可选项) 并销毁旧句柄

## 对象
+ [object_get_child](syscalls/object_get_child.md) - 通过koid查找一个对象的子对象
+ [object_get_cookie](syscalls/object_get_cookie.md) - 读取对象cookie
+ [object_get_info](syscalls/object_get_info.md) - 获取对象信息
+ [object_get_property](syscalls/object_get_property.md) - 读取对象属性
+ [object_set_cookie](syscalls/object_set_cookie.md) - 设置对象cookie
+ [object_set_property](syscalls/object_set_property.md) - 更改对象属性
+ [object_signal](syscalls/object_signal.md) - 、设置或清除对象上的用户信号
+ [object_signal_peer](syscalls/object_signal.md) - 在另一端设置或清除用户信号
+ [object_wait_many](syscalls/object_wait_many.md) - 在多个对象上等待信号
+ [object_wait_one](syscalls/object_wait_one.md) - 在一个对象上等待信号
+ [object_wait_async](syscalls/object_wait_async.md) - 信号变化的异步通知

## 线程
+ [thread_create](syscalls/thread_create.md) - 在进程内创建新线程
+ [thread_exit](syscalls/thread_exit.md) - 退出当前线程
+ [thread_read_state](syscalls/thread_read_state.md) - 读取线程状态
+ [thread_start](syscalls/thread_start.md) - 开始执行新线程
+ [thread_write_state](syscalls/thread_write_state.md) - 更改线程状态

## 进程
+ [process_create](syscalls/process_create.md) - 在作业中创建一个新进程
+ [process_read_memory](syscalls/process_read_memory.md) - 从进程的地址空间读
+ [process_start](syscalls/process_start.md) - 开始执行新进程
+ [process_write_memory](syscalls/process_write_memory.md) - 向进程的地址空间写
+ [process_exit](syscalls/process_exit.md) - 退出当前进程

## 作业
+ [job_create](syscalls/job_create.md) - 在一个作业中创建一个新作业
+ [job_set_policy](syscalls/job_set_policy.md) - 修改作业及其后代的策略

## 任务 (线程、进程，或者作业)
+ [task_bind_exception_port](syscalls/task_bind_exception_port.md) - 绑定端口异常给任务
+ [task_kill](syscalls/task_kill.md) - 停止一个任务
+ [task_resume](syscalls/task_resume.md) - 使暂停的任务继续运行
+ [task_resume_from_exception](syscalls/task_resume_from_exception.md) - 从先前捕获的异常中恢复任务
+ [task_suspend](syscalls/task_suspend.md) - 暂停一个任务

## 通道
+ [channel_call](syscalls/channel_call.md) - 同步发送消息并接收回复
+ [channel_create](syscalls/channel_create.md) - 创建新的通道
+ [channel_read](syscalls/channel_read.md) - 从通道接收消息
+ [channel_read_etc](syscalls/channel_read.md) - 使用句柄信息从通道接收消息
+ [channel_write](syscalls/channel_write.md) - 向通道写消息

## 套接字
+ [socket_create](syscalls/socket_create.md) - 创建一个新套接字
+ [socket_read](syscalls/socket_read.md) - 从套接字读取信息
+ [socket_write](syscalls/socket_write.md) - 写数据到套接字

## 先进先出队列
+ [fifo_create](syscalls/fifo_create.md) - 创建一个新的队列
+ [fifo_read](syscalls/fifo_read.md) - 从队列读数据
+ [fifo_write](syscalls/fifo_write.md) - 向队列写数据

## 事件和事件对
+ [event_create](syscalls/event_create.md) - 创建一个事件
+ [eventpair_create](syscalls/eventpair_create.md) - 创建一对连接事件

## 端口
+ [port_create](syscalls/port_create.md) - 创建一个端口
+ [port_queue](syscalls/port_queue.md) - 发送一个包给端口
+ [port_wait](syscalls/port_wait.md) - 等待一个包到达端口
+ [port_cancel](syscalls/port_cancel.md) - 取消来自异步等待的通知

## 互斥体
+ [futex_wait](syscalls/futex_wait.md) - 在futex上等待
+ [futex_wake](syscalls/futex_wake.md) - 唤醒futex上的等待者
+ [futex_requeue](syscalls/futex_requeue.md) - 唤醒一些等待者，并重新排列等待者

## 虚拟内存对象 (VMOs)
+ [vmo_create](syscalls/vmo_create.md) - 创建一个新的虚拟内存对象
+ [vmo_read](syscalls/vmo_read.md) - 从虚拟内存对象读取
+ [vmo_write](syscalls/vmo_write.md) - 向虚拟内存地址写入
+ [vmo_clone](syscalls/vmo_clone.md) - 克隆一个虚拟内存对象
+ [vmo_get_size](syscalls/vmo_get_size.md) - 获取虚拟内存对象大小
+ [vmo_set_size](syscalls/vmo_set_size.md) - 调整虚拟内存对象大小
+ [vmo_op_range](syscalls/vmo_op_range.md) - 在一系列虚拟内存对象上操作

## 虚拟内存地址区 (VMARs)
+ [vmar_allocate](syscalls/vmar_allocate.md) - 创建新的子虚拟内存地址区
+ [vmar_map](syscalls/vmar_map.md) - 将虚拟内存对象映射到进程中
+ [vmar_unmap](syscalls/vmar_unmap.md) - 从进程地址空间解映射内存
+ [vmar_protect](syscalls/vmar_protect.md) - 调整内存权限
+ [vmar_destroy](syscalls/vmar_destroy.md) - 销毁虚拟内存地址区和他的所有孩子们

## 加密安全RNG
+ [cprng_draw](syscalls/cprng_draw.md)
+ [cprng_add_entropy](syscalls/cprng_add_entropy.md)

## 时间
+ [nanosleep](syscalls/nanosleep.md) - 睡眠数纳秒
+ [clock_get](syscalls/clock_get.md) - 读取系统时钟
+ [clock_get_monotonic](syscalls/clock_get_monotonic.md) - 读取单调时钟
+ [ticks_get](syscalls/ticks_get.md) - 读取高精度计时器滴答
+ [ticks_per_second](syscalls/ticks_per_second.md) - 在一秒钟内读取高精度计时器滴答数

## 定时器
+ [timer_create](syscalls/timer_create.md) - 创建定时器对象
+ [timer_set](syscalls/timer_set.md) - 开始执行定时器
+ [timer_cancel](syscalls/timer_cancel.md) - 退出定时器

## 访客管理程序
+ [guest_create](syscalls/guest_create.md) - 创建一个访客
+ [guest_set_trap](syscalls/guest_set_trap.md) - 在访客中设置一个陷阱

## CPU虚拟化
+ [vcpu_create](syscalls/vcpu_create.md) - 创建一个虚拟CPU
+ [vcpu_resume](syscalls/vcpu_resume.md) - 恢复执行虚拟CPU
+ [vcpu_interrupt](syscalls/vcpu_interrupt.md) - 在虚拟CPU上引发中断
+ [vcpu_read_state](syscalls/vcpu_read_state.md) - 从虚拟CPU读取状态信息
+ [vcpu_write_state](syscalls/vcpu_write_state.md) - 将状态信息写入虚拟CPU

## 全局系统信息
+ [system_get_features](syscalls/system_get_features.md) - 获取硬件特有得特性
+ [system_get_num_cpus](syscalls/system_get_num_cpus.md) - 获取CPU数目
+ [system_get_physmem](syscalls/system_get_physmem.md) - 获取物理内存大小
+ [system_get_version](syscalls/system_get_version.md) - 获取版本字符串

## 日志
+ log_create - 创建内核管理的日志读取器或编写器
+ log_write - 将日志条目写入日志
+ log_read - 从日志中读取日志条目

## 多功能
+ [vmar_unmap_handle_close_thread_exit](syscalls/vmar_unmap_handle_close_thread_exit.md) - 三合一
+ [futex_wake_handle_close_thread_exit](syscalls/futex_wake_handle_close_thread_exit.md) - 三合一

## 驱动开发套件
+ [cache_flush](syscalls/cache_flush.md) - 刷新CPU数据和/或指令缓存
+ [interrupt_ack](syscalls/interrupt_ack.md) - 确认中断对象
+ [interrupt_bind](syscalls/interrupt_bind.md) - 绑定中断对象到端口
+ [interrupt_create](syscalls/interrupt_create.md) - 创建物理或虚拟中断对象
+ [interrupt_destroy](syscalls/interrupt_destroy.md) - 销毁一个中断对象
+ [interrupt_trigger](syscalls/interrupt_trigger.md) - 触发虚拟中断对象
+ [interrupt_wait](interrupt_wait.md) - 等待一个中断对象
+ [smc_call](syscalls/smc_call.md) - 从用户空间进行SMC调用
+ acpi_uefi_rsdp
+ ioports_request
+ framebuffer_set_range
+ vmo_create_contiguous
+ vmo_create_physical

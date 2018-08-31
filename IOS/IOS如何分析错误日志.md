---
title : oc notes
Time  : 2018-05-10
author: sunflowerseat

---

# 错误日志查看

## 步骤

打开CrashTest



## 分析

- **Bad Memory Access [EXC_BAD_ACCESS // SIGSEGV // SIGBUS]**

  进程试图访问无效的内存，或试图以内存的保护级别所不允许的方式去访问内存（例如，写入到只读存储器）。异常类型字段(Exception Subtype)包含一个kern_return_t描述错误，和错误的访问的内存地址。

  这里是调试一个Bad Memory Access的一些小技巧:

  1.如果objc_msgSend或者objc_release在回溯(Backtraces)的顶部附近，这个进程可能是尝试给一个释放的对象发送消息。你应该用Zombies instrument(调试僵尸对象的工具)来更好的理解这个崩溃。

  2.如果gpus_ReturnNotPermittedKillClient在回溯的顶部附近，这个进程是由于在后台尝试用OpenGL ES 或者 Metal来渲染，而被杀掉的。[See QA1766: How to fix OpenGL ES application crashes when moving to the background.](https://developer.apple.com/library/content/qa/qa1766/_index.html)

  3.用地址消毒剂([Address Sanitizer](https://developer.apple.com/videos/play/wwdc2015/413/),这是xcode7引入的新特性 )来跑程序。

  The address sanitizer adds additional instrumentation around memory access in your compiled code. As your application runs, Xcode will alert you if memory is accessed in a way that could lead to a crash.

- **Abnormal Exit (异常退出)[EXC_CRASH // SIGABRT]**

  进程异常退出。该异常类型崩溃最常见的原因是未捕获的Objective-C和C++异常和调用abort()。

  如果他们需要太多的时间来初始化，程序将被终止，因为触发了看门狗。如果是因为启动的时候被挂起，所产生的崩溃报告异常类型(Exception Subtype)将是launch_hang。

- **Trace Trap (追踪捕获)[EXC_BREAKPOINT // SIGTRAP]**

  类似于异常退出,这个异常是为了给附加的调试器中断的过程的机会在其执行一个特定的点。您可以从您自己的代码引发此异常使用__builtin_trap()函数。如果没有调试器连接,进程将被终止并生成崩溃报告。

- **Illegal Instruction(非法指令) [EXC_BAD_INSTRUCTION // SIGILL]**

  进程试图执行非法或未定义指令。这个进程可能试图通过一个配置错误的函数指针，跳到一个无效的地址。在英特尔处理器,ud2操作码造成EXC_BAD_INSTRUCTION异常但通常用于调试的时候的追踪。在英特尔处理器上的swift代码因为这个异常类型而被终止，如果在运行时遇到意外情况。有关详细信息,请参阅追踪捕获。

- **Resource Limit [EXC_RESOURCE]**

  这个进程超出了资源消耗的限制。这是一个从操作系统通知,进程是使用太多的资源。这虽然不是崩溃但也会生成崩溃日志。

  其它的异常信息

  0x8badf00d: 读做 “ate bad food”! (把数字换成字母，是不是很像 :p)该编码表示应用是因为发生watchdog超时而被iOS终止的。 通常是应用花费太多时间而无法启动、终止或响应用系统事件。

  0xbad22222: 该编码表示 VoIP 应用因为过于频繁重启而被终止。

  0xdead10cc: 读做 “dead lock”!该代码表明应用因为在后台运行时占用系统资源，如通讯录数据库不释放而被终止 。

  0xdeadfa11: 读做 “dead fall”! 该代码表示应用是被用户强制退出的。根据苹果文档, 强制退出发生在用户长按开关按钮直到出现 “滑动来关机”, 然后长按 Home按钮。强制退出将产生 包含0xdeadfa11 异常编码的崩溃日志, 因为大多数是强制退出是因为应用阻塞了界面。

  待补充
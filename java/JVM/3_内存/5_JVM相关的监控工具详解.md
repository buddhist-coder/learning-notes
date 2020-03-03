# JVM 相关的监控工具详解

- ## `jps` 查看所有java进程
  
  - `jps -l`：输出主类的全名，如果进程执行的是 Jar 包，输出 Jar 路径。

    ```no
    3761 org.jetbrains.jps.cmdline.Launcher
    4692 jdk.jcmd/sun.tools.jps.Jps
    ```

  - `jps -v`：输出虚拟机进程启动时 JVM 参数。
  - `jps -m`：输出传递给 Java 进程 main() 函数的参数。

- ## `jstat` 监视虚拟机各种运行状态信息
  
    jstat（JVM Statistics Monitoring Tool） 使用于监视虚拟机各种运行状态信息的命令行工具。 它可以显示本地或者远程（需要远程主机提供 RMI 支持）虚拟机进程中的类信息、内存、垃圾收集、JIT 编译等运行数据，在没有 GUI，只提供了纯文本控制台环境的服务器上，它将是运行期间定位虚拟机性能问题的首选工具。
  - 选项
    - `-class` 类加载器统计

    列 | 描述
    ---|---
    Loaded   | 加载的类数
    Bytes    | 加载的字节数(kb)
    Unloaded | 卸载的类数
    Bytes    | 卸载的字节数(kb)
    Time     | 执行类加载和卸载操作所花费的时间
    - `-compiler` HotSpot即时编译器统计信息
  
    列 | 描述
    ---|---
    Compiled     | 完成编译任务
    Failed      | 失败的编译任务数
    Invalid      | 无效的编译任务数
    Time     | 执行编译任务所花费的时间
    FailedType   | 上次失败的编译的编译类型
    FailedMethod | 上次编译失败的类名和方法
    - `-gc` 垃圾收集的堆统计信息

    列 | 描述
    ---|---
    SOC | 当前幸存者空间0容量 (KB).
    S1C | 当前幸存者空间1的容量 (KB).
    S0U | 幸存者空间0的使用大小 (KB).
    S1U | 幸存者空间1的使用大小 (KB).
    EC  | 当前新生代空间容量 (KB).
    EU  | 当前新生代空间使用大小 (KB).
    OC  | 当前的老年代空间容量 (KB).
    OU  | 当前的老年代空间使用大小(KB).
    PC  | 当前的永久空间容量 (KB).
    PU  | 当前的永久空间使用大小 (KB).
    YGC | 新生代GC活动数量.
    YGCT    | 新生代垃圾收集时间.
    FGC | full GC事件数.
    FGCT    | 全部垃圾收集时间.
    GCT | 垃圾收集总时间.
    - `-gccapacity` 内存池生成和空间容量

    列 | 描述
    ---|---
    NGCMN  | 最小新一代容量 (KB).
    NGCMX  | 最大新一代容量 (KB).
    NGC    | 当前新生代容量 (KB).
    S0C    | 当前幸存者空间0容量 (KB).
    S1C    | 当前幸存者空间1的容量 (KB).
    EC     | 当前新生代空间容量 (KB).
    OGCMN  | 最小老年代容量(KB).
    OGCMX  | 最大老年代容量 (KB).
    OGC    | 当前的老年代容量 (KB).
    OC     | 当前的旧空间容量 (KB).
    PGCMN  | 最小的永久空间容量 (KB).
    PGCMX  | 最大的永久空间容量 (KB).
    PGC    | 当前永久生成空间 (KB).
    PC     | 当前的永久空间容量 (KB).
    VGC    | 新生代GC事件的数量.
    FGC    | Full GC 时间的数量.
    - `-gccause` 此选项显示与`-gcutil选`项相同的垃圾收集统计信息摘要，但包括上一个垃圾收集事件和（如果适用）当前垃圾收集事件的原因。 除了为`-gcutil`列出的列之外，此选项还添加了以下列：

    列 | 描述
    ---|---
    LGCC   | 最后一次垃圾回收的原因。
    GCC    | 当前GarbagCollection的原因。
    - `-gcnew` 新生代统计

    列 | 描述
    ---|---
    SOC | 当前幸存者空间0容量 (KB).  
    S1C | 当前幸存者空间1的容量 (KB).  
    S0U | 幸存者空间0的使用大小 (KB).
    S1U | 幸存者空间1的使用大小 (KB).
    TT  | 任职期限.
    MTT | 最大任职期限.
    DSS | 所需的幸存者大小 (KB).
    EC  | 当前新生代空间容量 (KB).
    EU  | 当前新生代空间使用大小(KB).
    VGC | 新生代GC事件的数.  
    VGCT    | 新生代垃圾收集时间.
    - `-gcnewcapacity` 新生代空间大小统计

    列 | 描述
    ---|---
    NGCMN  | 最小新生代容量 (KB).
    NGCMX  | 最大新生代容量(KB).
    NGC | 当前新生代容量 (KB).  
    S0CMX  | 最大幸存者0区容量 (KB).
    S0C  | 当前幸存者0区容量 (KB).
    S1CMX  | 最大幸存者1区容量 (KB).
    S1C  | 当前幸存者1区容量 (KB).
    ECMX    | 最大的出生区大小 (KB).
    EC  | 当前的出生区大小 (KB).
    YGC | 新生代GC事件的数.
    FGC | full GC 事件的数.
    - `-gcold` 老年代和永久代统计

    列 | 描述
    ---|---
    PC   | 当前永久代容量(KB).
    PU   | 当前永久代已使用大小 (KB).  
    OC   | 当前老年代容量 (KB).
    OU   | 当前老年代已使用大小(KB).
    YGC  | 新生代GC事件的数.
    FGC   | full GC 事件的数.
    FGCT   | Full GC 收集时间.
    GCT   | 全部的垃圾收集时间.
    - `-gcoldcapacity` 老年代统计

    列 | 描述
    ---|---
    OGCMN  | 最小的老年代容量 (KB).
    OGCMV  | 最大的老年代容量 (KB).
    OGC  | 当前的老年代容量 (KB).
    OC  | 当前的旧空间容量 (KB).
    YGC  | 新生代GC事件的数.
    FGC  | full GC 事件的数.
    FGCT   | Full GC 收集时间.
    GCT  | 全部的垃圾收集时间.
    - `-gcpermcapacity` 永久代统计

    列 | 描述
    ---|---
    PGCMN  | 最小的永久区容量(KB).
    PGCMX  | 最大的永久区容量 (KB).
    PGC    | 当前的永久代容量 (KB).
    PC     | 当前的永久代容量 (KB).
    YGC    | 新生代GC事件的数.
    FGC    | full GC 事件的数.
    FGCT   | Full GC 收集时间.
    GCT    | 全部的垃圾收集时间.
    - `-gcutil` 垃圾收集统计摘要

    列 | 描述
    ---|---
    |S0 | 生存空间0利用率占空间当前容量的百分比
    |S1 | 生存空间1利用率占空间当前容量的百分比
    |E  | 新生代空间利用率占空间当前容量的百分比
    |O  | 老年代空间利用率占空间当前容量的百分比
    |P  | 永久区空间利用率占空间当前容量的百分比
    |YGC    | 新生代GC事件的数.
    |YGCT| 新生代垃圾收集时间.
    |FGC| full GC 事件的数.
    |FGCT| Full GC 收集时间.
    |GCT| 全部的垃圾收集时间.
    - `-printcompilation` HotSpot编译器方法统计

    列 | 描述
    ---|---
    Compiled | 执行的编译任务数.
    Size| 该方法的字节码字节数.  
    Type| 编译类型.
    Method| 类名和方法名，标识已编译的方法。 类名使用“/”代替“.” 作为名称空间分隔符。 方法名称是给定类中的方法。 这两个字段的格式与HotSpot-XX：+ PrintComplation选项一致。
    - 案例
      - `jstat -gcutil 21891 250 7`此示例附加到lvmid 21891，并以250毫秒的间隔进行7个采样，并显示-gcutil选项指定的输出。

        ```no
        S0     S1     E      O      P     YGC    YGCT    FGC    FGCT     GCT
        12.44   0.00  27.20   9.49  96.70    78    0.176     5    0.495    0.672
        12.44   0.00  62.16   9.49  96.70    78    0.176     5    0.495    0.672
        12.44   0.00  83.97   9.49  96.70    78    0.176     5    0.495    0.672
        0.00   7.74   0.00   9.51  96.70    79    0.177     5    0.495    0.673
        0.00   7.74  23.37   9.51  96.70    79    0.177     5    0.495    0.673
        0.00   7.74  43.82   9.51  96.70    79    0.177     5    0.495    0.673
        0.00   7.74  58.11   9.51  96.71    79    0.177     5    0.495    0.673
        ```

        此示例的输出显示在第3个样本和第4个样本之间发生了年轻代收集。 收集耗时0.001秒，将对象从出生代空间（E）提升到旧空间 空间（O），导致旧空间利用率从9.49％增加到9.51％。 在收集之前，幸存者空间的利用率为12.44％，但是在收集之后，仅使用了7.74％

      - `jstat -gcnew -h3 21891 250`此示例附加到lvmid 21891，并以250毫秒的间隔进行采样，并显示-gcutil选项指定的输出。 另外，它使用-h3选项输出列 每3行数据后的标头。

        ```no
        S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
        64.0   64.0    0.0   31.7 31  31   32.0    512.0    178.6    249    0.203
        64.0   64.0    0.0   31.7 31  31   32.0    512.0    355.5    249    0.203
        64.0   64.0   35.4    0.0  2  31   32.0    512.0     21.9    250    0.204
        S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
        64.0   64.0   35.4    0.0  2  31   32.0    512.0    245.9    250    0.204
        64.0   64.0   35.4    0.0  2  31   32.0    512.0    421.1    250    0.204
        64.0   64.0    0.0   19.0 31  31   32.0    512.0     84.4    251    0.204
        S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
        64.0   64.0    0.0   19.0 31  31   32.0    512.0    306.7    251    0.204
        ```

        除了显示重复的标题字符串外，此示例还显示在第2个和第3个样本之间，出现了年轻的GC。 持续时间为0.001秒。 收集发现了足够的存活数据，以至于生存空间0的利用率（S0U）将超过所需的生存空间大小（DSS）。 结果，对象被提升为旧一代（在此输出中不可见），并且任职期限（TT）从31降低为2。

    - `jstat -gcoldcapacity -t 21891 250 3` 此示例连接到lvmid 21891，并以250毫秒的间隔进行3个采样。 -t选项用于为第一列中的每个样本生成时间戳记

        ```no
            Timestamp  OGCMN    OGCMX    OGC      OC       YGC   FGC   FGCT    GCT
            150.1      1408.0   60544.0  11696.0  11696.0  194   80    2.874   3.799
            150.4      1408.0   60544.0  13820.0  13820.0  194   81    2.938   3.863
            150.7      1408.0   60544.0  13820.0  13820.0  194   81    2.938   3.863
        ```

- ## `jinfo` 实时地查看和调整虚拟机各项参数

  - 选项
    - `jinfo vmid`:输出当前 jvm 进程的全部参数和系统属性 (第一部分是系统的属性，第二部分是 JVM 的参数)。
    - `jinfo -flag name vmid` :输出对应名称的参数的具体值。比如输出 MaxHeapSize、查看当前 jvm 进程是否开启打印 GC 日志 ( -XX:PrintGCDetails :详细 GC 日志模式，这两个都是默认关闭的)。

        ```no
        C:\Users\SnailClimb>jinfo  -flag MaxHeapSize 17340
        -XX:MaxHeapSize=2124414976
        C:\Users\SnailClimb>jinfo  -flag PrintGC 17340
        -XX:-PrintGC
        ```

    - 使用 jinfo 可以在不重启虚拟机的情况下，可以动态的修改 jvm 的参数。尤其在线上的环境特别有用,请看下面的例子：`jinfo -flag [+|-]name vmid`开启或者关闭对应名称的参数。

        ```no
        C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
        -XX:-PrintGC

        C:\Users\SnailClimb>jinfo  -flag  +PrintGC 17340

        C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
        -XX:+PrintGC
        ```

- ## `jmap` 生成堆转储快照

    `jmap`（Memory Map for Java）命令用于生成堆转储快照。 如果不使用 `jmap` 命令，要想获取 Java 堆转储，可以使用 `“-XX:+HeapDumpOnOutOfMemoryError”` 参数，可以让虚拟机在 OOM 异常出现之后自动生成 dump 文件，Linux  命令下可以通过 kill -3 发送进程退出信号也能拿到 dump 文件。  
    `jmap` 的作用并不仅仅是为了获取 dump 文件，它还可以查询 finalizer 执行队列、Java 堆和永久代的详细信息，如空间使用率、当前使用的是哪种收集器等。和jinfo一样，jmap有不少功能在 Windows 平台下也是受限制的。  
    示例：将指定应用程序的堆快照输出到桌面。后面，可以通过 jhat、Visual VM 等工具分析该堆文件。

    ```no
    C:\Users\SnailClimb>jmap -dump:format=b,file=C:\Users\SnailClimb\Desktop\heap.hprof 17340
    Dumping heap to C:\Users\SnailClimb\Desktop\heap.hprof ...
    Heap dump file created
    ```

- ## `jhat` 分析 heapdump 文件
  
    `jhat` 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。  

    ```no
    C:\Users\SnailClimb>jhat C:\Users\SnailClimb\Desktop\heap.hprof
    Reading from C:\Users\SnailClimb\Desktop\heap.hprof...
    Dump file created Sat May 04 12:30:31 CST 2019
    Snapshot read, resolving...
    Resolving 131419 objects...
    Chasing references, expect 26 dots..........................
    Eliminating duplicate references..........................
    Snapshot resolved.
    Started HTTP server on port 7000
    Server is ready.
    ```

    访问 `http://localhost:7000/`

- ## jstack 生成虚拟机当前时刻的线程快照
  
    jstack（Stack Trace for Java）命令用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合.  
    生成线程快照的目的主要是定位线程长时间出现停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的原因。线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做些什么事情，或者在等待些什么资源。  
    **下面是一个线程死锁的代码。我们下面会通过 jstack 命令进行死锁检查，输出死锁信息，找到发生死锁的线程。**

    ```java
    public class DeadLockDemo {
        private static Object resource1 = new Object();//资源 1
        private static Object resource2 = new Object();//资源 2

        public static void main(String[] args) {
            new Thread(() -> {
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread() + "waiting get resource2");
                    synchronized (resource2) {
                        System.out.println(Thread.currentThread() + "get resource2");
                    }
                }
            }, "线程 1").start();

            new Thread(() -> {
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread() + "waiting get resource1");
                    synchronized (resource1) {
                        System.out.println(Thread.currentThread() + "get resource1");
                    }
                }
            }, "线程 2").start();
        }
    }
    ```

    输出

    ```no
    Thread[线程 1,5,main]get resource1
    Thread[线程 2,5,main]get resource2
    Thread[线程 1,5,main]waiting get resource2
    Thread[线程 2,5,main]waiting get resource1
    ```

    线程 A 通过 synchronized (resource1) 获得 resource1 的监视器锁，然后通过 Thread.sleep(1000);让线程 A 休眠 1s 为的是让线程 B 得到执行然后获取到 resource2 的监视器锁。线程 A 和线程 B 休眠结束了都开始企图请求获取对方的资源，然后这两个线程就会陷入互相等待的状态，这也就产生了死锁。
    **通过 jstack 命令分析：**

    ```no
    C:\Users\SnailClimb>jps
    13792 KotlinCompileDaemon
    7360 NettyClient2
    17396
    7972 Launcher
    8932 Launcher
    9256 DeadLockDemo
    10764 Jps
    17340 NettyServer

    C:\Users\SnailClimb>jstack 9256
    ```

    **输出的部分内容如下：**

    ```no
    Found one Java-level deadlock:
    =============================
    "线程 2":
      waiting to lock monitor 0x000000000333e668 (object 0x00000000d5efe1c0, a java.lang.Object),
      which is held by "线程 1"
    "线程 1":
      waiting to lock monitor 0x000000000333be88 (object 0x00000000d5efe1d0, a java.lang.Object),
      which is held by "线程 2"

    Java stack information for the threads listed above:
    ===================================================
    "线程 2":
            at DeadLockDemo.lambda$main$1(DeadLockDemo.java:31)
            - waiting to lock <0x00000000d5efe1c0> (a java.lang.Object)
            - locked <0x00000000d5efe1d0> (a java.lang.Object)
            at DeadLockDemo$$Lambda$2/1078694789.run(Unknown Source)
            at java.lang.Thread.run(Thread.java:748)
    "线程 1":
            at DeadLockDemo.lambda$main$0(DeadLockDemo.java:16)
            - waiting to lock <0x00000000d5efe1d0> (a java.lang.Object)
            - locked <0x00000000d5efe1c0> (a java.lang.Object)
            at DeadLockDemo$$Lambda$1/1324119927.run(Unknown Source)
            at java.lang.Thread.run(Thread.java:748)

    Found 1 deadlock
    ```

    可以看到 jstack 命令已经帮我们找到发生死锁的线程的具体信息。

- ## `jcmd`

    在JDK1.7以后，新增了一个命令行工具 jcmd。他是一个多功能的工具，可以用它来导出堆、查看Java进程、导出线程信息、执行GC、还可以进行采样分析（jmc 工具的飞行记录器）。

- `jcmd pid VM.flags` 查看对应的VM启动参数

    ```no
    xiexiaofeis-MBP:~ xiexiaofei$ jcmd 3761 VM.flags
    3761:
    -XX:CICompilerCount=4 -XX:ConcGCThreads=2 -XX:G1ConcRefinementThreads=8 -XX:G1HeapRegionSize=1048576 -XX:GCDrainStackTargetSize=64 -XX:InitialHeapSize=268435456
    -XX:MarkStackSize=4194304 -XX:MaxHeapSize=734003200 -XX:MaxNewSize=440401920 -XX:MinHeapDeltaBytes=1048576 -XX:NonNMethodCodeHeapSize=5836300
    -XX:NonProfiledCodeHeapSize=122910970 -XX:ProfiledCodeHeapSize=122910970 -XX:ReservedCodeCacheSize=251658240 -XX:+SegmentedCodeCache
    -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseG1GC
    ```

- `jcmd pid hlep` 列出当前运行的java进程可以执行的操作

    ```no
    xiexiaofeis-MBP:~ xiexiaofei$ jcmd 7840 help
    7840:
    The following commands are available:
    Compiler.CodeHeap_Analytics
    Compiler.codecache
    Compiler.codelist
    Compiler.directives_add
    Compiler.directives_clear
    Compiler.directives_print
    Compiler.directives_remove
    Compiler.queue
    GC.class_histogram
    GC.class_stats
    GC.finalizer_info
    GC.heap_dump
    GC.heap_info
    GC.run
    GC.run_finalization
    JFR.check
    JFR.configure
    JFR.dump
    JFR.start
    JFR.stop
    JVMTI.agent_load
    JVMTI.data_dump
    ManagementAgent.start
    ManagementAgent.start_local
    ManagementAgent.status
    ManagementAgent.stop
    Thread.print
    VM.check_commercial_features
    VM.class_hierarchy
    VM.classloader_stats
    VM.classloaders
    VM.command_line
    VM.dynlibs
    VM.flags
    VM.info
    VM.log
    VM.metaspace
    VM.native_memory
    VM.print_touched_methods
    VM.set_flag
    VM.stringtable
    VM.symboltable
    VM.system_properties
    VM.systemdictionary
    VM.unlock_commercial_features
    VM.uptime
    VM.version
    help
    ```

- `jcmd pid help <command>` 查看具体命令的选项有哪些

    ```no
    xiexiaofeis-MBP:~ xiexiaofei$ jcmd 7839 help VM.flags
    7839:
    VM.flags
    Print VM flag options and their current values.

    Impact: Low

    Permission: java.lang.management.ManagementPermission(monitor)

    Syntax : VM.flags [options]

    Options: (options must be specified using the <key> or <key>=<value> syntax)
        -all : [optional] Print all flags supported by the VM (BOOLEAN, false)
    ```

- `jcmd pid PerfCounter.print` 查看JVM性能相关的参数
- `jcmd pid VM.uptime` 查看JVM的启动时长
- `jcmd pid GC.class_histogram` 查看系统中类的统计信息
- `jcmd pid Thread.print` 查看系统的当前堆栈信息
- `jcmd pid GC.heap_dump filename.hprof` 导出heap dump文件，导出的文件可通过jvisualvm查看
- `jcmd pid VM.system_properties` 查看jvm的属性信息

- ## `jmc`
  
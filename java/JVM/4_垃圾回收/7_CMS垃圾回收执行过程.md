# CMS 垃圾回收执行过程

## 运行参数

```no
    -verbose:gc
    -Xms20M
    -Xmx20M
    -Xmn10M
    -XX:+PrintGCDetails
    -XX:SurvivorRatio=8
    -XX:+UseConcMarkSweepGC
```

## 代码案例

```java
    package com.xie.jvm.gc;

    public class MyTest5 {
        public static void main(String[] args) {
            //1m容量
            int size = 1024 * 1024;

            byte[] myAllocl = new byte[4 * size];
            System.out.println("myAllocl");
            byte[] myAlloc2 = new byte[4 * size];
            System.out.println("myAlloc2");
            byte[] myAlloc3 = new byte[4 * size];
            System.out.println("myAlloc3");
            byte[] myAlloc4 = new byte[2 * size];
            System.out.println("myAlloc4");
        }
    }
```

## 输出结果

```no
    myAllocl
    [GC (Allocation Failure) [ParNew: 5119K->386K(9216K), 0.0053722 secs] 5119K->4484K(19456K), 0.0054441 secs] [Times: user=0.02 sys=0.01, real=0.00 secs]
    myAlloc2
    [GC (Allocation Failure) [ParNew: 4641K->468K(9216K), 0.0054803 secs] 8739K->8664K(19456K), 0.0055062 secs] [Times: user=0.04 sys=0.00, real=0.01 secs]
    [GC (CMS Initial Mark) [1 CMS-initial-mark: 8196K(10240K)] 12760K(19456K), 0.0002606 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [CMS-concurrent-mark-start]
    myAlloc3
    [CMS-concurrent-mark: 0.000/0.000 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [CMS-concurrent-preclean-start]
    [CMS-concurrent-preclean: 0.000/0.000 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [CMS-concurrent-abortable-preclean-start]
    [CMS-concurrent-abortable-preclean: 0.000/0.000 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [GC (CMS Final Remark) [YG occupancy: 6934 K (9216 K)][Rescan (parallel) , 0.0011391 secs][weak refs processing, 0.0000275 secs][class unloading, 0.0002312 secs][scrub symbol table, 0.0003384 secs][scrub string table, 0.0002085 secs][1 CMS-remark: 8196K(10240K)] 15130K(19456K), 0.0020070 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [CMS-concurrent-sweep-start]
    [CMS-concurrent-sweep: 0.000/0.000 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    [CMS-concurrent-reset-start]
    myAlloc4
    [CMS-concurrent-reset: 0.000/0.000 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
    Heap
    par new generation   total 9216K, used 7098K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
    eden space 8192K,  80% used [0x00000007bec00000, 0x00000007bf279a08, 0x00000007bf400000)
    from space 1024K,  45% used [0x00000007bf400000, 0x00000007bf475010, 0x00000007bf500000)
    to   space 1024K,   0% used [0x00000007bf500000, 0x00000007bf500000, 0x00000007bf600000)
    concurrent mark-sweep generation total 10240K, used 8196K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
    Metaspace       used 2684K, capacity 4486K, committed 4864K, reserved 1056768K
    class space    used 288K, capacity 386K, committed 512K, reserved 1048576K
```

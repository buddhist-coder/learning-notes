# MaxTenuringThreshold与阈值的动态调整

MaxTenuringThreshold作用：在可以自动调节对象晋升(Promote)到老年代阈值的GC中，设置该值得最大值。即经历过最大值次数的GC，对象就会晋升到老年代；

该参数的默认值是15，CMS中默认值是6，G1中默认值15（在JVM中，该数值是由4个bit来表示的，所以最大值1111，即15)；

经历了多次GC后，存活的对象会在FromSurvivor和ToSurvivor之间来回存放，而这里面的一个前提是这两个空间有足够的大小来存放这些数据，在GC算法中，会计算每个对象年龄的大小，如果达到某个年龄后发现总大小已经大于Survivor空间的50%，那么这时就需要调整阈值，不能再继续等到默认的15次GC后才晋升，因为这样会导致Survivor空间不足，所以需要调整阈值，让这些存活的对象尽快完成晋升。

## 案例分析1

- 运行参数

    ```no
        -verbose:gc
        -Xms20M
        -Xmx20M
        -Xmn10M
        -XX:+PrintGCDetails
        -XX:+PrintCommandLineFlags          打印JVM启动参数
        -XX:SurvivorRatio=8
        -XX:+PrintGCDetails
        -XX:MaxTenuringThreshold=5          设置在晋升到老年代对象的最大存活年龄（经历过Minor GC 的次数）。当有些对象一直在FromSurvivor区和ToSurvivor区来回复制时，是耗费性能的。
        -XX:+PrintTenuringDistribution      打印各个年龄对象占用的字节
    ```

- 代码案例

    ```java
        package com.xie.jvm.gc;

        /**
        *  -verbose:gc
        *  -Xms20M
        *  -Xmx20M
        *  -Xmn10M
        *  -XX:+PrintGCDetails
        *  -XX:SurvivorRatio=8
        *  -XX:+PrintGCDetails
        *  -XX:MaxTenuringThreshold=5
        *  -XX:+PrintTenuringDistribution
        */
        public class MyTest3 {
            public static void main(String[] args) {
                //1m容量
                int size = 1024 * 1024;

                byte[] myAllocl = new byte[2 * size];
                System.out.println("myAllocl");
                byte[] myAlloc2 = new byte[2 * size];
                System.out.println("myAlloc2");
                byte[] myAlloc3 = new byte[2 * size];
                System.out.println("myAlloc3");
                byte[] myAlloc4 = new byte[2 * size];
                System.out.println("myAlloc4");
            }
        }
    ```

- 输出结果

    ```no
        -XX:InitialHeapSize=20971520 -XX:InitialTenuringThreshold=5 -XX:MaxHeapSize=20971520 -XX:MaxNewSize=10485760 -XX:MaxTenuringThreshold=5 -XX:NewSize=10485760 -XX:+PrintCommandLineFlags -XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintTenuringDistribution -XX:SurvivorRatio=8 -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseParallelGC
        myAllocl
        myAlloc2
        myAlloc3
        [GC (Allocation Failure)
        Desired survivor size 1048576 bytes, new threshold 5 (max 5)
        [PSYoungGen: 7167K->496K(9216K)] 7167K->6648K(19456K), 0.0068179 secs] [Times: user=0.03 sys=0.00, real=0.01 secs]
        [Full GC (Ergonomics) [PSYoungGen: 496K->0K(9216K)] [ParOldGen: 6152K->6488K(10240K)] 6648K->6488K(19456K), [Metaspace: 2678K->2678K(1056768K)], 0.0094247 secs] [Times: user=0.03 sys=0.00, real=0.01 secs]
        myAlloc4
        Heap
        PSYoungGen      total 9216K, used 2290K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
        eden space 8192K, 27% used [0x00000007bf600000,0x00000007bf83c9a0,0x00000007bfe00000)
        from space 1024K, 0% used [0x00000007bfe00000,0x00000007bfe00000,0x00000007bff00000)
        to   space 1024K, 0% used [0x00000007bff00000,0x00000007bff00000,0x00000007c0000000)
        ParOldGen       total 10240K, used 6488K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
        object space 10240K, 63% used [0x00000007bec00000,0x00000007bf256360,0x00000007bf600000)
        Metaspace       used 2684K, capacity 4486K, committed 4864K, reserved 1056768K
        class space    used 288K, capacity 386K, committed 512K, reserved 1048576K
    ```

## 案例分析2

- 运行参数
  
    ```no
        -verbose:gc
        -Xmx200M
        -Xmn50M
        -XX:TargetSurvivorRatio=60          当某个Survivor空间占据的容量超过60%时，会重新计算对象晋升的阈值，而不会使用配置的阈值（这里配置的阈值是3）
        -XX:+PrintGCDetails
        -XX:+PrintTenuringDistribution
        -XX:+PrintGCDateStamps
        -XX:+UseConcMarkSweepGC             老年代使用CMS垃圾收集器
        -XX:+UseParNewGC                    新生代使用ParNew垃圾收集器
        -XX:MaxTenuringThreshold=3          设置对象晋升的阈值为3
    ```

- 代码案例

    ```java
        package com.xie.jvm.gc;

        public class MyTest4 {
            public static void main(String[] args) throws InterruptedException {
                byte[] byte_1 = new byte[512 * 1024];
                byte[] byte_2 = new byte[512 * 1024];

                myGC();
                Thread.sleep(1000);

                System.out.println("111111");

                myGC();
                Thread.sleep(1000);

                System.out.println("222222");

                myGC();
                Thread.sleep(1000);

                System.out.println("333333");

                myGC();
                Thread.sleep(1000);

                System.out.println("444444");

                byte[] byte_3 = new byte[1024 * 1024];
                byte[] byte_4 = new byte[1024 * 1024];
                byte[] byte_5 = new byte[1024 * 1024];

                myGC();
                Thread.sleep(1000);

                System.out.println("555555");

                myGC();
                Thread.sleep(1000);

                System.out.println("666666");

                System.out.println("hello world");

            }

            private static void myGC() {
                for (int i = 0; i < 40; i++) {
                    byte[] byteArray = new byte[1024 * 1024];
                }
            }
        }
    ```

- 输出结果

    ```no
        2020-02-27T18:08:00.530-0800: [GC (Allocation Failure) 2020-02-27T18:08:00.530-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 3 (max 3)
        - age   1:    1403000 bytes,    1403000 total
        : 40551K->1389K(46080K), 0.0163765 secs] 40551K->1389K(199680K), 0.0165060 secs] [Times: user=0.00 sys=0.00, real=0.02 secs]
        111111
        2020-02-27T18:08:01.558-0800: [GC (Allocation Failure) 2020-02-27T18:08:01.558-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 3 (max 3)
        - age   1:     345288 bytes,     345288 total
        - age   2:    1391736 bytes,    1737024 total
        : 42129K->1840K(46080K), 0.0011310 secs] 42129K->1840K(199680K), 0.0011588 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
        222222
        2020-02-27T18:08:02.563-0800: [GC (Allocation Failure) 2020-02-27T18:08:02.563-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 3 (max 3)
        - age   1:         56 bytes,         56 total
        - age   2:     344608 bytes,     344664 total
        - age   3:    1391304 bytes,    1735968 total
        : 42358K->1785K(46080K), 0.0030415 secs] 42358K->1785K(199680K), 0.0030921 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
        333333
        2020-02-27T18:08:03.582-0800: [GC (Allocation Failure) 2020-02-27T18:08:03.582-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 3 (max 3)
        - age   1:         56 bytes,         56 total
        - age   2:         56 bytes,        112 total
        - age   3:     344632 bytes,     344744 total
        : 42508K->688K(46080K), 0.0080123 secs] 42508K->2067K(199680K), 0.0080706 secs] [Times: user=0.02 sys=0.00, real=0.01 secs]
        444444
        2020-02-27T18:08:04.597-0800: [GC (Allocation Failure) 2020-02-27T18:08:04.597-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 1 (max 3)
        - age   1:    3145832 bytes,    3145832 total
        - age   2:         56 bytes,    3145888 total
        - age   3:         56 bytes,    3145944 total
        : 41417K->3160K(46080K), 0.0027181 secs] 42796K->4895K(199680K), 0.0027534 secs] [Times: user=0.02 sys=0.00, real=0.01 secs]
        555555
        2020-02-27T18:08:05.604-0800: [GC (Allocation Failure) 2020-02-27T18:08:05.604-0800: [ParNew
        Desired survivor size 3145728 bytes, new threshold 3 (max 3)
        - age   1:         56 bytes,         56 total
        : 43893K->14K(46080K), 0.0026547 secs] 45627K->4821K(199680K), 0.0026888 secs] [Times: user=0.02 sys=0.00, real=0.00 secs]
        666666
        hello world
        Heap
        par new generation   total 46080K, used 15969K [0x00000007b3800000, 0x00000007b6a00000, 0x00000007b6a00000)
        eden space 40960K,  38% used [0x00000007b3800000, 0x00000007b4794990, 0x00000007b6000000)
        from space 5120K,   0% used [0x00000007b6000000, 0x00000007b6003b40, 0x00000007b6500000)
        to   space 5120K,   0% used [0x00000007b6500000, 0x00000007b6500000, 0x00000007b6a00000)
        concurrent mark-sweep generation total 153600K, used 4806K [0x00000007b6a00000, 0x00000007c0000000, 0x00000007c0000000)
        Metaspace       used 3190K, capacity 4486K, committed 4864K, reserved 1056768K
        class space    used 350K, capacity 386K, committed 512K, reserved 1048576K
    ```

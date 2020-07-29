# Java NIO 简介

Java NIO （new IO / Non Blocking IO）是从Java 1.4 版本开始引入的一个新的IO API，可以替代标准的Java IO API。NIO与原来的IO有同样的作用的目的，但是使用的方式完全不同，NIO支持面向缓冲区的、基于通道的IO操作。NIO将以更高效的方式进行文件的读写操作。

## Java NIO 与 IO 的主要区别

| IO | NIO |
| :-----:| :-----: |
| 面向流(Stream Oriented) | 面向缓冲区(Buffer Oriented) |
| 阻塞IO(Blocking IO) | 非阻塞IO(Non Block IO) |
| 无 | 选择器(Selectors) |

Java NIO系统的核心在于：通道(channel)和缓冲区(Buffer)。通道表示打开到IO设备（例如：文件，套接字）的连接。若需要使用NIO系统，需要获取用于连接IO设备的通道以及用于容纳数据的缓冲区。然后操作缓冲区，对数据进行处理。
简而言之，**Channel负责传输，Buffer负责存储。**

- 缓冲区（Buffer）：一个用于特定基本数据类型的容器。由java.nio包定义的，所有缓冲区都是Buffer抽象类的子类。
- Java NIO中的Buffer主要用于与NIO通道进行交互，数据是从通道读入缓冲区，从缓冲区写入通道中。
- 示例代码

```java
package nio.test.day01;

import org.junit.Test;

import java.nio.ByteBuffer;

/**
 * 一、缓冲区（Buffer）：在Java NIO中负责数据的存取。缓冲区就是数组。用于存储不同类型的数据
 * 根据数据类型不同(boolean 除外)，提供相应类型的缓冲区：
 * ByteBuffer
 * CharBuffer
 * ShortBuffer
 * IntBuffer
 * LongBuffer
 * FloatBuffer
 * DoubleBuffer
 * 上述缓冲区的管理方式几乎一致，通过allocate()获取缓冲区
 * <p>
 * 二、缓冲区存取数据的两个核心方法：
 * put()：存入数据到缓冲区
 * get(): 获取缓冲区中的数据
 * <p>
 * 三、缓冲区的四个核心属性：
 * capacity:容量，表示缓冲区中最大存储数据的容量。一旦声明不能改变。
 * limit：界限，表示缓冲区中可以操作数据的大小。(limit 后数据不能进行读写)
 * position:位置，表示缓存区中正在操作的位置。
 * mark：标记，表示记录当前position的位置。可以通过reset()将position恢复到mark的位置
 * 0 <= mark <= position <= limit <= capacity
 *
 * 四、直接缓冲区与非直接缓冲区
 * 非直接缓冲区：通过allocate()方法分配缓冲区，将缓冲区建立在JVM的内存中。
 * 直接缓冲区：通过allocateDirect()方法分配直接缓冲区，将缓冲区建立在物理内存中。可以提交效率。
 */
public class TestBuffer {
    @Test
    public void test1() {
        String str = "abcde";

        //1. 分配一个指定大小的缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        System.out.println(" --------------allocate()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        //2. 利用put()存入数据到缓冲区
        buffer.put(str.getBytes());
        System.out.println(" --------------put()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        //3. 切换到读取数据模式
        buffer.flip();
        System.out.println(" --------------flip()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        //4. 利用get()读取缓冲区中的数据
        byte[] dst = new byte[buffer.limit()];
        buffer.get(dst);
        System.out.println(new String(dst));

        System.out.println(" --------------get()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        //5. rewind(): 可重复读数据
        buffer.rewind();
        System.out.println(" --------------rewind()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        //6. 清空缓冲区.但是缓冲区中的数据依然存在，但是处于"被遗忘"状态
        buffer.clear();
        System.out.println(" --------------clear()-------------- ");
        System.out.println(buffer.position());
        System.out.println(buffer.limit());
        System.out.println(buffer.capacity());

        System.out.println((char) buffer.get());

        /*
        --------------allocate()--------------
        0
        1024
        1024
                --------------put()--------------
        5
        1024
        1024
                --------------flip()--------------
        0
        5
        1024
        abcde
                --------------get()--------------
        5
        5
        1024
                --------------rewind()--------------
        0
        5
        1024
                --------------clear()--------------
        0
        1024
        1024
        a
        */
    }

    @Test
    public void test2() {
        String str = "abcde";
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        buffer.put(str.getBytes());
        buffer.flip();

        byte[] dst = new byte[buffer.limit()];
        buffer.get(dst, 0, 2);
        System.out.println(new String(dst, 0, 2));
        System.out.println(buffer.position());

        //标记
        buffer.mark();
        buffer.get(dst, 2, 2);
        System.out.println(new String(dst, 2, 2));
        System.out.println(buffer.position());

        //reset():恢复到mark位置
        buffer.reset();
        System.out.println(buffer.position());

        //判断缓冲区是否还有剩余数据
        if (buffer.hasRemaining()) {
            //获取缓冲区中可以操作的数量
            System.out.println(buffer.remaining());
        }

        /*
        ab
        2
        cd
        4
        2
        3
        */
    }
}
```

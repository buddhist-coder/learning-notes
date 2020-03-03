# 通过字节码分析java异常处理机制

## 源代码

```java
package com.xie.jvm.bytecode;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;

public class MyTest3 {
    public void test() {
        try {
            InputStream is = new FileInputStream("test.txt");

            ServerSocket serverSocket = new ServerSocket(9999);
            serverSocket.accept();
        } catch (FileNotFoundException e) {

        } catch (IOException e) {

        } catch (Exception e) {

        } finally {
            System.out.println("finally");
        }
    }
}

```

## 反编译后的字节码

```no
Classfile /Users/xiexiaofei/work_sapce/idea_work/jvm_lecture/target/classes/com/xie/jvm/bytecode/MyTest3.class
  Last modified 2020年2月22日; size 1064 bytes
  MD5 checksum 042ea06f07af49e7bcef70d038cf5295
  Compiled from "MyTest3.java"
public class com.xie.jvm.bytecode.MyTest3
  minor version: 0
  major version: 52
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #14                         // com/xie/jvm/bytecode/MyTest3
  super_class: #15                        // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #15.#35        // java/lang/Object."<init>":()V
   #2 = Class              #36            // java/io/FileInputStream
   #3 = String             #37            // test.txt
   #4 = Methodref          #2.#38         // java/io/FileInputStream."<init>":(Ljava/lang/String;)V
   #5 = Class              #39            // java/net/ServerSocket
   #6 = Methodref          #5.#40         // java/net/ServerSocket."<init>":(I)V
   #7 = Methodref          #5.#41         // java/net/ServerSocket.accept:()Ljava/net/Socket;
   #8 = Fieldref           #42.#43        // java/lang/System.out:Ljava/io/PrintStream;
   #9 = String             #44            // finally
  #10 = Methodref          #45.#46        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #11 = Class              #47            // java/io/FileNotFoundException
  #12 = Class              #48            // java/io/IOException
  #13 = Class              #49            // java/lang/Exception
  #14 = Class              #50            // com/xie/jvm/bytecode/MyTest3
  #15 = Class              #51            // java/lang/Object
  #16 = Utf8               <init>
  #17 = Utf8               ()V
  #18 = Utf8               Code
  #19 = Utf8               LineNumberTable
  #20 = Utf8               LocalVariableTable
  #21 = Utf8               this
  #22 = Utf8               Lcom/xie/jvm/bytecode/MyTest3;
  #23 = Utf8               test
  #24 = Utf8               is
  #25 = Utf8               Ljava/io/InputStream;
  #26 = Utf8               serverSocket
  #27 = Utf8               Ljava/net/ServerSocket;
  #28 = Utf8               StackMapTable
  #29 = Class              #47            // java/io/FileNotFoundException
  #30 = Class              #48            // java/io/IOException
  #31 = Class              #49            // java/lang/Exception
  #32 = Class              #52            // java/lang/Throwable
  #33 = Utf8               SourceFile
  #34 = Utf8               MyTest3.java
  #35 = NameAndType        #16:#17        // "<init>":()V
  #36 = Utf8               java/io/FileInputStream
  #37 = Utf8               test.txt
  #38 = NameAndType        #16:#53        // "<init>":(Ljava/lang/String;)V
  #39 = Utf8               java/net/ServerSocket
  #40 = NameAndType        #16:#54        // "<init>":(I)V
  #41 = NameAndType        #55:#56        // accept:()Ljava/net/Socket;
  #42 = Class              #57            // java/lang/System
  #43 = NameAndType        #58:#59        // out:Ljava/io/PrintStream;
  #44 = Utf8               finally
  #45 = Class              #60            // java/io/PrintStream
  #46 = NameAndType        #61:#53        // println:(Ljava/lang/String;)V
  #47 = Utf8               java/io/FileNotFoundException
  #48 = Utf8               java/io/IOException
  #49 = Utf8               java/lang/Exception
  #50 = Utf8               com/xie/jvm/bytecode/MyTest3
  #51 = Utf8               java/lang/Object
  #52 = Utf8               java/lang/Throwable
  #53 = Utf8               (Ljava/lang/String;)V
  #54 = Utf8               (I)V
  #55 = Utf8               accept
  #56 = Utf8               ()Ljava/net/Socket;
  #57 = Utf8               java/lang/System
  #58 = Utf8               out
  #59 = Utf8               Ljava/io/PrintStream;
  #60 = Utf8               java/io/PrintStream
  #61 = Utf8               println
{
  public com.xie.jvm.bytecode.MyTest3();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 9: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lcom/xie/jvm/bytecode/MyTest3;

  public void test();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=3, locals=4, args_size=1
         0: new           #2                  // class java/io/FileInputStream
         3: dup
         4: ldc           #3                  // String test.txt
         6: invokespecial #4                  // Method java/io/FileInputStream."<init>":(Ljava/lang/String;)V
         9: astore_1
        10: new           #5                  // class java/net/ServerSocket
        13: dup
        14: sipush        9999
        17: invokespecial #6                  // Method java/net/ServerSocket."<init>":(I)V
        20: astore_2
        21: aload_2
        22: invokevirtual #7                  // Method java/net/ServerSocket.accept:()Ljava/net/Socket;
        25: pop
        26: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
        29: ldc           #9                  // String finally
        31: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        34: goto          84
        37: astore_1
        38: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
        41: ldc           #9                  // String finally
        43: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        46: goto          84
        49: astore_1
        50: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
        53: ldc           #9                  // String finally
        55: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        58: goto          84
        61: astore_1
        62: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
        65: ldc           #9                  // String finally
        67: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        70: goto          84
        73: astore_3
        74: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
        77: ldc           #9                  // String finally
        79: invokevirtual #10                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        82: aload_3
        83: athrow
        84: return
      Exception table:
         from    to  target type
             0    26    37   Class java/io/FileNotFoundException
             0    26    49   Class java/io/IOException
             0    26    61   Class java/lang/Exception
             0    26    73   any
      LineNumberTable:
        line 12: 0
        line 14: 10
        line 15: 21
        line 23: 26
        line 24: 34
        line 16: 37
        line 23: 38
        line 24: 46
        line 18: 49
        line 23: 50
        line 24: 58
        line 20: 61
        line 23: 62
        line 24: 70
        line 23: 73
        line 24: 82
        line 25: 84
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
           10      16     1    is   Ljava/io/InputStream;
           21       5     2 serverSocket   Ljava/net/ServerSocket;
            0      85     0  this   Lcom/xie/jvm/bytecode/MyTest3;
      StackMapTable: number_of_entries = 5
        frame_type = 101 /* same_locals_1_stack_item */
          stack = [ class java/io/FileNotFoundException ]
        frame_type = 75 /* same_locals_1_stack_item */
          stack = [ class java/io/IOException ]
        frame_type = 75 /* same_locals_1_stack_item */
          stack = [ class java/lang/Exception ]
        frame_type = 75 /* same_locals_1_stack_item */
          stack = [ class java/lang/Throwable ]
        frame_type = 10 /* same */
}
SourceFile: "MyTest3.java"
```

## 分析

查看字节码中test方法的code属性中的`stack=3, locals=4, args_size=1`，发现`args_size=1`，而源代码的test方法中并没有传入参数，这就是编译器在编译后自动将this传入实例方法。  

查看字节码中test方法，发现几处都是`goto 84`的指令码，而84对应的字节码就是`return`，即`84: return`，并且在goto 之前都执行了打印的指令，即将finally 的字节码都在goto之前执行了。具体执行次数可根据字节码中异常表`Exception table`来比对。

## 总结

1. 对于Java类中的每一个实例方法（非静态方法），其在编译后所生成的字节码中，方法参数的数量总是会比源代码中方法参数的数量多一个(this),它位于方法的第一个参数位置处；这样，我们就可以在java的实例方法中使用this来访问当前对象的属性以及其他方法。这个操作是在编译器完成的，即由javac编译器在编译的时候将对this的访问转化为对一个普通实例方法参数的访问；接下来在运行期间，由JVM在调用实例方法时，自动向实例方法传入该this参数。所以，在实例方法的局部变量表中，至少会有一个指向当前对象的局部变量。
2. Java字节码对于异常的处理方式：
    - 统一采用异常表的方式对异常进行处理。
    - 在JDK1.4.2之前的版本中，并不是使用异常表的方式来对异常进行处理的，而是采用特定的指令方式。
    - 当异常处理存在finally语句块时，现在化的JVM采用的处理方式是将finally语句块拼接到每一个catch块后面，换句话说程序中存在多少个cathc块，就会在每一个catch块后面重复多少个finally语句块的字节码。
  
# 通过字节码分析Java方法的静态分配与动态分配机制

## 栈帧（stack frame）

栈帧是一种用于帮助虚拟机执行方法调用与方法执行的数据结构。  
栈帧本身是一种数据结构，封装了方法的局部变量表，动态链接信息，方法的返回地址以及操作数栈等信息。

## 符号引用与直接引用

符号引用以一组符号来描述所引用的目标。符号引用可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可，符号引用和虚拟机的布局无关  
直接引用和虚拟机的布局是相关的，不同的虚拟机对于相同的符号引用所翻译出来的直接引用一般是不同的。如果有了直接引用，那么直接引用的目标一定被加载到了内存中。  
直接引用可以是：

1. 直接指向目标的指针。（个人理解为：指向对象，类变量和类方法的指针）
2. 相对偏移量。      （指向实例的变量，方法的指针）
3. 一个间接定位到对象的句柄。  
有些符号引用在类加载阶段或者是第一次使用时就会转换为直接引用，这种转换叫做静态解析；
另外一些符号引用则是在每次运行期转换为直接引用，这中转换叫做动态链接，这体现为Java多态性。

## 静态分派

静态解析的四种情形

1. 静态方法
2. 父类方法
3. 构造方法
4. 私有方法（无法被重写）

以上4类方法称作非虚方法，它们实在类加载阶段就可以将符号引用转换为直接引用的。

## 动态分派

方法的动态分派涉及到一个重要概念：方法接受者。  
invokevirtual字节码指令的动态查找流程：

1. 首先到操作数的栈顶去寻找栈顶的元素所指向的对象的实际类型。
2. 如果寻找到的类型与常量池中描述符和名称都相同的方法，并且也具备相应的访问权限，那么就直接返回目标方法的直接引用，整个过程就结束。
3. 如果找不到，就按继承的层次关系从子类往父类依次对它的各个父类重复这个查找的流程，如果找到就去执行。如果一直都找不到就抛异常。

## 关于方法调用的字节码解释

1. invokeinterface:调用接口中的方法，实际上是在运行期决定的，决定到底调用该接口的哪个对象的特定方法
2. invokestatic:调用静态方法
3. invokespecial:调用自己的私有方法、构造方法`(<init>)`以及父类的方法
4. invokevirtual:调用虚方法，运行期动态查找的过程
5. invokedynamic:动态调用方法。

## 静态分派案例

```java
public class MyTest5 {

    //方法重载，是一种静态行为，编译器就可以完全确定

    public void test(Grandpa grandpa) {
        System.out.println("grandpa");
    }

    public void test(Father father) {
        System.out.println("father");
    }

    public void test(Son son) {
        System.out.println("son");
    }

    public static void main(String[] args) {
        Grandpa g1 = new Father();
        Grandpa g2 = new Son();

        MyTest5 myTest5 = new MyTest5();

        myTest5.test(g1);
        myTest5.test(g2);

        /**
         * grandpa
         * grandpa
         */
    }
}

class Grandpa {

}

class Father extends Grandpa {

}

class Son extends Father {

}

```

其中main方法对应的字节码如下：

```no
    0: new           #7                  // class com/xie/jvm/bytecode/Father
    3: dup
    4: invokespecial #8                  // Method com/xie/jvm/bytecode/Father."<init>":()V
    7: astore_1
    8: new           #9                  // class com/xie/jvm/bytecode/Son
    11: dup
    12: invokespecial #10                 // Method com/xie/jvm/bytecode/Son."<init>":()V
    15: astore_2
    16: new           #11                 // class com/xie/jvm/bytecode/MyTest5
    19: dup
    20: invokespecial #12                 // Method "<init>":()V
    23: astore_3
    24: aload_3
    25: aload_1
    26: invokevirtual #13                 // Method test:(Lcom/xie/jvm/bytecode/Grandpa;)V
    29: aload_3
    30: aload_2
    31: invokevirtual #13                 // Method test:(Lcom/xie/jvm/bytecode/Grandpa;)V
    34: return
```

在`Grandpa g1 = new Father();`中，g1的静态类型是Grandpa，而g1的实际类型（真正指向的类型）是Father。对于JVM来说方法的重载是一种静态行为，只会根据g1的静态类型去判断。

## 动态分派分析

```java
public class MyTest6 {
    public static void main(String[] args) {
        Fruit apple = new Apple();
        Fruit orange = new Orange();

        apple.test();
        orange.test();

        apple = new Orange();
        apple.test();

        /**
         * apple
         * orange
         * orange
         */
    }
}

class Fruit {
    public void test() {
        System.out.println("fruit");
    }
}

class Apple extends Fruit {
    @Override
    public void test() {
        System.out.println("apple");
    }
}

class Orange extends Fruit {
    @Override
    public void test() {
        System.out.println("orange");
    }
}
```

其中main方法对应的字节码如下：

```no
 0: new           #2                  // class com/xie/jvm/bytecode/Apple
 3: dup
 4: invokespecial #3                  // Method com/xie/jvm/bytecode/Apple."<init>":()V
 7: astore_1
 8: new           #4                  // class com/xie/jvm/bytecode/Orange
11: dup
12: invokespecial #5                  // Method com/xie/jvm/bytecode/Orange."<init>":()V
15: astore_2
16: aload_1
17: invokevirtual #6                  // Method com/xie/jvm/bytecode/Fruit.test:()V
20: aload_2
21: invokevirtual #6                  // Method com/xie/jvm/bytecode/Fruit.test:()V
24: new           #4                  // class com/xie/jvm/bytecode/Orange
27: dup
28: invokespecial #5                  // Method com/xie/jvm/bytecode/Orange."<init>":()V
31: astore_1
32: aload_1
33: invokevirtual #6                  // Method com/xie/jvm/bytecode/Fruit.test:()V
36: return
```

可以看到在调用方法指令`invokevirtual`的参数都是Friut，对于JVM来说，方法的重写在转换成直接引用时是在运行期决定的，编译时是无法确定的。

比较重载(overload)与方法重写(overwrite)，我们可以得到这个结论：方法重载是静态的，编译器行为；方法重写是动态的，是运行期行为。

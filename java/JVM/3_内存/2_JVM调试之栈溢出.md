# JVM调试之栈溢出

- ## 代码案例
  
    ```java
    public class MyTest2 {
        private int length;

        public int getLength() {
            return length;
        }

        public void test() throws InterruptedException {
            this.length++;
            Thread.sleep(300);
            test();
        }

        public static void main(String[] args) {

            MyTest2 myTest2 = new MyTest2();

            myTest2.test();

        }
    }
    ```

- ## 涉及的JVM参数
  
    `-Xss160k`:设置每个线程的堆栈大小,JDK1.8版本的`Hotspot`虚拟机最小设置160k

- ## 运行结果
  
    ```no
    Exception in thread "main" java.lang.StackOverflowError
    at com.xie.jvm.memory.MyTest2.test(MyTest2.java:14)
    at com.xie.jvm.memory.MyTest2.test(MyTest2.java:15)
    ```

- ## 捕获异常并打印运行次数

    ```no
    try{
        myTest2.test();
    }catch (Throwable ex){
        System.out.println(myTest2.getLength());
    }
    ```

    运行772次。

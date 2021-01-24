## JVM

1.类加载器

- 负责加载class文件，class文件在文件开头有特定的文件标识，将class文件字节码内容加载到内存中，并将这些内容转换成方法区中的运行时数据结构并且ClassLoader只负责class文件的加载，置于他是否可以运行，则由ExecutionEngine决定

- 虚拟机自带加载器
  - Bootstrap

    - C++
    - 启动类加载器

    - $JAVAHME/jre/lib/rt.jar

  - ExtensionClassLoader

    - Java
    - 扩展类加载器
    - $JAVAHOME/jre/lib/ext/*.jar

  - SystemClassLoader

    - 应用程序类加载器（AppClassLoader）
    - 也叫系统类加载器，加载当前应用的classpath的所有类
    - AppClassLoader
    - $CLASSPATH

- 用户自定义加载器
  - Java.lang.ClassLoader的子类，用户可以定制类的加载方式

- 双亲委派
  - 当一个类收到了类加载请求，他首先不会尝试自己去加载这个类，而是把这个请求委派给父类去完成，每一个层次的类加载器都是如此，因此所有的加载请求都应该传送到启动类加载器中，只有当父类加载器反馈自己无法完成这个请求的时候（在他的加载器路径下没有找到所需加载的Class），子类加载器才会尝试自己去加载
  - 采用双亲委派的一个好处是比如加载位于rt.jar包中的类java.lang.Object，不管是哪个加载器加载这个类，最终都是委托给顶层的启动类加载器进行加载，这样就保证了使用不同的类加载器最终得到的都是一样的Object对象。
- 沙箱安全

2.本地方法

由于当年c语言盛行，为了生存必须调用c/c++程序，具体做法是Native Method Stack中登记native方法，在Execution Engine执行时加载native libraries。

3.程序计数器（pc寄存器）

线程私有的，就是一个指针指向方法区中的方法字节码，它是当前线程所执行的字节码的行号指示器，字节码解释器通过改变这个计数器的值来选取下一条需要执行的字节码指令。如果是一个Native方法，那这个计数器是空的。不会发生内存溢出错误（OutOfMemory=OOM ），占用内存非常小，几乎可以忽略，几乎不需要垃圾回收

cpu上的一小块内存空间

指示程序运行到哪里

4.方法区

- 存储类的结构信息
- 不同虚拟机里实现是不一样的，最典型的是永久代（PermGen space）和元空间（Me他space）


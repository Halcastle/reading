字节码，类加载器，虚拟机

文件系统-->classs文件(字节码)加载（类加载器）到jvm内存（虚拟机）中

# 字节码

>  Java bytecode由单字节（byte）的指令组成，理论上最多支持256个操作码（opcode）。实际上java只使用了200左右的操作码，还有一些操作码则保留给调试操作。

 根据指令的性质，主要分为四个大类：

1. 栈操作指令，包括与局部变量交互的指令；
2. 程序流程控制指令；
3. 对象操作指令，包括方法调用指令；
4. 算数运算以及类型转换指令

 //TODO HelloByteCode.java编译字节码

##  字节码的运行时结构

- JVM是一台基于栈的计算机器
- 每个线程都有一个独属于自己的线程栈（JVM Stack），用于存储线程帧（Frame）
- 每一次方法调用，JVM都会自动创建一个栈帧
- 栈帧由操作数栈，局部变量数组以及一个Class引用组成
- Class引用指向当前方法在运行时常量池中对应的Class

 store：将栈上的数据写回本地变量表

 load：将本地变量表的数据加载到栈上

## 从助记符到二进制

 将助记符转换为二进制，可以在class文件中找到

## 数值处理与本地变量表



## 算数操作与类型转换

- 对字节码操作来说，int为最小量
- 字节码操作仅有int、long、float、double类型，其他数据类型均用int表示
- int-32位，long-64位，目前主流机器为64位，所以以上四种类型操作均为原子性

## 一个完整的循环控制



## 方法调用的指令

- invokestatic:用于调用某个类的静态方法，这是方法调用指令中最快的一个
- invokespecial:用来调用构造函数，但也可以用于调用同一个类中的private方法，以及可见的超类方法
- invokevirtual:如果是具体类型的目标对象，invokevirtual用于调用公共、受保护和package级的私有方法
- invokeinterface:当通过接口引用来调用方法时，将会编译为invokeinterface指令
- invokedunamic：JDK7新增的指令，是实现“动态类型语言”支持而进行的升级改造，同时也是JDK8以后支持lambda表达式的实现基础

# 类加载器

## 类的生命周期

1. 加载（Loading）：找Class文件
2. 验证（Verification）：验证格式、依赖
3. 准备（Preparation）：静态字段、方法表
4. 解析（Resolution）：符号解析为引用
5. 初始化（Initialization）：构造器、静态变量赋值、静态代码块
6. 使用（Using）
7. 卸载（Unloading）

> 其中，验证、准备、解析可以统称为链接，故类的生命周期分为：加载、链接、初始化、使用、卸载。

## 类的加载时机

1. 当虚拟机启动时，初始化用户指定的主类，就是启动执行的main方法所在的类
2. 当遇到用以新建目标类实例的new指令时，初始化new指令的目标类，就是new一个类的时候要初始化
3. 当遇到调用静态方法的指令时，初始化该静态方法所在的类
4. 当遇到访问静态字段的指令时，初始化该静态字段所在的类
5. 子类的初始化会触发父类的初始化
6. 如果一个接口定义了default方法，那么直接实现或者间接实现该接口的类的初始化，会触发该接口的初始化
7. 使用反射API对某个类进行反射调用时，初始化这个类，其实跟前面一样，反射调用要么是已经有实例了，要么是静态方法，都需要初始化
8. 当初次调用MethodHandle实例时，初始化该MethodHandle指向的方法所在的类

## 不会初始化（可能会加载）

1. 通过子类引用父类的静态字段，只会出发父类的初始化，而不会出发子类的初始化
2. 定义对象数组，不会触发该类的初始化
3. 常量在编译期间会存入调用类的常量池中，本质上并没有直接引用定义常量的类，不会触发定义常量所在的类
4. 通过类名获取Class对象，不会触发类的初始化，Hello.class不会让Hello类初始化
5. 通过Class.forName加载指定类时，如果指定参数initialize为false时，也不会触发类初始化，其实这个参数是告诉虚拟机，是否要对类进行初始化（Class.forName("jvm.Hello")默认会加载Hello类）
6. 通过ClassLoader默认的loadClass方法，也不会触发初始化动作（加载了，但是不初始化）

## 三类加载器

> 参考链接：https://zhuanlan.zhihu.com/p/73359363

1. 启动类加载器BootstrapClassLoader

   ​	加载JVM系统类（如：rt.jar【java.lang.*,java.util.*】等）

   ​	不可见，其是在jdk底层实现的

2. 扩展类加载器ExtClassLoader

   ​	加载jdk扩展目录中的jar

3. 应用类加载器AppClassLoader

   ​	加载自己写的类，jar等

   

   ### 加载器的特点

   1. 双亲委托

      如图：

      ![image-20210128173511462](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210128173511462.png)

      箭头所指为寻找方向，应用类加载器会向上找，以此类推

   2. 负责依赖

      若类加载时有依赖则加载对应依赖

   3. 缓存加载

      在加载某个类完成后，会进行缓存，也就是说对于某个类来说，仅加载一次

   > 自定义的两个应用类加载器A、B，同时加载某个类型，在程序中使用B.class转换为A.class会发生转换失败。
   >
   > 基于此可进行模板化，版本化，因为两个应用类加载器是不相同的

   类加载器的子父类关系，如图（启动类加载器不可见，因为其是在jdk内部实现的）：

   ![image-20210128180615305](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210128180615305.png)

   ##### 打印类加载器的代码

   ```java
   import java.lang.reflect.Field;
   import java.net.URL;
   import java.net.URLClassLoader;
   import java.util.ArrayList;
   
   public class JvmClassLoaderPrintPath {
   
       public static void main(String[] args){
           //启动类加载器,加载jre和jre\\lib下的核心库
           URL[] urls = sun.misc.Launcher.getBootstrapClassPath().getURLs();
           System.out.println("BootStrapClassLoader");
           for(URL url : urls){
               System.out.println(" ==>"+url.toExternalForm());
           }
   
           //扩展类加载器,加载jre\\lib\\ext目录下的扩展包
           printClassLoader("ExtClassLoader",JvmClassLoaderPrintPath.class.getClassLoader().getParent());
   
           //应用类加载器
           printClassLoader("AppClassLoader",JvmClassLoaderPrintPath.class.getClassLoader());
       }
   
       public static void printClassLoader(String name,ClassLoader cl){
           if(cl != null){
               System.out.println(name + "ClassLoader -> "+cl.toString());
               printURLForClassLoader(cl);
           }else{
               System.out.println(name + "ClassLoader -> null");
           }
       }
   
       public static void printURLForClassLoader(ClassLoader cl){
           Object ucp = insightField(cl,"ucp");
           Object path = insightField(ucp,"path");
           ArrayList ps = (ArrayList) path;
           for(Object p : ps){
               System.out.println(" ==> "+p.toString());
           }
       }
   
       private static Object insightField(Object obj,String fName){
           try{
               Field f = null ;
               if(obj instanceof URLClassLoader){
                   f = URLClassLoader.class.getDeclaredField(fName);
               }else{
                   f = obj.getClass().getDeclaredField(fName);
               }
               f.setAccessible(true);
               return f.get(obj);
           } catch (Exception e){
               e.printStackTrace();
               return null;
           }
       }
   }
   
   ```

   ##### 自定义ClassLoader

   //TODO

   ##### 添加引用类的几种方式

   1. 放到JDK下的lib/ext下，或者-Djava.ext.dirs
   2. java -cp/classpath或者class文件放到当前路径
   3. 自定义ClassLoader加载
   4. 拿到当前执行类的ClassLoader，反射调用addUrl方法添加Jar或路径（对JDK9无效）



# JVM的内存模型



## JVM内存结构

- 每个线程都只能访问自己的线程栈

- 每个线程都不能访问（看不见）其他线程的局部变量

- 所有原生类型的局部变量都存储在线程栈中，因此对其他线程是不可见的

- 线程可以将一个原生变量值的副本传给另一个线程，但不能共享原生局部变量本身

- 堆内存中包含了Java代码中创建的所有对象，不管是哪个线程创建的，其中也涵盖了包装类型（例如Byte、Integer、Long等）

- 不管是创建一个对象并将其赋值给局部变量，还是赋值给另一个对象的成员变量，创建的对象都会被保存到堆内存中。

  如图：![image-20210202103643452](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210202103643452.png)

- 如果是原生数据类型的局部变量，那么它的内容就全部保留在线程栈上

- 如果是对象引用，则栈中的局部变量槽位中保存着对象的引用地址，而实际的对象内容保存在堆中

- 对象的成员变量与对象本身一起存储在堆上，不管成员变量的类型是原生数值，还是对象引用

- 类的静态变量则和类定义一样都保存在堆中

  如图：![image-20210202111830352](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210202111830352.png)

### 总结一下

- 方法中使用原生数据类型和对象引用地址在栈上存储
- 对象、对象成员与类定义、静态变量在堆上
- 堆内存又称为“共享堆”，堆中的所有对象，可以被所有线程访问，只要他们能拿到对象的引用地址
- 如果一个线程可以访问某个对象时，也就可以访问该对象的成员变量
- 如果两个线程同时调用某个对象的同一方法，则它们都可以访问到这个对象的成员变量，但每个线程的局部变量副本是独立的。

## JVM内存整体结构

- 每启动一个线程，JVM就会在栈空间分配对应的一个线程栈，比如1MB的空间（-Xss1m）

- 线程栈也叫做Java方法栈。如果使用了JNI方法，则会分配一个单独的本地方法栈（Native Stack）

- 线程执行过程中，一般会有多个方法组成调用栈（Stack Trace），比如A调用B，B调用C……每执行到一个方法都会创建对应的栈帧

  如图：![JVM内存结构2](D:\files\99-otherFiles\01-进阶班级\Java进阶训练营0期预习资料\week_01\99-课程总结\JVM内存结构2.png)

  > -Xms所配置的最大堆内存，仅指JVM中的堆内存，而JVM还包含栈、非堆、JVM自身的内存，所以一般情况下建议堆内存配置为机器本身内存的70%

## JVM栈内存结构

- 栈帧是一个逻辑上的概念，具体的大小在一个方法编写完成后基本上就能确定。

  > 比如返回值，需要有一个空间存放，每个局部变量都需要对应的地址空间，此外还有给指令使用的操作数栈，以及class指针（标识这个栈帧对应的是哪个类的方法，指向非堆里面的Class对象）

  如图：![JVM栈内存结构](D:\files\99-otherFiles\01-进阶班级\Java进阶训练营0期预习资料\week_01\99-课程总结\JVM栈内存结构.png)

## JVM堆内存结构

- 堆内存是所有线程共用的内存空间，JVM将Heap内存分为年轻代（young generation）和老年代(Old generation也叫Tenured)两部分

- 年轻代还划分为3个内存池，新生代（Eden space）和存活区（Survivor space），在大部分GC算法中有2个存活区（S0和S1），在我们可以观察到的任何时刻，S0和S1总有一个是空的，但一般比较小，也不浪费多少空间

  > 之所以为空是因为在垃圾回收时，出现多个无用对象，则直接将有用对象从S0/S1转移到S1/S0

- Non_heap本质上还是Heap，只是一般不归GC管理，里面划分为3个内存池

  - MetaSpace，以前叫持久代（永久代，Permanent generation），Java8换了个名字叫Metaspace
  - CCS，Compressed Class Space，存放class信息的，和Metaspace有交叉
  - Code Cache，存放JIT编译器编译后的本地机器代码

  > 对象刚生成：Eden-Space-->s0/s1-->Old_gen
  >
  > 特别地：若对象刚刚生成既很大，则直接出现在老年代

  如图：![JVM堆内存结构](D:\files\99-otherFiles\01-进阶班级\Java进阶训练营0期预习资料\week_01\99-课程总结\JVM堆内存结构.png)

## CPU和内存行为

- CPU为乱序执行

- volatile关键字

- 原子性操作

- 内存屏障

  如图：![image-20210208230005339](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210208230005339.png)

## 小结：

> 什么是JMM？

JMM规范对应的是“【JSR-133，Java Memory Model and Tread Specification】”《Java语言规范》的[$17.4. Memory Model章节]

JMM规范明确定义了不同的线程之间，通过哪些方式，在什么时候可以看见线程保存到共享变量中的值；以及在必要时，如何对共享变量的访问进行同步。这样的好处是屏蔽各种硬件平台和操作系统之间的内存访问差异，实现了Java并发程序真正的跨平台。

所有的对象（包括内部的实例成员变量），static变量，以及数组，都必须存放到堆内存中。

局部变量，方法的形参、入参，异常处理语句的入参不允许在线程之间共享，所以不受内存模型的影响。

多个线程同时对一个变量访问时【读取/写入】，这时候只要有某个线程执行的是写操作，那么这种现象就称之为“冲突”。

可以被其他线程影响或感知的操作，称之为线程间的交互行为，可分为：读取、写入、同步操作、外部操作等等。其中同步操作包括：对volatile变量的读写，对管程（monitor）的锁定与解锁，现成的起始操作与结尾操作，线程启动和结束等等。外部操作则是指对线程执行环境之外的操作，比如停止其他线程等等。

JMM规范的是线程间的交互操作，而不是线程内部对局部变量进行的操作。


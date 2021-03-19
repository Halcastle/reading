# JDK内置命令行工具

1. java:Java应用的启动程序
2. javac：JDK内置的编译工具
3. javap：反编译class文件的工具
4. javadoc：根据Java代码和标准注释，自动生成相关
5. javah：JNI开发时，根据java代码生成需要的.h文件
6. extcheck:检查某个jar文件和运行时扩展jar有没有版本冲突（很少使用）
7. jdb：Java Debugger：可以调试本地和远端程序；属于JPDA中的一个demo实现，供其他调试器参考，开发时很少使用
8. jdeps：探测class或jar包需要的依赖
9. jar：打包工具，可以将文件和目录打包成为.jar文件；.jar文件本质上就是zip文件，只是后缀不同，使用时按顺序对应好选项和参数即可
10. keytool：安全证书和密钥的管理工具（支持生成、导入、导出等操作）
11. jarsigner：JAR文件签名和验证工具
12. policytool：实际上这是一款图形界面工具，管理本机的java安全策略
13. jps/jinfo：查看java进程
14. jstat：查看JVM内部gc相关信息
15. jmap：查看heap或类占用空间统计
16. jstack：查看线程信息
17. jcmd：执行JVM相关分析（整合命令）
18. jrunscript/jjs：执行js命令

## jps/jinfo 查看java进程

![image-20210318151630637](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318151630637.png)

## jstat 查看JVM内部gc相关信息

jstat -gcutil pid 1000 1000 :查看指定pidgc信息，1000ms刷新一次，打印1000次(百分比显示)

![image-20210318151846942](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318151846942.png)

![image-20210318153620833](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318153620833.png)

jstat -gc pid 1000 1000 :查看指定pidgc信息，1000ms刷新一次，打印1000次（单位kb）

![image-20210318153446177](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318153446177.png)

## jmap 查看heap或者类占用空间统计

> 常用选项：
>
> -heap 打印堆内存（/内存池）的配置和使用信息
>
> -histo 看哪些类占用的空间最多，直方图
>
> -dump:format=b,file=xxx.hprof  Dump堆内存

![image-20210318161443432](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318161443432.png)

![image-20210318161737530](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318161737530.png)

## jstack 查看线程信息

> -F 强制执行thread dump，可在Java进程卡死（hung住）时使用，此选项可能需要系统权限
>
> -m 混合模式（mixed mode），将Java帧和native帧一起输出，此选项可能需要系统权限。
>
> -l 长列表模式，将线程相关的locks信息一起输出，比如持有的锁，等待的锁

![image-20210318162503228](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318162503228.png)

## jcmd 综合命令

![image-20210318165201039](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318165201039.png)

## jrunscript/jjs 执行脚本、curl、js文件等

![image-20210318165405865](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318165405865.png)



# JDK内置图形化工具

## jconsole

> 在命令行输入jconsole即可打开

共有 6 个面板
第一个为概览，四项指标具体为：
**堆内存使用量：**此处展示的就是前面 Java 内存模型课程中提到的堆内存使用情况，从图上可以看到，堆内存使用了 94MB 左右，并且一 直在增长。
**线程：**展示了 JVM 中活动线程的数量，当前时刻共有 17 个活动线程。
**类：**JVM 一共加载了 5563 个类，没有卸载类。
**CPU占用率：**目前 CPU 使用率为 0.2%，这个数值非常低，且最高的 时候也不到 3%，初步判断系统当前并没有什么负载和压力。

有如下几个时间维度可供选择：
1分钟、5分钟、10分钟、30分钟、1小时、2小时、3小时、6小时、12小时、1天、7天、1个月、3个月、6个月、1年、全部，一共是16档。
当我们想关注最近1小时或者1分钟的数据，就可以选择对应的档。旁边的3个标签页(内存、线程、类)，也都支持选择时间范围。

![image-20210318165925239](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318165925239.png)

内存图表包括：
堆内存使用量，主要包括老年代（内存池 “PS Old Gen”）、新生代（“PS Eden Space”）、存活区（“PS Survivor Space”）；
非堆内存使用量，主要包括内存池“Metaspace”、 “Code Cache”、“Compressed Class Space”等；
可以分别选择对应的 6 个内存池。
通过内存面板，我们可以看到各个区域的内存使用和变化情况，并且可以：
1.手动执行 gc，见图上的标号1，点击按钮即可执行JDK 中的 System.gc()
2.通过图中右下角标号 2 的界面，可以看到各个内存 池的百分比使用率，以及堆/非堆空间的汇总使用 情况
3.从左下角标号 3 的界面，可以看到 JVM 使用的垃 圾收集器，以及执行垃圾收集的次数，以及相应的时间消耗。

![image-20210318165943705](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318165943705.png)



线程面板展示了线程数变化信息，以及监测到的线程列表。
我们可以常根据名称直接查看线程的状态（运行还是等待中）和调用栈（正在执行什么操作）。
特别地，我们还可以直接点击“检测死锁”按钮来检测死锁，如果没有死锁则会提示“未检测到死锁”。

![image-20210318170040569](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170040569.png)



类监控面板，可以直接看到 JVM 加载和卸载的类数量汇总信息，以及随着时间的动态变化。

![image-20210318170127360](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170127360.png)



VM 概要的数据有五个部分：
第一部分是虚拟机的信息；
第二部分是线程数量，以及类加载的汇总信息；
第三部分是堆内存和 GC 统计。
第四部分是操作系统和宿主机的设备信息，比如 CPU 数量、物理内存、虚拟内存等等。
第五部分是 JVM 启动参数和几个关键路径，这些信息其实跟 jinfo 命令看到的差不多。

![image-20210318170213810](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170213810.png)

## jvisualvm

![image-20210318170253192](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170253192.png)

![image-20210318170422804](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170422804.png)

![image-20210318170437841](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170437841.png)

![image-20210318170448969](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170448969.png)

## visualGC(idea插件)

![image-20210318170528658](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170528658.png)

## jmc

![image-20210318170653470](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170653470.png)

![image-20210318170721391](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170721391.png)

![image-20210318170730456](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170730456.png)

![image-20210318170759099](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170759099.png)

![image-20210318170808145](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170808145.png)

![image-20210318170816719](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170816719.png)

![image-20210318170938710](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318170938710.png)

# GC的背景与一般原理

> 为什么会有gc
>
> 本质上是内存资源的有限性
>
> 因此需要大家共享使用，手工申请，手动释放
>
> ![image-20210318171647291](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210318171647291.png)

## 标记清除算法（Mark and Sweep）

- Marking(标记)：遍历所有的可达对象，并在本地内存（native）中分门别类记下
- Sweeping(清除)：这一步保证了，不可达对象所占用的内存，在之后进行内存分配时可以重用

并行GC和CMS（并发GC，GC线程和业务线程同时发起）的基本原理

优势：可以处理循环依赖，只扫描部分对象

当然，在做了标记-清除后，还要进行**压缩**空间，使用STW（stop the world），让全世界停下来

![image-20210319171811265](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319171811265.png)

分带假设：大部分新生对象很快无用；存活较长时间的对象，可能存活更长时间

![image-20210319172014270](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319172014270.png)

内存池划分：不同类型对象不同区域，不同策略处理

![image-20210319172117275](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319172117275.png)

对象分配在新生代的Eden区，标记阶段Eden区存活的对象就会复制到存活区。（年轻代之间的对象复制采用‘复制’）

由如下参数控制提升阈值：

-XX:+MaxTenuringThreshold=15（对象经历多少代复制进入到老年代，默认15）

老年代默认都是存活对象，采用移动方式:

1. 标记所有通过GC roots可达的对象；
2. 删除所有不可达的对象；
3. 整理老年代空间中的内容，方法是将所有的存活对象复制，从老年代空间开始的地方依次存放。

持久代/元数据区

1.8之前：-XX:MaxPermSize=256m

1.8之后：-XX:MaxMetaspaceSize=256m

![image-20210319173342962](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319173342962.png)

## 可以作为GC Roots的对象

1. 当前正在执行的方法里的局部变量和输入参数
2. 活动线程（Active threads）
3. 所有类的静态字段（static field）
4. JNI引用

此阶段暂停时间，与堆内存大小，对象的总数没有直接关系，而是由存活对象（alive objects）的数量来决定，所以增加堆内存的大小并不会直接影响标记阶段占用的时间

![image-20210319173734408](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319173734408.png)

![image-20210319173917842](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210319173917842.png)



# 串行GC/并行GC（Serial GC/Parallel GC）

## 串行GC（Serial GC）/ParNewGC

-XX:UseSerialGC 配置串行GC

串行GC对年轻代使用mark-copy（标记-复制）算法，对老年代使用mark-sweep-compact（标记-清除-整理）算法

两者都是单线程的垃圾收集器，不能进行并行处理，所以都会触发全线暂停（STW），停止所有的应用线程

因此这种GC算法不能充分利用多核CPU。不管有多少CPU内核，JMVM在垃圾收集时都只能使用单个核心。

CPU利用率高，暂停时间长，简单粗暴，就像老式的电脑，动不动就卡死。

该选项只适合几百MB堆内存的JVM，而且时单核CPU时比较有用。

-XX：+UseParNewGC改进版本的Serial GC,可以配合CMS使用

https://blog.csdn.net/w903328615/article/details/108960758



# CMS GC/G1 GC

# ZGC/Shenandoah GC

# 总结


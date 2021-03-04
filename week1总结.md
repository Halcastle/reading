字节码，类加载器，虚拟机

文件系统-->classs文件(字节码)加载（类加载器）到jvm内存（虚拟机）中

# 字节码

>  Java bytecode由单字节（byte）的指令组成，理论上最多支持256个操作码（opcode）。实际上java只使用了200左右的操作码，还有一些操作码则保留给调试操作。

 根据指令的性质，主要分为四个大类：

1. 栈操作指令，包括与局部变量交互的指令；
2. 程序流程控制指令；
3. 对象操作指令，包括方法调用指令；
4. 算数运算以及类型转换指令

## 查看字节码的方法

*使用`javap`命令查看*

> javap命令介绍：
>
> javap是JDK自带的反汇编器，可以查看java编译器为我们生成的字节码。
> 语法：
> 　　javap [ 命令选项 ] class. . .
> 　　javap 命令用于解析类文件。其输出取决于所用的选项。若没有使用选项，javap 将输出传递给它的类的 public 域及方法。javap 将其输出到标准输出设备上。
> 命令选项
> 　　-help 输出 javap 的帮助信息。
> 　　-l 输出行及局部变量表。
> 　　-b 确保与 JDK 1.1 javap 的向后兼容性。
> 　　-public 只显示 public 类及成员。
> 　　-protected 只显示 protected 和 public 类及成员。
> 　　-package 只显示包、protected 和 public 类及成员。这是缺省设置。
> 　　-private 显示所有类和成员。
> 　　-J[flag] 直接将 flag 传给运行时系统。
> 　　-s 输出内部类型签名。
> 　　-c 输出类中各方法的未解析的代码，即构成 Java 字节码的指令。
> 　　-verbose 输出堆栈大小、各方法的 locals 及 args 数,以及class文件的编译版本
> 　　-classpath[路径] 指定 javap 用来查找类的路径。如果设置了该选项，则它将覆盖缺省值或 CLASSPATH 环境变量。
>
> ​    目录用冒号分隔。
>   -bootclasspath[路径] 指定加载自举类所用的路径。缺省情况下，自举类是实现核心 Java 平台的类，位于 jrelib下面。
>
> 　 -extdirs[dirs] 覆盖搜索安装方式扩展的位置。扩展的缺省位置是 jrelibext。
>
> 如图：
>
> ![image-20210210205331205](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210210205331205.png)

 //TODO ByteCodeAnalysis.java编译字节码

```java
public class ByteCodeAnalysis {

    /*Java基本数据类型:
        六种数字类型（四个整数型，两个浮点型）
        一种字符型
        一种布尔型
        byte：
            八位有符号的，以二进制补码表示的整数
            最小值：-128（-2^7）
            最大值：127（2^7-1）
            默认值为0
            byte类型用在大型数组中节约空间，主要代替整数，因为byte变量占用的空间只有int类型的四分之一
        short：
            十六位有符号的，以二进制补码表示的整数
            最小值是：-32768（-2^15）
            最大值是：32767(2^15-1)
            short类型是int类型的1/2
        int:
            三十二位有符号的，以二进制补码表示的整数
            最小值是：-2147483648（-2^32）
            最大值是：2147483647（2^32-1）
            一般地整数型变量默认为int类型
            默认值是0
        long：
            六十四位有符号的，以二进制补码表示的整数；
            最小值是：-9223372036854775808（-2^63）
            最大值是：9223372036854775807（2^63 -1）
        float:
            三十二位，单精度，符合IEEE754标准的浮点数
            float在存储大型浮点数组的时候可以节省内存空间；
            默认值是0.0f；
            浮点数不能用来表示精确的值，如货币
        double：
            六十四位，双精度，符合IEEE754标准的浮点数
            浮点数的默认类型为double型
            double型同样不能表示精确的值，如货币
            默认值为0.0d
        boolean：
            boolean数据类型表示一位的信息
            只有两个取值：true和false
            默认值为false
        char：
            char类型为一个单一的16为Unicode字符
            最小值：\u0000(即为0)
            最大值:\uffff(即为65、535)
            char数据类型可以存储任何字符

     */



    public static void main(String[] args){
         final byte b_a = 100;
         final byte b_b = -1;

         final short s_a = 10;
         final short s_b = -5;

         int i_a = 11;
         int i_b = -12;

         long l_a = 200;
         long l_b = -200;

         float f_a = -10.5f;
         float f_b = 10.1f;

         double d_a = 17.12;
         double d_b = -11.22;

         boolean is_a = false;
         boolean is_b = true;

         final char c_a = 'A';
         final char c_b = 'c';
        //byte四则运算
        byte b_result_addition = b_a + b_b ;
        byte b_result_subtraction = b_a - b_b;
        byte b_result_multiplication = b_a * b_b;
        byte b_result_division = b_a / b_b;
        System.out.println("b_result_addition: "+b_result_addition);
        System.out.println("b_result_subtraction: "+b_result_subtraction);
        System.out.println("b_result_multiplication: "+b_result_multiplication);
        System.out.println("b_result_division: "+b_result_division);

        //short四则运算
        short s_result_addition = s_a + s_b;
        short s_result_subtraction = s_a - s_b;
        short s_result_multiplication = s_a * s_b;
        short s_result_division = s_a / s_b;
        System.out.println("s_result_addition: "+s_result_addition);
        System.out.println("s_result_subtraction: "+s_result_subtraction);
        System.out.println("s_result_multiplication: "+s_result_multiplication);
        System.out.println("s_result_division: "+s_result_division);

        //int四则运算
        int i_result_addition = i_a + i_b;
        int i_result_subtraction = i_a - i_b;
        int i_result_multiplication = i_a * i_b;
        int i_result_division = i_a / i_b;
        System.out.println("i_result_addition: "+i_result_addition);
        System.out.println("i_result_subtraction: "+i_result_subtraction);
        System.out.println("i_result_multiplication: "+i_result_multiplication);
        System.out.println("i_result_division: "+i_result_division);

        //long四则运算
        long l_result_addition= l_a + l_b;
        long l_result_subtraction = l_a - l_b;
        long l_result_multipication = l_a * l_b;
        long l_result_division = l_a / l_b;
        System.out.println("l_result_addition: "+l_result_addition);
        System.out.println("l_result_subtraction: "+l_result_subtraction);
        System.out.println("l_result_multipication: "+l_result_multipication);
        System.out.println("l_result_division: "+l_result_division);


        //float四则运算
        float f_result_addition = f_a + f_b;
        float f_result_subtraction = f_a - f_b;
        float f_result_multipication = f_a * f_b;
        float f_result_division = f_a / f_b;
        System.out.println("f_result_addition: "+f_result_addition);
        System.out.println("f_result_subtraction: "+f_result_subtraction);
        System.out.println("f_result_multipication: "+f_result_multipication);
        System.out.println("f_result_division: "+f_result_division);

        //double四则运算
        double d_result_addition = d_a + d_b;
        double d_result_subtraction = d_a - d_b;
        double d_result_multipication = d_a * d_b;
        double d_result_division = d_a / d_b;
        System.out.println("d_result_addition: "+d_result_addition);
        System.out.println("d_result_subtraction: "+d_result_subtraction);
        System.out.println("d_result_multipication: "+d_result_multipication);
        System.out.println("d_result_division: "+d_result_division);


        //char四则运算
        char c_result_addition = c_a + c_b;
        char c_resullt_subtraction = c_b - c_a;
        char c_result_multipication = c_a * c_b;
        char c_result_division = c_a / c_b;
        System.out.println("c_result_addition: "+c_result_addition);
        System.out.println("c_resullt_subtraction: "+c_resullt_subtraction);
        System.out.println("c_result_multipication: "+c_result_multipication);
        System.out.println("c_result_division: "+c_result_division);


        //if运算
        if(i_a>i_b){
            is_a = true;
            System.out.println("is_a: "+is_a);
        }

        //for运算
        for(int i = 0;i<10;i++){
            i_a += i_b;
            System.out.println("i_a: "+i_a);
        }


    }
}

```

```C
 javap -c .\ByteCodeAnalysis.class
Compiled from "ByteCodeAnalysis.java"
public class ByteCodeAnalysis {
  public ByteCodeAnalysis();
    Code:
       0: aload_0  //从局部变量0中装载引用类型值入栈
       1: invokespecial #1  /*编译时方法绑定调用方法,用来调用构造函数，但也可以用于调用同一个类中的 private 方法, 以及
可见的超类方法*/                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: bipush        100  //将常量100压入栈中，当int取值-1~5采用iconst指令，取值-128~127采用bipush指令，取值-32768~32767采用sipush指令，取值-2147483648~2147483647采用 ldc 指令。
       2: istore_1  //将栈顶int型数值存入第二个本地变量,既将100存储到第二个本地变量
       3: iconst_m1  //将常量-1压入栈中
       4: istore_2  //将栈顶int型数值存入第三个本地变量，既将-1存储到第三个本地变量
       5: bipush        10  //将常量10压入栈中
       7: istore_3  //将栈顶int型数值存入第四个本地变量，既将10存储到第四个本地变量
       8: bipush        -5  //将常量-5压入栈中
      10: istore        4  //将栈顶int型数值存入第五个本地变量，既将-5存储到第五个本地变量
      12: bipush        11 //将常量11压入栈中
      14: istore        5  //将栈顶int型数值存入第六个本地变量，既将11存储到第六个本地变量
      16: bipush        -12  //将常量-12压入栈中
      18: istore        6  //将栈顶int型数值存入第七个本地变量，既将-12存储到第七个本地变量
      20: ldc2_w        #2  //从常量池索引2的位置取出long类型的200压入栈中                  // long 200l
      23: lstore        7  //弹出栈顶元素200L存入第八个本地变量
      25: ldc2_w        #4  //从常量池索引4的位置取出long类型的-200压入栈中                  // long -200l
      28: lstore        9  //弹出栈顶元素-200L存入第十个本地变量，可能因为有符号long型，故占两个位置
      30: ldc           #6  //从常量池索引6的位置取出float类型的-10.5f压入栈中                  // float -10.5f
      32: fstore        11  //弹出栈顶元素-10.5f存入第十二个本地变量，可能因为有符号float，故占两个位置
      34: ldc           #7  //从常量池索引7的位置取出float类型的10.1f压入栈中                  // float 10.1f
      36: fstore        12  //弹出栈顶元素10.1f存入第十三个本地变量
      38: ldc2_w        #8  //从常量池索引8的位置取出double类型的17.12d压入栈中                  // double 17.12d
      41: dstore        13  //弹出栈顶元素17.12d存入第十四个本地变量
      43: ldc2_w        #10  //从常量池索引10的位置取出double类型的-11.22d压入栈中                 // double -11.22d
      46: dstore        15  //弹出栈顶元素-11.22d存入第十六个本地变量
      48: iconst_0  //常量int类型的0入栈，0-false，1-true
      49: istore        17  //将栈顶int类型的数值存入第十八个本地变量，既将0存储到第十八个本地变量
      51: iconst_1  //常量int类型的1入栈,0-false,1-true
      52: istore        18  //将栈顶int类型的数值存入第十九个本地变量，既将1存储到第十九个本地变量
      54: bipush        65  //将常量65压入栈中，因为A对应到ASCII码值为65，见附录
      56: istore        19  //将栈顶int类型的数值存入第二十个本地变量，既将65存储到第二十个本地变量
      58: bipush        99  //将常量99压入栈中，因为c对应到ASCII码值为99，见附录
      60: istore        20  //将栈顶int类型的数值存入第二十一个本地变量，既将99存储到第二十一个本地变量
      62: bipush        99  //将常量99压入栈中,因为两个byte求和为99
      64: istore        21  //将栈顶int类型的数值存入第二十二个本地变量，既将99存储到第二十二个本地变量
      66: bipush        101  //将常量101压入栈中，因为两个byte做差为101
      68: istore        22  //弹出栈顶元素101，将其存入第二十三个本地变量
      70: bipush        -100  //将常量-100压入栈中，因为两个byte相乘为-100
      72: istore        23  //弹出栈顶元素-100，将其存入第二十四个本地变量
      74: bipush        -100  //将常量-100压入栈中，因为两个byte相除为-100
      76: istore        24  //弹出栈顶元素-100，将其存入第二十五个本地变量
      78: getstatic     #12  //获取静态字段，因为程序中有System.out.println();                 // Field java/lang/System.out:Ljava/io/PrintStream;
      81: new           #13  //为新对象分配内存                // class java/lang/StringBuilder
      84: dup  //复制栈顶的值
      85: invokespecial #14  /*用来调用构造函数，但也可以用于调用同一个类中的 private 方法, 以及
可见的超类方法*/                 // Method java/lang/StringBuilder."<init>":()V
      88: ldc           #15  //从常量池15索引取b_result_addition值并压入栈中                 // String b_result_addition:
      90: invokevirtual #16  /*如果是具体类型的目标对象，invokevirtual 用于调用公共，受保护和
package 级的私有方法*/                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      93: iload         21  //将局部变量表第二十一个变量压入栈中
      95: invokevirtual #17  /*用来调用构造函数，但也可以用于调用同一个类中的 private 方法, 以及
可见的超类方法*/                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      98: invokevirtual #18  /*用来调用构造函数，但也可以用于调用同一个类中的 private 方法, 以及
可见的超类方法*/                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     101: invokevirtual #19  /*用来调用构造函数，但也可以用于调用同一个类中的 private 方法, 以及
可见的超类方法*/                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     104: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     107: new           #13                 // class java/lang/StringBuilder
     110: dup
     111: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     114: ldc           #20                 // String b_result_subtraction:
     116: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     119: iload         22
     121: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     124: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     127: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     130: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     133: new           #13                 // class java/lang/StringBuilder
     136: dup
     137: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     140: ldc           #21                 // String b_result_multiplication:
     142: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     145: iload         23
     147: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     150: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     153: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     156: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     159: new           #13                 // class java/lang/StringBuilder
     162: dup
     163: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     166: ldc           #22                 // String b_result_division:
     168: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     171: iload         24
     173: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     176: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     179: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     182: iconst_5  //将常量5压入栈中
     183: istore        25  //弹出栈顶元素5，并将其存储入第二十六个本地变量表
     185: bipush        15  //将常量15压入栈中
     187: istore        26  //弹出栈顶元素16，并将其存储入第二十七个本地变量表
     189: bipush        -50  //将常量-50压入栈中
     191: istore        27  //弹出栈顶元素-50，并将其存储入第二十八个本地变量表
     193: bipush        -2  //将常量-2压入栈中
     195: istore        28  //弹出栈顶元素-2，并将其存储入第二十九个本地变量表
     197: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     200: new           #13                 // class java/lang/StringBuilder
     203: dup
     204: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     207: ldc           #23                 // String s_result_addition:
     209: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     212: iload         25
     214: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     217: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     220: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     223: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     226: new           #13                 // class java/lang/StringBuilder
     229: dup
     230: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     233: ldc           #24                 // String s_result_subtraction:
     235: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     238: iload         26
     240: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     243: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     246: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     249: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     252: new           #13                 // class java/lang/StringBuilder
     255: dup
     256: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     259: ldc           #25                 // String s_result_multiplication:
     261: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     264: iload         27
     266: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     269: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     272: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     275: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     278: new           #13                 // class java/lang/StringBuilder
     281: dup
     282: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     285: ldc           #26                 // String s_result_division:
     287: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     290: iload         28
     292: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     295: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     298: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     301: iload         5  //将第五个int型变量从本地变量表加载到栈顶
     303: iload         6  //将第六个int型变量从本地变量表加载到栈顶
     305: iadd  //将栈顶两个int型的数值相加并将结果压入栈顶
     306: istore        29  //将栈顶的数值弹出保存到第三十个本地变量表
     308: iload         5  //将第五个int型变量从本地变量表加载到栈顶
     310: iload         6  //将第六个int型变量从本地变量表加载到栈顶
     312: isub  //将栈顶两个int型数值相减并将结果压入栈顶
     313: istore        30  //将栈顶的数值弹出保存到第三十一个本地变量表
     315: iload         5  //将第五个int型变量从本地变量表加载到栈顶
     317: iload         6  //将第六个int型变量从本地变量表加载到栈顶
     319: imul  //将栈顶两个int型数值相乘并将结果压入栈顶
     320: istore        31  //弹出栈顶数值保存到第三十二个本地变量表
     322: iload         5  //将第五个int型变量从本地变量表加载到栈顶
     324: iload         6  //将第六个int型变量从本地变量表加载到栈顶
     326: idiv  //将栈顶两个int型数值相除并将结果压入栈顶
     327: istore        32  //弹出栈顶数值并保存到第三十三个本地变量表
     329: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     332: new           #13                 // class java/lang/StringBuilder
     335: dup
     336: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     339: ldc           #27                 // String i_result_addition:
     341: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     344: iload         29
     346: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     349: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     352: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     355: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     358: new           #13                 // class java/lang/StringBuilder
     361: dup
     362: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     365: ldc           #28                 // String i_result_subtraction:
     367: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     370: iload         30
     372: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     375: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     378: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     381: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     384: new           #13                 // class java/lang/StringBuilder
     387: dup
     388: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     391: ldc           #29                 // String i_result_multiplication:
     393: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     396: iload         31
     398: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     401: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     404: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     407: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     410: new           #13                 // class java/lang/StringBuilder
     413: dup
     414: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     417: ldc           #30                 // String i_result_division:
     419: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     422: iload         32
     424: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
     427: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     430: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     433: lload         7
     435: lload         9
     437: ladd
     438: lstore        33
     440: lload         7
     442: lload         9
     444: lsub
     445: lstore        35
     447: lload         7
     449: lload         9
     451: lmul
     452: lstore        37
     454: lload         7
     456: lload         9
     458: ldiv
     459: lstore        39
     461: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     464: new           #13                 // class java/lang/StringBuilder
     467: dup
     468: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     471: ldc           #31                 // String l_result_addition:
     473: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     476: lload         33
     478: invokevirtual #32                 // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
     481: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     484: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     487: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     490: new           #13                 // class java/lang/StringBuilder
     493: dup
     494: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     497: ldc           #33                 // String l_result_subtraction:
     499: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     502: lload         35
     504: invokevirtual #32                 // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
     507: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     510: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     513: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     516: new           #13                 // class java/lang/StringBuilder
     519: dup
     520: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     523: ldc           #34                 // String l_result_multipication:
     525: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     528: lload         37
     530: invokevirtual #32                 // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
     533: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     536: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     539: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     542: new           #13                 // class java/lang/StringBuilder
     545: dup
     546: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     549: ldc           #35                 // String l_result_division:
     551: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     554: lload         39
     556: invokevirtual #32                 // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
     559: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     562: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     565: fload         11
     567: fload         12
     569: fadd
     570: fstore        41
     572: fload         11
     574: fload         12
     576: fsub
     577: fstore        42
     579: fload         11
     581: fload         12
     583: fmul
     584: fstore        43
     586: fload         11
     588: fload         12
     590: fdiv
     591: fstore        44
     593: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     596: new           #13                 // class java/lang/StringBuilder
     599: dup
     600: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     603: ldc           #36                 // String f_result_addition:
     605: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     608: fload         41
     610: invokevirtual #37                 // Method java/lang/StringBuilder.append:(F)Ljava/lang/StringBuilder;
     613: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     616: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     619: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     622: new           #13                 // class java/lang/StringBuilder
     625: dup
     626: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     629: ldc           #38                 // String f_result_subtraction:
     631: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     634: fload         42
     636: invokevirtual #37                 // Method java/lang/StringBuilder.append:(F)Ljava/lang/StringBuilder;
     639: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     642: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     645: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     648: new           #13                 // class java/lang/StringBuilder
     651: dup
     652: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     655: ldc           #39                 // String f_result_multipication:
     657: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     660: fload         43
     662: invokevirtual #37                 // Method java/lang/StringBuilder.append:(F)Ljava/lang/StringBuilder;
     665: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     668: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     671: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     674: new           #13                 // class java/lang/StringBuilder
     677: dup
     678: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     681: ldc           #40                 // String f_result_division:
     683: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     686: fload         44
     688: invokevirtual #37                 // Method java/lang/StringBuilder.append:(F)Ljava/lang/StringBuilder;
     691: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     694: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     697: dload         13
     699: dload         15
     701: dadd
     702: dstore        45
     704: dload         13
     706: dload         15
     708: dsub
     709: dstore        47
     711: dload         13
     713: dload         15
     715: dmul
     716: dstore        49
     718: dload         13
     720: dload         15
     722: ddiv
     723: dstore        51
     725: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     728: new           #13                 // class java/lang/StringBuilder
     731: dup
     732: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     735: ldc           #41                 // String d_result_addition:
     737: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     740: dload         45
     742: invokevirtual #42                 // Method java/lang/StringBuilder.append:(D)Ljava/lang/StringBuilder;
     745: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     748: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     751: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     754: new           #13                 // class java/lang/StringBuilder
     757: dup
     758: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     761: ldc           #43                 // String d_result_subtraction:
     763: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     766: dload         47
     768: invokevirtual #42                 // Method java/lang/StringBuilder.append:(D)Ljava/lang/StringBuilder;
     771: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     774: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     777: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     780: new           #13                 // class java/lang/StringBuilder
     783: dup
     784: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     787: ldc           #44                 // String d_result_multipication:
     789: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     792: dload         49
     794: invokevirtual #42                 // Method java/lang/StringBuilder.append:(D)Ljava/lang/StringBuilder;
     797: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     800: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     803: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     806: new           #13                 // class java/lang/StringBuilder
     809: dup
     810: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     813: ldc           #45                 // String d_result_division:
     815: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     818: dload         51
     820: invokevirtual #42                 // Method java/lang/StringBuilder.append:(D)Ljava/lang/StringBuilder;
     823: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     826: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     829: sipush        164
     832: istore        53
     834: bipush        34
     836: istore        54
     838: sipush        6435
     841: istore        55
     843: iconst_0
     844: istore        56
     846: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     849: new           #13                 // class java/lang/StringBuilder
     852: dup
     853: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     856: ldc           #46                 // String c_result_addition:
     858: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     861: iload         53
     863: invokevirtual #47                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
     866: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     869: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     872: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     875: new           #13                 // class java/lang/StringBuilder
     878: dup
     879: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     882: ldc           #48                 // String c_resullt_subtraction:
     884: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     887: iload         54
     889: invokevirtual #47                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
     892: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     895: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     898: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     901: new           #13                 // class java/lang/StringBuilder
     904: dup
     905: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     908: ldc           #49                 // String c_result_multipication:
     910: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     913: iload         55
     915: invokevirtual #47                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
     918: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     921: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     924: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     927: new           #13                 // class java/lang/StringBuilder
     930: dup
     931: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     934: ldc           #50                 // String c_result_division:
     936: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     939: iload         56
     941: invokevirtual #47                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
     944: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     947: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     950: iload         5
     952: iload         6
     954: if_icmple     986
     957: iconst_1
     958: istore        17
     960: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
     963: new           #13                 // class java/lang/StringBuilder
     966: dup
     967: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
     970: ldc           #51                 // String is_a:
     972: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     975: iload         17
     977: invokevirtual #52                 // Method java/lang/StringBuilder.append:(Z)Ljava/lang/StringBuilder;
     980: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
     983: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     986: iconst_0
     987: istore        57
     989: iload         57
     991: bipush        10
     993: if_icmpge     1035
     996: iload         5
     998: iload         6
    1000: iadd
    1001: istore        5
    1003: getstatic     #12                 // Field java/lang/System.out:Ljava/io/PrintStream;
    1006: new           #13                 // class java/lang/StringBuilder
    1009: dup
    1010: invokespecial #14                 // Method java/lang/StringBuilder."<init>":()V
    1013: ldc           #53                 // String i_a:
    1015: invokevirtual #16                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
    1018: iload         5
    1020: invokevirtual #17                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
    1023: invokevirtual #18                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
    1026: invokevirtual #19                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
    1029: iinc          57, 1
    1032: goto          989
    1035: return
}
```

> 参考链接：JVM助记符 https://blog.csdn.net/qq_33295042/article/details/114095551?spm=1001.2014.3001.5501

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

# JVM启动参数

![image-20210304092638480](C:\Users\pwang6\AppData\Roaming\Typora\typora-user-images\image-20210304092638480.png)

1. 以-开头为标准参数，所有的JVM都要实现这些参数，并且向后兼容

2. -D设置设置系统属性

3. 以-X开头为非标准参数，基本都是传给JVM的，默认JVM实现这些参数的功能，但是并不能保证所有的JVM实现都满足，且不保证向后兼容。可以使用java -X命令来查看当前JVM支持的非标准参数

4. 以-XX：开头为非稳定参数，专门用于控制JVM的行为，跟具体的JVM实现有关，随时可能会在下个版本取消

5. -XX：+-Flags形式，+-是对布尔值进行开关

6. -XX：key=value形式，指定某个选项的值

   > -server
   >
   > -Dfile.encoding=UTF-8
   >
   > -Xmx8g
   >
   > -XX:+UseG1GC
   >
   > -XX:MaxPermSize=256m

## 系统属性参数

> 类似于环境变量

-Dfile.encoding=UTF-8

-Duser.timezone=GMT+08

-Dmaven.test.skip=true

-Dio.netty.eventLoopThreads=8

-Dproperty=value（在应用程序可以使用String a = System.getProperty("a");）

System.setProperty("a","A10001");

String a = System.getProperty("a");

## 运行模式参数

1. -server：设置JVM使用server模式，特点是启动速度比较慢，但运行时性能和内存管理效率很高，适用于生产环境，在具有64位能力的JDK环境下将默认启用该模式，而忽略-client参数。【设置JIT编译器的模式】
2. -client：JDK1.7之前在32位的x86机器上的默认值是-client选项，设置JVM使用client模式，特点是启动速度比较快，但运行时性能和内存管理效率不高，通常用于客户端应用程序或者PC应用开发和调式，此外，我们知道JVM加载字节码之后，可以解释执行，也可以编译成本地代码再执行，所以可以配置JVM对字节码的处理模式【设置JIT编译器的模式】
3. -Xint：在解释模式（interpreted mode）下运行，-Xint标记会强制JVM解释执行所有的字节码，这当然会降低运行速度，通常低十倍或更多【设置JVM是采用解释器还是JIT编译器，此模式为完全采用解释器模式执行程序】
4. -Xcomp：-Xcomp参数与-Xint正好相反，JVM在第一次使用时会把所有的字节码编译成本地代码，从而带来最大程度的优化【注意预热】【设置JVM是采用解释器还是JIT编译器，此模式为完全采用即时编译器模式执行程序，如果即时编译出现问题，解释器会介入执行】
5. -Xmixed:-Xmixed是混合模式，将解释模式和编译模式混合使用，由JVM自己决定，这是JVM的默认模式，也是推荐模式，我们使用java -version可以看到mixed mode等信息【设置JVM是采用解释器还是JIT编译器，此模式为采用解释器和JIT编译器并存的方式共同执行程序，默认模式】

## 堆内存设置参数

> 堆内（Xms-Xmx）
>
> 非堆+堆外

- -Xmx指定最大堆内存，如-Xmx4g，这只是限制了Heap部分的最大值为4g，这个内存不包括堆外使用的内存
- -Xms指定堆内存空间的初始大小，如-Xms4g，而且指定的内存大小，并不是操作系统实际分配的初始值，而是GC先规划好，用到才分配。专用服务器上需要保持-Xms和-Xmx一直，否则应用可能就会有好几个FullGC，当两者配置不一致时，堆内存扩容可能导致性能抖动
- -Xmn等价于-XX：newSize，使用G1垃圾收集器不应该设置该选项，在其他的某些业务场景下可以设置。官方建议设置为-Xmx的1/2~1/4
- -XX：MaxPermSize，这是JDK1.7之前使用的，Java8默认允许的Meta空间无限大，此参数无效
- -XX：MaxMetaspaceSize=size,Java8默认不限制Meta空间，一般不允许设置该选项
- -XX：MaxDirectMemorySize=size,系统可以使用的最大堆外内存，这个参数跟-Dsun.nio.MaxDirectMemorySize效果相同
- -Xss,设置每个线程栈的字节数。例如-Xss1m指定线程栈为1MB，与-XX：TreadStackSize=1m等价

> **JVM参数默认值：**
>
> **-Xms：**指定jvm堆的初始大小，默认为物理内存的1/64，最小为1M；可以指定单位，比如k、m，若不指定，则默认为字节。
>
> **-Xmx:**指定jvm堆的最大值，默认为物理内存的1/4或者1G，最小为2M；单位与-Xms一致，一般建议设置为机器内存的一半
>
> **-Xss：**设置单个线程栈的大小，一般默认为512k

## GC设置参数

1. -XX:+UseG1GC：使用G1垃圾回收器
2. -XX:+UseConcMarkSweepGC:使用CMS垃圾回收器
3. -XX:+UseSerialGC：使用串行垃圾回收器
4. -XX:+UseParallelGC:使用并行垃圾回收器
5. -XX:+UnlockExperimentalVMOptions-XX:UseZGC //JAVA11
6. -XX:+UnlockExperimentalVMOptions-XX:+UseShenandoahGC //Java12

## 分析诊断参数

1. -XX：+-HeapDumpOnOutOfMemoryError 选项, 当 OutOfMemoryError 产生，即内存溢出(堆内存或持久代)时，自动 Dump 堆内存

   > 示例用法： java -XX:+HeapDumpOnOutOfMemoryError -Xmx256m ConsumeHeap

2. -XX：HeapDumpPath 选项, 与 HeapDumpOnOutOfMemoryError 搭配使用, 指定内存溢出时 Dump 文件的目录。【如果没有指定则默认为启动 Java 程序的工作目录。】

   > 示例用法： java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/usr/local/ ConsumeHeap 
   >
   > 自动 Dump 的 hprof 文件会存储到 /usr/local/ 目录下

3. -XX：OnError 选项, 发生致命错误时（fatal error）执行的脚本。

   例如, 写一个脚本来记录出错时间, 执行一些命令, 或者 curl 一下某个在线报警的 url.

   > 示例用法：java -XX:OnError="gdb - %p" MyApp 
   >
   > 可以发现有一个 %p 的格式化字符串，表示进程 PID。

4. -XX：OnOutOfMemoryError 选项, 抛出 OutOfMemoryError 错误时执行的脚本
5. -XX：ErrorFile=filename 选项, 致命错误的日志文件名,绝对路径或者相对路径
6. -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=1506，远程调试

## JavaAgent参数

Agent 是 JVM 中的一项黑科技, 可以通过无侵入方式来做很多事情，比如注入 AOP 代码，执行统计等等，权限非常大。这里简单介绍一下配置选项，详细功能需要专门来讲。

设置 agent 的语法如下: 

-agentlib:libname[=options] 启用 native 方式的 agent, 参考 LD_LIBRARY_PATH 路径。

-agentpath:pathname[=options] 启用 native 方式的 agent。 

-javaagent:jarpath[=options] 启用外部的 agent 库, 比如 pinpoint.jar 等等。

-Xnoagent 则是禁用所有 agent。

以下示例开启 CPU 使用时间抽样分析:

JAVA_OPTS="-agentlib:hprof=cpu=samples,file=cpu.samples.log"

# 附录

## **英文字母的ASCII码值**

*小写字母的ASCII码值比大写字母ASCII码值大32*

| 大写字母 | 十进制ASCII码 | 小写字母 | 十进制ASCII码 |
| -------- | ------------- | -------- | ------------- |
| A        | 65            | a        | 97            |
| B        | 66            | b        | 98            |
| C        | 67            | c        | 99            |
| D        | 68            | d        | 100           |
| E        | 69            | e        | 101           |
| F        | 70            | f        | 102           |
| G        | 71            | g        | 103           |
| H        | 72            | h        | 104           |
| I        | 73            | i        | 105           |
| J        | 74            | j        | 106           |
| K        | 75            | k        | 107           |
| L        | 76            | l        | 108           |
| M        | 77            | m        | 109           |
| N        | 78            | n        | 110           |
| O        | 79            | o        | 111           |
| P        | 80            | p        | 112           |
| Q        | 81            | q        | 113           |
| R        | 82            | r        | 114           |
| S        | 83            | s        | 115           |
| T        | 84            | t        | 116           |
| U        | 85            | u        | 117           |
| V        | 86            | v        | 118           |
| W        | 87            | w        | 119           |
| X        | 88            | x        | 120           |
| Y        | 89            | y        | 121           |
| Z        | 90            | z        | 122           |

## JVM启动参数大全及默认值

Java启动参数共分为三类；

其一是标准参数（-），所有的JVM实现都必须实现这些参数的功能，而且向后兼容；

其二是非标准参数（-X），默认jvm实现这些参数的功能，但是并不保证所有jvm实现都满足，且不保证向后兼容；

其三是非Stable参数（-XX），此类参数各个jvm实现会有所不同，将来可能会随时取消，需要慎重使用；

### 一、JVM标准参数(-)

JVM的标准参数都是以”-“开头，通过输入”java -help”或者”java -?”，可以查看JVM标准参数列表。如
![这里写代码片](http://static.tianshouzhi.com/ueditor/upload/image/20160220/1455983729301030637.png)

以下是JVM标准参数的详细介绍(红色标记的参数请着重注意):
以下是JVM标准参数的详细介绍(红色标记的参数请着重注意):

-client

设置jvm使用client模式，特点是启动速度比较快，但运行时性能和内存管理效率不高，通常用于客户端应用程序或者PC应用开发和调试。

-server

设置jvm使server模式，特点是启动速度比较慢，但运行时性能和内存管理效率很高，适用于生产环境。在具有64位能力的jdk环境下将默认启用该模式，而忽略-client参数。

-agentlib:libname[=options]

用于装载本地lib包；

其中libname为本地代理库文件名，默认搜索路径为环境变量PATH中的路径，options为传给本地库启动时的参数，多个参数之间用逗号分隔。在Windows平台上jvm搜索本地库名为libname.dll的文件，在linux上jvm搜索本地库名为libname.so的文件，搜索路径环境变量在不同系统上有所不同，比如Solaries上就默认搜索LD_LIBRARY_PATH。

比如：-agentlib:hprof

用来获取jvm的运行情况，包括CPU、内存、线程等的运行数据，并可输出到指定文件中；windows中搜索路径为JRE_HOME/bin/hprof.dll。

-agentpath:pathname[=options]

按全路径装载本地库，不再搜索PATH中的路径；其他功能和agentlib相同；更多的信息待续，在后续的JVMTI部分会详述。

-classpath classpath

-cp classpath

告知jvm搜索目录名、jar文档名、zip文档名，之间用分号;分隔；使用-classpath后jvm将不再使用CLASSPATH中的类搜索路径，如果-classpath和CLASSPATH都没有设置，则jvm使用当前路径(.)作为类搜索路径。

jvm搜索类的方式和顺序为：Bootstrap，Extension，User。

Bootstrap中的路径是jvm自带的jar或zip文件，jvm首先搜索这些包文件，用System.getProperty(“sun.boot.class.path”)可得到搜索路径。

Extension是位于JRE_HOME/lib/ext目录下的jar文件，jvm在搜索完Bootstrap后就搜索该目录下的jar文件，用System.getProperty(“java.ext.dirs”)可得到搜索路径。

User搜索顺序为当前路径.、CLASSPATH、-classpath，jvm最后搜索这些目录，用System.getProperty(“java.class.path”)可得到搜索路径。

-Dproperty=value

设置系统属性名/值对，运行在此jvm之上的应用程序可用System.getProperty(“property”)得到value的值。

如果value中有空格，则需要用双引号将该值括起来，如-Dname=”space string”。

该参数通常用于设置系统级全局变量值，如配置文件路径，以便该属性在程序中任何地方都可访问。

-enableassertions[:”…” | : ]

-ea[:”…” | : ]

上述参数就用来设置jvm是否启动断言机制（从JDK 1.4开始支持），缺省时jvm关闭断言机制。

用-ea 可打开断言机制，不加和classname时运行所有包和类中的断言，如果希望只运行某些包或类中的断言，可将包名或类名加到-ea之后。例如要启动包com.wombat.fruitbat中的断言，可用命令java -ea:com.wombat.fruitbat…。

-disableassertions[:”…” | :

### 二、JVM非标准参数(-X)

通过”java -X”可以输出非标准参数列表，如下所示：
![这里写图片描述](http://static.tianshouzhi.com/ueditor/upload/image/20160221/1455984309783026624.png)

非标准参数又称为扩展参数，其列表如下：

-Xint

设置jvm以解释模式运行，所有的字节码将被直接执行，而不会编译成本地码。

-Xbatch

关闭后台代码编译，强制在前台编译，编译完成之后才能进行代码执行；

默认情况下，jvm在后台进行编译，若没有编译完成，则前台运行代码时以解释模式运行。

-Xbootclasspath:bootclasspath

让jvm从指定路径（可以是分号分隔的目录、jar、或者zip）中加载bootclass，用来替换jdk的rt.jar；若非必要，一般不会用到；

-Xbootclasspath/a:path

将指定路径的所有文件追加到默认bootstrap路径中；

-Xbootclasspath/p:path

让jvm优先于bootstrap默认路径加载指定路径的所有文件；

-Xcheck:jni

对JNI函数进行附加check；此时jvm将校验传递给JNI函数参数的合法性，在本地代码中遇到非法数据时，jmv将报一个致命错误而终止；使用该参数后将造成性能下降，请慎用。

-Xfuture

让jvm对类文件执行严格的格式检查（默认jvm不进行严格格式检查），以符合类文件格式规范，推荐开发人员使用该参数。

-Xnoclassgc

关闭针对class的gc功能；因为其阻止内存回收，所以可能会导致OutOfMemoryError错误，慎用；

-Xincgc

开启增量gc（默认为关闭）；这有助于减少长时间GC时应用程序出现的停顿；但由于可能和应用程序并发执行，所以会降低CPU对应用的处理能力。

-Xloggc:file

与-verbose:gc功能类似，只是将每次GC事件的相关情况记录到一个文件中，文件的位置最好在本地，以避免网络的潜在问题。

若与verbose命令同时出现在命令行中，则以-Xloggc为准。

-Xms

指定jvm堆的初始大小，默认为物理内存的1/64，最小为1M；可以指定单位，比如k、m，若不指定，则默认为字节。

-Xmx

指定jvm堆的最大值，默认为物理内存的1/4或者1G，最小为2M；单位与-Xms一致。

-Xss

设置单个线程栈的大小，一般默认为512k。

-Xprof

输出 cpu 配置文件数据

-Xrs

减少jvm对操作系统信号（signals）的使用，该参数从1.3.1开始有效；

从jdk1.3.0开始，jvm允许程序在关闭之前还可以执行一些代码（比如关闭数据库的连接池），即使jvm被突然终止；

jvm关闭工具通过监控控制台的相关事件而满足以上的功能；更确切的说，通知在关闭工具执行之前，先注册控制台的控制handler，然后对CTRL_C_EVENT, CTRL_CLOSE_EVENT, CTRL_LOGOFF_EVENT, and CTRL_SHUTDOWN_EVENT这几类事件直接返回true。

但如果jvm以服务的形式在后台运行（比如servlet引擎），他能接收CTRL_LOGOFF_EVENT事件，但此时并不需要初始化关闭程序；为了避免类似冲突的再次出现，从jdk1.3.1开始提供-Xrs参数；当此参数被设置之后，jvm将不接收控制台的控制handler，也就是说他不监控和处理CTRL_C_EVENT, CTRL_CLOSE_EVENT, CTRL_LOGOFF_EVENT, or CTRL_SHUTDOWN_EVENT事件。

上面这些参数中，比如-Xmsn、-Xmxn……都是我们性能优化中很重要的参数；

-Xprof、-Xloggc:file等都是在没有专业跟踪工具情况下排错的好手；

### 三、JVM非Stable参数（-XX）

Java 6（update 21oder 21之后）版本， HotSpot JVM 提供给了两个新的参数，在JVM启动后，在命令行中可以输出所有XX参数和值。

```
-XX:+PrintFlagsFinal and -XX:+PrintFlagsInitial1
```

读者可以使用以下语句输出所有的参数和默认值

```
java -XX:+PrintFlagsInitial  -XX:+PrintFlagsInitial>>1.txt1
```

由于非State参数非常的多，因此这里就不列出所有参数进行讲解。只介绍我们比较常用的。

Java HotSpot VM中-XX:的可配置参数列表进行描述；

这些参数可以被松散的聚合成三类：

行为参数（Behavioral Options）：用于改变jvm的一些基础行为；

性能调优（Performance Tuning）：用于jvm的性能调优；

调试参数（Debugging Options）：一般用于打开跟踪、打印、输出等jvm参数，用于显示jvm更加详细的信息；

```sh
行为参数(功能开关)

-XX:-DisableExplicitGC  禁止调用System.gc()；但jvm的gc仍然有效

-XX:+MaxFDLimit 最大化文件描述符的数量限制

-XX:+ScavengeBeforeFullGC   新生代GC优先于Full GC执行

-XX:+UseGCOverheadLimit 在抛出OOM之前限制jvm耗费在GC上的时间比例

-XX:-UseConcMarkSweepGC 对老生代采用并发标记交换算法进行GC

-XX:-UseParallelGC  启用并行GC

-XX:-UseParallelOldGC   对Full GC启用并行，当-XX:-UseParallelGC启用时该项自动启用

-XX:-UseSerialGC    启用串行GC

-XX:+UseThreadPriorities    启用本地线程优先级

性能调优

-XX:LargePageSizeInBytes=4m 设置用于Java堆的大页面尺寸

-XX:MaxHeapFreeRatio=70 GC后java堆中空闲量占的最大比例

-XX:MaxNewSize=size 新生成对象能占用内存的最大值

-XX:MaxPermSize=64m 老生代对象能占用内存的最大值

-XX:MinHeapFreeRatio=40 GC后java堆中空闲量占的最小比例

-XX:NewRatio=2  新生代内存容量与老生代内存容量的比例

-XX:NewSize=2.125m  新生代对象生成时占用内存的默认值

-XX:ReservedCodeCacheSize=32m   保留代码占用的内存容量

-XX:ThreadStackSize=512 设置线程栈大小，若为0则使用系统默认值

-XX:+UseLargePages  使用大页面内存

调试参数

-XX:-CITime 打印消耗在JIT编译的时间

-XX:ErrorFile=./hs_err_pid<pid>.log 保存错误日志或者数据到文件中

-XX:-ExtendedDTraceProbes   开启solaris特有的dtrace探针

-XX:HeapDumpPath=./java_pid<pid>.hprof  指定导出堆信息时的路径或文件名

-XX:-HeapDumpOnOutOfMemoryError 当首次遭遇OOM时导出此时堆中相关信息

-XX:OnError="<cmd args>;<cmd args>" 出现致命ERROR之后运行自定义命令

-XX:OnOutOfMemoryError="<cmd args>;<cmd args>"  当首次遭遇OOM时执行自定义命令

-XX:-PrintClassHistogram    遇到Ctrl-Break后打印类实例的柱状信息，与jmap -histo功能相同

-XX:-PrintConcurrentLocks   遇到Ctrl-Break后打印并发锁的相关信息，与jstack -l功能相同

-XX:-PrintCommandLineFlags  打印在命令行中出现过的标记

-XX:-PrintCompilation   当一个方法被编译时打印相关信息

-XX:-PrintGC    每次GC时打印相关信息

-XX:-PrintGC Details    每次GC时打印详细信息

-XX:-PrintGCTimeStamps  打印每次GC的时间戳

-XX:-TraceClassLoading  跟踪类的加载信息

-XX:-TraceClassLoadingPreorder  跟踪被引用到的所有类的加载信息

-XX:-TraceClassResolution   跟踪常量池

-XX:-TraceClassUnloading    跟踪类的卸载信息

-XX:-TraceLoaderConstraints 跟踪类加载器约束的相关信息
```

## JVM调优实践

### 大型网站服务器案例

承受海量访问的动态Web应用

**服务器配置：**8 CPU, 8G MEM, JDK 1.6.X

**参数方案：**

-server -Xmx3550m -Xms3550m -Xmn1256m -Xss128k -XX:SurvivorRatio=6 -XX:MaxPermSize=256m -XX:ParallelGCThreads=8 -XX:MaxTenuringThreshold=0 -XX:+UseConcMarkSweepGC

**调优说明：**

-Xmx 与 -Xms 相同以避免JVM反复重新申请内存。-Xmx 的大小约等于系统内存大小的一半，即充分利用系统资源，又给予系统安全运行的空间。
-Xmn1256m 设置年轻代大小为1256MB。此值对系统性能影响较大，Sun官方推荐配置年轻代大小为整个堆的3/8。
-Xss128k 设置较小的线程栈以支持创建更多的线程，支持海量访问，并提升系统性能。
-XX:SurvivorRatio=6 设置年轻代中Eden区与Survivor区的比值。系统默认是8，根据经验设置为6，则2个Survivor区与1个Eden区的比值为2:6，一个Survivor区占整个年轻代的1/8。【SurvivorRatio配置为N，那么比例为---eden:s0:s1=N:1:1，计算时候Xmn/(N+1+1) 就是每一份的大小，对应乘就是Eden，s0,s1的大小】
-XX:ParallelGCThreads=8 配置并行收集器的线程数，即同时8个线程一起进行垃圾回收。此值一般配置为与CPU数目相等。
-XX:MaxTenuringThreshold=0 设置垃圾最大年龄（在年轻代的存活次数）。如果设置为0的话，则年轻代对象不经过Survivor区直接进入年老代。对于年老代比较多的应用，可以提高效率；如果将此值设置为一个较大值，则年轻代对象会在Survivor区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概率。根据被海量访问的动态Web应用之特点，其内存要么被缓存起来以减少直接访问DB，要么被快速回收以支持高并发海量请求，因此其内存对象在年轻代存活多次意义不大，可以直接进入年老代，根据实际应用效果，在这里设置此值为0。
-XX:+UseConcMarkSweepGC 设置年老代为并发收集。CMS（ConcMarkSweepGC）收集的目标是尽量减少应用的暂停时间，减少Full GC发生的几率，利用和应用程序线程并发的垃圾回收线程来标记清除年老代内存，适用于应用中存在比较多的长生命周期对象的情况。

### 内部集成构建服务器案例

高性能数据处理的工具应用
**服务器配置：**1 CPU, 4G MEM, JDK 1.6.X

**参数方案：**

-server -XX:PermSize=196m -XX:MaxPermSize=196m -Xmn320m -Xms768m -Xmx1024m

**调优说明：**

-XX:PermSize=196m -XX:MaxPermSize=196m 根据集成构建的特点，大规模的系统编译可能需要加载大量的Java类到内存中，所以预先分配好大量的持久代内存是高效和必要的。
-Xmn320m 遵循年轻代大小为整个堆的3/8原则。
-Xms768m -Xmx1024m 根据系统大致能够承受的堆内存大小设置即可。
在64位服务器上运行应用程序，构建执行时，用 jmap -heap 11540 命令观察JVM堆内存状况如下：

Attaching to process ID 11540, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 20.12-b01


using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio = 40
   MaxHeapFreeRatio = 70
   MaxHeapSize      = 1073741824 (1024.0MB)
   NewSize          = 335544320 (320.0MB)
   MaxNewSize       = 335544320 (320.0MB)
   OldSize          = 5439488 (5.1875MB)
   NewRatio         = 2
   SurvivorRatio    = 8
   PermSize         = 205520896 (196.0MB)
   MaxPermSize      = 205520896 (196.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 255852544 (244.0MB)
   used     = 101395504 (96.69828796386719MB)
   free     = 154457040 (147.3017120361328MB)
   39.63044588683081% used
From Space:
   capacity = 34144256 (32.5625MB)
   used     = 33993968 (32.41917419433594MB)
   free     = 150288 (0.1433258056640625MB)
   99.55984397492803% used
To Space:
   capacity = 39845888 (38.0MB)
   used     = 0 (0.0MB)
   free     = 39845888 (38.0MB)
   0.0% used
PS Old Generation
   capacity = 469762048 (448.0MB)
   used     = 44347696 (42.29325866699219MB)
   free     = 425414352 (405.7067413330078MB)
   9.440459523882184% used
PS Perm Generation
   capacity = 205520896 (196.0MB)
   used     = 85169496 (81.22396087646484MB)
   free     = 120351400 (114.77603912353516MB)
   41.440796365543285% used

结果是比较健康的。
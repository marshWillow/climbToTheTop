#### 1. java基础

##### 1.1  Java最新版本是多少？

​	从2017年8月开始，jdk固定在每年3月份和9月份发布新版本。

最早的jdk发布于1996年，很多读者还未出生，发布频次如下：

![](F:\博文\marshWillow\JavaStudy\笔试\aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy82ZnVUM2VtV0k1SVRZcnVpYkNMcVhOQzFvWm02Rk1nZTdPc0trOERzdUwwNHhCYklodzhWdmhZbllBemlhZzBoVzFJdXFWaEJ2N1BCblpWdGlhNG14UkIydy82NDA.jpg)

从Java 8到至今，虽然已经发展到了Java 14 ，但使用最多的还是java8，一句话就是

**“新版任你发，我用Java 8”**

下面我们看两个统计结果感受一下：

![](F:\博文\marshWillow\JavaStudy\笔试\aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy82ZnVUM2VtV0k1SVRZcnVpYkNMcVhOQzFvWm02Rk1nZTcxaWEyTjE3YzdnUWJxNzRPbmFtVGV0NWpGcG96QndFNmtoWjg1MHVBQkFUWFh0OU9CcU9nT2pBLzY0MA.jpg)

![](F:\博文\marshWillow\JavaStudy\笔试\aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy82ZnVUM2VtV0k1SVRZcnVpYkNMcVhOQzFvWm02Rk1nZTdMNTNYY29KMGhBTFpTVnRVV2VmTjBmSFcyMmJHUlFsVkhqUEFpY3VpYkVHRkpJSGhJR0NaTFV4QS82NDA.jpg)

可以看到使用最多的是java 8，其次就是Java 11

之所以这样，是因为这两个版本都是官方宣布长期支持的版本。

新版本层出不穷，如果使用的是java 8，就完全没必要升级，就算要升级，也要选Java 11，但是，开发者可以安装新版的jdk，了解一下新特性。

##### 1.2  Java语言由什么公司创立的？

 	1995年5月sun公司开发,2009年04年20日oracle 收购sun

##### 1.3 "白富美"在java编程中最接近的是什么？

​	接口,他就定义了一个概念,到底怎么样 算白,富 ,美没实际的内容。

#####  1.4 Java 中三剑客指的是什么？

​	JSF2.0、EJB3.1、JPA2.0

##### 1.5 Java如何执行

​		java程序是圣旨，而程序员是皇帝，jdk就是一套成熟的官僚系统。

​	java代码写好了，要编译成.class文件，放到jvm里执行。

圣旨需要润色，内阁还要审核，无误后才让下面的人执行。

![image-20210820113645831](C:\Users\H0137197\AppData\Roaming\Typora\typora-user-images\image-20210820113645831.png)

##### 1.6 Java虚拟机如何判定热点代码

判定热点有两种，基于采样和基于计数器

hotspot虚拟机采用计数器方法，方法调用计数器和回边计数器

##### 1.7 Java语言都有那些特点

面向对象编程，跨平台兼容，执行性能好，运行效率高，提供大量的api跨站，有多线程支持，安全性好。

##### 1.8 跨平台实现的原理是什么？

jvm来应对不同服务器的差异，java程序只需要生成jvm可执行的代码即可

##### 1.9 JDK, JRE , JVM有什么区别

jdk包含java运行和开发环境，jre是java运行的最小单元，jvm是一种规范，是java程序运行的载体。
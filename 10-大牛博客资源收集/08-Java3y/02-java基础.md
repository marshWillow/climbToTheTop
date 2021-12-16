#### 1. 聊一聊注解

##### 啥是注解？

我觉得就是一种加标记，像打游戏时候的买装备，每个英雄都有自己的技能，而加了注解美就相当于买了装备，这个装备给他加了属性

注解可能在编译、类加载，或者运行等不同的时间点生效。就像游戏装备，可能你买了就加了属性，也可能是你攻击的时候才加buff，更有的有主动技能，你只有释放才能生效。

而就像装备是打游戏必不可少的一样，注解在开发中更是非常常见。

特别是我们用框架的时候，最常见的Spring中有@Controller、@Select等，还有生成日志的logo4J2,生成文档的Swagger等等

除了框架注解，Java也有几个基本注解，大多数用于标记和检查。

超脱基本注解之外的，叫元注解，是注解的注解，作用呢，就是给注解设定一些基本法则，比如生命周期，适用范围。

##### 自己实现注解？

注解一大本质就是，将业务代码和基本属性隔离。

就比如给我加了一个我是贵公司员工这个标记，那么我只需要干自己本职工作就行，工作地点不用自己操心，也不用考虑电脑，房租，水电，保险公积金啥的也不用操心。

写一个注解，第一要考虑的是有啥业务需求，

注解的本质是接口，但又不是注解

###### 业务背景

监控告警系统，监控一般的指标就是QPS(Query Per Second:吞吐能力指标)、RT（Responset Time 响应时间）和错误

```java
public void send(String userName) {
  try {
    // qps 上报
    qps(params);
    long startTime = System.currentTimeMillis();

    // 构建上下文(模拟业务代码）
    ProcessContext processContext = new ProcessContext();
    UserModel userModel = new UserModel();
    userModel.setAge("22");
    userModel.setName(userName);
    //...

    // rt 上报
    long endTime = System.currentTimeMillis();
    rt(endTime - startTime);
  } catch (Exception e) {
    
    // 出错上报
    error(params);
  }
}
```

面对这种情况，最好的就是用AOP切面配合注解，让实现变得优雅

先定义这个注解，先不管它的具体功能，要设定好它本身的特征，也就是用元注解给它加相关配置。

最重要的就是这个注解在啥时候生效，用@Rentention，它定义了三个常量，SOURCE、CLASS和RUNTIME.

三个常量代表了三个层次，在源码中保留，在编译器保留，和最终的JVM保留

显然意见，我们定义的注解都是要在jvm中保留的，因为我们专注的是业务问题，都要根据运行时去做处理。

写自定义注解需要配合反射来用，为啥呢?

那就要考虑反射的作用是啥，我们现在知道它是Java获取运行时信息的重要手段就行，具体晚点聊。

#### 2.聊一聊泛型

泛型，我觉得就是薛定谔的类型，只有在对象创建或方法调用的时候才能明确具体类型

因为类型不确定，那就不需要什么强制转换，也没有编译是类型异常

感觉很像强类型语言和弱类型语言的中间产物

```java
List<String> lists = new ArrayList<>();
lists.add("对线面试官");
```

其他就是写组件了

一般组件都是泛型+反射来实现的。

把参数设置成泛型，通过反射获取字段具体的值，然后累加

```java
// 传入 需要group by 和 sum 的字段名
public cacheMap(List<String> groupByKeys, List<String> sumValues) {
  this.groupByKeys = groupByKeys;
  this.sumValues = sumValues;
}

private void excute(T e) {
  
  // 从pojo 取出需要group by 的字段 list
  List<Object> key = buildPrimaryKey(e);
  
  // primaryMap 是存储结果的Map
  T value = primaryMap.get(key);
  
  // 如果从存储结果找到有相应记录
  if (value != null) {
    for (String elem : sumValues) {
      // 反射获取对应的字段，做累加处理
      Field field = getDeclaredField(elem, e);
      if (field.get(e) instanceof Integer) {
        field.set(value, (Integer) field.get(e) + (Integer) field.get(value));
      } else if (field.get(e) instanceof Long) {
        field.set(value, (Long) field.get(e) + (Long) field.get(value));
      } else {
        throw new RuntimeException("类型异常,请处理异常");
      }
    }
    
    // 处理时间记录
    Field field = getDeclaredField("updated", value);
    if (null != field) {
      field.set(value, DateTimeUtils.getCurrentTime());
    }
  } else {
    // group by 字段 第一次进来
    try {
      primaryMap.put(key, Tclone(e));
      createdMap.put(key, DateTimeUtils.getCurrentTime());
    }catch (Exception ex) {
      log.info("first put value error {}" , e);
    }
  }
}
```

#### 3.聊一聊 Java  NIO

NIO (no-blocking io 或者new io)，jdk1.4开始有，为了提高速度

传统io是

#### 4.聊一聊反射和动态代理

##### 反射

反射就是在Java **运行时**获取类的信息

我们写的代码都是.java，然后javac编译成.class文件，JVM运行class文件

我们写的轮子，最简单的叫工具类，打成jar包叫组件，更复杂的就叫框架

而好的轮子都是一步一步演变的，兼容了无数的情况。

但是，无论如何，你都无法控制用户传人什么东西，但是你必须设定最终的结果。

然后就有了一个思想：**约定大于配置，配置大于硬编码**

通过约定，不用你去配置什么，我们用反射在运行时获取响应的信息，实现代码功能的通用性和灵活性

引申问题：泛型只存在编译阶段，会被擦除，在class字节码就看不到了呢

泛型擦除不是一定的，有自己的范围，定义在类上的不会被擦除，

而Type是顶级接口，下面有几种类型，通过这些接口我们就可以在运行时获取泛型的相关信息。





##### 动态代理

代理模式是设计模式之一，

代理分静态和动态，

在Java中，动态代理有两种实现方式：JDK和CGLIB

后者是通过修改字节码来处理，而JDK则是通过InvokeHandler对方法进行增强

Mybatis只写接口，不写实现类就能执行SQL，用的就是动态代理
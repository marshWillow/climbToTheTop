#### 问答题

1. 详细描述ThreadPoolExecutor的各个参数的含义，介绍一个任务提交到线程池后的执行流程。

```java
 ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long  keepAliveTime,TimeUnit unit,BlockingQueue<Runnable> workQueue  ,ThreadFactory threadFactory,RejectedExecutionHandler handler) 

  1.corePoolSize:核心线程池大小 

  2.maximumPoolSize:最大线程池大小 

  3.keepAliveTime:线程最大空闲时间 

  4.unit:时间单位 

  5.workQueue:线程等待队列 

  6.threadFactory:线程创建工厂 

  7handler:拒绝策略 

 执行流程： 
   当在execute(Runnable)方法中提交新任务并且少于corePoolSize线程正在运行时，即使其他工作线程处于空闲状态，会创建一个新线程来处理该请求。如果有多于corePoolSize但小于maximumPoolSize线程正在运行，则仅当队列已满时才会创建新线程。如果maximumPoolSize达到最大值会执行拒绝策略。 
```



2. 请简要说明Servlet中的生命周期

   ```java
   
       Servlet 通过调用 init () 方法进行初始化。
       Servlet 调用 service() 方法来处理客户端的请求。 
       Servlet 通过调用 destroy() 方法终止（结束）。 
       答出 init 和 destroy方法只调用一次的可以适当加分
   
   ```

   

3. 开启两个线程A、B，打印1到10，线程A打印奇数（1、3、5、7、9），线程B打印偶数（2、4、6、8、10）

   ```
   new Thread(()->{
       for(int i=1;i<=10;i++) {
           if(i%2!=0) {
               system.out.println(i);
           }
       }
   }, A).start();
   new Thread(()->{
       for(int i=1;i<=10;i++) {
           if(i%2==0) {
               system.out.println(i);
           }
       }
   }, B).start();
   ```

   

4. 请编写代码实现单例模式 ，类名为Singleton

```java
class Singleton{
    private Singleton(){}
    //饿汉式
    static private Singleton instance=new Singleton();//因为无法实例化，所以必须是静态的
    static public Singleton getInstance(){
        return instance;
    }
    //懒汉线程安全
    private static volatile Singleton instance2;
    public static Singleton getInstance2(){//双锁检查，线程安全
        if(instance2==null){
            synchronized (Singleton.class){
                if(instance2==null)
                    instance2=new Singleton();
            }
        }
        return instance2;
    }
}

```



5. 写一个Map转换成JavaBean的工具类方法，实现如下mapToObject方法（使用Java反射，不允许使用第三方类库）  

```java
public static Object mapToObject(Map<String, Object> map, Class<?> beanClass){      
        if (map == null)    
            return null;      
        Object obj = null;  
        try {  
            obj = beanClass.newInstance();    
            Field[] fields = obj.getClass().getDeclaredFields();                         for (Field field : fields) {      
                int mod = field.getModifiers();      
                if(Modifier.isStatic(mod) || Modifier.isFinal(mod)){      
                    continue;      
                }      
                field.setAccessible(true);      
                field.set(obj, map.get(field.getName()));     
            }  
        } catch (Exception e) {  
            e.printStackTrace();  
        }     
        return obj;      
    }

```

6. 数据库操作是我们经常使用的一个技能， 请你完成一个简单的用户密码验证过程 ，给定的条件如下： 


  数据库中存在个用户表:users ,表结构如下： 

```mysql
CREATE TABLE `users` (`` ```uid` bigint(``20``) NOT NULL COMMENT ``'用户ID'``,`` ```user_name` varchar(``32``) NOT NULL COMMENT ``'用户账号'``,`` ```password` varchar(``64``) NOT NULL COMMENT ``'用户混淆密码'``,`` ``PRIMARY KEY (`uid`),`` ``UNIQUE KEY `u_user_name` (`user_name`)``) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT=``'用户表'` `  
```

  完善以下方法 

 

```java
public boolean verifyPassword(String username,String password){
        Connection con=getConnection;
        String sql="SELECT password FROM users WHERE user_name=?";
        PreparedStatement pst=null;
        ResultSet rs=null;
        boolean flag=false;
        try{
            pst=con.prepareStatement(sql);
            pst.setObject(1,username);
            rs=pst.executeQuery();
            while(rs.next()){
                if(rs.getString("password").equals(password)){
                    flag=true;
                }
            }
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }catch (SQLException e){
            e.printStackTrace();
        }finally {
            try{
                if(rs!=null) rs.close();
                if(pst!=null) pst.close();
                if(con!=null) con.close();
            }catch(SQLException e){
                e.printStackTrace();
            }
        }
        return flag;
    }

```

写Import 语句，只需要补充关键步骤即可 

7. 题目描述

介绍HashMap的数据结构、扩容机制，HashMap与Hashtable的区别，是否是线程安全的，并介绍ConcurrentHashMap的实现机制。

```java
  HashMap的数据结构：数组+链表+红黑树，当链表的值大于8的时候，就会转成红黑树存储。 

  HashMap默认初始容量16，加载因子0.75，当存储容量大于16*0.75的时候，扩容两倍，并将原数据复制到新的数组上。 

  HashMap是线程不安全的，性能高，用迭代器遍历索引从小到大，初始容量16，扩容是容量*2，HashTable线程安全，性能低，用迭代器遍历索引从大到小，初始容量11，扩容是容量*2 +1； 

  ConCurrentHashMap是线程安全的，通过进行数据的分段加锁，避免了锁的竞争。 
```

8. 介绍数据库连接池的实现方式。如何从连接池中获取连接、将连接放回连接池？使用连接池的优势是什么？列举一下自己用过的连接池。

​     druid

9. 什么是死锁？JAVA程序中什么情况下回出现死锁？如何避免出现死锁？



10. 分布式锁有几种实现方式，并介绍每种方式的优缺点。 



11. 什么是TCP粘包拆包？为什么会出现粘包拆包？如何在应用层面解决此问题？



12. 请大致描述一下BIO，AIO和NIO的区别？ 

13. 在JAVA语法中加载类的的方式有哪些？ 

    

14. 建立三个线程A、B、C，A线程打印10次字母A，B线程打印10次字母B,C线程打印10次字母C，但是要求三个线程同时运行，并且实现交替打印，即按照ABCABCABC的顺序打印。

15. 请列举5个spring框架中的注解，并说明注解的用法以及使用场景 

#### 编程题

16. [编程题]大值求和

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

```
给定一组自然数，数字的值有可能会大于``2``^``64` `，要求计算出所有数字的和
```

17. [编程题]计算二进制数中1的个数

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

  给定一个int 数字，要求计算出int数字对应的二进制中1的个数 



18. [编程题]日期计算

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

```
根据产品策略某本书可以设置包月到期时间，需要计算指定时间到包月到期时间还有多少分钟,不足60S的不计入。
```

19. 对MAP进行排序

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  map是一种开发过程中经常使用的k-v数据结构，有个map保存了书名和书字数的关系，编写代码对map里面的书按照字数进行升序排序 

20. [编程题]字符串替换

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  起点APP上允许用户对作品进行评论，为了防止用户恶意评论，发表不当内容，需要对用户发布的内容进行过滤，请写程序过滤用户发布内容中带有的QQ号（6~10位数字组成） 

  允许对内容严格操作，如用户发表了 **作者大大666666，为你点赞** ，经过过滤后也可以为**作者大大****，为你点赞** ，将666666过滤掉了。 

21. [编程题]判断是否是素数

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

  质数(又称素数),是指在大于1的自然数中,除了1和它本身外,不能被其他自然数整除(除0以外)的数称之为素数(质数)。请写个程序判断输入的数字是否是质数，如果是素数请输出：true，不是请输出false  

22. [编程题]爬楼梯次数

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  有 n 个台阶，你一次能走 1 个或者 2 个台阶，那么请问，走完这 n 个台阶共有几种方式？ 

23. [编程题]回文子串数量

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

​    给定一个字符串，返回这个字符串中有多少个回文子串。    两个相同的回文子串出现在不同的位置，认为是2个回文子串。    a、aa、aaa、aba、aabaa、abcba均认为是回文子串。  

24. [编程题]链表翻转

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

将一个给定的单链表反转，例：1-2-3-4-5，反转为5-4-3-2-1

25. [编程题]二叉树的最近公共祖先

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。  

26. [编程题]二分查找

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  给定一个递增排序的数组，查找某个数字是否在数组中，如果在数组中，则返回该数字在数组中第一次出现的位置（从0开始）；如果不在数组中，返回-1 。 

  不需要考虑给定的数组不是递增的情况。 

  务必使用二分查找的方式。 

27. [编程题]矩阵乘法

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

  ![img](https://uploadfiles.nowcoder.com/images/20200215/323196_1581771442014_62A98EF446AE2FA37BBBD0037E5004FE)  

  


  请编写程序实现矩阵的乘法 

28. [编程题]二进制1的位数

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  求出一个正整数转换成二进制后的数字“1”的个数。 

  例：数字23转为二进制为 10111，其中1的个数为4 

29 [编程题]去除重复字符

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  去除字符串中的重复字符，对于出现超过2次（包含2次）的字符，只保留第一个。 

  例：输入abcbdde，输出abcde。 

30. [编程题]计算数组的最大乘积

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

给定一个整型数组，移除数组的某个元素使其剩下的元素乘积最大，如果数组出现相同的元素 ，请输出第一次出现的元素

31. [编程题]旋转数组

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 32M，其他语言64M

  给定一个整型正方形矩阵 Matrix，请把该矩阵调整成顺时针旋转90度的样子。 

32. [编程题]第一个不重复的字符

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  在字符串中找到第一个不重复的字符。 

  例：对于字符串“hellohehe”，第一个不重复的字符是“o”。 

  如果每个字符都有重复，则抛出运行时异常。 

33. [编程题]计算朋友圈

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

   假设有N个用户，其中有些人是朋友，有些则不是。A和B是朋友，B和C是朋友，这样ABC就是一个朋友圈，请计算给定的朋友关系的朋友圈数。  

   给定一个 N * N 的矩阵 M，表示用户之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个人互为朋友关系，否则为不知道。你必须输出所有用户中的已知的朋友圈总数。

34. [编程题]数据多项排序

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  假设有个文件，文件的每一行是书信息数据，分4个部分用逗号（,）进行分割,格式如下 

  


  id,category,words,updatetime


  id 表示书id，long类型，id不重复； 

  category 表示书的分类，int类型，请注意全部数据的分类只有几个 

  words 表示书的字数，int类型 

  updatetime 表示书的更新时间 ，格式为2020-02-01 23:00:00 

  


  请编写程序对文件数据进行排序后输出id，排序优先级为： **category>updatetime > words > id , 增序排序**  

35. [编程题]使用堆栈实现队列功能

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  请使用堆栈这**一个**数据结构实现简单FIFO（先入先出）队列，队列要实现两个方法： push、pop。 

  为自动测试方便，使用每行输入模拟操作： 

  1） push 1 表明向队列里面新增一个元素 1 , push 和元素之间用空格表示； 

  2） pop 表明输出当前队列里面的第一个元素，如果当前队列为空请输出null 

  


  请将每个输出以英文逗号拼接到一个字符串中。 

36. [编程题]计算参数的hash值

时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 256M，其他语言512M

  在和外部公司联调HTTP接口时，对方要求调用的接口需要计算token，给到的规则如下： 

  1） 所有的参数值必须经过urlencode,编码为utf-8； 

  2） 对编码后数据按照key值进行字典升序排序； 

  3）将所有的参数按照排序的顺序拼接成字符串 ，格式如下: k1=v1&k2=v2&k3=v3; 

  4) 将第三步的算出的值计算md5值，md5加密的值小写 

  


  请你编写一段方法计算token值 
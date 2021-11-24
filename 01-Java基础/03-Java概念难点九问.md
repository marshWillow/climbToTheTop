#### 1. final，finally，finalize的区别

##### final:

​		关键字，修饰类，意味它不再派生新的子类，不能被继承。

​		一个类不能即声明abstract，又被声明为final。变量和方法被声明为final，保证他们在使用中不被改变。必须给定初值，以后引用只能读取，不可修改。方法也只能使用，不能重载。

##### finally：

​		异常处理时，提供fianlly块，来执行任何清除操作。

##### finalize:

​			方法名，在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。

​			在Object类中定义的，所有类都继承了它，子类覆盖fianlize()方法以整理系统资源或者执行其他清理工作。

#### 2. HashMap和Hashtable的区别

​		都属于Map接口的类，实现了将唯一键映射到特定值上。

​		HashMap类没有分类或者排序，允许一个null键和多个null值。

​		Hashtable不允许null键和null值。比HashMap慢，他是同步的。

#### 3. String s= new String("xyz");

​		创建了两个对象，一个是"xyz",一个是指向"xyz"的引用对象s

#### 4. sleep()和wait()有什么区别？

##### sleep()方法

​		是让线程停止一段时间的方法，在sleep时间间隔期满后，线程不一定立即回复执行，这是因为在哪个时刻，其他线程可能正在运行而且没有被调度为放弃执行，除非醒来的线程具有更高的优先级或正在运行的线程因为其它原因而阻塞。

##### wait()

​		是线程交互时，如果线程对一个同步对象x发出一个wait()调用，该线程会暂停执行，被调对象进入等待状态，直到被唤醒或等待时间到。

#### 5. short s1 = 1; s1 = s1 + 1;

​		有错，s1是short型，s1+1是int型,不能显式转化为short型。

​		可修改为s1 =(short)(s1 + 1) 。short s1 = 1; s1 += 1正确。

​		int 4字节

​		short 2字节
​		short+int编译器为了避免内存溢出,就给它向上转型(int) 而int不能直接付给short所以编译时就会报错,而		s1+=1时,底层会帮我们自动强制类型转换把int转成short

#### 6. Overload和Override的区别

​		重写和重载是多态性的不同表现。

​		重写是父类和子类之间多态性的表现，重载是一个类多态性的表现。

子类中定义某方法和某父类有相同的名称和参数，我们说该方法被重写，子类的对象使用这个方法，将调用子类中的定义。如果一个类中定义了多个同名的方法，他们或有不同的参数个数或有不同的参数类型，则为方法的重载。

#### 7. Set里元素是不能重复的

那个用iterator()方法来区分重复与否。equals()是判读两个Set是否相等。

对于==，如果作用于基本数据类型的变量，直接比较其存储的值是否相等。

如果作用于引用类型的变量，比较的是所指向的对象的地址。

对于eauals方法，不能作用于基本数据类型的变量。没有重写，比较的是引用功德变量所指向的对象的地址。重写的话，比较的是所指向的对象的内容。

#### 8. error和exception的区别

error表示恢复不是不可能但是很困难的一种严重问题。比如内存溢出，不能指望程序处理这样情况。

exception表示一种设计或实现问题。也就是说，如果程序运行正常，不该发生的情况。

#### 9. 最常见的runtime exception

ClassCastException，类型强制转换异常

ArtithmeticException，算术异常；

IndexOutOfBoundsExecption:下标越界

NullPointerException，空指针异常；

ArrayIndexOutOfBoundsException，数组下标越界异常

FileNotFoundException：文件未找到

SQLException:操作数据库异常；

IOException:输入输出异常
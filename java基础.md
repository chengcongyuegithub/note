# java基础
## 重载和重写的区别
重载：发送在同一个类中，方法名相同，参数列表不同，参数列表不同表示的是参数的类型，顺序，个数不同，访问权限修饰符和返回值也可以不同，发生在编译期
重写：发生在父子类中，方法名相同，参数列表相同，(两个小于，一个大于)，抛出的异常要小于等于父类，返回值的类型要小于等于父类，访问权限修饰符要大于等于父类。如果父类的方法是private的话，则无法重写该方法。
## String,StringBuffer,StringBuilder
### 可变性
String的字符串内部是final修饰的字符数组存储的，所以是不可变的。
StringBuffer和StringBuilder两个都是继承AbstractStringBuilder的，AbstractStringBuilder内部的字符串是字符数组修饰的，没有final修饰，所以StringBuffer和StringBuilder都是可变的
### 多线程
String是不可变的，相当于是常量，没有线程安全问题。
StringBuffer的方法是由同步锁修饰的，所以是线程安全的。
StringBuilder没有同步锁的修饰，所以是线程不安全的
### 性能
String是不可变的，所以修改一个String对象的时候，我们会创建一个新的对象，然后指针指向这个新的对象。
StringBuffer和StringBuilder修改字符串都是对同一个对象进行修改。
StringBuilder没有同步锁的修饰，性能更高，但是会有线程安全的问题
### 总结
少量字符串的操作就使用String，单线程的大量字符串操作使用StringBuilder，多线程的大量字符串操作使用StringBuffer
## 自动拆装箱(待补充)
装箱：将基本数据类型转化为对应的引用数据类型包装
拆箱：将包装器类型转化为基本数据类型
## ==和equals
### 概述
==比较的是两个对象的内存地址。如果是基本数据类型，那么就比较两个数据的值，如果是引用数据类型，那么就比较两个对象的地址。
equals分为两种情况：
1，该类重写了equals。一般就是比较两个对象的内容。
2，该类没有重写equals。这样的话比较的是两个内存地址，和==等价。
### String为例
String重写了equals方法，两个String对象比较的是两个对象的字符内容
在String str="ab";在我们这样创建字符串的时候，我们会在常量池中先查找有没有ab这样的字符串，如果有的话直接指向，相当于没有创建新的字符串对象，如果没有就创建
## final关键字总结(待补充)
final会使用在三个地方，属性，方法，类
如果一个属性被final修饰，那么这个属性相当于是一个常量，如果是基本数据类型，那么它初始化之后就不能在进行修改，如果是引用数据类型，它的指向就不能发生变化。
如果一个类被final修饰的话，那么这个类就不能被继承，它内部的方法也都隐式的加上了final修饰
如果一个方法被final修饰的话，那么这个方法不能被重写，private修饰的方法相当于加上了final
## Object类的常用方法总结
1,getClass():返回当前对象的class对象，这是一个final方法，不能被重写
2,hashCode():返回当前对象的哈希码
3,equal():比较两个对象的内存地址，是否是同一个对象
4,clone():获取一个当前对象的拷贝。一般来说x.clone()!=x为true，x.clone().getClass()!=x.getClass()为false。如果一个类没有实现cloneable接口，那么调用clone()方法就会抛出异常
5,toString():返回当前对象的类型名@当前对象的hashCode的16进制
6,notify() 唤醒一个等待监视器的线程
7,notifyAll()唤醒所有等待监视器的线程
8,wait() 舍弃锁，等待
9,wait() 。。。
10,wait()。。。
11,finalize()实例被垃圾回收触发的操作
`上面的问题紧接着就是hashCode与equals的`
###  hashCode与equals
`你重写过 hashcode 和 equals 么，为什么重写equals时必须重写hashCode方法？`
#### hashCode
hashCode获取当前对象的哈希码，哈希码也叫散列码，表示当前对象在哈希表中的索引位置。hashCode是在object.java中实现的，也就是任何一个对象都有这个方法。hashCode是本地方法，是由C或者C++实现的。
散列表的特点，就是根据键可以快速的定位到对应的值，这个时候，我们就使用到了散列码
#### 为什么要有hashCode
大大减少了equal的次数
#### hashCode()与equals()的相关规定
1，如果两个对象相等，它们的哈希值一定相等
2，如果两个对象相等，分别调用equals方法都是true
3，`如果两个对象哈希值相等，那么两个对象不一定相等`
4，如果要重写equal方法，那么一定要重写hashCode犯法
5，hashCode默认给堆上的对象一个特殊的值，如果不重写的话，两个对象即使指向相同的数据也是不相等的
#### 如果两个对象哈希值相等，那么两个对象不一定相等
hashCode()底层是使用杂凑算法的，该算法可能让多个对象返回相同的值。越糟糕的杂凑算法，发生碰撞的几率也就越大。这和数据的值域也有关系。
我们在在使用hashSet的时候，我们先通过hashCode进行查找，如果返回多个对象，那么在通过equals来判断是否是真的相同，使用hashCode就是来减少成本
### 参考文章
[JAVA基础-自问自答学hashCode和equals](https://juejin.im/post/59b25f825188257e7e11500c)
## java的异常处理(待补充)
## 抽象类和接口的区别
1，抽象类可有方法实现的细节，接口只能提供public abstract方法
2，抽象类可有各式各样的方法，接口只能提供public static final类型的变量
3，接口中不能有静态方法和静态代码块，抽象类中可以有静态方法和静态代码
4，一个类有一个抽象类，可以实现多个接口
5，接口不能new实例化，但是可以声明，必须引用一个实现该接口的对象。从设计层面上来说
```
接口是对类的抽象，是模板设计。接口是对行为的抽象，是一种行为规范
```

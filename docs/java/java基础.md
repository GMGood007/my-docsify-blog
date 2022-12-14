## 1.Java语言有哪些特点

1. 面向对象
2. 简单易学、有丰富的类库
3. 与平台无关性（JVM是java跨平台使用的根本）
4. 可靠安全
5. 支持多线程

## 2.面向对象和面向过程的区别

**面向过程**

是分析解决问题的步骤，然后用函数把这些步骤一步一步地实现，然后在使用的时候一一调用则可。性能较高，所以单片机、嵌入式开发等一般采用面向过程开发。

**面向对象**

是把构成问题的事务分解成各个对象，而建立对象的目的也不是为了完成一个个步骤，而是为了描述某个事物在解决整个问题的过程中所发生的行为。面向对象有**封装、继承、多态**的特性，所以易维护、易复用、易扩展。可以设计出低耦合的系统。 但是性能上来说，比面向过程要低。

## 3.八种基本数据类型的大小，以及他们的封装类

1.1 基本类型
1、Java 有哪些基本数据类型？
众所周知，Java 是一门强类型语言，对于程序中的每一个变量都明确定义了一种数据类型，以及分配不同大小的内存空间。

其中，Java 提供了八种基本数据类型格式，包括 4 种整型，2 种浮点，1 种字符（char 是两字节）以及 1 种布尔型类型。

| 数据类型 | 位数  | 默认值 |    取值范围    |      |
| :------: | :---: | :----: | :------------: | ---- |
|   byte   |   8   |   0    |   -128 ~ 127   |      |
|  short   |  16   |   0    | -32768 ~ 32767 |      |
|   int    |  32   |   0    | -2^31 ~ 2^31-1 |      |
|   long   |  64   |   0    | -2^63 ~ 2^63-1 |      |
|  float   |  32   |   0f   | -2^63 ~ 2^63-1 |      |
|  double  |  64   |   0d   | -2^63 ~ 2^63-1 |      |
|   char   |  16   | 空字符 |   0 ~ 2^16-1   |      |
| boolean  | false | false  |      true      |      |

## 	4.**instanceof** 关键字的作用


​			instanceof 严格来说是Java中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法

为：

```java
boolean result = obj instanceof Class
```

其中 obj 为一个对象，Class 表示一个类或者一个接口，当 obj 为 Class 的对象，或者是其直接或间接子类，或者是其接口的实现类，结果result 都返回 true，否则返回false。

注意：编译器会检查 obj 是否能转换成右边的class类型，如果不能转换则直接报错，如果不能确定类型，则通过编译,具体看运行时定。

```java

int i = 0;
System.out.println(i instanceof Integer);//编译不通过 i必须是引用类型，不能是基本类型
System.out.println(i instanceof Object);//编译不通过
Integer integer = new Integer(1);
System.out.println(integer instanceof Integer);//true
//false ,在 JavaSE规范 中对 instanceof 运算符的规定就是：如果 obj 为 null，那么将返
回 false。
System.out.println(null instanceof Object);
```

​			

## 			5.Java自动装箱与拆箱

**装箱就是自动将基本数据类型转换为包装器类型（**int-->Integer）；调用方法：**Integer**的**valueOf(int)** **方法**

**拆箱就是自动将包装器类型转换为基本数据类型（Integer-->int）。调用方法：Integer的**intValue方法

## 6.重载和重写的区别

**重写**

 1.发生在父类与子类之间

 2.方法名，参数列表，返回类型（除过子类中方法的返回类型是父类中返回类型的子类）必须相同 。

3.访问修饰符的限制一定要大于被重写方法的访问修饰符（public>protected>default>private) 4.重写方法一定不能抛出新的检查异常或者比被重写方法申明更加宽泛的检查型异常。

**重载**

 1.重载Overload是一个类中多态性的一种表现 

 2.重载要求同名方法的参数列表不同(参数类型，参数个数甚至是参数顺序)

 3.重载的时候，返回值类型可以相同也可以不相同。无法以返回类型别作为重载函数的区分标准 

## 7.equals与==的区别

**==**

是一个比较运算符，基本数据类型比较的是值，引用数据类型比较的是地址值，即 比较的是变量(栈)内存中存放的对象的(堆)内存地址。

> [堆和栈的区别](https://blog.csdn.net/K346K346/article/details/80849966/)

**equals**

equals()是一个方法，只能比较引用数据类型。重写前比较的是地址值，重写后比一般是比较对象的属性，调用的却是==的判断。

## 8.Hashcode的作用



## 9.String StringBuffer 和 StringBuilder的区别是什么？



## 10.ArrayList和 LinkedList的区别



## 11.HashMap和HashTable的区别
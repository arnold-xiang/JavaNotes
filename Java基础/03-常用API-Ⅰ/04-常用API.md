# 一. Scaner类

功能：键盘输入数据

导包：

- 使用的目标类，和当前类位于同一个包，则可省略导包语句
- 只有java.lang包下的内容不需要导包，其他的包都需要import语句

```java
import java.util.Scanner;

public class Demo01Scaner {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println(sc.next());
        sc.close();
    }
}
```



## 匿名对象

只有右边的对象，没有左边的名字和赋值运算符

`new 类名称`

```java
new Person().name = "arnold";//第一个Person对象
new Person().showName();//第二个Person对象
```

每`new`一个就创建了一个对象，指向不同的内存地址

- 匿名对象只能使用唯一一次，下一次再用不得不再创建一个新对象
- 如果确定有一个对象只需要使用唯一的一次，就可以使用匿名对象

# 二. Random类

功能：产生随机数

```java
import java.util.Random;

public class Demo01Random {
    public static void main(String[] args) {
        Random r = new Random();
        System.out.println(r.nextInt()); //所有int类型范围的值，有正负
        System.out.println(r.nextInt(10000)); //范围为[0,bound)的随机数
    }
}
```

# 三. ArrayList类

## 对象数组

数组中存放的是地址值

`类名[] 数组名 = new 类名[数组长度]`

## 1. 和数组的区别

Arraylist集合的长度是可以随意变化的

## 2. 使用

- 对于Arraylist，有一个尖括号<E>代表泛型———`Arraylist<E>`
  - 泛型：装在集合内的所有元素全都是统一类型
  - 泛型只能是引用类型---有地址值，不能是基本类型---基本类型没有地址值
- 从`JDK1.7+`开始，**右侧**尖括号内可以不写内容，但<>还是要写的

注意事项：

- 对于Arraylist来说，直接打印得到的不是地址值，而是内容。若内容为空，则为[ ]

## 3. 常用方法

- 添加：`public boolean add(E e)`
- 获取：`public E get(int index)`
- 删除：`public E remove(int index)`

- 获取集合尺寸长度：`public int size()`

## 4. 基本类型使用Arraylist

必须使用基本类型对应的“包装类”

| 基本类型 | 包装类（引用类型，位于Java.lang包下） |
| -------- | ------------------------------------- |
| byte     | Byte                                  |
| short    | Short                                 |
| int      | Integer                               |
| long     | Long                                  |
| float    | Float                                 |
| double   | Double                                |
| char     | Character                             |
| boolean  | Boolean                               |

从`JDK1.5+`开始，支持自动装箱、自动拆箱

- 自动装箱：基本类型 ---> 包装类型
- 自动拆箱：包装类型 ---> 基本类型

# 四. String类

`String` 类代表字符串。Java 程序中的所有字符串字面值（如 `"abc"` ）都作为此类的实例实现。

即程序中所有双引号字符串，都是`String`类的对象（就算没有`new`，也是字符串对象）

## 1. 字符串的特点：

- 字符串是常量，字符串的内容永不可变
- 正式因为字符串内容不可改变，所以字符串可以共享使用
- 字符串效果上相当于是`char[]`型的字符串数组，但底层原理是`byte[]`字节型数组

## 2. 常见的3+1种创建方法

三种构造方法：

- `public String();`创建一个空白字符串，不含任何内容
- `public String(char[] array);`根据字符数组的内容，来创建对应的字符串
- `public String(byte[] array);`根据字节数组的内容，来创建对应的字符串

一种直接创建：

- `String str = “hello”;`

## 3. 字符串常量池

程序当中直接写上的双引号字符串，就在字符串常量池中-----**在堆中**。`String str = “hello”;`

池中的字符串可以重复利用---地址值相同

- 对于基本类型：`==`是进行数值的比较
- 对于引用类型：`==`时进行内存地址值的比较

## 4. 常用方法

- 字符串内容比较：
  - `public boolean equals(Object obj);`参数可以是任何对象
    - 如果与字符串常量比较：推荐把常量写在前面(避免出现空指针异常)--->`“hello".equals(str);`
  - `public boolean euqalsIgnoreCase(Object obj);`忽略大小写进行内容比较

- 获取字符串长度：
  - `public int length();`
- 字符串拼接：
  - `public String concat(String str);`
- 获取指定索引位置的单个字符：
  - `public char charAt(int index);`
- 查找参数字符串在本次字符串首次出现的索引值，没有返回-1：
  - `public int indexOf(String str);`
- 字符串的截取：
  - 截取从参数位置一直到字符串末尾：
    - `public String substring(int index);`
  - 截取指定位置字符串：
    - `public String substring(int begin,int end);`
- 字符串转换：
  - 将当前字符串拆分为字符数组作为返回值：
    - `public char[] toCharArray();`
  - 获得当前字符串底层的字节数组：
    - `public byte[] getBytes();`
  - 将所有老字符串替换成为新的字符串：
    - `public String replace(CharSequence oldString,CharSequence newString);`
    - `CharSequence `的意思就是可以接收字符串类型
- 字符串分割：
  - 按照参数规则，将字符串切割成若干字符串：`public String[] split(String regex);`
  - `regex`：参数是**正则表达式**

# 五. Static关键字

一旦使用`static`关键字，那么这个内容不再属于对象自己，而是属于类的，所以凡是本类的对象，都共享同一份

- 修饰成员变量：
  - 一旦使用`static`关键字，那么这个成员变量不再属于对象本身，而是属于类的。凡是本类的对象，都共享同一份数据

- 修饰方法：
  - 一旦使用`static`关键字，那么就成为了静态方法，静态方法不属于对象，而是属于类的
  - 对于静态方法，可以通过对象名进行调用，也可以直接通过类名称来调用（推荐）

无论是成员变量，还是成员方法，如果有了static关键字修饰，都推荐使用类名称进行调用

- 静态变量：类名称.静态变量
- 静态方法：类名称.静态方法
- 对于本类中的静态方法调用，可以省略类名称

注意事项：

- 静态不能直接访问非静态
  - 原因：在内存中，【先】有静态内容，【后】有非静态变量-----先不知后，后知道先
- 静态方法中不能使用`this`关键字
  - 原因：this代表当前对象

内存原理：

- `方法区`中有一个静态区，存放数据
- 通过类访问静态变量时，全程和对象没关系，只和类有关系

静态代码块：

- 当第一次用到本类时，静态代码块执行唯一的一次
- 静态内容总是优先于非静态，优先于构造方法执行
- 格式:

```java
public class 类名称{
    static {
        静态代码；
    }
}
```

- 典型用途：
  - 用来一次性的对静态成员变量进行赋值

# 六. Arrays类

`java.util.Arrays`是一个与数组相关的工具类，里面提供了大量的静态方法，用来实现数组的常见操作

- 将参数数组变成字符串：
  - `public static String toString(数组);`
- 默认按照升序对数组排序：
  - `public static void sort(数组);`
  - 如果是数值，默认按照升序排列
  - 如果是字符串，默认按照字母升序
  - 如果是自定义类型，那么这个自定义的类需要有`Comparable`或者`comparable`接口的支持

# 七. Math类

`java.util.Math`是一个与数学相关的工具类，里面提供了大量的静态方法，用来实现数学运算的常见操作

- 获取绝对值：
  - `public static double abs(double num);`
- 向上取整：3.1 ---> 4.0
  - `public static double ceil(double num);`
- 向下取整：3.9 ---> 3.0
  - `public static double floor(double num);`
- 四舍五入：
  - `public static long round(double num);`
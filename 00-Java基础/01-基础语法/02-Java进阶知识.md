# 一. IntelliJ IDEA

1.项目结构：

- 项目 - Project
  
  - 模块 - module
    
    - 包 - Package -- 命名建议：cn.arnold.day01.demo01

2.自动保存，无需手动保存

3.自动补全设置：File-Settings-keymap-Duplicate-Main Menu-Code-Completion-Basic

4.常用快捷键：

| 快捷键              | 功能                         |
| ---------------- | -------------------------- |
| `Alt+Enter`      | 导入包、自动修正代码                 |
| `Ctrl+Y`         | 删除光标所在的行                   |
| `Ctrl+D`         | 复制光标所在的内容，插入到光标位置下面        |
| `Ctrl+Alt+L`     | 格式化代码                      |
| `Ctrl+/`         | 单行注释，再按取消注释                |
| `Ctrl+Shift+/`   | 选中代码注释，多行注释，再按取消注释         |
| `Alt+Ins`        | 自动生成代码，toString，get，set等方法 |
| `Alt+Shift+上下箭头` | 移动当前代码行                    |

# 二. 方法

## 1. 定义

1.方法就是若干语句的功能集合

- 参数（原料）：进入方法的数据

- 返回值（产出物）：从方法出来的数据

2.格式：

```java
修饰符 返回值类型 方法名称（参数类型1 参数名称1，参数类型2 参数名称2，...）{
    方法体；
    return 返回值；
}
```

3.return作用：1.停止当前方法；2.将返回值还给调用处

## 2. 调用

1. 单独调用：`方法名称 (参数)`

2. 打印调用：`System.out.println(方法名称 (参数))`

3. 赋值调用：`数据类型 变量名称 = 方法名称 (参数)`

## 3. 注意事项

- 方法定义在类中，方法不能嵌套

- 定义后不会执行，调用后再执行

- 有返回值，必须写上`return`

- `return`的返回值类型必须与定义方法的类型一致

- 对于`void`方法，不能写return，只能return自己（结束方法的执行）

- 一个方法中可以有多个return语句，但要保证同时只有一个被执行到

## 4. 方法重载

1.重载：**Overload**

2.多个方法的名称一样，参数列表不同

- 参数个数不同

- 参数类型不同

- 参数的多类型顺序不同

3.与下列因素无关：

- 与修饰符无关

- 与参数名称无关

# 三. 数组

1.容器：可以同时存放多个数据值

2.特点：

- 数组是一种引用类型

- 数组中的数据类型必须统一

- 数组的长度在程序运行期间不可改变

## 1. 定义

1.标准格式：

- 动态初始化（指定长度）
  
  - `数据类型[] 数组名称 = new 数据类型[数组长度];`

- 静态初始化（指定内容）
  
  - `数据类型[] 数组名称 = new 数据类型[] {元素1,元素2,...};`

2.省略格式：

- 静态创建数组时使用省略格式
  
  - `数据类型[] 数组名称 = {元素1,元素2,...};`

3.注意事项：

- 虽然静态创建时没有指定长度，但也是有长度的
- 静态/动态初始化的标准格式（省略格式不行）可以拆分为两个步骤----先定义，再赋值

4.使用建议：

- 不确定数组的内容用动态初始化，否则用静态初始化

## 2. 访问

1.格式：

- `数组名称[索引值]`
  
  - 索引值从0开始，到【数组长度-1】

2.使用动态创建数组时：其中元素都有一个默认值

- 整型---0

- 浮点型---0.0

- 字符类型---'\u0000'

- 布尔类型---false

- 引用类型---null

3.注意事项：

- 静态初始化其实也有默认值的过程，只不过系统马上替换掉了

## 3. 内存

1.为了提高运算效率，对内存空间进行了不同空间的划分，每一片区域都有特定的处理数据方式和内存管理方式

- **栈（Stack）**：存放的都是方法中的局部变量，**方法的运行在栈中**
  
  - 局部变量：方法的参数，或者是方法{}内部的变量
  
  - 作用域：一旦超出作用域，立刻从栈内存中消失

- **堆（Heap**）：凡是`new`出来的东西，都在堆当中
  
  - 堆内存里的东西都有一个地址值：16进制
  
  - 堆内存中的数据，都有默认值。规则见上：

- **方法区（Method Area**）：存储`.class`文件相关信息，包含方法的信息（名称、返回值等）

- 本地方法栈：（Native Method Stack）：与操作系统相关

- 寄存器（pc Register）：与CPU相关

2.数组名称---地址 

## 4. 常见操作

### 1. 数组越界异常

`java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3`

### 2. 空指针异常

没有用`new`创建数组

`NullPointerException`

### 3. 数组遍历

1.获取数组长度：`数组名称.length`

2.IDEA中输入`数组名称.fori`能自动生成如下代码：

```java
for (int i = 0; i < array.length; i++) {
            System.out.println(array[i]);
        }
```

### 4. 数组反转

要求：不使用新数组

反转：

- 对称位置的元素交换
  - 索引值0---索引值length-1 交换
  - 索引值1---索引值length-2 交换
- 交换两个变量的值
  - `int temp = a;a = b; b = temp;`

- 停止交换
  - min = = max 或者 min > max
  - min < max 时应该交换

```java
for (int left = 0, right = arr.length - 1; left < right; left++, right--) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
        }
```

### 5. 数组作为参数和返回值

- 数组作为方法参数传递，传递的参数是数组的内存地址
- 数组作为方法的返回值，返回的是数组的内存地址

> 方法的参数为基本数据是，传递的是数据值；方法的参数为引用类型时，传递的是内存地址
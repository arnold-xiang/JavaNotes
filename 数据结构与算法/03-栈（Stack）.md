#  栈（Stack）

## 介绍

1. 栈的英文为（stack）
2. 栈是一个`先入后出`（FILO-First In Last Out）的有序列表。
3. 栈（stack）是限制线性表中元素的插入和删除只能在线性表的同一端进行的一种特殊线性表。允许插入和删除的一端，为变化的一端，称为栈顶（Top），另一端为固定的一端，称为栈底（Bottom）。
4. 根据栈的定义可知，最先放入栈中元素在栈底，最后放入的元素在栈顶，而删除元素刚好相反，最后放入的元素最先删除，最先放入的元素最后删除
5. 图解方式说明出栈（pop）和入栈（push）的概念

## 栈的应用场景

1. `子程序的调用`：在跳往子程序前，会先将下个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。
2. `处理递归调用`：和子程序的调用类似，只是除了储存下一个指令的地址外，也将参数、区域变量等数据存入堆栈中。
3. 表达式的转换[`中缀表达式转后缀表达式`]与求值（实际解决）。
4. `二叉树的遍历`。
5. 图形的`深度优先`（depth-first）搜索法。

## 栈的快速入门

1. 用数组模拟栈的使用，由于栈是一种有序列表，当然可以使用数组的结构来储存栈的数据内容，下面我们就用数组模拟栈的出栈，入栈等操作。
2. 实现思路分析，并画出示意图

![image-20200529161856190](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200529161856190.png)

> 1. 定义一个top表示栈顶，初始化为-1
> 2. 入栈：当有数据加入时
>    1. top++
>    2. stack[top]=value
> 3. 出栈
>    1. value=stack[top]
>    2. top--

3. 实现

```java
package cn.arnold.stack;

public class Stack {

    private int maxSize;    //数组（栈）的大小
    private int[] arrayStack;   //数组模拟栈
    private int top = -1;    //栈顶指针，初始化为-1

    // 构造器
    public Stack(int maxSize) {
        this.maxSize = maxSize;
        arrayStack = new int[this.maxSize];	// 初始化数组
    }

    // 栈满
    public boolean isFull() {
        return top == maxSize - 1;
    }

    // 栈空
    public boolean isEmpty() {
        return top == -1;
    }

    // 入栈
    public void push(int value) {
        if (isFull()) {
            System.out.println("栈满...");
            return;
        }
        top++;
        arrayStack[top] = value;
    }

    // 出栈
    public int pop() {
        int res = 0;
        if (isEmpty()) {
            throw new RuntimeException("栈空...");
        }
        res = arrayStack[top];
        top--;
        return res;
    }

    // 遍历
    public void showStack() {
        if (isEmpty()) {
            System.out.println("栈空...");
            return;
        }
        // 从栈顶开始显示数据
        for (int i = top; i >= 0; i--) {
            System.out.println("arrayStack[" + i + "]=" + arrayStack[i]);
        }
    }
}
```

## 中缀表达式转后缀表达式

- 中缀表达式：`8+(7-3)*2+8`

- 后缀表达式（逆波兰表达式）：`8 7 3 - 2 + 8 * +`

> 中缀表达式符合人们的习惯，但计算机底层不容易处理；后缀表达式适合计算式进行运算，但是人却不太容易写出来，尤其是表达式很长的情况下，因此在开发中，我们需要将中缀表达式转成后缀表达式

**具体步骤如下：**

1. 初始化两个栈：运算符栈s1和储存中间结果的栈s2；

2. 从左至右扫描中缀表达式；

   1. 遇到操作数时，将其压s2；

   2. 遇到运算符时，比较其与s1栈顶运算符的优先级：
      1.如果s1为空，或栈顶运算符为左括号"("，则直接将此运算符入栈；
      2.否则，若优先级比栈顶运算符的高，也将运算符压入s1；
      3.否则，将s1栈顶的运算符弹出并压入到s2中，再次转到（2.1）与s1中新的栈顶运算符相比较；

   3. 遇到括号时：
       1.如果是左括号"("，则直接压入s1
       2.如果是右括号")"，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃

   4. 重复步骤1至3，直到表达式的最右边


3. 将s1中剩余的运算符依次弹出并压入s2；

4. 依次弹出s2中的元素并输出，结果的逆序即为中缀表达式对应的后缀表达式。

- 将中缀表达式转为List：

```java
  public List<String> infixExptoList(String infix) {
        // 存放中缀表达式转化后的值
        List<String> list = new ArrayList<String>();
        int index = 0;    //遍历指针
        char c;         //每次遍历到的字符
        String str;     //用于拼接多位数
        /*do {
            // c是非数字则直接加入到list
            if ((c = infix.charAt(index)) < 48 || (c = infix.charAt(index)) > 57) {
                list.add("" + c);   // char转为String
                index++;
            } else { // c是数字则要考虑多位数
                str = "";
                while (index < infix.length() && (c = infix.charAt(index)) >= 48 && (c = infix.charAt(index)) <= 57) {
                    str += c; //拼接
                    index++;
                }
                list.add(str);
            }
        } while (index < infix.length());*/
        for (int i = 0; i < infix.length(); i++) {
            c = infix.charAt(i);
            // c是非数字则直接加入到list
            if (c < 48 || c > 57) {
                list.add("" + c);
            } else {
                // c是数字则要考虑多位数
                str = "";
                while (i < infix.length() && (c = infix.charAt(i)) >= 48 && (c = infix.charAt(i)) <= 57) {
                    str += c; //拼接
                    i++;
                }
                list.add(str);
            }
        }
        return list;
    }
```

- 中缀转后缀

```java
//即 ArrayList [1,+,(,(,2,+,3,),*,4,),-,5]  =》 ArrayList [1,2,3,+,4,*,+,5,–]
	//方法：将得到的中缀表达式对应的List => 后缀表达式对应的List
	public static List<String> parseSuffixExpreesionList(List<String> ls) {
		//定义两个栈
		Stack<String> s1 = new Stack<String>(); // 符号栈
		//说明：因为s2 这个栈，在整个转换过程中，没有pop操作，而且后面我们还需要逆序输出
		//因此比较麻烦，这里我们就不用 Stack<String> 直接使用 List<String> s2
		//Stack<String> s2 = new Stack<String>(); // 储存中间结果的栈s2
		List<String> s2 = new ArrayList<String>(); // 储存中间结果的Lists2
		
		//遍历ls
		for(String item: ls) {
			//如果是一个数，加入s2
			if(item.matches("\\d+")) {
				s2.add(item);
			} else if (item.equals("(")) {
				s1.push(item);
			} else if (item.equals(")")) {
				//如果是右括号“)”，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
				while(!s1.peek().equals("(")) {
					s2.add(s1.pop());
				}
				s1.pop();//!!! 将 ( 弹出 s1栈， 消除小括号
			} else {
				//当item的优先级小于等于s1栈顶运算符, 将s1栈顶的运算符弹出并加入到s2中，再次转到(4.1)与s1中新的栈顶运算符相比较
				//问题：我们缺少一个比较优先级高低的方法
				while(s1.size() != 0 && Operation.getValue(s1.peek()) >= Operation.getValue(item) ) {
					s2.add(s1.pop());
				}
				//还需要将item压入栈
				s1.push(item);
			}
		}
		
		//将s1中剩余的运算符依次弹出并加入s2
		while(s1.size() != 0) {
			s2.add(s1.pop());
		}

		return s2; //注意因为是存放到List, 因此按顺序输出就是对应的后缀表达式对应的List
		
	}
```

## 逆波兰计算器


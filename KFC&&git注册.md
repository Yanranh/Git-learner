# 2021.09.26 java学习小记 浙传oj KFC.Array问题



问题汇总：

Q1：如何快速替换字符？

Q2：如何找出最短队伍？->字符串长度最短

Q3:如何把字符串转换为字符数组？->具体到遍历字符进行相应的操作

方法：先整体，再局部





### 1.eclipse 全局搜索关键字替换： CTRL+H



### 2.String.toCharArray()

将字符串转为字符数组

public class Test {

  public static void main(String[] args) {

​    Scanner input = new Scanner(System.in);

​    String str = input.next();

​    char ss[] = str.toCharArray(); //利用toCharArray方法转换

​    for (int i = 0; i < ss.length; i++) {

​      System.out.println(ss[i]);

​    }

}

### 3.区分nextline()和next()

#### Scanner 类的 next() 方法

- 1、一定要读取到有效字符后才可以结束输入。
- 2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
- 3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
- **next() 不能得到带有空格的字符串。**





#### Scanner 类的 nextLine() 方法

- 1、以Enter为结束符,也就是说 nextLine()方法**返回的是输入回车之前的所有字符**。（因此可以被用来跳过回车符号）
- 2、可以获得空白。

#### Scanner 类的 nextInt() 方法

1.返回值是int类型的，以有效数字后的空格作为两个输入的数据的间隔
**2.next() 和nextLine()返回类型都是String**
Scanner 类的nextFloat()方法：返回值是float类型的，以有效数字后的空格作为两个输入的数据的间隔



#### java输入一个字符

import java.util.Scanner; 

Scanner scanner = new Scanner(System.in);

 char c = scanner.next().charAt(0);	





将git bash here添加进regedit中，使其在文件里右键即可找到相应操作指令~

# 学习笔记2021.9.27

# Reverse 

学习平台：BUUCTF

学习题目：XOR1

常规思路：



![system](https://i.loli.net/2021/10/12/pyklczDj4FWKSIT.png)

## 1.查壳

### 1.什么是壳？

**	专门负责保护软件不被非法修改或反编译的程序。**

像动植物的壳一般都是在身体外面，保护源程序不受侵害。

特点：先于程序运行，拿到控制权，然后完成它们保护软件的任务



查壳工具：万能脱壳工具

![tool1](https://i.loli.net/2021/10/12/islS7ZVB6mXtg4G.png)

**一、查壳功能：**

支持文件拖拽，目录拖拽，可设置右键对文件和目录的查壳功能，除了FFI自带壳库unpack.avd外，

还可以使用扩展壳库(必须命名为userdb.txt，此库格式兼容PEID库格式，

可以把自己收集的userdb.txt放入增强壳检测功能)。

注：如果是使用扩展库里特征查出的壳，在壳信息后面会有 * 标志。

**二、脱壳功能：**

如果在查壳后，Unpack按钮可用，则表示可以对当前处理文件进行脱壳处理，

采用**虚拟机脱壳**技术，您不必担心当前处理文件可能危害系统。

**三、PE编辑功能：**

本程序主界面可显示被检查的程序的入口点/入口点物理偏移，区段等信息，并且提供强大的编辑功能。

其中PE Section后按钮可以编辑当前文件的节表，点击后出现Sections Editor窗口。

#### 1.1

将文件夹下的xor文件拖拽进查壳工具，发现没有壳



![tool2](https://i.loli.net/2021/10/12/IPB3OEvMxDbUsgZ.png)

#### 1.2

用IDA打开XOR，按下F5对其进行反汇编，阅读代码





![code1](https://i.loli.net/2021/10/12/hTjapZnUyfQmCdF.png)

### 2.什么是异或？

参与运算的两个数，对应位置的数值相同，则结果为0，反之则为1

如

0^0=0     1^1=0           0^1=1     1^0=1

异或只能在数字间进行

#### 2.1函数strncmp()

##### 2.1.1函数声明

`int strncmp(const char *str1,const char *str2,size_t n)`

##### 2.1.2

str1：比较的字符串1

str2：比较的字符串2

n：要比较的最大字符数

##### 2.1.3比较

在前n字节中，两个字符串对应字符总的ASCLL码大小

##### 2.1.4结果

str1>str2,return x（x>0）

str1<str2,return y (y<0)

str1=str2,return 0



进入global

![global](https://i.loli.net/2021/10/12/Obd7QfKej2TiGqW.png)

## 2.用结果实现逆运算

 'f',0Ah              ; DATA XREF: __data:_global↓o
__cstring:0000000100000F6E                 db 'k',0Ch,'w&O.@',11h,'x',0Dh,'Z;U',11h,'p',19h,'F',1Fh,'v"M#D',0Eh,'g'
__cstring:0000000100000F6E                 db 6,'h',0Fh,'G2O',0
__//cstring:0000000100000F90     aInputYourFlag  db 'Input your flag:',0Ah

,0 

有效信息：'f',0Ah'k',0Ch,'w&O.@',11h,'x',0Dh,'Z;U',11h,'p',19h,'F',1Fh,'v"M#D',0Eh,'g' 'h',0Fh,'G2O',0
提炼字符串：

`'f',0xA,'k',0xC,'w&O.@',0x11,'x',0xD,'Z;U',0x11,'p',0x19,'F',0x1F,'v"M#D',0xE,'g'6'h',0xF,'G2O',0`

注意：’ ‘内的为值，其他要转换为ASCII码

提取已进行异或可读字符串：

{fkw&O.@xZ;UpFvM#DGhG2O}

将数值变量转化为ASCLL码

例：0Ch-->0xC

### 2.1逆运算实现语言：Python

Python3.x 

 range() 函数返回的结果是一个整数序列的对象



#### 函数认知

#####  range() 函数

可创建一个整数列表，一般用在 for 循环中

------

##### 语法

`range(start, stop[, step])`

##### 参数

start--->>从某数开始计数，默认0

stop--->>从某数结束，顾头不顾尾

step--->>步长

##### isinstance（）函数

```
isinstance(object, classinfo)
```

##### 参数

- object -- 实例对象。
- classinfo -- 可以是直接或间接类名、基本类型或者由它们组成的元组。

##### 返回值

如果对象的类型与参数二的类型（classinfo）相同则返回 True，否则返回 False。



##### Ord（）函数

ord() 函数是 chr() 函数（对于8位的ASCII字符串）或 unichr() 函数（对于Unicode对象）的配对函数，它以一个字符（长度为1的字符串）作为参数，返回对应的 ASCII 数值，或者 Unicode 数值，如果所给的 Unicode 字符超出了你的 Python 定义范围，则会引发一个 TypeError 的异常。

### 语法

以下是 ord() 方法的语法:

```
ord(c)
```

### 参数

- c -- 字符。

### 返回值

返回值是对应的十进制整数。



下面贴出逆运算代码：



```python
s=['f',0x0A,'k',0x0C,'w','&','O','.','@',0x11,'x',0x0D,'Z',';','U',0x11,'p',0x19,'F',0x1F,'v','"','M','#','D',0x0E,'g',6,'h',0xF,'G','2','O']
x='f'

for i in range(1,len(s)):
 if(isinstance(s[i],str)):
     if(isinstance(s[i-1],str)):
       x+=chr(ord(s[i])^ ord(s[i-1]))
     else:
       x+=chr(ord(s[i])^s[i-1])
 else:
     x+=chr(s[i]^ord(s[i-1]))

print(x)
```

s：初始化一个字符数组，界定，赋值

range：计算字符数组的有效长度

isinstance：判断元组里是否是字符

x+=chr(ord(s[i])^ord (s[i-1])):将前后两个字符转换为Unicode/AScii码，对相应数值进行异或运算，并以char类型返回字符值，组合在x变量中（含+）。（前文提到的异或逆运算）

else1：s[i-1]不是字符，则为数值，可以直接进行异或运算

else2：s[i]不是字符，同上



![flag](https://i.loli.net/2021/10/12/GXauEZLwJP35sWo.png)



得到flag{QianQiuWanDai_YiTongJiangHu}

# Matlab 基础入门

## 软件安装教程

教程链接：[点击下载资料]( https://pan.baidu.com/s/1_T_7Ga1QfrckvG1ikx4cPA?pwd=ge9g)

内涵安装包以及破解教程，如若和谐，[点击此处](https://enjoydown.com/home)查找相关资料



## 基础部分

### 页面结构

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820154154693.png" alt="image-20250820154154693" style="zoom: 200%;" />



如图所示可分为文件夹区，编辑器区，命令行窗口区和工作区

### 文件夹区

用来显示当前文件夹所包含的文件，可以在此处双击打开.m文件或其他文本文件 **.m文件是Matlab文件，点击右上角新建即可创建m文件** m文件内包含你所编写的代码脚本。

### 编辑器区

当你在文件管理器或者文件夹区打开m文件时，其内容会在编辑器区域中显示，在此处你可以随意更改代码内容。同时运行也可在编辑器中实现，在matlab中，你只需选中目标代码，右键选择在命令行窗口执行所选内容即可，如图 ![image-20250820155345243](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820155345243.png)	 

### 

### 命令行窗口区

执行的内容会在命令行窗口处呈现出来 ![image-20250820155605248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820155605248.png) 

### 工作区

显示运行中出现的变量以及函数 ![image-20250820155712470](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820155712470.png) 



## **语法部分**

### ；的用法

；作为每一句的结束语在matlab中是非必须的，你可以选择性加上，以下是它的两个基本作用

- 告诉编辑器是否打印结果。加了；表示不打印结果，不加表示需要在命令行窗口打印相应结果 ![image-20250820160442892](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820160442892.png)   

- 起到区分行的作用如数列[1 2 3;4 5 6]的结果为：![image-20250820160645427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820160645427.png) 



### 注释

在需要注释内容前加上%即可

快捷键 ctrl+R

取消 ctrl+T

#### clear与clc命令

clear：运行clear后，工作区的内容将被清空

clc ：运行clc后，命令行窗口内容将被清空

一般clear， clc会连着一起使用，如  clear；clc 。起到清空数据作用，防止对下一次运行产生干扰。



### 输入与输出函数

#### 输出函数 

disp函数，源自display缩写

用法：disp();分号可加可不加，括号内容可以是数字，字符或者字符串

数字：![image-20250820163052164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820163052164.png)    字符： ![image-20250820163240417](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820163240417.png)    字符串： ![image-20250820163419768](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820163419768.png)  



单引号括起的内容为字符，实际上是一个字符数组，其中每单个字符都是它的成员。

双引号括起的内容是字符串，可将其看作一个整体。



#### 向量的表示

中括号[]一般用来表示向量

其中的成员，可以是数字，字符甚至字符串。

要点：1.行向量用逗号 或者 空格 隔开；2.列向量用分号隔开 ；3.字符用单引号，字符串用双引号

​    ![image-20250820164500370](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820164500370.png)   ![image-20250820164535449](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820164535449.png)   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820164733574.png" alt="image-20250820164733574" style="zoom:80%;" /> 



##### []的另一个左右———拼接字符（串）

用法：['字符串1' '字符串2' '字符串3']   ![image-20250820165427588](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820165427588.png) 



注意只可使用单引号，不能使用双引号，输出的结果本质上仍然是字符数组。



#### strcat拼接函数

同样有拼接功能的还有strcat函数（是string catenate 的缩写）

用法：strcat(A,B,C,...)其中元素可以是字符，字符串（要加单/双引号），拼接的各部分用逗号隔开。

拼接内容不可以是数字，但你可以使用num2str函数将数字转换为字符

##### eg.  直接转化 ![image-20250820170814201](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820170814201.png)  保留四位有效数字（自动四舍五入）![image-20250820170934037](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820170934037.png)  

保留小数点后两位（自动四舍五入）![image-20250820173158859](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820173158859.png) *注意%.2f需要用单引号括起来*



#### input函数

用法 ： 变量名 = input('提示词')；

命令行会先打印出提示词，然后你在输入对应的数据，即可存入变量中。

加不加·分号只决定是否打印结果，对变量储存无影响。

- 无分号


![image-20250820174406710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820174406710.png)  ![image-20250820174418170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820174418170.png)  

- 有分号

![image-20250820174505328](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820174505328.png) ![image-20250820174520383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820174520383.png) 






























































































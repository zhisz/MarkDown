# 如何用pwlink  通过keil5 给stm32烧录



- 1.点击魔术棒

  ![image-20250820190843583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820190843583.png) 



- 2.选中CMSIS-DAP Debugger，然后再点击设置

 ![image-20250820191023380](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820191023380.png) 

- 插上烧录器，选择含有DAP的选项，port模式选择SW，最大时钟选择10MHz

  ![image-20250820191324462](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820191324462.png)  

- 点击flash download卡面，勾选住这四个选项，

  下面的flash要与你的芯片一致，否则点add进行添加，不要添加相同的flash，否则不能烧录

 ![image-20250820191726569](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250820191726569.png) 

- 最后一直点ok即可

- 编译通过的话就可以烧录了
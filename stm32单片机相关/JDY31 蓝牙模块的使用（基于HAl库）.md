# JDY31 蓝牙模块3.0的使用（基于HAl库）





## 教程来源

本教程 基于 [哔站kesking教学视频](https://www.bilibili.com/video/BV1114y1D7a4?vd_source=34568abd4a1cbed529b104f4439d1ef6) 编写，点击即可转到





## 配置cubeMX



### 打开所创立工程的ioc文件，进入MX 



### 配置串口

1，进入connectivity选项卡

2，选择USART1，2，3其中一个就行

3，选择模式为 Asynchronous 异步模式 （异步模式的特点是在进行任务等待时，可以同时进行其他任务的调度，而Synchronous 同步模式只有等待当前任务完成后才能进行下一个任务）

4，波特率选择9600，与我们的蓝牙默认配置一样

 ![image-20250830151033661](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830151033661.png) 





###  配置DMA



选择DMA Setting 选项卡，点击add添加该串口的RX和TX

 ![image-20250830152053566](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830152053566.png) 





### 配置串口中断



当我们接收到蓝牙模块给我们发的特定消息时，我们可以通过收/发中断函数触发并进入相应的中断回调函数，从而运行我们特定的指令。



选择NVIC选项卡， 在USART处开启全局中断。



 ![image-20250830152353172](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830152353172.png) 





至此，CubeMX配置完成，生成代码后在kail中，编译无误即可进行下一步。





## 代码编写

打开main.c 文件，

在Private variables私有变量中的User code中定义一个数组，数组名自拟，长度按需求设置，用于接收存储DMA的数据

 ![image-20250830154251781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830154251781.png) 

还是在main.c中，

在所有固件初始化之后，在USER CODE 中调用以下两个函数

### 串口不定长数据接收函数

```c
  HAL_UARTEx_ReceiveToIdle_DMA(&huart1,ReceiveDate,sizeof(ReceiveDate));

```

这是hal库中的串口不定长数据接收函数，ReceiveToIdle意为Receive until Idle（空闲） line detected（直到空闲被检测就接收（完成））

简单来说该函数的作用就是 实时监测 数据是否一直在发送，如果超出特定时间没有检测到数据，就判定这一条消息发送完成。

该函数的效果是，识别到一帧数据后立马通过DMA传给用户（也就是放在我们前面定义的ReceiveDate数组里面）



### 关闭DMA半传输完成中断

```c
__HAL_DMA_DISABLE_IT(&hdma_usart1_rx,DMA_IT_HT);
```

  **IT ** （Interrupt） 意为中断，传入的第二个参数为中断的类型，该函数的作用就是关闭相应中断

DMA_IT_HT （DMA半传输中断）中的HT 意为 Half Transfer（半传输），该中断的作用是 当dma一次传输的数据很大时，通过 **HT 中断**，你可以在数据到一半时就先处理一部分数据，等到 **TC（ Transfer Complete传输完成） 中断**  再处理剩余部分。

优势：单次数据量庞大时可以大大缩短dma数据处理时间，从而提高其他任务相应速度，使整个系统更加流畅。

而我们这里，dma传输的数据量很小，效果不明显，而且 DMA_IT_HT 毕竟也是一个中断，频繁调用 会占用 CPU（处理器）资源，

所以在这里我们就选择关掉就好（实测关不关效果都一样）



 ![image-20250830153201094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830153201094.png)  





### 重定义串口接收回调函数



```c
void HAL_UARTEx_RxEventCallback(UART_HandleTypeDef *huart, uint16_t Size)
{

	if( huart == &huart1)
	{
		HAL_UART_Transmit_DMA(huart,ReceiveDate,Size);
		
		HAL_UARTEx_ReceiveToIdle_DMA(&huart1,ReceiveDate,sizeof(ReceiveDate));
		__HAL_DMA_DISABLE_IT(&hdma_usart1_rx,DMA_IT_HT);
	}
}
```

HAL_UARTEx_RxEventCallback 串口接收回调函数，当idle（空闲中断）或 TC（传输完成中断）触发，该回调函数就会被调用，

从而处理接收到的数据。

这是  **HAL 库里一个弱定义（`__weak`）的回调函数** ，我们能够重写它

该弱定义函数可在uart.c中找到

 ![image-20250830164816614](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830164816614.png)  



#### 弱定义函数

在 C 语言里，如果一个函数被 **`__weak`** 修饰，就表示它是一个 **弱定义** 

在编译时，如果程序中没有别的同名函数，就使用这个弱定义，

反之，若用户重写了该函数（不改变名称），则会覆盖掉该弱定义函数



#### 重写函数



我们就简单写个函数，让蓝牙模块把接收到的消息原封不动发出来

```c
if( huart == &huart1)
	{
		HAL_UART_Transmit_DMA(huart,ReceiveDate,Size);
		
		HAL_UARTEx_ReceiveToIdle_DMA(&huart1,ReceiveDate,sizeof(ReceiveDate));
		__HAL_DMA_DISABLE_IT(&hdma_usart1_rx,DMA_IT_HT);
	}
```



首先判断一下是不是该串口发出来的，防止串口设置错误。

紧接着 调用 串口DMA发送函数（HAL_UART_Transmit_DMA），传入相应的串口，数据源和大小。

到这里还没结束，还需要 调用 我们 串口不定长数据接收 那里的 两个函数

因为 **DMA接收不会自动循环**，触发一次 Idle 或 Buffer Full（接收完成） 后，DMA 就算“完成”了。
这时候 DMA 已经停下来了，不会再接收新的数据。

也就是说 如果我们 不调用的话，我们就只能执行一次，不符合我们的预期。

重写完的函数放在Private user code（私有用户代码）处的user code即可

![image-20250830170919058](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250830170919058.png) 



### 编译，烧录

最后编译烧录即可



## 硬件连接与配置



### JDY3-1蓝牙模块资料

  [ 百度网盘地址]( https://pan.baidu.com/s/1Ee86UTVnpR5cvH9Pjk87RQ?pwd=qcfk) 

打开资料，有如下四个文件，pdf为用户手册，里面有详细的AT指令以及引脚配置信息，.exe文件为电脑串口调试程序，我们通过usb转串口设备将电脑与蓝牙模块相连接，电脑就能与蓝牙模块相通信。
.apk文件为手机端调试app，下载安装即可。此时，手机端作为主机 蓝牙模块 为从机，手机主动连接蓝牙模块 ，实现主从机互通信。

![image-20250831082516558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831082516558.png)  



蓝牙模块通常有两个模式：AT指令模式 和 透传模式

**AT指令模式** ：在该模式下 蓝牙与主机相连 ，主机通过串口 向 蓝牙模块 发送AT指令，用于查询或修改 蓝牙模块 的配置。

注意AT指令的结尾需要加上回车，具体用法 可查询 用户手册。

（以下是商家提供的串口调试助手，其他调试助手程序也是一样用的，但是注意要选对 串口号 和 波特率）

 ![image-20250831084534359](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831084534359.png) 



**透传模式** ： 当主机 设备连接到蓝牙模块后 ，进入 透传模式，此时蓝牙模块上的led由闪烁变为常亮， 且 无法 进入 AT指令模式。
在该模式下，主 从 机建立通信，进行数据传输。





### 接线



#### 蓝牙模块与usb转串口

如图，蓝牙模块只需要连接 RXD，TXD，VCC和GEN这四个引脚即可，其他两个暂时不用管。
RXD 连接 usb to ttl的 TXD， TXD 连接 usb to ttl的 RXD，VCC连接5v或直接连VCC都可，最后GND 连接 GND。

至此，两个设备就完成了连接。

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831090003861.png" alt="image-20250831090003861" style="zoom: 33%;" /> <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831090117241.png" alt="image-20250831090117241" style="zoom:33%;" />





#### 蓝牙模块与单片机



找到你单片机定义的串口的RX 和 TX引脚，与usb转串口一样， RX接TX ，TX接RX，然后VCC最好连接单片机的5v，GND连接GND，over。





## 程序现象



完成上述所有操作无误后（即程序编译下载，接线正确），我们手机端下载蓝牙调试app，单片机用电脑或者充电宝供电。



当蓝牙未连接时，此时蓝牙模块上的led处于闪烁 状态。

 <video src="C:\Users\Administrator\Documents\xwechat_files\wxid_od796bqzty3k12_014e\msg\video\2025-08\a4811ab1482494c4d94e2398449a2480.mp4"></video> 





打开手机端蓝牙调试app，找到 并连接 对应蓝牙模块，默认名称 为 JDY3-1 之类的，默认密码是1234，当然这些你都可以通过AT指令修改（注意蓝牙模块需要断开连接才能进入AT指令模式）



连接成功后，蓝牙模块指示灯常亮，我们通过手机端发送消息 来 观察 程序现象

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831092740631.png" alt="image-20250831092740631" style="zoom: 33%;" />





如下图，蓝牙模块接收消息并重新发送给我们的手机端，符合我们的预期。





<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250831092920060.png" alt="image-20250831092920060" style="zoom:33%;" />




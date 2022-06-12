# NRF52832-Libuarte（BLE_UART异步发送可变长数据包）
> Libuarte is a UARTE library that consists of the following layers:

* nrf_libuarte_drv – a low level UARTE driver, with extended functionality like a continuous counting of received bytes, double buffering, optional events for starting and stopping receiver, and an optional task that can be triggered when the receiver is started and stopped.
* nrf_libuarte_async – a library suitable for receiving and transmitting asynchronous packets. It manages the receive buffers and implements the receiver inactivity timeout. The library is using nrf_libuarte. It is meant to be used in a typical UART use case, in which the counterparty is asynchronously sending packets of variable length. In such case, user gets an event on packet boundary (that is, on timeout) and whenever the DMA buffer is full.

![](/Image/nRF_connect.jpg)  

### 更新记录
- 22.06.12
**要解决NRF_LIBUARTE_ASYNC_EVT_RX_DATA回调多次进入（超时，rx_buf满）的问题 ** 
开发板nrf52832的串口引脚改为P0.29和P0.12    
stm32F411测试过5ms发送一次，每次157字节，速度达到31400字节/s.  
NRF_LIBUARTE_ASYNC_DEFINE(libuarte, 0, 1, 2, NRF_LIBUARTE_PERIPHERAL_NOT_USED, 157, 3);   LIBUARTE的接收数组缓冲157要等与stm32发送的数据量大小，减少进入NRF_LIBUARTE_ASYNC_EVT_RX_DATA回调的次数。  

uart接收数据；  
NRF_LIBUARTE_ASYNC_EVT_RX_DATA接收完成中断，BLE_NUS_EVT_TX_RDY回调中  
queue-push的时候需要定义实际的数组  uint8_t m_data_array[6300];   

思路整理    
throughput_test() 建立定时器，CCCD使能时打开；蓝牙断开连接时关闭    
-> throughput_timer_handler() 定时中断函数，7ms进入一次    
-->模拟数据采集  
-->ble_data_send_with_queue()，在定时器中断和BLE_NUS_EVT_TX_RDY回调中，去查询queue是否为空，如果不空就把队列中的数据通过蓝牙发出去    
当“丢包”发生时，一般都是舍弃相关数据以让程序可以正常往下跑.  

- 22.06.11  
参考“手把手教你开发BLE数据透传应用程序”，测试了接近实际应用场景的代码，发送速度达到60K字节/s.  
测试过程中出现几次断开连接、卡顿的情况，调节手机屏幕常亮后排除.  
暂时屏蔽了DFU.  
修改蓝牙设备名称：SeerLikeJazz  

![](/Image/Speed.png)  

- 22.03.01  
基于examples\ble_peripheral\ble_app_uart的例程修改.  
加入了[Libuarte库](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_libuarte.html)，DFU（DFU的教程后面在做总结补充）.  
包含2个服务：  
Nordic UART Service  
Secure DFU Service  

### 项目说明
- **Firmware**：nordic52832的固件，Keil开发。
- **Hardware**：BLE原理图。使用立创EDA设计。主要是nrf52832的外围电路。
- **Image**：nRF Connect 的一些截图显示

### 使用教程
整理中……
## NRF52832-Libuarte（BLE_UART异步发送可变长数据包）
> Libuarte is a UARTE library that consists of the following layers:

* nrf_libuarte_drv – a low level UARTE driver, with extended functionality like a continuous counting of received bytes, double buffering, optional events for starting and stopping receiver, and an optional task that can be triggered when the receiver is started and stopped.
* nrf_libuarte_async – a library suitable for receiving and transmitting asynchronous packets. It manages the receive buffers and implements the receiver inactivity timeout. The library is using nrf_libuarte. It is meant to be used in a typical UART use case, in which the counterparty is asynchronously sending packets of variable length. In such case, user gets an event on packet boundary (that is, on timeout) and whenever the DMA buffer is full.

![Screenshot](/Image/Screenshot.jpg "nRF Connect Screenshot")

### 更新记录
22.03.01
基于examples\ble_peripheral\ble_app_uart的例程修改
加入了[Libuarte库](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_libuarte.html)，DFU（DFU的教程后面在做总结补充）。
包含2个服务：
Nordic UART Service
Secure DFU Service

### 项目说明
- **Firmware**：nordic52832的固件，Keil开发。
- **Hardware**：BLE原理图。使用立创EDA设计。主要是nrf52832的外围电路。
- **Image**：nRF Connect 的一些截图显示

### 使用教程
整理中……
# stm32F103ZETx之RT-Thread——USB串口
tags = #STM32 #RT-Thread #USB #Serial #串口 #通信

# emmm
使用armfly的stm32F103ZET6 V2开发板。

串口移植过程参考 https://blog.csdn.net/newbie_Blogger/article/details/92617242

使用中问题参考 https://blog.csdn.net/qq_27508477/article/details/103673793

移植可以参考bsp stm32f103-fire-arbitrary，都是stm32F103ZET6。

# Kconfig编辑
修改board/Kconfig，增加如下内容

```
    config BSP_USING_USBD
        bool "Enable USB device"
        select RT_USING_USB_DEVICE
        default n
        if BSP_USING_USBD
            config BSP_USB_CONNECT_PIN
                int "USB connect pin"
                default 30

            config BSP_USB_PULL_UP_STATUS
                int "USB PULL UP STATUS"
                default 0
        endif
```

其中 BSP_USB_CONNECT_PIN 为USB使能引脚，armfly原理图中PB14（30号引脚）为使能引脚，输出低电平使能。

BSP_USB_PULL_UP_STATUS为使能电平，因为低电平使能，所以是0。

# stm32cube
在cube中使能usb。

配置usb时钟。

# 编译下载
在console中使用list_device命令就能看到usb及vcp设备。

# FAQ
## 无法发送或收不到stm32发送的数据
串口助手打开DTR或者cdc_vcom.c中的_interface_handler中的CDC_SET_CONTROL_LINE_STATE中data->connected为1


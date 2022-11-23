# stm32 usb host
tags = #STM32 #USB #USBHost #android-accessory #arduino 
 #stm32 系列F105、F2XX、F4XX、F7XX都具有 #USBHost 接口。
 
 使用stm32cube配置，配置usb时钟为48mhz即可。
 
 ip配置上，可以配置支持大容量存储，也可以支持mtd等，或者都支持。
 
 # android accessory
 #android-accessory  是android作为usb device与外设连接的一种方式，外设需要具备usb host功能，切可以提供电源。
 
 这种使用方式由外设提供电源，android设备工作时，不会因为电量不够而无法工作。
 
 # arduino 支持 android accessory
 #arduino 使用USB Host Shield 也能够完成android accessory功能。
 代码在mega网盘，arduino_sketch_android_usb.zip 和 arduino_usb_libraries.zip。
 
 # stm32 支持 android accessory
 
 [stm32 anrdoid accessory](https://github.com/once2go/Stm32-Android-Accessory) 这个链接里面有讲解stm32cube如何配置，以及代码如何移植。
 
 已上传至mega网盘，Stm32-Android-Accessory-master.zip。
 
 实现代码在mega网盘，stm32f407_usb_host_android_accessory.zip。
 
 电路设计不能忘记D+ D- 的电阻。
 
 但是其中有个问题没有提及，在配置usb class时，需要把支持的class数量填多一些，否则就会出现android手机进入aoa模式，但是stm32却找不到对应的class。
 
 代码中有些许地方编写不是特别规范，需要手动调整。
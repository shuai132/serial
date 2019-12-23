# Serial Communication Library

对[wjwwood/serial](https://github.com/wjwwood/serial)进行优化

推荐使用这个仓库进行串口编程[SmartSerial](https://github.com/shuai132/SmartSerial)
在这个库的基础上提供了更多方便功能和线程安全支持

## Features

* 优化cmake文件 去除catkin
* 修复了一些编译警告和api限制
* 优化了异常继承体系 串口相关的异常均继承自SerialException

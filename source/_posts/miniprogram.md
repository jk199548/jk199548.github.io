---
title: 微信小程序
date: 2020-05-21 15:20:48
tags: css
---
## 微信小程序文字超出显示省略号
```
overflow : hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 1;
-webkit-box-orient: vertical;
word-break: break-all; 
```
## 使用微信蓝牙模块的一些注意事项
```
IOS里面蓝牙状态变化以后不能马上开始搜索，否则会搜索不到设备，必须要等待2秒以上
开启notify以后并不能马上发送消息，蓝牙设备有个准备的过程，需要在setTimeout中延迟1秒以上才能发送，否则会发送失败
搜索到设备后记得释放资源stopBluetoothDevicesDiscovery
不需要使用蓝牙的时候一定要关闭蓝牙.wx.openBluetoothAdapter(OBJECT)和wx.closeBluetoothAdapter(OBJECT)成对使用
Android蓝牙发送指令不能大于20字节,大于20字节需要分包发送
```

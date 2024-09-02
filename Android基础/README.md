# Android基础
## 四大组件
### Activity
用户的可视化界面
### Service
没有用户界面的后台活动，用户切换到其他应用，也会在后台保持活动
### Content Provider
将程序内部的数据对外部分享，其他应用可以通过ContentResolver类从内容提供者中获取或存入数据
### Broadcast Receiver广播
接收来自系统和应用中的广播的系统组件，例如开机了、锁屏了、电量不足、正在呼叫、收到短信等等
## 安卓启动流程
### 启动bootLoader
### 加载系统内核
### 启动init进程，进程号为1
init进程启动后，还会启动一些重要的守护进程。usbd进程、adbd进程、debuggerd进程、rild进程。
### 启动Zygote进程
* 初始化一个Dalvik VM虚拟机，最大程度复用自身。
### 启动Runtime进程
Runtime进程首先初始化服务管理器（Service Manager），并把它注册为绑定服务（Binder services）的默认上下文管理器，负责绑定服务的注册与查找。
### 启动本地服务
### 启动Home Laucher

## 安卓内存泄漏
https://juejin.cn/post/6844904067534159880

## 安卓隐私合规检测
https://juejin.cn/post/7213642622074273849

https://github.com/allenymt/PrivacySentry
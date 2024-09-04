# Android基础
## 四大组件
### Activity
用户的可视化界面
#### 启动模式
* standard 标准模式，始终创建新实例，并加在当前栈上
* singleTop 栈顶模式，和标准模式几乎一致，只是当要开启的activity已经处于task栈顶，系统不会创建新实例，而是直接复用栈顶

* singleTask 特点

1）默认情况下，开启singleTask的Activity并不会创建新任务，要配合android:taskAffinity来决定是否创建新任务。

2）在任务栈中先判断是否有Activity实例，若不存在，则直接在任务栈中创建Activity实例；若存在，则从任务栈中通过onNewIntent()激活该Activity实例，并将该实例上面的其他Activity实例给清空。

3）可以通过android:taskAffinity在另外一个应用中复用任务栈。

4）在Activity实例所在的任务栈中，该实例有且仅有一个。

* singleInstance 特点

1）独占一个任务栈，该任务栈中有且仅有该Activity实例

2）整个系统就只有一个实例。

3）被singleIntance的Activity启动的其他Activity，默认的在包名的任务栈中，如果配合android:taskAffinity，也可以新的任务栈中创建的实例。

4）被singleIntance的Activity启动的其他Activity，一定不在singleInstance的Activity所在的栈中，其他Activity的实例的创建和使用取决于该Activity设置的launchMode和android:taskAffinity

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
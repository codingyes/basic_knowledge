# 基础知识梳理
## Java基础
### 语法
#### 字节码的优势
#### 面向对象和面向过程
#### 对象相等和引用相等
* 对象相等：对象的内容是否相等
* 引用相等：地址是否相等
#### 构造方法的特点
* 没有返回值
* 名称和类一样
* 自动执行
#### 面向对象三大特征
* 封装：将对象的状态和属性，隐藏在对象内部，不允许外部直接访问
* 继承：不同的对象之间有相似点。用已经存在的类作为基础，通过继承的方式定义新类。
* 多态：对象具有多种状态，父类引用指向子类实例
#### 接口和抽象类
* 共同点：不可实例化；可以包含抽象方法。
* 区别：接口是行为约束，抽象类强调代码复用；可以实现多个接口，但只能继承一个抽象类；接口中的成员变量不能被修改且一定有初始值，抽象类的成员变量可以被子类重新定义和赋值。
#### 深拷贝、浅拷贝和引用拷贝
* 浅拷贝：创建一个新对象，但如果原对象内部存在引用类型，浅拷贝会直接复用引用地址
* 深拷贝：完全复制整个对象，包括原对象中所有的引用类型
* 引用拷贝：复用对象的地址，两个不同的引用指向同一个地址
#### String、StringBuffer、StringBuilder 的区别？
* 可变性：String是不可变的。
* 线程安全：String和StringBuffer是线程安全，StringBuilder是非线程安全
* 性能：StringBuilder效率高，String变化时产生新对象，StringBuffer对原对象操作
#### String 为什么是不可变的?
* 保存字符串数组被final修饰且是私有，且String类没有提供修改方法
* String类被final修饰，导致不可被继承，避免子类破坏
#### 字符串常量池
String s1 = new String("abc") 创建了1或2个对象。如果字符串常量池中不存在"abc"的引用，则会创建两个对象，一个放在字符串常量池中，一个创建后被s1指向；如果字符串常量池中存在"abc"的引用，则只会创建1个对象。

如果String s1 = "abc"，这样的方式创建String对象，则最多产生1个对象。如果字符串常量池中有引用，则不会产生新对象。

常量池的数据结构类似hashTable<字符串，String对象引用>，存放的是引用而不是对象本身。
#### 异常处理
* 顶层类Throwable
* 两个子类：Exception和Error
* Error不建议被try-catch，因为此时jvm大概率已经停止工作。常见Error有OOM、IOError
* Exception下面分为CheckedException和UnCheckedException
* CheckedException在编译期就会提示，不处理无法通过编译，例如IOException、ClassNotFound
* UnCheckedException是运行时异常，都是继承自RuntimeException，例如NPE、类型转换异常、入参类型错误
#### 反射
* 定义：运行时可以获取任意一个类的所有属性和方法，还可以调用这些属性和方法
* 优点：让代码更加灵活，为框架的开箱就用提供了便利
* 缺点：安全问题；性能差一些。
* 应用场景：动态代理；注解
#### 注解
解析方式
* 编译期扫描。比如重写检测
* 运行期通过反射处理
#### 静态代理和动态代理
代理模式：使用代理对象来代替对真实对象(real object)的访问，这样就可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。
* 静态代理：对目标对象的增强手动完成，接口一旦增加方法，目标对象和代理对象都要修改
* 动态代理：不需要针对每个目标类都单独创建一个代理类，并且也不需要我们必须实现接口，我们可以直接代理实现类。
#### 动态代理的机制

JDK动态代理：

* Proxy类的newProxyInstance()方法，用来生成一个代理对象。三个参数，loader是类加载器、interfaces是被代理类实现的一些接口、h是实现了 InvocationHandler 接口的对象。
* InvocationHandler 接口。使用代理类调用方法时，实际会调用InvocationHandler接口的invoke方法。invoke方法也有3个参数，proxy是动态生成的代理类、method 与代理对象调用的方法对应、args是当前方法的参数

JDK动态代理使用步骤：
* 定义一个接口及其实现类；
* 自定义 InvocationHandler 并重写invoke方法，在 invoke 方法中我们会调用原生方法（被代理类的方法）并自定义一些处理逻辑；
* 通过 Proxy.newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h) 方法创建代理对象 

JDK动态代理致命问题：只能代理实现了接口的类

CGLIB 动态代理：解决了JDK动态代理只能代理实现了接口的类

#### Java序列化
* 序列化：将数据结构或对象转为二进制字节流的过程
* 反序列化：二进制字节流转换为数据结构或对象的过程

### Java集合
#### ArrayList和LinkedList
区别：
* 都不是线程安全的
* 数据结构：ArrayList是Object数组，LinkedList底层数据结构是双向链表
* 内存占用：ArrayList会在尾部有一些空间浪费，而LinkedList是每一个元素都消耗更多空间，因为需要存放指针

ArrayList的扩容机制
* 无参创建ArrayList时，默认初始化一个空数组。当add第一个元素时，会扩容成10个元素
* 当添加到11个元素时，进行扩容，每次扩容到原来的1.5倍左右
#### HashMap
扩容：默认初始大小16，之后每次扩充，都变为原来是2倍

扩容条件：
* loadFactor 负载因子，控制数组存放数据的疏密程度，默认是0.75
* threshold = capacity * loadFactor，当size > threshold，就需要对hashMap的数组进行扩容。扩容时，由于数组的大小变化了，所以每一个元素对应的存放位置也变化了，需要重新排布每一个元素在hashMap中存放的位置，非常消耗性能

底层数据结构

JDK1.8之前
* 数组+链表。key的hashCode经过扰动函数处理得到hash值，然后根据(n-1) & hash得到数组位置。如果发生冲突，就用拉链法解决。
JDK1.8之后
* 主要是解决冲突的变化。当链表长度大于阈值（默认8），并且此时hashMap数组的大小大于等于64时，才会转换为红黑树加快查找效率。如果hashMap数组没有超过64，则仅仅是会对hashMap的数组进行扩容。

### Java并发
#### 进程和线程的关系、区别和优缺点
* 一个进程中包含多个线程，多个线程共享堆区和方法区，每个线程有自己的程序计数器、虚拟机栈和本地方法栈
* 进程是独立的，而线程可能会互相影响。线程开销小，但不利于资源的管理和保护
#### 创建线程的方式
* 继承Thread类
* 实现runnable接口
* 实现callable接口
* 使用线程池
#### 线程生命周期
* 初始状态，线程创建，但没有执行start
* 运行状态，调用了start等待运行
* 阻塞状态，需要等待锁释放
* 等待状态，需要等待其他线程的特定条件
* 超时等待状态，指定时间后自己返回，而不是一直等待条件
* 终止状态，线程运行完成
#### Thread#sleep和Object#wait
共同点：两者都可以暂停线程的执行

区别
* sleep没有释放锁，而wait释放了锁
* wait通常用于线程间通信，sleep用于暂停
* wait调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的notify。sleep会自动苏醒。
* sleep是Thread的静态本地方法，而wait是Object的本地方法
#### 为什么 wait() 方法不定义在 Thread 中？
wait() 是让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。每个对象（Object）都拥有对象锁，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要操作对应的对象（Object）而非当前的线程（Thread）。

而 sleep() 是让当前线程暂停执行，不涉及到对象类，也不需要获得对象锁。
#### 可以直接调用 Thread 类的 run 方法吗？
调用start后自动执行run方法，这是真正多线程工作。直接调用Thread的run方法，只会当做main线程下一个普通方法执行
#### 并行和并发
并行：两个以上的任务在同一时刻执行。

并发：两个以上的任务在同一时间段执行。
#### 线程安全和线程不安全
线程安全：对于同一份数据，不管有多少个线程同时访问，都能保证数据的正确性和一致性

线程不安全：对于同一份数据，多个线程同时访问时可能会导致数据混乱、错误或者丢失
#### 死锁
线程死锁：多个线程同时被阻塞，它们中一个或者全部，都在等待某个资源被释放。

产生死锁的必要条件：
* 互斥条件。某资源任意时刻只由一个线程占用
* 请求与保持。一个线程因为请求资源被阻塞，对于已经获得的资源不放
* 不剥夺条件。线程已经获得的资源，在未使用完成不可被其他线程抢占，只有自己使用完成才会释放
* 循环等待。若干线程形成一种头尾相连的循环等待资源关系
#### 预防和避免死锁
* 破坏请求与保持条件：一次性申请所有资源。
* 破坏不剥夺条件：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源
* 破坏循环等待条件：：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放
#### volatile关键字
* 保证变量的可见性。每次使用该变量，都会在主内存中进行读取。
* 禁止指令的重排序。以双重校验单例模式为例，在创建单例对象时，实际是三条指令完成。1. 为instance分配内存空间。2. 初始化instance。3. 将instance指向内存地址。由于指令重排的特点，三条指令的顺序可能是1->3->2，因此可能导致线程1分配内存、将instance指向内存地址后，此时线程2判断instance不为空直接返回，而instance实际还没有进行初始化。
#### synchronized 关键字
主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

使用synchronized
* 修饰实例方法：锁当前对象实例。
* 修饰静态方法：锁当前类，会作用到类的所有实例。
* 修饰代码块：synchronized(object)表示进行同步代码前要获取对象的锁；synchronized(类.class)表示进入同步代码前要获取当前class的锁。
#### volatile关键字和 synchronized 关键字的区别
* volatile是线程同步的轻量级实现，性能较好。但volatile关键词只能用于变量
* volatile只能保证数据的可见性，但不能保证原子性
* volatile多用于解决变量在多个线程见的可见性，而synchronized关键字解决的是多个线程之间访问资源的同步性
#### 乐观锁和悲观锁
悲观锁：假定共享资源每次被访问时都会出现问题，因此每次获取资源时都会上锁。这样，共享资源每次只给一个线程使用，其他线程阻塞，用完后再把资源转让给其他线程。synchronized和ReentrantLock就是这种思想体现。用于写场景较多。

乐观锁：假定在最好的情况下，每次访问共享资源都不会有问题。因此，只在提交修改的时候去验证是否被修改过（版本号机制和CAS）。例如，AtomicInteger等原子变量类，就是CAS实现。用于读场景较多。
#### 公平锁和非公平锁
公平锁：锁被释放后，先申请的线程先得到锁，保证绝对的顺序。

非公平锁：锁被释放后，是随机或按照其他优先级获取锁
### JVM

## Android基础
### 四大组件
#### Activity
用户的可视化界面
#### Service
没有用户界面的后台活动，用户切换到其他应用，也会在后台保持活动
#### Content Provider
将程序内部的数据对外部分享，其他应用可以通过ContentResolver类从内容提供者中获取或存入数据
#### Broadcast Receiver广播
接收来自系统和应用中的广播的系统组件，例如开机了、锁屏了、电量不足、正在呼叫、收到短信等等
### 安卓启动流程
#### 启动bootLoader
#### 加载系统内核
#### 启动init进程，进程号为1
init进程启动后，还会启动一些重要的守护进程。usbd进程、adbd进程、debuggerd进程、rild进程。
#### 启动Zygote进程
* 初始化一个Dalvik VM虚拟机，最大程度复用自身。
#### 启动Runtime进程
Runtime进程首先初始化服务管理器（Service Manager），并把它注册为绑定服务（Binder services）的默认上下文管理器，负责绑定服务的注册与查找。
#### 启动本地服务
#### 启动Home Laucher
## 计算机基础
### 网络
#### TCP/IP四层模型
* 应用层
* 传输层
* 网络层
* 网络接口层
#### 应用层协议
* HTTP协议
* SMTP简单邮件发送协议
* FTP文件传输协议
* SSH安全的网络传输协议
* DNS域名管理系统
#### HTTP协议
输入URL到页面展示的过程？
* 通过DNS解析，找到对应的IP
* 根据IP地址和端口号，向目标服务发起TCP连接请求
* TCP连接后，客户端向服务器发送一个HTTP请求报文
* 服务器收到HTTP请求报文后，处理请求，并返回HTTP响应报文给客户端
* 客户端收到响应报文后，解析HTTP报文内容
#### HTTP协议状态码
* 1XX：正在处理请求
* 2XX：请求处理完毕
* 3XX：需要附加操作才能完成请求
* 4XX：服务器无法处理请求，请求不存在，客户端传参错误等
* 5XX：服务器内部错误，处理出错
#### Ping
作用：常用的网络诊断工具，经常用来测试网络中主机之间的连通性和网络延迟。

工作原理：基于网络层的ICMP协议，主要原理就是通过在网络上发送和接收 ICMP 报文实现的。
#### DNS
作用：解决域名和IP的映射问题

DNS服务器层级
* 根DNS服务器
* 顶级域DNS服务器
* 权威DNS服务器
* 本地DNS服务器

DNS劫持：一种网络攻击，它通过修改 DNS 服务器的解析结果，使用户访问的域名指向错误的 IP 地址，从而导致用户无法访问正常的网站，或者被引导到恶意的网站。
#### TCP与UDP
区别
* TCP面向连接，UDP不面向连接
* UDP不保证数据可靠交互，TCP提供可靠的服务
* TCP有状态，UDP无状态
* 传输效率：TCP传输时多了连接、确认、重传机制，导致TCP的效率要低不少
* 传输形式：TCP是面向字节流，而UDP是面向报文的

应用场景
* UDP用于即时传输
* TCP用于准确传输
#### TCP的三次握手和四次挥手
三次握手
* 一次握手：客户端发送SYN（SEQ=x）标志数据包，发送到服务端，客户端进入SYN_SENT状态
* 二次握手：服务端发送带有SYN+ACK（SEQ=y，ACK=x+1）的数据包到客户端，服务端进入SYN_RECV状态
* 三次握手：客户端发送带有ACK(ACK=y+1)数据包到服务端，客户端和服务端都进入ESTABLISHED状态，完成三次握手

三次握手的目的
* 第一次握手，客户端什么也无法确定，服务端只能确认自己接收正常、对方发送正常。
* 第二次握手，客户端确认了自己发送正常、接收正常，对方发送、接收正常。服务端确认了，对方发送正常、自己接收正常。
* 第三次握手，客户端确认了，自己发送、接收正常，对方发送、接收正常。服务端确认了，自己发送、接收正常，对方发送、接收正常。

为什么握手是三次，而不是两次或者四次？
* 两次不安全：两次服务端不能确定对方的接收是否正常
* 四次没必要

四次挥手
* 第一次挥手：客户端发送一个FIN（SEQ=x）给服务端，然后客户端进入FIN-WAIT-1状态
* 第二次挥手：服务端收到FIN后，发送一个ACK（ACK=x+1）给客户端。服务端进入CLOSE-WAIT状态，客户端进入FIN-WAIT-2状态。
* 第三次挥手：服务端发出一个FIN（SEQ=y）给客户端，服务端进入LAST-ACK状态
* 第四次挥手：客户端发送ACK（ACK=y+1）给服务端，然后客户端进入TIME-WAIT状态，服务端进入CLOSE状态。此时，客户端等待2MSL后依然没有收到回复，就证明服务端正常关闭了。

不能将第二次挥手和第三次挥手结合起来，因为服务端可能还有一些数据没有发完，需要等发完后再发FIN。
#### TCP如何保证传输可靠性
* 基于数据块传输
* 对失序的包进行重排和去重
* 校验和
* 重传机制
* 流量控制：滑动窗口
* 拥塞控制
### 操作系统
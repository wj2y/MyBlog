---
title: JMeter的基本使用与性能测试，完整入门篇保姆式教程
published: 2025-07-24
description: 'JMeter压力测试基本使用'
image: 'https://img.quenjoy.com/blog/1753695971952-4ed84a78-d867-40b6-bb0d-599c5665c425.png'
tags: [JMeter]
category: 'BackEnd'
draft: false 
lang: ''
---
转载文章：[JMeter的基本使用与性能测试，完整入门篇保姆式教程](https://blog.csdn.net/Zachyy/article/details/139717444?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522172317400316800188515781%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=172317400316800188515781&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-139717444-null-null.142^v100^control&utm_term=jemter%E4%BD%BF%E7%94%A8&spm=1018.2226.3001.4187)

### Jmeter 的简介

JMeter 是一个纯 Java 编写的开源软件，主要用于进行**性能测试**和**功能测试**。它支持测试的应用 / 服务 / 协议包括 Web (HTTP, HTTPS)、SOAP/REST Webservices、FTP、Database via JDBC 等。我们最常使用的是 HTTP 和 HTTPS 协议。

#### Jmeter 主要组件

1.  **线程组**（Thread Group）：

    *   线程数（Number of Threads）：模拟的用户数量。

    *   Ramp-Up 时间（Ramp-Up Period）：达到指定线程数所需要的时间。例如，线程数为 100，Ramp-Up 时间为 5 秒，则每秒启动 20 个线程。

    *   循环次数（Loop Count）：测试循环的次数。

2.  **取样器**（Sampler）：

    *   用于向服务器发送请求，如 HTTP 请求、JDBC 请求等。

    *   在线程组下添加取样器，配置请求的 URL、方法、参数等。

3.  **逻辑控制器**（Logical Controller）：

    *   用于控制测试的执行逻辑，如循环、条件判断等。

    *   常见的逻辑控制器有 If 控制器、循环控制器等。

4.  **前置处理器**（PreProcessor）和**后置处理器**（PostProcessor）：

    *   前置处理器在请求发送前执行操作，如设置请求头、生成数据等。
    *   后置处理器在请求发送后执行操作，如处理响应数据、提取需要的值等。
5.  **断言**（Assertion）：

    *   用于验证响应是否符合预期。
    *   可以使用正则表达式、XPath、JSON Path 等方式进行断言。
6.  **监听器**（Listener）：

    *   用于展示测试结果，如查看结果树、聚合报告等。

    *   在测试计划或线程组下添加监听器，以查看和分析测试结果。

#### Jmeter 高级功能

1.  **参数化**：

    *   使用 CSV Data Set Config 等方式实现参数化，方便进行批量测试。
    *   在测试计划中配置 CSV 文件路径和变量名，然后在取样器中使用这些变量。
2.  **定时器**（Timer）：

    *   用于控制请求的发送频率。
    *   在线程组或取样器下添加定时器，配置延迟时间和执行时间等参数。
3.  **分布式测试**：

    *   通过配置多个 Jmeter 实例，实现分布式测试，提高测试效率。

    *   需要设置主节点和从节点，以及配置相关的测试计划和数据文件等。

### 一、Jmeter 的安装、配置、搭建

#### 1.1 Java 环境配置

由于 Jemter 是基于 java 语言开发的，所以使用 Jemter 需要安装 JDK。（Jemter5.0 版本要求 JDK 版本为 1.8 或 1.9，一般安装 **JDK1.8**）。

*   JDK 1.8 安装地址：[JDK 1.8 官网下载](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

*   Java 环境变量配置：[环境变量配置](https://jingyan.baidu.com/article/597035529700548fc0074039.html)

#### 1.2 Jmeter 下载

*   最新版 Jemter 下载路径：[Jmeter 官网下载](http://jmeter.apache.org/download_jmeter.cgi)

*   其他版本 Jmeter 下载路径：[Jmeter 二进制 bin 文件下载](https://www.apache.org/dist/jmeter/binaries)

![](https://img-blog.csdnimg.cn/direct/385d85cb63444d488bf29b756137ad74.png)

Jmeter 下载

#### 1.3 Jmeter 安装及环境变量配置

*   下载安装包后，解压安装 Jmeter 即可

*   环境变量配置

    *   找到 Jmeter 下载后的文件所在目录  
        ![](https://img-blog.csdnimg.cn/direct/dd593695363d44039c8a1af1c6af0798.png)
    *   在环境变量中配置 **JMETER_HOME**：  
        ![](https://img-blog.csdnimg.cn/direct/ae5e28de186348b9ba86bd31fedd4a4d.png)

![](https://img-blog.csdnimg.cn/direct/8bcd455d288947489f94942378b20648.png)

*   Path：**%JMETER_HOME%\bin**

![](https://img-blog.csdnimg.cn/direct/54851ef862644695a1426437d2bf998d.png)

*   查看环境变量是否配置成功
    *   运行 -> `cmd-> jmeter -v` 命令查看是否能查看到 Jmeter 版本信息
    *   运行 -> `cmd->jmeter` 命令查看是否能启动 jmeter

![](https://img-blog.csdnimg.cn/direct/3d27abfa07924c1c94aa737f13e6e0bf.png)

#### 1.4 启动 Jmeter

进入 `apache-jmeter-5.6.3/bin` 文件夹，点击 `jmeter.bat` 文件，运行 JMeter

![](https://img-blog.csdnimg.cn/direct/0b90e0273c2e47c5b8b5d0e7fac06ca8.png)

*   或在 cmd 中输入 jmeter 指令打开 Jmeter：

![](https://img-blog.csdnimg.cn/direct/8f246db3e943475b9f9633833c621519.png)

*   **注意**：不管用使用哪一种方式打开，都会打开一个 cmd 窗口。如果关闭这个 cmd 窗口，打开的 jmeter 也会被关闭。  
    ![](https://img-blog.csdnimg.cn/direct/b6af353e781c45428541eccbc0b73471.png)

初始界面如下：

![](https://img-blog.csdnimg.cn/direct/7e1c54bbcfe343f7b60f792c9ab9d00a.png)

#### 1.5 Jemter 切换中文环境

在上方导航栏，选择 `Options -> Choose Language -> Chinese(Simplified)`，将语言切换为中文  
![](https://img-blog.csdnimg.cn/direct/4370c5fdd02d4d25becf9336ff550923.png)  
更换语言过后的界面如下：

![](https://img-blog.csdnimg.cn/direct/925965d7b8a64d0d99b0eeba86fd5e07.png)

### 二、Jmeter 的基本使用

#### Jmeter 常用按钮

![](https://img-blog.csdnimg.cn/direct/4cb85739e29a4e2a8c6c4489bfdf6b02.png)

#### 2.1 JMeter 脚本编写

首先介绍 Jemter 基本使用，这里以一个 `www.baidu.com` 为例，来进行基本脚本编写。

##### 2.1.1 添加线程组

`右键点击“Test Plan” -> 添加 -> 线程（用户） -> 线程组`，可添加测试需要的线程组  
![](https://img-blog.csdnimg.cn/direct/a89382f619b84d0f9070038f747c86e3.png)  
线程组可配置线程组名称、注释、线程数、Ramp-up 时间、循环次数、调度器等参数

![](https://img-blog.csdnimg.cn/direct/7e0fcd83a0b443b5a58f57b4c3c7d666.png)  
**参数解释**：

*   **线程数**：虚拟用户数。一个虚拟用户占用一个进程或线程。设置多少虚拟用户数就是设置多少个线程数。

*   **Ramp-Up Period(in seconds) 准备时长**：设置的虚拟用户数需要多长时间全部启动。如果线程数为 5，准备时长为 1，那么需要 1 秒钟启动 5 个线程，也就是每秒钟启动 5 个线程。

*   **循环次数**：每个线程发送请求的次数。如果线程数为 100，循环次数为 10，那么每个线程发送 10 次请求。总请求数为 100*10=1000 。若勾选 “永远”，则所有线程会一直发送请求，直到选择停止运行脚本。

*   **Same user on each iteration**：用于控制每次迭代是否使用相同的线程（即用户）。当该参数被勾选时，JMeter 在每次迭代时都会使用相同的线程来模拟用户行为。在连续的请求中，会保持相同的用户身份（如会话、Cookie 等）。

*   **调度器**：设置线程组启动的开始时间和结束时间 (配置调度器时，需勾选循环次数为永远)

    *   启动延迟（秒）：测试延迟的启动时间
    *   持续时间（秒）：测试持续的时间

##### 2.1.2 添加 HTTP 请求

JMeter 的 HTTP 请求是性能测试中常用的功能，用于模拟用户向服务器发送 HTTP 请求并获取响应。

`右键点击线程组 -> 添加 -> 取样器 -> HTTP请求`，添加一个 HTTP 请求  
![](https://img-blog.csdnimg.cn/direct/fd74331345d040ed9fb9ac0470bf3e90.png)  
对本地接口进行性能测试，参考下图进行配置

![image-20240809144041221](https://img.quenjoy.com/202408/image-20240809144041221-be86fa45d881407e9e4ae51f68b63d76.png)  

参数解释：

*   **Web 服务器**

    *   协议：向目标服务器发送 HTTP 请求协议，可以是 HTTP 或 HTTPS，默认为 HTTP
    *   服务器名称或 IP ：HTTP 请求发送的目标服务器名称或 IP
    *   端口号：目标服务器的端口号，默认值为 80
*   **Http 请求**

    *   方法：发送 HTTP 请求的方法，可用方法包括 GET、POST、HEAD、PUT、DELETE 等。
    *   路径：目标 URL 路径（URL 中去掉服务器地址、端口及参数后剩余部分）
    *   内容编码：编码方式，默认为 ISO-8859-1 编码，这里配置为 utf-8
*   **同请求一起发送参数** ：在请求中发送的 URL 参数，可以将 URL 中所有参数设置在本表中，表中每行为一个参数（对应 URL 中的 name=value），参数传入中文时需要勾选 “编码”

##### 2.1.3 添加查看结果树

JMeter 的结果查看树用于查看和分析 HTTP 请求的响应结果。

`右键点击线程组 -> 添加 -> 监听器 -> 查看结果树`，添加一个查看结果树

![](https://img-blog.csdnimg.cn/direct/9e7b6e9d82074448b4a15edbf6512414.png)  
将查找下方的响应数据格式改为 `HTML Source Formatted`，点击上方的绿色三角按钮，运行 http 请求

![](https://img-blog.csdnimg.cn/direct/71f74aceceb3476fadcc4304403223a1.png)  
运行结果如下：

取样器结果  
![](https://img-blog.csdnimg.cn/direct/c7ea4705b2394feaa25035ce36aa01de.png)

本次搜索返回结果页面标题为 “JMeter 性能测试_百度搜索”，与之前设置的发送参数相吻合。

![](https://img-blog.csdnimg.cn/direct/642339fd0c8c4992b1674bda29cbda36.png)

##### 2.1.4 添加聚合报告

聚合报告是 JMeter 中用于汇总和分析测试结果的工具。它提供了关于测试运行的各种性能指标，如响应时间、吞吐量、错误率等。

`右键点击 线程组 -> 添加 -> 监听器 -> 聚合报告`，用以存放性能测试报告  
![](https://img-blog.csdnimg.cn/direct/a964d3719a514cf58ea2f1200b6908d7.png)

##### 2.1.5 添加用户自定义变量

用户自定义变量作为存储和管理测试期间需要的值，这些变量可以在测试计划中的任何地方引用。

`右键点击 线程组 -> 添加 -> 配置元件 -> 用户定义的变量`，以添加用户自定义变量  
![](https://img-blog.csdnimg.cn/direct/81cdfd16b7b14e66b33d5a9a1de30e94.png)  
添加一个参数 `wd`，用于**存放搜索词**

![](https://img-blog.csdnimg.cn/direct/911744add9364b5eb09e40632623b3b8.png)  
在 HTTP 请求中使用该参数，格式为 `${变量名称}`，即 `${wd}`

![](https://img-blog.csdnimg.cn/direct/7db5a52eb9504a679077b3f78962f8a8.png)

##### 2.1.6 添加断言

JMeter 中，断言用于验证测试结果是否符合预期。

在 HTTP 请求中添加响应断言：`右键点击 HTTP请求 -> 添加 -> 断言 -> 响应断言`  
![](https://img-blog.csdnimg.cn/direct/c1a5aefcb1d346d3b101076f3800d34e.png)

需要校验返回的文本中是否包含搜索词，添加参数 `${wd}` 到要测试的模式中

![](https://img-blog.csdnimg.cn/direct/ceb1a43bdbdc47559ed7a4c8d9374bd2.png)

##### 2.1.7 添加断言结果

为查看断言的结果，在 HTTP 请求中添加断言结果：`右键点击 HTTP请求 -> 添加 -> 监听器 -> 断言结果`

![](https://img-blog.csdnimg.cn/direct/4b22fd731f0547e49380936c3e2d926a.png)  
点击上方的绿色三角形按钮，即可运行并查看断言运行结果。

![](https://img-blog.csdnimg.cn/direct/fec607c8e8064c5fbf5419f3eee62477.png)

#### 2.2 执行性能测试

##### 2.2.1 配置线程组

点击线程组，配置本次性能测试相关参数：线程数，Ramp-Up 时间，循环次数等参数。

![](https://img-blog.csdnimg.cn/direct/6da5e0ad6e284b22b1d3dcc3e3cecde2.png)  
这里我们配置线程数为 20，Ramp-Up 时间为 5 秒，循环次数为 1 次。

##### 2.2.2 执行测试

进入聚合报告，进行测试。测试之前需要点击上方的扫把按钮清楚之前的调试结果。

![](https://img-blog.csdnimg.cn/direct/c24c4399a7c04d2db37c5d9fabb0488c.png)  
点击上方的绿色按钮，即可运行测试，性能测试执行完成后，可通过聚合报告看到测试结果。  

![image-20240809143910915](https://img.quenjoy.com/202408/image-20240809143910915-5d2d81fca7f54d1387838a805376f20f.png)

一般情况下，性能测试中需要重点关注的数据有**请求数**、**平均响应时间**、**最小响应时间**、**最大响应时间**、**吞吐量**和**错误率**

*   参数说明：
    *   **Label**：每个 JMeter 的元素（如 HTTP 请求）都有一个 Name 属性，Label 显示的就是 Name 属性的值

    *   **#样本**：请求数，表示这次测试中一共发出了多少个请求。如模拟 100 个线程数（即 100 个用户），每个线程迭代 10 次，这里就显示 100*10 = 1000

    *   **平均值**：平均响应时间，默认情况下是单个请求的平均响应时间。

    *   **中位数**：50％ 用户的响应时间

    *   **90%/ 95%/ 99% 百分位**：90%/ 95%/ 99% 用户的响应时间

    *   **最小值**：最小响应时间

    *   **最大值**：最大响应时间

    *   **异常 %**：请求错误率，即错误请求数 / 请求总数

    *   **吞吐量**：——默认情况下表示每秒完成的请求数（Request per Second）

    *   **接收 KB/Sec**：每秒从服务器端接收到的数据量

    *   **发送 KB/Sec**：每秒发送到服务器端的数据量

以上，就是使用测试工具 JMeter 对 Web 应用程序进行性能测试的流程。

### 三、注意事项

*   **关闭不必要的监听器**：在测试过程中，如果不需要实时查看结果，可以关闭不必要的监听器，以减少资源消耗。

*   **合理使用断言**：过多的断言会增加测试的复杂性，应根据实际需求合理使用断言。

*   **优化测试计划**：在测试过程中，应不断优化测试计划，如调整线程数、循环次数等参数，以达到最佳的测试效果。

*   **注意测试结果的分析**：通过聚合报告等结果分析工具，对测试结果进行深入分析，找出潜在的性能问题。

---



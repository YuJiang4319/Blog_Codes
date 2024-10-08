---
title: Modbus
date: 2024-08-20 18:30:38
tags:
  - Modbus
categories:	
  - Embedded
description: 

---

# Modbus

Modbus是一种用于工业自动化和监控系统的通信协议，最初由施耐德电气（Schneider Electric）在1979年开发。它是一种开放的协议，广泛应用于工业控制系统、可编程逻辑控制器（PLC）、人机界面（HMI）、仪表和传感器之间的通信。Modbus的设计简单且易于实现，特别适合在多种工业环境中进行数据交换。

### 一、Modbus协议的基本概念

Modbus是一种**主从（Master-Slave）**通信协议，通常用于串行通信。它有三种常见的通信方式：**Modbus RTU**、**Modbus ASCII** 和 **Modbus TCP**。

1. **Modbus RTU（Remote Terminal Unit）**：
   - 这是最常见的Modbus通信方式，使用二进制编码的帧结构，数据紧凑，通信效率高。
   - 通信介质一般为RS232、RS485或RS422。
   - **帧结构**：Modbus RTU使用CRC（循环冗余校验）进行错误检测，帧由设备地址、功能码、数据和校验码组成。
   - 典型应用：PLC、变频器、传感器等设备之间的数据传输。

2. **Modbus ASCII**：
   - Modbus ASCII使用ASCII字符编码，相比RTU更易于调试，但效率较低。
   - 同样支持RS232、RS485等串行通信方式。
   - **帧结构**：Modbus ASCII使用LRC（纵向冗余校验）来检测错误，帧由冒号（“:”）开始，以换行符结束。

3. **Modbus TCP/IP**：
   - Modbus TCP/IP是基于以太网的通信方式，数据帧通过TCP/IP网络传输。
   - **帧结构**：Modbus TCP/IP去除了RTU或ASCII中的错误检测机制（如CRC或LRC），因为TCP/IP本身具备错误检测和纠错能力。
   - 典型应用：工厂网络系统中的PLC、工业电脑、HMI和SCADA系统。

### 二、Modbus的基本通信结构

Modbus通信采用**主从架构**，即主设备发起通信，从设备做出响应。通常有一个主设备和多个从设备，每个从设备有唯一的设备地址。

#### 1. **主设备**：
   - 主设备控制通信过程，发送请求帧给从设备，读取或写入数据。
   - 典型主设备包括：SCADA系统、HMI、工业电脑或PLC。

#### 2. **从设备**：
   - 从设备在接收到主设备的请求后进行处理，并返回相应的数据。
   - 每个从设备都有一个唯一的地址（1-247，Modbus RTU中），以确保设备之间不会发生冲突。

#### 3. **地址空间**：
   - Modbus通过设备地址来区分不同的从设备。每个Modbus从设备的寄存器分为四个类型：
     1. **线圈（Coils）**：离散输出，单比特，0/1。
     2. **离散输入（Discrete Inputs）**：单比特的只读输入。
     3. **保持寄存器（Holding Registers）**：16位的读/写寄存器，用于存储模拟输出值。
     4. **输入寄存器（Input Registers）**：16位的只读寄存器，用于存储模拟输入值。

### 三、Modbus数据帧结构

Modbus通信帧的结构因协议变体而异，但基本组成部分相似，包括以下几个主要部分：

1. **设备地址（Address）**：
   - 用于标识从设备的地址，范围在1到247之间。

2. **功能码（Function Code）**：
   - 功能码用于指示从设备执行的操作，例如读写寄存器。常见的功能码包括：
     - 0x01：读线圈状态
     - 0x02：读离散输入
     - 0x03：读保持寄存器
     - 0x04：读输入寄存器
     - 0x05：写单个线圈
     - 0x06：写单个寄存器
     - 0x0F：写多个线圈
     - 0x10：写多个寄存器

3. **数据域（Data Field）**：
   - 数据域包含具体的寄存器地址、寄存器数目或需要写入的数据。

4. **错误检测码**：
   - 在Modbus RTU中使用CRC（循环冗余校验）来检测传输错误，在Modbus ASCII中使用LRC（纵向冗余校验），而Modbus TCP/IP依赖TCP/IP本身的校验机制。

### 四、Modbus功能码

Modbus协议的核心在于通过功能码来执行各种操作，功能码确定了要执行的操作类型以及目标寄存器。例如：

1. **读操作**：
   - `0x01`：读线圈状态（读取多个线圈的当前状态，返回1或0）。
   - `0x03`：读保持寄存器（读取多个寄存器的数值，返回16位的无符号整数）。

2. **写操作**：
   - `0x05`：写单个线圈（向线圈写入一个1或0的值，控制设备开关）。
   - `0x10`：写多个寄存器（同时向多个寄存器写入数值）。

3. **错误处理**：
   - 如果从设备无法处理主设备的请求，它会返回一个异常响应，功能码的最高位设为1，并附加错误码，表明发生了什么问题。

### 五、Modbus RTU和Modbus TCP的区别

| 特性       | Modbus RTU                   | Modbus TCP                     |
| ---------- | ---------------------------- | ------------------------------ |
| 传输介质   | RS232、RS485、RS422          | 以太网（TCP/IP）               |
| 数据帧结构 | 二进制编码，使用CRC          | 不包含CRC或LRC，使用TCP/IP校验 |
| 通信速度   | 低速（典型波特率9600bps）    | 高速，取决于网络环境           |
| 应用场景   | 适合点对点或多点工业控制系统 | 工厂内的现代以太网设备         |
| 设备地址   | 1-247                        | 依赖IP地址，理论上无限制       |

### 六、Modbus的应用场景

1. **工业自动化**：
   - Modbus广泛应用于工厂自动化和过程控制系统中，用于连接PLC、SCADA、HMI以及各种传感器和执行器。
   
2. **能源管理**：
   - 在电力系统中，Modbus用于连接电表、变压器、开关等设备，实现远程监控和管理。

3. **楼宇自动化**：
   - Modbus协议用于楼宇自动化系统中，如控制照明、暖通空调、安防系统等。

4. **水处理系统**：
   - Modbus常用于连接泵、阀门、流量计和其他水处理设备，实现系统的远程监控和控制。

### 七、Modbus的优势

1. **开放性**：
   - Modbus是开放协议，免费且没有版权限制，因此得到了广泛的应用和支持。
   
2. **简单性**：
   - Modbus协议结构简单，易于实现和调试，适合资源有限的设备。

3. **广泛兼容性**：
   - Modbus支持多种传输介质和通信方式，包括串行通信（RS232、RS485）和以太网通信（TCP/IP），能够在多种设备和系统中使用。

4. **成熟稳定**：
   - Modbus自1979年推出以来，已经在工业环境中使用了几十年，经过了长期的验证，稳定可靠。

### 八、Modbus的局限性

1. **带宽限制**：
   - Modbus RTU的数据传输速率较低，通常使用9600bps的波特率，这对于现代数据密集型应用来说可能不够。

2. **无认证和加密**：
   - Modbus协议本身没有提供任何安全机制，如加密和认证。因此，在需要高安全性的场合，必须通过其他手段（如VPN或防火墙）保护Modbus通信。

3. **点对点或有限的设备连接**：
   - Modbus RTU基于主从架构，只能实现一个主设备与多个从设备之间的通信，扩展性较差。

### 总结

Modbus作为一种简洁、开放和成熟的工业通信协议，广泛应用于工业控制、能源管理、楼宇自动化等领域。它提供了多种通信模式，包括Modbus RTU、ASCII和TCP/IP，满足不同的应用需求。虽然存在带宽、扩展性和安全
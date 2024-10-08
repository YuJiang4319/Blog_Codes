---
title: CAN
date: 2024-08-20 18:30:38
tags:
  - CAN
categories:	
  - Embedded
description: 

---

# CAN

CAN（Controller Area Network）是一种用于**实时数据通信**的总线协议，最初由博世公司于1980年代开发，广泛应用于汽车电子系统、工业自动化、医疗设备和其他嵌入式系统。CAN协议具有高效、可靠和低成本等特点，尤其适合高噪声环境中的短消息通信。

### 一、CAN协议概述

CAN是一种多主设备的**串行通信总线**，允许多个控制器或设备在同一总线上无冲突地发送和接收消息。每个设备都可以在任意时刻主动发起通信，而不需要中央控制器。CAN支持不同优先级的消息，通过仲裁机制来决定总线上不同设备的访问顺序。

#### 1. **全局性与多主性**：
   - 所有连接在CAN总线上的节点共享一条通信总线，任何一个节点都可以发送和接收消息，具有去中心化的特性。

#### 2. **消息优先级与仲裁机制**：
   - 每个消息都有一个唯一的标识符（ID），用于表示消息的优先级。当多个节点同时尝试发送数据时，CAN总线通过**位级仲裁**机制确保优先级最高的消息占用总线而不发生冲突。

#### 3. **实时性与可靠性**：
   - CAN非常适合需要**实时数据传输**的应用，因为它能够在节点繁忙的情况下，确保高优先级的消息得以传输。

### 二、CAN协议的技术特点

1. **高抗干扰能力**：
   - CAN在噪声环境中有很强的抗干扰能力，通过差分信号传输减少电磁干扰，并且内建错误检测和纠错机制。

2. **数据传输效率高**：
   - 每帧最多可传输8个字节的数据，但其协议的高效性体现在它能够快速传输大量小型数据包，这对汽车、工业控制等应用至关重要。

3. **错误处理和检测机制**：
   - CAN支持多种错误检测机制，如CRC校验、帧检查、位监控、响应超时等，保证数据的完整性。出错的节点可以自动从总线中分离，确保系统的可靠性。

4. **低成本**：
   - CAN的设计相对简单，不需要复杂的硬件，大大降低了实现成本，特别适合嵌入式系统。

### 三、CAN协议的帧结构

CAN数据帧的基本结构如下：
1. **帧起始位（Start of Frame, SOF）**：标志数据帧的开始，由发送节点发出。
2. **标识符（Identifier）**：用于表示数据的优先级和内容，标准帧使用11位标识符，扩展帧使用29位标识符。
3. **控制字段（Control Field）**：包含数据长度码（DLC），指示有效数据的字节数（0-8字节）。
4. **数据字段（Data Field）**：实际传输的数据，最多8字节。
5. **CRC字段**：用于校验数据帧的完整性。
6. **确认字段（ACK Field）**：接收节点确认是否正确接收到数据。
7. **帧结束位（End of Frame, EOF）**：标志数据帧的结束。

### 四、CAN通信中的关键概念

#### 1. **数据帧**：
   - 数据帧是最常用的CAN消息类型，包含从一个节点发送到其他节点的有效数据。

#### 2. **远程帧（Remote Frame）**：
   - 远程帧用于请求发送数据，发送节点不会发送数据部分，目标节点根据请求响应发送数据帧。

#### 3. **错误帧（Error Frame）**：
   - 如果任何节点检测到错误，它会发送一个错误帧，通知网络中存在错误。

#### 4. **过载帧（Overload Frame）**：
   - 用于强制网络延迟，以便给节点额外的处理时间。

### 五、CAN协议中的两种帧格式

CAN协议定义了两种帧格式：**标准帧（11位标识符）**和**扩展帧（29位标识符）**。

1. **标准帧格式（CAN 2.0A）**：
   - 使用11位标识符，适用于大多数汽车和工业应用。

2. **扩展帧格式（CAN 2.0B）**：
   - 使用29位标识符，允许更多节点使用独特的ID，适用于更复杂的网络结构。

### 六、CAN协议的工作原理

CAN采用一种**基于内容的仲裁**机制，优先级高的消息可以优先传输，而无需等待总线空闲。

#### 1. **差分信号传输**：
   - CAN使用两条线进行差分信号传输，即CAN_H和CAN_L。这种传输方式极大地提高了抗干扰能力，在长距离传输时也能保持数据的准确性。

#### 2. **位级仲裁**：
   - 当多个节点同时发送消息时，它们会逐位比较消息的标识符。如果一个节点检测到自己的消息在仲裁过程中“失败”，它会立即停止发送，优先级较高的消息继续发送。由于仲裁是在总线上的**显性位**和**隐性位**之间进行的，因此不会产生冲突或碰撞。

#### 3. **错误处理机制**：
   - CAN的内建错误检测机制包括CRC校验、帧校验和位监控等。当节点检测到错误时，它会发送错误帧并重新传输数据。此外，CAN网络可以将频繁出错的节点自动隔离，防止影响其他设备。

### 七、CAN FD（Flexible Data-rate）

**CAN FD**是CAN协议的增强版，支持更高的数据速率和更大的数据负载。其主要特点包括：

1. **更高的传输速率**：
   - CAN FD允许数据部分的传输速率提高到8 Mbps以上，而传统CAN速率通常限制在1 Mbps。

2. **更大的数据负载**：
   - CAN FD的每帧最多可以传输64字节的数据，而传统CAN限制在8字节。这使得CAN FD能够更高效地处理大数据量的传输需求，适合现代汽车和工业系统。

3. **向下兼容**：
   - CAN FD兼容传统CAN节点，但在同一个网络中，传统CAN节点无法处理CAN FD消息。

### 八、CAN的应用场景

1. **汽车电子**：
   - CAN是现代汽车电子系统中的核心通信协议，用于连接电子控制单元（ECU），如发动机控制、刹车系统、安全气囊等。

2. **工业自动化**：
   - 在工业领域，CAN用于设备之间的通信，如PLC、传感器和执行器的控制与监控。

3. **医疗设备**：
   - CAN用于连接和控制复杂的医疗设备，如输液泵、监护仪等，确保可靠的数据通信。

4. **机器人**：
   - 机器人系统中常用CAN来实现传感器、伺服电机控制器和中央控制单元之间的高速、实时通信。

### 九、CAN的优势

1. **高可靠性**：
   - 内置多种错误检测机制，保证通信的可靠性和数据的完整性，适合对安全性要求高的应用，如汽车和医疗领域。

2. **实时性**：
   - 通过基于内容的仲裁机制，确保高优先级的消息能够优先传输，满足实时应用需求。

3. **抗干扰能力强**：
   - 使用差分信号传输，具有良好的抗噪能力，适合恶劣的工业环境。

4. **低成本**：
   - CAN的实现简单，不需要复杂的硬件支持，降低了系统的开发成本。

### 十、CAN的局限性

1. **数据传输速率有限**：
   - 传统CAN的最大传输速率通常为1 Mbps，尽管CAN FD提高了速率，但在一些需要大带宽的应用中仍然不足。

2. **数据帧较小**：
   - 传统CAN的每帧数据最大只能传输8字节，对于需要大数据传输的应用，这可能会导致效率下降。

3. **网络扩展性有限**：
   - 虽然CAN适用于小型网络，但在大量节点和复杂网络中，性能可能受到限制。

### 总结

CAN协议以其高效、可靠的实时数据传输能力成为汽车电子和工业控制领域的主流通信协议。CAN通过其仲裁机制和错误检测机制，保证了在多节点网络中的高效通信。CAN FD的引入进一步提高了数据传输速率和容量，使其适应了现代复杂系统的需求。尽管CAN有一些局限性，如带宽和数据负载较小，但它在高可靠
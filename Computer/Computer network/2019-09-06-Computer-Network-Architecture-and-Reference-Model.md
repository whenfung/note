---
title: 1.2 计算机网络体系结构与参考模型
date: 2019-11-1
---

## 1. 计算机网络分层结构

两个系统中实体间的通信时一个很复杂的过程，为了降低协议设计和调试过程的复杂性，也为了便于对网络进行研究、实现和维护，促进标准化工作，通常对计算机网络的体系结构以分层的方式进行建模。

我们把计算机网络的各层及其协议的集合称为网络的体系结构（Architecture）。换言之，计算机网络的体系结构就是这个计算机网络及其所应完成的功能的精确定义，它是计算机网络中的层次、各层的协议及层间接口的集合。需要强调的是，这些功能究竟是用何种硬件或者软件完成的，则是一个遵循这种体系结构的实现（Implementation）问题。体系结构是抽象的，而实现是具体的，是真正在运行的计算机硬件和软件。

计算机网络的体系结构通常都具有可分层的特性，它将复杂的大系统分成若干较容易实现的层次。分层的基本原则如下：

1. 每层都实现一种相对独立的功能，降低大系统的复杂度。
2. 各层之间界面自然清晰，易于理解，相互交流尽可能少。
3. 各层功能的精确定义独立于具体的实现方法，可以采用最合适的技术来实现。
4. 保持下层对上层的独立性，上层单向使用下层提供的服务。
5. 整个分层结构应能促进标准化工作。

由于分层后各层之间相互独立，灵活性好，因而分层的体系结构易于更新（替换单个模块），易于调试，易于交流，易于抽象、易于标准化。但层次越多，有些功能在不同层中难免重复出现，产生额外的开销，导致整体运行效率越低。层次越少，就会使每层的协议太复杂。因此，在分层时应考虑层次的清晰程度与运行效率间的折中、层次数量的折中。

依据一定的规则，将分层后的网络从底层到高层依次称为第 1 层、第 2 层 …… 第 n 层，通常还为每层取一个特定的名称，如第 1 层的名称为物理层。

在计算机网络的分层结构中，第 n 层中的活动元素通常称为 n 层实体。具体来说，实体指任何可发送或接收信息的硬件或软件进程，通常时一个特定的软件模块。不同机器上的同一层称为对等层，同一层的实体称为对等实体。n 层实体实现的服务为 n+1 层所利用。在这种情况下，n 层被称为服务提供者，n+1 层则服务于用户。

每一层还有自己传送的数据单位，其名称、大小、含义也各有不同。

在计算机网络体系结构的各层次中，每个报文都可以分为两个部分：一是数据部分，即 SDU；二是控制信息部分，即 PCI，它们共同组成 PDU。

服务数据单元（SDU）：为完成用户所要求的功能而应传送的数据。第 n 层的服务数据单元记为 n-SDU。

协议控制信息（PCI）：控制协议操作的信息。第 n 层的协议控制信息记为 n-PCI。

协议数据单元（PDU）：对等层次之间传送的数据单元称为该层的 PDU。第 n 层的协议数据单元记为 n-PDU。在实际的网络中，每层的协议数据单元都有一个通俗的名称，如物理层的 PDU 称为比特，链路层的 PDU 称为帧，网络层的 PDU 称为分组，传输层的 PDU 称为报文。

在各层间传输数据时，把从第 n+1 层收到的 PDU 作为第 n 层的 SDU，加上第 n 层的 PCI，就变成了第 n 层的 PDU，交给第 n-1 层后作为 SDU 发送，接收方接收时做相反的处理，故可知三者的关系为

n-SDU + n-PCI = n-PDU = (n-1)-SDU

![PDU](/computer-network/img/PDU.png)

具体地，层次结构的含义包括以下几方面：

1. 第 n 层的实体不仅要使用第 n-1 层的服务来实现自身定义的功能，还要向第 n+1 层提供本层的服务，该服务是第 n 层及其下面各层提供的服务总和。
2. 最底层只提供服务，是整个层次结构的基础；中间各层既是下一层的服务使用者，又是上一层的服务提供者；最高层面向用户提供服务。
3. 上一层只能通过相邻层间的接口使用下一层的服务，而不能调用其他层的服务；下一层所提供服务的实现细节对上一层透明。
4. 两台主机通信时，对等层在逻辑上有一条直接信道，表现为不经过下层就把信息传送到对方。

## 2. 计算机网络协议、接口、服务的概念

### 2.1 协议

协议，就是规则的集合。在网络中要做到有条不紊地交换数据，就必须遵循一些事先约定好的规则。这些规则明确规定了所交换的数据的格式及有关的同步问题。这些为进行网络中的数据交换而建立的规则、标准或约定称为网络协议（Network Protocol），它时控制两个（或多个）对等实体进行通信的规则的集合，是水平的。不对等实体之间是没有协议的，比如用 TCP/IP 协议栈通信的两个结点，结点 A 的传输层和结点 B 的传输层之间存在协议，但结点 A 的传输层和结点 B 的网络层之间不存在协议。网络协议也简称协议。

协议由语法、语义和同步三部分组成。语法规定了传输数据的格式；语义规定了所要完成的功能，即需要发出何种控制信息、完成何种动作及做出何种应答；同步规定了执行各种操作的条件、时序关系等，即时间实现顺序的详细说明。一个完整的协议通常应具有线路管理（建立、释放连接）、差错控制、数据转换等功能。

### 2.2 接口

接口是同一结点内相邻两层间交换信息的连接点，是一个系统内部的规定。每层只能为紧邻的层次之间定义接口，不能跨层定义接口。在典型的接口上，同一结点相邻两层的实体通过服务访问点（service access point,SAP）进行交互。服务是通过 SAP 提供给上一层使用的，第 n 层的 SAP 就是第 n+1 层可以访问第 n 层服务的地方。每个 SAP 都有一个能够识别它的地址。SAP 是一个抽象的概念，它实际上是一个逻辑接口（类似于邮政邮箱），但和通常所说的两个设备之间的硬件接口是很不一样的。

### 2.3 服务

服务是指下层为紧邻的上层提供的功能调用，它是垂直的。对等实体在协议的控制下，使得本层能为上一层提供服务，但要实现本层协议还需要使用下一层所提供的服务。

上层使用下层所提供的服务时必须与下层交换一些命令，这些命令在 OSI 中被称为服务原语。OSI 将原语分为 4 类：

1. 请求（request）。由服务用户发往服务提供者，请求完成某项工作。
2. 指示（indication）。由服务提供者发往服务用户，指示用户做某件事情。
3. 响应（response）。由服务用户发往服务提供者，作为对指示的响应。
4. 证实（confirmation）。由服务提供者发往服务用户，作为对请求的证实。

这 4 类原语用于不同的功能，如建立连接、传输数据和断开连接等。有应答服务包括全部 4 类原语，而无应答服务则只有请求和指示两类原语。

一定要注意，协议和服务在概念上是不一样的。首先，只有本层协议的实现才能保证向上一层提供服务。本层的服务用户只能看见服务而无法看见下面的协议，即下面的协议对上层的服务用户是透明的。其次，协议是 ”水平的“ 。即协议是控制对等实体之间通信的规则。但服务是 ”垂直的“ ，即服务是由下层通过层间接口向上层提供的。另外，并非在一层内完成的全部功能都能称为服务，只有哪些能够被高一层实体 ”看得见“ 的功能才称为服务。

![protocol](/computer-network/img/protocol.png)

计算机网络提供的服务可按以下三种方式分类。

1. 面向连接服务与无连接服务

   在面向连接服务中，通信前双方必须先建立连接，分配相应的资源（如缓冲区），以保证通信能正常进行，传输结束后释放连接和所占用的资源。因此这种服务可以分为连接建立、数据传输和连接释放三个阶段。例如 TCP 就是一种面向连接服务的协议。

   在无线连接服务中，通信前双方不需要先建立连接，需要发送数据时可以直接发送，把每个带有目的地址的包（报文分组）传送到线路上，由系统选定路线进行传输。这是一种不可靠的服务。这种服务常被描述为 ”尽最大努力交付“（Best-Effort-Delivery），它并不保证通信的可靠性。例如 IP、UDP 就是一种无连接服务的协议。

2. 可靠服务和不可靠服务

   可靠服务是指网络具有纠错、检错、应答机制，能保证数据正切、可靠地传输到目的地。

   不可靠服务是指网络只是尽量正确、可靠地传送，而不能保证数据正确、可靠地传送到目的地，是一种尽力而为的服务。

   对于提供不可靠服务的网络，其网络的正确性、可靠性要由应用或用户来保障。例如，用户收到信息后要判断信息的正确性，如果不正确，那么用户要把出错信息报告给信息的发送者，以便发送者采取纠正措施。通过用户的这些措施，可以把不可靠的服务变成可靠的服务。

   注意：在一层内完成的全部功能并非都称之为服务，只有那些能够被高一层实体 ”看得见“ 的功能才能称为服务。

3. 有应答服务和无应答服务

   有应答服务是指接收方在收到数据后向发送方给出相应的应答，该应答由传输系统内部自动实现，而不由用户实现。所发送的应答既可以是肯定应答，也可以是否定应当，通常在接收到的数据有错误时发送否定应答。例如，文件传输服务就是一种有应答服务。

   无应答服务是指接收方收到数据后不自动给出应答。若需要应答，则由高层实现。例如，对于 WWW 服务，客户端收到服务器发送的页面后不给出应答。

## 3. ISO/OSI 参考模型和 TCP/IP 模型

### 3.1 OSI 参考模型

国际标准化组织（ISO）提出的网络体系结构模型，称为开发系统互连参考模型（OSI / RM），通常简称为 OSI 参考模型。OSI 有 7 层，自下而上依次为物理层、数据链路层、网络层、传播层、会话层、表示层、应用层。低三层统称为通信子网，它是为了联网而附加的通信设备（路由器等），完成数据的传输功能；高三层统称为资源子网，它相当于计算机系统，完成数据的处理等功能。传输层承上启下。

![OSI model](/computer-network/img/OSI.jpg)

下面详述 OSI 参考模型各层的功能。

1. 物理层（physical layer）

   物理层的传输单位是比特，任务是透明的传输比特流，功能是在物理媒体上为数据端设备透明地传输原始比特流。

   物理层主要定义数据终端设备（DTE）和数据通信设备（DCE）的物理与逻辑连接方法，所以物理层协议页称物理层接口标准。由于在通信技术的早期阶段，通信规则称为规程（procedure），故物理层协议也称物理层规程。

   物理层接口标准很多，如 `EIA-232C` 、`EIA/TIA RS-449` 、`CCITT` 的 `X.21` 等。

   物理层主要研究以下内容：

   1. 通信链路与通信结点的连接需要一些电路接口，物理层规定了这些接口的一些参数，如机械形状和尺寸、交换电路的数量和排列等，例如，笔记本电脑上的网络接口，就是物理层规定的内容之一。
   2. 物理层也规定了通信链路上传输的信号的意义和电气特征。例如物理层规定信号 A 代表 0，那么当结点要传输数字 0 时，就会发出信号 A，当结点接收到信号 A 时，就知道自己接收到的实际上是数字 0 。

   注意，传输信息所利用的一些物理媒体，如双绞线、光缆、无线信道等，并不在物理层协议之内而在物理层协议下面。因此，有人把物理媒体当作第 0 层。

2. 数据链路层（data link layer）

   数据链路层的传输单位是帧，任务是将网络传来的 IP 数据报组装成帧。数据链路层的功能可以概括为成帧、差错控制、流量控制和传输管理等。

   由于外界噪声的干扰，原始的物理连接在传输比特流时可能发生错误。例如发送端发送的 0 信号在传输过程中收到干扰可能会变成 1，导致接收端接收的信息有误。两个结点之间如果规定了数据链路层协议，那么就可以检测出这些差错，然后把收到的错误信息丢弃，这就是差错控制功能。

   在两个相邻结点之间传送数据时，由于两个结点性能的不同，可能发送端发送数据的速率会比接收端接收数据的速率快，如果不加控制，那么接收端就会丢弃很多来不及接收的正确数据，造成传输线路效率的下降。流量控制可以协调两个结点的速率，使发送数据的速率刚好时接收端可以接收的速率。

   广播式网络在数据链路层还要处理新的问题，即如何控制对共享信道的访问。数据链路层的一个特殊的子层——介质访问子层，就是专门处理这个问题的。

   典型的数据链路层协议有 `SDLC` 、`HDLC` 、`PPP` 、`STP` 和帧中继等。

3. 网络层（network layer）

   网络层的传输单位是数据报，它关心的是通信子网的运行控制，主要任务是把网络层的协议数据单元（分组）从源端传到目的端，为分组交换网上的不同主机提供通信服务。关键问题是对分组进行路由选择，并实现流量控制、拥塞控制、差错控制和网际互联等功能。

   通信子网是网状结构，发送端到接收端有多条路径，而网络层的作用就是根据网络的情况，利用相应的路由算法计算出一条合适的路径，使发送端的分组可以顺利到达接收端。

   流量控制和数据链路层的流量控制含义一样，都是协调发送端的发送速率和接收端的接收速率。

   差错控制使通信两结点之间约定的特定检错规则，如奇偶校验码，接收方根据这个规则检查接收到的分组是否出现差错，如果出现了差错，那么能纠错就纠错，不能纠错就丢弃，确保向上提交的数据都是无误的。

   如果网络中的结点（包括中间结点）都处于来不及接收分组而要丢弃大量分组的情况，那么网络就处于拥塞状态，拥塞状态使得网络中的两个结点无法正常通信。网络层要采取一定的措施来缓解这种拥塞，这就是拥塞控制。

   因特网是一个很大的互联网，它由大量异构网络通过路由器（router）相互连接起来。因特网的主要网络层协议是无连接的网际协议（Internet protocol，IP）和许多路由选择协议，因此因特网的网络层也称网际层或 IP 层。

   注意，网络层中的 “网络” 一词并不是我们通常谈及的具体网络，而是在计算机网络体系结构中使用的专用名词。

   网络层的协议有 `IP` 、 `IPX` 、  `ICMP` 、 `IGMP` 、`ARP` 、 `RARP` 和 `OSPF` 等。

4. 传输层（transport layer）

   传输层也称运输层，传输单位是报文段（TCP）或用户数据报（UDP），传输层负责主机中两个进程之间的通信，功能是为端到端连接提供可靠的传输服务，为端到端连接提供流量控制、差错控制、服务质量、数据传输管理等服务。

   数据链路层提供的是点到点的通信，传输层提供的是端到端的通信，两者不同。通俗地说，点到点可以理解为主机到主机之间的通信，一个点是指一个硬件地址或 IP 地址，网络中参与通信的主机是通过硬件地址或 IP 地址识别的；端对端的通信是指运行在不同主机内的两个进程之间的通信，一个进程由一个端口来识别，所以称端对端通信。

   使用传输层的服务，高层用户可以直接进行端对端的数据传输，从而忽略通信子网的存在。通过传输层的屏蔽，高层用户看不到子网的交替和变化。由于一台主机可同时运行多个进程，因此传输层具有复用和分用的功能。复用是指多个应用层进程可同时使用下面传输层的服务，分用是指传输层把收到的信息分别交付给上面应用层中相应的进程。

   传输层的协议有 `TCP` 、`UDP` 。

5. 会话层（session layer）

   会话层允许不同主机上的各个进程之间进行会话。会话层利用传输层提供的端对端的服务，向表示层提供它的增值服务。这种服务主要为表示层实体或用户进程建立连接并在连接上有序地传输数据，这就是会话，也称建立同步（SYN）。

   会话层负责管理主机间的会话进程，包括建立、管理及终止进程间的会话。会话层可以使用校验点使通信会话在通信失效时从校验点继续恢复通信，实现数据同步。

6. 表示层（presentation layer）

   表示层主要处理在两个通信系统中交换信息的表示方式。不同机器采用的编码和表示方法不用，使用的数据结构也不同。为了使不同表示方法的数据和信息之间能互相交换，表示层采用抽象的标准方法定义数据结构，并采用标准的编码格式，数据压缩、加密和解密也是表示层可提供的数据表示变换功能。

7. 应用层（application layer）

   应用层是 OSI 模型的最高层，是用户与网络的界面。应用层为特定类型的网络应用提供访问 OSI 环境的手段。因为用户的实际应用多种多样，这就要求应用层采用不同的应用协议来解决不同类型的应用要求，因此应用层是最复杂的一层，使用的协议也最多。典型的协议有用于文件传送的 FTP、用于电子邮件的 STMP、用于万维网的 HTTP 等。

### 3.2 TCP/IP 模型

ARPA 在研究 `ARPAnet` 时提出了 TCP/IP 模型，模型从低到高依次为网络接口层（对应 OSI 参考模型中的物理层和数据链路层）、网际层、传输层和应用层（对应 OSI 参考模型中的会话层、表现层和应用层），TCP/IP 由于得到广泛应用而称为事实上的国际标准。TCP/IP 的层次结构及各层的主要协议如下

![IP model](/computer-network/img/IP.jpg)

网络接口层的功能类似于 OSI 的物理层和数据链路层。它表示与物理网络的接口，但实际上 TCP/IP 本身并未真正描述这一部分，只是指出主机必须使用某种协议与网络连接，以便在其上传递 IP 分组。具体的物理网络既可以是各种类型的局域网，如以太网、令牌环网、令牌总线网等，也可以是诸如电话网、`SDH` 、`X.25` 、帧中继和 ATM 等公共数据网络。网络接口层的作用是从主机或结点接收 IP 分组，并把它们发送到指定的物理网络上。

网际层（主机-主机）是 TCP/IP 体系结构的关键部分。它和 OSI 网络层在功能上非常相似。网际层将分组发往任何网络，并为之独立地选择合适的路由，但它不保证各个分组有序地到达，各个分组的有序交付由高层负责。网际层定义了标准的分组格式和协议，即 IP。当前采用的 IP 协议是第 4 版，即 IPv4，它的下一版本是 IPv6 。

传输层（应用-应用或进程-进程）的功能同样和 OSI 中的传输层类似，即使得发送端和目的端主机上的对等实体进行会话。传输层主要使用以下两种协议：

1. 传输控制协议（transmission control protocol，TCP）。它是面向连接的，数据传输的单位是报文段，能够提供可靠的交付。
2. 用户数据报协议（user datagram protocol，UDP）。它是无连接的，数据传输的单位是用户数据报，不保证提供可靠的交付，只能提供 “尽最大努力交付” 。

应用层（用户-用户）包括所有的高层协议，如虚拟终端协议（telnet）、文件传输协议（FTP）、域名解析服务（DNS）、电子邮件协议（SMTP）和超文本传输协议（HTTP）。

从上图可以看出，IP 协议是因特网中的核心协议；TCP/IP 可以为各式各样的应用提供服务（所谓的 everything over IP），同时 TCP/IP 也允许 IP 协议在由各种网络构成的互联网上允许（所谓的 IP over everything）。正因为如此，因特网才会发展到今天的规模。

### 3.3 TCP/IP 模型与 OSI 参考模型的比较

TCP/IP 模型和 OSI 参考模型有很多相似之处。

首先，两者都采用分层的体系结构，将庞大且复杂的问题划分为若干较容易处理的、范围较小的问题，而且分层的功能也大体相似。

其次，两者都是基于独立的协议栈的概念。

最后，两者都可以解决异构网络的互联，实现世界上不同厂家生产的计算机之间的通信。

![OSI vs IP](/computer-network/img/OSIvsIP.png)

两个模型除具有这些基本的相似之处外，也有很多差别。

1. OSI 参考模型的最大贡献就是精确定义了三个主要概念：服务、协议、接口，这与现代的面向对象程序设计思想非常吻合。而 TCP/IP 模型在这三个概念上却没有明确的区分，不符合软件工程的思想。
2. OSI 参考模型产生在协议发明之前，没有偏向于任何特定的协议，通用性良好。但设计者在协议方面没有太多经验，不知道把哪些功能放在哪一层更好。TCP/IP 模型正好相反，首先出现的是协议，模型实际上是对已有协议的描述，因此不会出现协议不能匹配模型的情况，但该模型不适合任何其他非 TCP/IP 的协议栈。
3. TCP/IP 模型在设计之初就考虑到了多种异构网的互联问题，并将网际协议（IP）作为一个单独的重要层次。OSI 参考模型最初只考虑到用一种标准的共用数据网将各种不同的系统互联。OSI 参考模型认识到网际协议 IP 的重要性后，只好将网络层中划分出一个子层来完成类似于 TCP/IP 模型中的 IP 的功能。
4. OSI 参考模型在网络层支持无连接和面向连接的通信，但在传输层仅有面向连接的通信。而 TCP/IP 模型认为可靠性是端对端的问题，因此它在网际层仅有一种无连接的通信模型，但传输层支持无连接和面向连接两种模式。这个不同点常常作为考查点。

无论是 OSI 模型还是 TCP/IP 模型，都是不完美的，对二者的讨论和批评都很多。OSI 参考模型的设计者从工作的开始，试图建立一个全世界的计算机网络都要遵循的统一标准。从技术角度看，它们希望追求一种完美的理想状态，这也导致基于 OSI 参考模型的软件效率极低。OSI 参考模型缺乏市场与商业动力，结构复杂，实现周期长，运行效率低，这是它未能达到预期目标的重要原因。

学习计算机网络时，我们往往采取折中的办法，即综合 OSI 和 TCP/IP 的优点，采用一种只有五层协议的体系结构，即我们熟悉的物理层、数据链路层、网络层、传输层和应用层。

![the five layer model](/computer-network/img/FLM.jpg)

最后简单介绍使用通信协议栈进行通信的结点的数据传输过程。每个协议栈的最顶端都是一个面向用户的接口，下面各层是为通信服务的协议。用户传输一个数据报时，通常给出用户能够理解的自然语言，然后通过应用层，将自然语言转化为用于通信的通信数据。通信数据到达传输层，作为传输层的数据部分（传输层 SDU），加上传输层的控制信息（传输层 PCI），组成传输层的 PDU，然后交到网络层，传输层的 PDU 下放到网络层后，就成为网络层的 SDU，然后加上网络层的 PCI，又组成了网络层的 PDU，下方到数据链路层，就这样层层下放，层层包裹，最后形成的数据报通过通信线路传输，到达接收方结点协议栈，接收方在逆向的逐层把 “包裹” 拆开，然后把收到的数据提交给用户。

## 4. 习题

1. 网络模型进行分层的目标不包括

   提供标准语言；定义功能执行的方法；定义标准界面；增加功能之间的独立性

2. 数据链路层不具备（）功能

   物理寻址；流量控制；差错校验；拥塞控制

3. 描述 OSI 数据链路层的功能

4. OSI 参考模型不参与数据封装工作的是哪层

5. OSI 提供端对端的应答、分组排序和流量控制功能的协议层是哪层？

6. OSI 同步管理在哪层？

7. 数据格式转换及压缩属于 OSI 参考模型的哪一层功能？

8. OIS 参考模型中，功能需由应用层的相邻层实现的是？

9. OSI 参考模型中，提供流量控制的有哪些协议层？

10. 为网络实体提供数据发送和接收功能及过程的是哪一层？

11. 端对端通信属于哪一层理念

12. 路由器（router）、交换机（switch）、集线器（hub）实现的最高功能分别是

13. 直接为会话层提供服务的是哪一层？

## 5. 习题答案

1. 定义功能执行的方法，方法很多种，不规定哪种
2. 拥塞控制，这是网络层和传输层干的事
3. 保证数据正确的顺序和完整性
4. 物理层，直接翻译成比特流传输
5. 传输层
6. 会话层（SYN 功能）
7. 表现层
8. 表现层的数据格式转化功能
9. 数据链路层、网络层、传输层
10. 数据链路层
11. 传输层
12. 路由器实现点对点服务（实现下三层），交换机事一个多端口的网桥，实现数据交换（数据链路层）、集线器是一个多端口的中继器，工作在物理层
13. 传输层


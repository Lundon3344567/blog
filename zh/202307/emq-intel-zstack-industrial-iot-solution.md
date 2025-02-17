近日，EMQ 携手英特尔与云轴科技 ZStack 推出泛工业物联网联合解决方案，基于云原生超融合，在挖掘生产数据价值的同时有效降低综合建设成本，为用户提供一站式数据链路及 IT 基础设施解决方案。

## **工业能耗大户面临的关键挑战**

工业正迈入一个全新的物联网时代，海量数据计算需求涌现，边缘计算将在其中发挥重要作用。IDC 预计到2026 年，在中国工业企业的边缘 IT 支出将达到 658.2 亿元人民币（《中国工业边缘市场分析》，2023）。

 工业物联网场景中，新型应用部署位置靠近业务生产侧，部署和运维时面临资源紧缺、管理复杂、兼容性差、调试困难、无高可用方案、运维门槛高等问题，导致整体 TCO 居高不下。**尤其针对工业能耗大户，若要实现低碳转型达成降本增效目标，在数字化转型过程中将会面临从上层软件应用到底层基础设施的 4 个关键问题：**

- **设备多：海量异构设备和数据感知问题。**一个中等规模的工厂涉及到产线、人员、环控、能源、安全、储存、运输、厂房等环节，这些环节中的工业或物联网设备数量庞大，不同设备的协议不统一，建立通信并统一数据格式是重点和难点。
- **数据多：生产数据从感知到传输过程中包含大量冗余数据的问题。**传统工业在控制层之上的事件告警和预案处理都是通过信息中心统一的平台进行控制协调，网络延时、信息量过大等情况会导致告警和处理滞后。
- **运营难：厂级条件有限，如何确保核心业务及数据高可用/故障处理。**在面对工业数据海量、传输频率高、应用结构复杂的现状情况下，如何确保核心业务具备高可用及故障自愈能力、高频数据预处理与持久化存储。
- **资源紧：传统行业运维人员配置不足，厂级运维支撑条件有限。**在面对新型应用的容器化交付趋势，再到底层基于容器编排基础设施的运维管理，都将对传统行业 IT 带来较大的使用门槛。

##  **泛工业物联网联合解决方案**

本次三方合作推出的泛工业物联网联合解决方案，旨在提供一种全面的、可扩展的 IoT 业务解决方案。

![泛工业物联网联合解决方案](https://assets.emqx.com/images/4bddb9996f592b6d520bbd40c06fd81a.png)

该方案主要由以下 3 部分组成：

**边缘应用**

[边缘数采软件 Neuron](https://neugates.io/zh) 通过配置南向设备和点位信息连接到设备端进行数据采集，并配置北向驱动把采集到的数据上传到边缘流式计算引擎 eKuiper；

[eKuiper](https://ekuiper.org/zh) 配置 Neuron 为数据源，并以 Neuron 源设计计算规则，计算结果以 MQTT 形式发送到同边缘网络的 [NanoMQ](https://nanomq.io/zh)；

边缘 Broker NanoMQ 与云端 EMQX 做桥接处理，将边端数据源源不断地以 MQTT 协议发送消息到云端 [EMQX Enterprise](https://www.emqx.com/zh/products/emqx)。

**中心应用**

云端 Broker EMQX Enterprise 从边缘端接收消息，经过规则引擎处理后提供给上层应用。云边一体化平台 [EMQX ECP](https://www.emqx.com/zh/products/emqx-ecp)，负责云端 EMQX Enterprise 的全生命周期管理，通过与云边协同代理 ECP Edge Agent 通信进行边缘服务指令下发、远程配置，可以将 Neuron、eKuiper 的配置抽象成模版，批量下发到有类似配置需求的其他边缘端。

**平台底座**

ZStack Edge 云原生超融合基于通用服务器部署，直接在物理机上并行运行「容器+虚拟机」双引擎，结合分布式存储、应用市场、一键运维、云边协同、云原生灾备等服务模块，为厂级数据中心/边缘计算场景下的业务应用实现开箱即投产、应用级故障自愈、数据高可用、一键运维等关键能力。

英特尔为 ZStack Edge 云原生超融合平台提供性能强劲、稳定可靠、弹性可扩展的处理器基座，以及兼具性能和能效优化的虚拟化技术，同时英特尔也提供了针对最新英特尔®至强®可扩展处理器的软件加速和优化支持。

## **方案优势**

### **一体化**

- 采集归一体化：实现各类设备数据格式的标准化、统一化采集与接入，为业务应用开发提供了灵活的数据接口，大幅降低开发成本，帮助企业快速开发创新业务应用；
- 业务级开箱即用：通过 ZStack Edge 云原生超融合内置应用市场，为 EMQ 所有组件提供可视化部署以应用一键发布/回滚/版本管理能力，屏蔽底层异构资源，实现开箱即投产。

### **低延时**

- 边缘计算：可靠近生产现场部署，毫秒级响应时间，支持实时数据过滤、处理、计算、离线缓存等服务，大大缩短了数据与决策之间的等待时间，提高服务质量 ；
- 云边协同：支持从边到云的数据采集，和从云到边的指令下发，在边缘端不支持 HTTP 协议时可提供 MQTT 协议的云边通道功能，实现云端对边端的管控。

### **高可用**

- 故障自愈：核心业务基于容器编排能力，具备故障自愈能力，消除业务停机时间，提升业务延续性；
- 稳定可靠：全平台高可用，内置分布式存储底座，提供多种类型存储满足高频数据预处理与持久化归档存储需求。

### **易运维**

- 业务可视化：Neuron、NanoMQ、eKuiper、EMQX 等组件均可实现高效对接，通过 EMQX ECP 统一管理，便于维护人员配置/监控/编写/启停，提升业务管理效率；
- 一键运维：一键对关键资源和服务进行自动化巡检，内置评分机制对所巡检项目健康状态进行量化评分，自动生成针对性的修复建议，降低现场运维门槛。

## **典型场景**

目前该方案主要针对以下三大应用场景：

### **工厂数据可视化数字孪生**

工业生产数据是海量且连续的，且数字孪生对数据量和延迟有很高要求，用传统的批处理方法处理流数据几乎不可能，因此流处理在边缘侧成为刚需，同时需要厂级信息中心提供生产数据提供高可用、高并发、低延时的数据传输、分析、对接能力。联合解决方案可在车间/厂级可视化建立统一的数据传输通道，消除数据孤岛，轻松实现各级可视化应用之间的数据流转、处理、存储，对新系统的扩展和创新应用开发提供巨大的便利性。

### **设备异常感知及处理**

在工业生产弱网环境下，要将各类工业设备接入实现异常告警、消除安全隐患，需要使用轻量级 MQTT 协议进行数据实时感知和稳定传输。同时，还需要打通不同网络中设备、不同系统间的数据壁垒，以便实现设备间的联动。最终，需要在厂级指挥调度中心汇聚和管理所有异常事件，联合解决方案可有效提高厂级生产效率和质量。

### **生产线视觉 AI 缺陷检测**

引入 AI 缺陷检测能力可优化工厂生产工艺，需要在边缘端支持 Rest、gRPC、msgpack RPC 等服务对接 AI 缺陷监测数据，联动自动化设备进行告警通知或纠错流程，实现生产质量追溯和工艺优化。联合解决方案可构建完整的云边一体 AI 模型训练流程，实时汇聚图像流并持久化到中心端进行模型训练和优化，同时汇聚新模型推理结果。

## **未来展望**

近两年，随着国家对产业升级和节能减排的支持，数字经济正从消费互联网向产业互联网高速演进。在 EMQ 与 ZStack 及英特尔的深度合作下，泛工业物联网联合解决方案将实现从上层业务流到底层基础设施的全新升级，助力智能制造、智慧能源、智慧交通等行业数字化转型，为用户实现提质、降本、增效。





<section class="promotion">
    <div>
        联系 EMQ 工业领域解决方案专家
    </div>
    <a href="https://www.emqx.com/zh/contact?product=solutions" class="button is-gradient px-5">联系我们 →</a>
</section>

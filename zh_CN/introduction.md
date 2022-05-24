# 什么是 Neuron？

Neuron 是运行在各类物联网边缘网关硬件上的工业协议网关软件，旨在解决工业 4.0 背景下设备数据统一接入难的问题。

通过将来自繁杂多样工业设备的不同协议类型数据转换为统一标准的物联网 MQTT 消息，实现设备与工业物联网系统之间、设备彼此之间的互联互通，进行远程的直接控制和信息获取，为智能生产制造提供数据支撑。

Neuron 支持同时为多个不同通讯协议设备、数十种工业协议进行一站式接入及 MQTT 协议转换，仅占用超低资源，即可以原生或容器的方式部署在 X86、ARM、RISC-V 等架构的各类边缘硬件中。同时，用户可以通过基于 Web 的管理控制台实现在线的网关配置管理。


Neuron提供下列产品特点：
- 支持了 [Modbus，OPCUA，Ethernet/IP，IEC104 和 BACnet](module-plugins/module-list.md) 等众多协议和设备；
- 北向标准 MQTT 数据发送，根据用户指定配置，将数据发送至指定的 MQTT 消息服务器中；
- 南向控制接口，监听及控制设备数据上报，将相关的控制命令转发给设备；
- 内存占用少，小于 100k，能在低配置硬件上运行。
- 结合 [eKuiper](https://www.lfedge.org/projects/ekuiper) 提供的规则引擎功能，快速实现基于规则的设备控制；
- 集成其他应用，通过 [API](api.md) 或 [MQTT](mqtt.md) 服务控制工业设备或更改配置。
- [管理控制台](dashboard-operation/login.md)，用户可以在浏览器中进行可视化的数据监控，设备状态及配置，实现跨工业设备数据的接入；
- 支持加密TLS、HTTPS和JWT auth，确保传输中的数据安全；


Neuron 与 EMQ 在边缘的其它产品集成，可以轻松实现一个端到端的[云边协同的工业互联网解决方案](https://www.emqx.com/zh/use-cases/industrial-iot)。
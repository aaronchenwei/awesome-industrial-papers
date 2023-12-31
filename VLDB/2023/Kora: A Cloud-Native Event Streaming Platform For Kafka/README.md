# Kora: A Cloud-Native Event Streaming Platform For Kafka

> Povzner, Anna, et al. "Kora: A Cloud-Native Event Streaming Platform for Kafka." _Proceedings of the VLDB Endowment_ 16.12 (2023): 3822-3834.

"Kora: A Cloud-Native Event Streaming Platform For Kafka" was awarded **the Best Industry Paper at VLDB 2023** out of 29 other prestigious research papers from Meta, Google, Microsoft, Alibaba, Bytedance, and other innovative companies.

同时，也可以读一下 Confluent CEO Jay Kreps 写的 Blog - https://www.confluent.io/blog/cloud-native-data-streaming-kafka-engine/

> When we launched Confluent Cloud in 2017, we had a grand vision for what it would mean to offer Kafka in the cloud. But despite the work we put into it, our early Kafka offering was far from that—it was basically just open source Kafka on a Kubernetes-based control plane with simplistic billing, observability, and operational controls. It was the best Kafka offering of its day, but still far from what we envisioned.

`Kora`作为基于 Kafka 的云原生 event streaming 平台，在架构设计上面，值得学习和借鉴。

## Abstract

## 1. Introduction

Confluent Cloud provides a fully-managed, cloud-native event streaming platform based on Apache Kafka. Our platform, called **_Kora_**, is highly available, scalable, elastic, secure, and globally interconnected. As a true cloud-native platform, it abstracts low-level resources such as Kafka brokers and hides operational complexities such as system upgrades. It supports a pay-as-you-go model: users can start small and scale their workloads to GBs/sec and back when needed while only paying for resources they use. Users can choose between a cost-effective multi-tenant configuration as well as dedicated solutions if stronger isolation is required.

Confluent Cloud 提供了一个基于 Apache Kafka 的完全托管的云原生事件流平台。这个平台被称为**_Kora_**，具有高可用性、可扩展性、弹性、安全性，和具备全球互联性。作为一个真正的云原生平台，

- 抽象了底层资源，如 Kafka brokers，
- 隐藏了运维复杂性，如系统升级
- 支持按需付费模型：用户可以从小规模开始使用。根据需要将其工作负载扩展到每秒 GB，并在仅使用的资源上支付费用。
- 用户可以在成本效益的多租户配置和需要更强隔离的情况下选择专用解决方案。

论文的 contributions:

- We present the architecture of Kora, a cloud-native event streaming platform that synthesizes well-known techniques from literature to deliver the promise of cloud: **high availability, durability, scalability, elasticity, cost efficiency, performance, multi-tenancy, and multi-cloud support**. For example, our architecture decouples its storage and compute tiers to facilitate elasticity, performance, and cost efficiency.
- We describe Kora’s abstractions that have been in production use for several years and enabled the service to grow to **massive scale**. These abstractions have helped our customers distance themselves from the underlying hardware and think in terms of application requirements, while allowing us to experiment with newer hardware types and configurations in our quest for optimum price-performance ratio.
- Finally, we give insights into **key customer pain points and the challenges** we faced to address them. For example, while the promise of newer, faster hardware is tempting to meet increasing customer performance demands, the reality is that evaluating new hardware for an optimum price-performance ratio takes significant investment given large and diverse workload requirements.

## 2. Background

描述了 Kafka 的基本原理。

<p align="center">
  <img src="https://github.com/aaronchenwei/awesome-industrial-papers/assets/9360415/20c285d4-8882-49a5-9a7a-67db261d910e" />
</p>

- **Latency** is a critical measure of the performance of event streaming systems since applications often operate with real-time expectations.
- We measure _end-to-end_ latency as shown in Figure 1 as the elapsed time between event creation by a producer and delivery to the consumer.

## 3. Overview

### 3.1 Design goals

The design and architecture of Kora is motivated by the following key objectives:

- _Availability and Durability_.
  - Our customers use our service for business-critical services and for storing critical data. Lapses in durability or availability lead to direct revenue loss and are completely unacceptable. We offer an uptime SLA of 99.95% for single zone clusters and 99.99% for multi-zone clusters
- _Scalability_.
  - Scalability is crucial for most customers as changing infrastructure backends is very risky and expensive, especially in terms of engineering resources. Therefore, customers want to use a backend service that they know will continue to scale as their business grows year-over-year.
- _Elasticity_.
  - Customers can expand and shrink their clusters as their workloads scales. Additionally, Kora adapts to changes in workload patterns to provide optimal performance for a given cluster size.
- _Performance_.
  - Low latency at high throughput is the hallmark of event streaming platforms. We have made the conscious choice of directly passing all performance wins to the users. Therefore, over time, users may see the performance of their applications improve.
- _Low cost_.
  - Our customers want all the benefits of the cloud but at the cheapest possible net cost. Our design, therefore, puts a lot of emphasis on optimizing the cost for our users and we often lean towards choices that yield better price-performance ratio.
- _Multitenancy_.
  - Multitenancy is one of the key enablers of a low-price and highly elastic cloud experience in the form of a pay-as-you-go model. Our design features several key mechanisms required by a truly multi-tenant Event Streaming platform work.
- _Multi-cloud support_.

  - Kora runs on AWS, GCP, and Azure. Many of our design choices are driven by our desire to provide a unified experience to our users while minimizing the operational burden due to differences between clouds.

---

- 可用性和耐久性。
  - 我们的客户依赖我们的服务进行业务关键服务和关键数据的存储。耐久性或可用性的中断将直接导致收入损失，这是绝对不可接受的。我们为单区集群提供了 99.95%的正常运行时间 SLA，对于多区集群则提供了 99.99%的正常运行时间 SLA。
- 可扩展性。
  - 对于大多数客户来说，可扩展性至关重要，因为更改基础设施后端是非常冒险和昂贵的，尤其是在工程资源方面。因此，客户希望使用他们知道将在业务逐年增长的同时继续扩展的后端服务。
- 弹性。
  - 客户可以根据其工作负载的规模扩展和收缩其集群。此外，Kora 会适应工作负载模式的变化，以为给定的集群大小提供最佳性能。
- 性能。
  - 在高吞吐量下低延迟是事件流平台的特点。我们已经做出了直接将所有性能优势传递给用户的明智选择。因此，随着时间的推移，用户可能会看到其应用程序的性能得到改善。
- 低成本。
  - 我们的客户希望以尽可能低的净成本获得云的所有好处。因此，我们的设计非常注重优化用户的成本，并经常倾向于选择产生更好价格性能比的选项。
- 多租户。
  - 多租户是以按需付费模式提供低价和高度弹性云体验的关键因素之一。我们的设计具有一些真正的多租户事件流平台工作所需的关键机制。
- 多云支持。
  - Kora 在 AWS、GCP 和 Azure 上运行。我们的许多设计选择是由我们希望为用户提供统一体验的愿望驱动的，同时尽量减少由于云之间的差异而带来的运维负担。

### 3.2 Architecture

<p align="center">
  <img width="60%" src="https://github.com/aaronchenwei/awesome-industrial-papers/assets/9360415/6e24ace2-0b71-44c0-ae86-1737ea76f219" />
</p>

### 3.3 Metadata Management

Kafka’s architecture relies on a centralized controller to manage cluster-wide metadata such as replica assignments and topic configurations. The controller is also responsible for tracking the liveness of brokers and for electing topic partition _leaders_. Brokers register with the controller on startup and maintain a session through heartbeats. If no heartbeats are received before a session timeout expires, the broker is considered offline and new leaders are elected as needed.

### 3.4 Data Storage

## 4. Cloud-Native Building Blocks

### 4.1 Abstractions for a cloud native cluster

### 4.2 Cluster organization for cost efficiency

#### 4.2.1 Core Kafka costs

#### 4.2.2 Network costs

#### 4.2.3 Microservice costs

### 4.3 Elasticity

#### 4.3.1 Load Balancing

#### 4.3.2 Shrink and Expand

### 4.4 Observability

#### 4.4.1 Client-centric end-to-end metrics

#### 4.4.2 Fleet-wide SLO

### 4.5 Automated Migigation

#### 4.5.1 Network Unavailability

#### 4.5.2 Storage Degradation

#### 4.5.3 Mitigation

### 4.6 Ensuring data durability

#### 4.6.1 Global replication using Cluster Linking

#### 4.6.2 Backup and restore

#### 4.6.3 Audit infrastructure

### 4.7 Upgrades

## 5. Multi-Tenancy

### 5.1 Logical Cluster as a Unit of Isolation

### 5.2 Performance Isolation

### 5.3 Isolation at scale

## 6. Conclusion

## 7. Acknowledgements

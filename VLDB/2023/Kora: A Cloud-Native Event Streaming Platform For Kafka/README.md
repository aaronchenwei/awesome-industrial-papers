# Kora: A Cloud-Native Event Streaming Platform For Kafka

> Povzner, Anna, et al. "Kora: A Cloud-Native Event Streaming Platform for Kafka." _Proceedings of the VLDB Endowment_ 16.12 (2023): 3822-3834.

"Kora: A Cloud-Native Event Streaming Platform For Kafka" was awarded **the Best Industry Paper at VLDB 2023** out of 29 other prestigious research papers from Meta, Google, Microsoft, Alibaba, Bytedance, and other innovative companies.

同时，也可以读一下 Confluent CEO Jay Kreps 写的 Blog - https://www.confluent.io/blog/cloud-native-data-streaming-kafka-engine/

> When we launched Confluent Cloud in 2017, we had a grand vision for what it would mean to offer Kafka in the cloud. But despite the work we put into it, our early Kafka offering was far from that—it was basically just open source Kafka on a Kubernetes-based control plane with simplistic billing, observability, and operational controls. It was the best Kafka offering of its day, but still far from what we envisioned.

`Kora`作为基于 Kafka 的云原生 event streaming 平台，在架构设计上面，值得学习和借鉴。

## Abstract

## 1. Introduction

Confluent Cloud provides a fully-managed, cloud-native event streaming platform based on Apache Kafka. Our platform, called **_Kora_**, is highly available, scalable, elastic, secure, and globally interconnected. As a true cloud-native platform, it abstracts low-level resources such as Kafka brokers and hides operational complexities such as system upgrades. It supports a pay-as-you-go model: users can start small and scale their workloads to GBs/sec and back when needed while only paying for resources they use. Users can choose between a cost-effective multi-tenant configuration as well as dedicated solutions if stronger isolation is required.

Confluent Cloud提供了一个基于Apache Kafka的完全托管的云原生事件流平台。这个平台被称为**_Kora_**，具有高可用性、可扩展性、弹性、安全性，和具备全球互联性。作为一个真正的云原生平台，
- 抽象了底层资源，如Kafka brokers，
- 隐藏了运维复杂性，如系统升级
- 支持按需付费模型：用户可以从小规模开始使用。根据需要将其工作负载扩展到每秒GB，并在仅使用的资源上支付费用。
- 用户可以在成本效益的多租户配置和需要更强隔离的情况下选择专用解决方案。

论文的 contributions:

- We present the architecture of Kora, a cloud-native event streaming platform that synthesizes well-known techniques from literature to deliver the promise of cloud: **high availability, durability, scalability, elasticity, cost efficiency, performance, multi-tenancy, and multi-cloud support**. For example, our architecture decouples its storage and compute tiers to facilitate elasticity, performance, and cost efficiency.
- We describe Kora’s abstractions that have been in production use for several years and enabled the service to grow to **massive scale**. These abstractions have helped our customers distance themselves from the underlying hardware and think in terms of application requirements, while allowing us to experiment with newer hardware types and configurations in our quest for optimum price-performance ratio.
- Finally, we give insights into **key customer pain points and the challenges** we faced to address them. For example, while the promise of newer, faster hardware is tempting to meet increasing customer performance demands, the reality is that evaluating new hardware for an optimum price-performance ratio takes significant investment given large and diverse workload requirements.

## 2. Background

描述了 Kafka 的基本原理。

![image](https://github.com/aaronchenwei/awesome-industrial-papers/assets/9360415/20c285d4-8882-49a5-9a7a-67db261d910e)

- Latency is a critical measure of the performance of event streaming systems since applications often operate with real-time expectations.
- We measure _end-to-end_ latency as shown in Figure 1 as the elapsed time between event creation by a producer and delivery to the consumer.

## 3. Overview

### 3.1 Design goals

### 3.2 Architecture

### 3.3 Metadata Management

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

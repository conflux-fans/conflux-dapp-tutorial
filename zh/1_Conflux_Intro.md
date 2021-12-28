# Conflux introduction

<img src="https://github.com/Conflux-Chain/design-resource-lab/blob/master/0.%20CONFLUX%20LOGO/Stacked%20lockup/with%20space/Conflux_with%20space_PNG/Stacked_with%20space-1.png?raw=true" width="800px"></img>

[Conflux](https://confluxnetwork.org/) 是一个兼容 EVM 的高性能，完全去中心化公链。具有高性能，安全，可扩展等特性，并且具有完善的基础设施和生态。

## 特性

### 树图账本结构

Conflux 协议采用创新的树图(TreeGraph)账本结构及[GHAST](https://confluxnetwork.org/files/Conflux_Technical_Presentation_20200309.pdf)共识协议，能够实现网络`并行出块`，从而大大提高网络吞吐率，降低确认时间。
Conflux 网络平均出块时间为 0.5s，每个区块的 gasLimit 为 `3000w gas`，总体可以达到 3000 TPS（CFX转账）

![](https://developer.confluxnetwork.org/img/tree_graph.jpg)

树图账本有以下特点：

1. 账本由多个区块组成，区块间的关系分为两种：父边(实线)，引用边(虚线)
2. 如果只看父边和区块账本结构是一棵树；如果同时考虑上引用边整个账本为一个图，确切说是一个 DAG （有向无环图）
3. 父边和引用边均用于确认区块的先后顺序，最终可实现所有交易的全排序

如果想深入了解 TreeGraph，可参看[这篇技术介绍](https://confluxnetwork.org/files/Conflux_Technical_Presentation_20200309.pdf)

### 高性能

因为 Conflux 可以实现并行出块，网络的吞吐率得到了大幅提高，经过实测可以达到 3000 TPS（CFX转账），并且可以在一分钟内实现交易确认。相比于比特币的 `7TPS` 和 以太坊的 `40TPS` 提高了一到两个数量级。

### 低手续费+代付机制

得益于网络的吞吐率大幅提高，使得目前网络的交易手续费大大降低, 目前交易平均手续费价格为 100-10000 Drip 左右，远远低于以太坊动辄几Gwei 到几十 Gwei 的手续费价格。

另外 Conflux 网络还实现了独特的赞助商机制：允许任何人为任意合约赞助手续费，合约被赞助后普通用户同合约交互所产生的燃气和存储费均由赞助商支付，普通用户可以零手续费与 Dapp 交互。

## 概念介绍

Conflux 实现了 EVM 兼容的虚拟机，几乎所有的以太坊智能合约可以直接部署到 Conflux 网络。但同以太坊网络还是有一些差别。

### base32 地址

以太坊采用 hex40 格式地址：

```txt
0x12c0ced72d5579b3DF107b0697948d267C98D3d9
```

Conflux 则采用了 base32 编码地址：

```txt
cfx:aakpbx01fzm1xp89cb7urf6yvyxh3ggx5e9hz07b38
```

详情参看 [Conflux 地址介绍](https://docs.confluxnetwork.org/crypto-notes/notes/conflux-address)

### Epoch

Conflux 采用`树图结构账本`，在树图中会从创世块开始通过`最重子树算法`确定账本的轴区块（pivot chain)，轴区块连接在一起形成主轴，轴区块连同其所`引用`的所有区块(未划分到其他Epoch的区块)，组成一个 Epoch(纪元)，该纪元的 EpochNumber 为轴区块在主轴上的高度。

* 区块顺序的确定需要先确定轴区块顺序，然后确定 epoch 内的区块顺序
* 交易的执行按照 Epoch by Epoch 的顺序执行
* 一个 Epoch 可能会包含多个区块
* 一笔交易可能会被打包进多个区块（因为并行出块），交易会在最先打包交易的区块中执行，之后区块中的同一笔交易会执行失败

### `cfx` RPC

Conflux 定义了一套以 `cfx` 为前缀的 RPC 方法，这些方法的概念和用途基本同以太坊相同，如果熟悉以太坊的 RPC 可以很快知道如何使用 Conflux 的方法。但因为方法名和某些字段的不同以太坊的主流 SDK 无法在 Conflux 网络使用，需要使用 Conflux 提供的 SDK 来同网络交互，目前包含如下三个 SDK：

* [js-conflux-sdk](https://docs.confluxnetwork.org/js-conflux-sdk)
* [go-conflux-sdk](https://github.com/conflux-chain/go-conflux-sdk)
* [java-conflux-sdk](https://github.com/conflux-chain/java-conflux-sdk)

注意：Conflux 大部分 RPC 方法中使用 `EpochNumber` 作为参数，而以太坊中则使用 `BlockNumber`

想了解 RPC 详情，可查看 [Conflux RPC 文档](https://developer.confluxnetwork.org/conflux-doc/docs/json_rpc)

### 内置合约

Conflux 内置多个合约，用于提供重要功能，目前包含：

* AdminControl
* SponsorWhitelistControl
* Staking
* ConfluxContext
* PoSRegister

具体文档可参看[内置合约文档](https://developer.confluxnetwork.org/conflux-rust/internal_contract/internal_contract)

## 与以太坊区别

[本篇文章](https://docs.confluxnetwork.org/crypto-notes/notes/conflux-vs-ethereum)梳理了 Conflux 与以太坊的区别

## 参考链接

* [Conflux 官网](https://confluxnetwork.org/)
* [开发者文档](https://developer.confluxnetwork.org/)

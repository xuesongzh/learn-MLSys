# 推理引擎

训练过程通过设定数据处理方式，并设计合适的网络模型结构以及损失函数和优化算法，在此基础上将数据集以小批量（mini-batch）反复进行前向计算并计算损失，然后反向计算梯度利用特定的优化函数来更新模型，来使得损失函数达到最优的结果。训练过程最重要的就是梯度的计算和反向传播。

而推理就是在训练好的模型结构和参数基础上，做一次前向传播得到模型输出的过程。相对于训练而言，推理不涉及梯度和损失优化。推理的最终目标是将训练好的模型部署生产环境中。真正让 AI 能够运用起来。推理引擎可以将深度学习模型部署到云（Cloud）端或者边缘（Edge）端，并服务用户的请求。模型训练过程好比是传统软件工程中的代码开发的过程，而开发完的代码势必要打包，部署给用户使用，那么推理系统就负责应对模型部署的生命周期中遇到的挑战和问题。

当推理系统将完成训练的模型进行部署，并在服务时还需要考虑设计和提供负载均衡，请求调度，加速优化，多副本和生命周期管理等支持。相比深度学习框架等为训练而设计的系统，推理系统不仅关注低延迟，高吞吐，可靠性等设计目标，同时受到资源，服务等级协议（Service-Level Agreement），功耗等约束。本章将围绕深度学习推理系统的设计，实现与优化内容展开，同时还会在最后介绍部署和 MLOps 等内容。

移动端的推理引擎应该挺多的了，google在2017年推出了TF-Lite，腾讯在2017年推出了ncnn，Apple在2017也推出了CoreML，阿里在2018年推出了MNN，华为2019年推出了MindSpsore-Lite。距今已经过去了快5年的时间，技术上也接近收敛。下面让我们一起打开推理引擎的技术吧！

## 课程简介

- 《推理系统》推理系统是本分享的重点概述，推理就是在训练好的模型结构和参数基础上，执行前向传播得到模型输出的过程。相对于训练而言，推理不涉及梯度和损失优化。推理的最终目标是将训练好的模型部署生产环境中，真正让 AI 能够运用起来。推理引擎可以将深度学习模型部署到云（Cloud）端或者边缘（Edge）端，并服务用户的请求。模型训练过程好比是传统软件工程中的代码开发的过程，而开发完的代码势必要打包，部署给用户使用，那么推理系统就负责应对模型部署的生命周期中遇到的挑战和问题。

- 《轻量网络》在端侧推理引擎中，主要是执行轻量的模型结构。主要思想是针对神经网络模型设计更高效的网络计算方式，从而使神经网络模型的参数量减少的同时，不损失网络精度，并进一步提高模型的执行效率。本节主要集中介绍模型小型化中需要注意的参数和指标，接着深入了解CNN经典的轻量化模型和Transformer结构的轻量化模型。

- 《模型压缩》模型压缩跟轻量化网络模型不同，压缩主要是对轻量化或者非轻量化模型执行剪枝、蒸馏、量化等压缩算法和手段，使得模型更加小、更加轻便、更加利于执行。

- 《模型转换&优化》在这一节当中分为模型转换和模型优化，在整体架构图中属于离线模型转换模块。一方面，推理引擎需要把不同 AI 框架训练得到的模型进行转换；另外一方面需要对转换后的模型进行图优化等技术。

- 《Kernel优化》在上层应用或者 AI 网络模型中，看到的是算子；但是在推理引擎实际执行的是具体的 Kernel，而推理引擎中 CNN 占据了主要是得执行时间，因此其 Kernel 优化尤为重要。

希望这个系列能够给大家、朋友们带来一些些帮助，也希望自己能够继续坚持完成所有内容哈！

## 课程目标

通过《推理引擎：核心原理》这个课程，以及这门课程后面的几门课程，你将获取并且掌握的技能：

|        |                         |                            |
|:------:|:----------------------- |:-------------------------- |
| **编号** | **名称**                  | **具体内容**                   |
| 1      | [推理系统](./Inference/)    | 推理系统整体介绍，推理引擎架构梳理          |
| 2      | [轻量网络](./Mobilenet/)    | 轻量化主干网络，MobileNet等SOTA模型介绍 |
| 3      | [模型压缩](./Slim/)         | 模型压缩4件套，量化、蒸馏、剪枝和二值化       |
| 4      | [模型转换&优化](./Converter/) | AI框架训练后模型进行转换，并对计算图优化      |
| 5      | [Kernel优化](./Kernel/)   | Kernel层、算子层优化，对算子、内存、调度优化  |

## 课程细节

> *建议优先下载或者使用PDF版本，PPT版本会因为字体缺失等原因导致版本很丑哦~*

|     |          |                   |                                                                                                     |     |
| --- | -------- | ----------------- | --------------------------------------------------------------------------------------------------- | --- |
| 编号  | 名称       | 内容                | 资源                                                                                                  | 备注  |
| 1   | 推理系统     | 01 内容介绍           | [slide](./Inference/01.introduction.pdf), [video](https://www.bilibili.com/video/BV1J8411K7pj/)     |     |
|     | 推理系统     | 02 什么是推理系统        | [slide](./Inference/02.constraints.pdf), [video](https://www.bilibili.com/video/BV1nY4y1f7G5/)      |     |
|     | 推理系统     | 03 推理流程全景         | [slide](./Inference/03.workflow.pdf), [video](https://www.bilibili.com/video/BV1M24y1v7rK/)         |     |
|     | 推理系统     | 04 推理系统架构         | [slide](./Inference/04.system.pdf), [video](https://www.bilibili.com/video/BV1Gv4y1i7Tw/)           |     |
|     | 推理系统     | 05(上) 推理引擎架构      | [slide](./Inference/05.inference.pdf), [video](https://www.bilibili.com/video/BV1Mx4y137Er/)        |     |
|     | 推理系统     | 05(下) 推理引擎架构      | [slide](./Inference/06.architecture.pdf), [video](https://www.bilibili.com/video/BV1FG4y1C7Mn/)     |     |
|     |          |                   |                                                                                                     |     |
| 2   | 模型小型化    | 01 推理参数了解         | [slide](./Mobilenet/01.introduction.pdf), [video](https://www.bilibili.com/video/BV1KW4y1G75J/)     |     |
|     | 模型小型化    | 02(上) CNN模型小型化    | [slide](./Mobilenet/02.cnn.pdf), [video](https://www.bilibili.com/video/BV1Y84y1b7xj/)              |     |
|     | 模型小型化    | 02(下) CNN模型小型化    | [slide](./Mobilenet/02.cnn.pdf), [video](https://www.bilibili.com/video/BV1DK411k7qt/)              |     |
|     | 模型小型化    | 03 Transformer小型化 | [slide](./Mobilenet/03.transform.pdf), [video](https://www.bilibili.com/video/BV19d4y1V7ou/)        |     |
|     |          |                   |                                                                                                     |     |
| 3   | 模型压缩     | 01 基本介绍           | [slide](./Slim/01.introduction.pdf), [video](https://www.bilibili.com/video/BV1384y187tL/)          |     |
|     | 模型压缩     | 02 低比特量化原理        | [slide](./Slim/02.quant.pdf), [video](https://www.bilibili.com/video/BV1VD4y1n7AR/)                 |     |
|     | 模型压缩     | 03 感知量化训练 QAT     | [slide](./Slim/03.qat.pdf), [video](https://www.bilibili.com/video/BV1s8411w7b9/)                   |     |
|     | 模型压缩     | 04 训练后量化PTQ与部署    | [slide](./Slim/04.ptq.pdf), [video](https://www.bilibili.com/video/BV1HD4y1n7E1/)                   |     |
|     | 模型压缩     | 05 模型剪枝           | [slide](./Slim/05.pruning.pdf), [video](https://www.bilibili.com/video/BV1y34y1Z7KQ/)               |     |
|     | 模型压缩     | 06(上) 知识蒸馏原理      | [slide](./Slim/06.distillation.pdf), [video](https://www.bilibili.com/video/BV1My4y197Tf/)          |     |
|     | 模型压缩     | 06(下) 知识蒸馏算法      | [slide](./Slim/06.distillation.pdf), [video](https://www.bilibili.com/video/BV1vA411d7MF/)          |     |
|     |          |                   |                                                                                                     |     |
| 4   | 模型转换&优化  | 01 基本介绍           | [slide](./Converter/01.introduction.pdf), [video](https://www.bilibili.com/video/BV1724y1z7ep/)     |     |
| 4.1 | 模型转换模块   | 02 架构与文件格式        | [slide](./Converter/02.converter_princ.pdf), [video](https://www.bilibili.com/video/BV13P4y167sr/)  |     |
|     | 模型转换模块   | 03 自定义计算图IR       | [slide](./Converter/03.converter_ir.pdf), [video](https://www.bilibili.com/video/BV1rx4y177R9/)     |     |
|     | 模型转换模块   | 04 流程细节           | [slide](./Converter/04.converter_detail.pdf), [video](https://www.bilibili.com/video/BV13341197zU/) |     |
| 4.2 | 图优化模块    | 05 计算图优化策略        | [slide](./Converter/05.optimizer.pdf), [video](https://www.bilibili.com/video/BV1g84y1L7tF/)        |     |
|     | 图优化模块    | 06 常量折叠&冗余节点消除    | [slide](./Converter/06.basic.pdf), [video](https://www.bilibili.com/video/BV1fA411r7hr/)            |     |
|     | 图优化模块    | 07 算子融合/替换/前移     | [slide](./Converter/06.basic.pdf), [video](https://www.bilibili.com/video/BV1Qj411T7Ef/)            |     |
|     | 图优化模块    | 08 数据布局转换&内存优化    | [slide](./Converter/07.extend.pdf), [video](https://www.bilibili.com/video/BV1Ae4y1N7u7/)           |     |
|     |          |                   |                                                                                                     |     |
| 5   | Kernel优化 | 01 Kernel优化架构     | [slide](./Kernel/01.introduction.pdf), [video](https://www.bilibili.com/video/BV1Ze4y1c7Bb/)        |     |
|     | Kernel优化 | 02 卷积操作基础原理       | [slide](./Kernel/02.conv.pdf),[video](https://www.bilibili.com/video/BV1No4y1e7KX/)                 |     |
|     | Kernel优化 | 03 Im2Col算法       | [slide](./Kernel/03.im2col.pdf),[video](https://www.bilibili.com/video/BV1Ys4y1o7XW/)               |     |
|     | Kernel优化 | 04 Winograd算法     | [slide](./Kernel/04.winograd.pdf),[video](https://www.bilibili.com/video/BV1vv4y1Y7sc/)             |     |
|     | Kernel优化 | 05 QNNPack算法      | [slide](./Kernel/05.qnnpack.pdf),[video](https://www.bilibili.com/video/BV1ms4y1o7ki/)              |     |
|     | Kernel优化 | 06 推理内存布局         | [slide](./Kernel/06.memory.pdf),  [video](https://www.bilibili.com/video/BV1eX4y1X7mL/)             |     |
|     | Kernel优化 | 07 nc4hw4内存排布     | [slide](./Kernel/07.nc4hw4.pdf),  [video]()                                                         |     |
|     | Kernel优化 | 08 汇编与循环优化        | [slide](./Kernel/08.others.pdf),  [video]()                                                         |     |

## 目标学员

1. 对人工智能、深度学习感兴趣的学员

2. 渴望学习当今最热门最前沿AI技术的学员

3. 想储备深度学习技能的学员

4. AI系统开发工程师
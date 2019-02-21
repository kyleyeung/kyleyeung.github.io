---
title: 什么是计算社会科学？
date: 2018-06-21 16:18:45
categories: Computational Social Science
tags:
comments: true
---
计算社会科学是一个以计算为媒介，对社会科学问题进行研究的学科。

Claudio Cioffi-Revilla在其所著的*Introdcution to Computational Social Science: Principles and Appilcations*一书中，给出定义如下：

> The new field of **Computaional Social Science** can be defined as the interdisciplinary investigation of the social universe on many scales, ranging from individual actors to the largest groupings, through the medium of computation.

这是一个较为宽泛但准确的定义。同时参考*Wikipedia*上Computational Social Science词条：

> **Computational social science** refers to the academic sub-disciplines concerned with computational approaches to the social sciences. This means that computers are used to model, simulate, and analyze social phenomena.



计算社会科学的主要研究领域：

1. 自动化社会信息提取（Automatd Social Information Extraction）

   早期的社会科学家从广播、报纸或者其他出版物等来源收集数据，现在这些工作都可以通过计算化工具来完成（自然语言处理、语音识别技术等）。使用自动化计算方法提取出的社会信息有两个用处：

   其一，从影响、活动或其他研究者感兴趣的维度来分析数据内容：例如对演讲、立法委员会证词或其他公开记录进行计算化内容分析，提取出关于领导人或其他政府官员的政治倾向信息。即，在有明确衡量标准的前提下，通过数据分析来得出结论。

   其二，信息提取算法可被用来对存在于原始数据中，但难以通过人类手工过程发现的网络或其他结构进行建模：例如使用计算内容分析方法和文本挖掘技术，对法院卷宗及其他描述了与犯罪个体相关的个人、日期、位置、时间、属性的证据性法律文本进行分析，从而对犯罪组织及其非法活动建立模型。这更类似于模式识别（Pattern Recognition），即在不明确具体目标的情况下，通过计算化手段建立模式。

2. 社会网络(Social Networks)

   随着社交媒体（Facebook, Twitter等）的飞速发展，该领域近年来也随之热门起来。例如[这项研究](https://news.vice.com/en_us/article/d3xamx/journalists-and-trump-voters-live-in-separate-online-bubbles-mit-analysis-shows)

   实际上在Big Five领域中（现代人类学，经济学，政治科学，心理学和社会学）对网络的分析早已有之，社会网络分析同时也是CSS研究中唯一具有完备文本历史（well-documented history）的领域。社会网络分析不仅有其内在价值，同时也对其他计算社会科学领域的理论和研究起到了推动作用。

3. 社会复杂性(Social Complexity)

   计算社会科学的研究基石之一便是对社会复杂性的理解，同时该领域在帮助人们深入理解社会复杂性与文明之起源时发挥的作用也变得日益显著。计算社会科学的理论包含了很广泛的社会科学概念，比如decision-making、coalition theory等等；与此同时，例如non-equilibrium distributions, power laws这样与当代科学高度相关的理论也被包含在了计算社会科学对社会复杂性的研究之中。

4. 社会模拟建模(Social Simulation Modeling)

   社会科学领域的模拟建模开始于数十年前，而计算社会科学方向最早的该类方法是系统动力学模型（system dynamics models）。这种基于差分或微分方程的模型取决于条件和数据的需求。另一种主流传统模型是排队模型(queuing models)，其使用排队论和若干种概率分布来描述获得服务的实体（如顾客，患者，客人）、服务时长或者其他服务过程中的概率特性。当然，还有元胞自动机（cellular automata）这样面向对象的模型，基于agent的模型和演化计算模型（evolutionary computaion models）等等。



至于该领域的著名研究者，可以参考09年Science上的[这篇文章](https://gking.harvard.edu/files/LazPenAda09.pdf),顺着作者列表看一遍会比较清楚：比如MIT的Alex Pentland；必须要提及的还有Duncan watts,在该领域做了相当长时间的研究，youtube上可以找到不少他的讲座；同时，Jennifer Pan也是目前该领域的学术新星，但她隶属于斯坦福Department of Communication，这也体现出该方向的研究者实际上大多分布于各个相关领域之中，有独立CSS department的大学和机构目前并不多见。
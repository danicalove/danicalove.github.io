---
layout: post
title:  "高通量检测病毒的方法研究"
date:   2017-04-30 14:14:54
categories: virus detection highthroughout
tags: virus 
---

* content
{:toc}

传统方法的技术限制和耗时是亚病原体（类病毒和卫星病毒等）的发现比率较低的主要原因。在先前的研究中我们开发了一种新的非同源依赖的方法(PFOR)，这种方法能够高通量测序数据获得的植物环状RNA病原体。PFOR方法能够识别具有独立感染能力的新的病原体，但是，PFOR方法存在局限，首先改方法是用PERL语言编写，从而导致运行速率比较慢，而且也不够识别不能识别宿主中的small RNAs低含量的亚病原体。在本研究中，通过对PFOR进行优化和改进，获得了具有并行处理能力的快速运算程序PFOR2（C++改写），其运行速度比PFOR提高了3~8倍，从而能够处理高通路测序数据。除此之外，还开发了一个新的算法（SLS算法，该算法可以实现序列的虚拟分割），并且整合到了PFOR2中，从而是PFOR2不仅可以用于small RNA数据还可以用于long RNA数据来识别新的病毒。运用PFOR2分别在葡萄和苹果锈果病样品中发现Grapevine latent viroid (GLVd)和Apple hammerhead viroid-like RNA (AHVd-like RNA)。GLVd包含了类病毒中的常见的结构元件并且能够独立感染葡萄种子，所以可以认为是Apscaviroid属中一种新的species。AHVd-like RNA在正链和负链均可以编码具有生物活性hammerhead ribozyme，与在苹果植物中发现的病毒没有特异的相关性。总之，PFOR2能够促进植物，无脊椎动物和脊椎动物中新circular RNA的鉴定，而不必考虑small RNA在宿主中的含量或者是是否发生复制。






## Introduction

类病毒和卫星病毒是一类单链环状RNA分子，由220到457核苷酸分子组成。亚病毒RNAs（类病毒和卫星病毒等）不能编码蛋白质且病毒的复制需要宿主编码的DNA依赖的RNA聚合酶（类病毒）或者是辅助病毒编码的RNA 依赖性RNA 聚合酶（卫星病毒）。从古细菌到人类都广泛存在non-replicating circular RNAs。然而目前对于circRNA的研究还十分有限，所以功能尚不明确，有研究表明circRNA在基因调节和表达等方面发挥着重要功能，比如说充当microRNA海绵体。
类病毒能够感染大多数作物并且造成宿主有严重的感染症状。但是，replicating cicular RNAs的发现率低于病毒的发现率。例如，自从1971年第一种类病毒物种到现在已有差不多40中类病毒物种。类病毒的发现发展缓慢的原因主要是由于纯化技术方面和如何在感染宿主中识别低含量病毒的原因。在前面的研究中提出了一个新的算法（PFOR）, 这种算法不依赖于已知类病毒遗传物质的同源性，而是基于环状RNA的结构特征而开发的非同源依赖的类病毒鉴定方法。PFOR通过逐渐将移除非重叠的small RNAs并保留那些重叠但是又不能组装成一个direct repeat RNA得到的类病毒特异的siRNAs进行基因组的组装。将PFOR方法运用于grapevine small RNA library 中发现一个375个核苷酸分子的拟病毒，这种拟病毒正链和负链均能够编码活性hammerhead ribozymes。但是，该拟病毒是否能够进行独立的感染还不确定。
PFOR（perl语言编写）有两大主要的限制。首先，small RNA的过滤步骤耗时较长，大约占据PFOR算法整个的90%的时间；其次，如果circRNAs发生复制或者是被真核细胞中的Dicer切割产生siRNA则PFOR不能识别cicRNAs.
在本研究中，通过对PFOR的算法进行了优化和改进，获得了具有并行处理能力的快速运算程序PFOR2。PFOR2既能够识别replicate的circRNA又能识别siRNAs，从而加快新circular RNAs的发现和known host species的范围。

### Methods

PFOR方法步骤：

第一步：移除与任何small RNAs均没有重叠的non-overlapping small RNA,保留overlapping small RNAs
第二步，将overlapping small RNAs分为2类，一类是TSR，定义为在该small RNA的一端并且只有一端与至少一个别的small RNA存在重叠（重叠的碱基数是该算法的一个参数），另外一类是ISR，定义为该small RNA的两端与至少一个别的small RNA存在重叠区域。一步一步迭代移除TSR，直到剩下的small RNA为True ISRs.
Notes:TSR和True ISRs是不同的，分别为上图的红色字1和2；TSR为线性的dsRNAJ经过Dicer nuclease（一种酶）切割后产生的siRNA，这种small RNA在后面迭代的过滤过程中会被全移除，而True TSR（the presence of the small RNAs derived from the junction，如上图的右上角中的序列read 9 和read 10）为来自于incomplete or complete head-to-tail direct repeat dsRNA或者是circular RNA的small RNAs，最后只有True TSR能够被用于后续的组装过程。
第三步，将Trur ISRs组装成contigs
在PFOR2中，加入了一个新的程序(SLS,下图紫色箭头所指),可以实现long RNA的虚拟分割得到small RNA.

 
PFOR2流程图

(A) Flowchart showing the steps followed by PFOR2 for filtering all non-overlapping small RNAs (singleton) and terminal small RNAs (TSRs) with multi-thread processing and the assembly of the remaining true internal small RNAs (ISRs). (B) Schematic view of the virtual dicing of longer RNA reads into small overlapping sRNAs by SLS. (C) Closeup view of the filtering criterion of TSRs implemented in PFOR2. Each sRNA in the small fragments pool is placed into one of four groups based on the presence or absence of 5’ and 3’ overlapping sRNAs
在本研究中，还有运用实验验证和一些结构预测的软件来证明算法的分析效能。

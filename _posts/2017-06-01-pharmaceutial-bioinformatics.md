---
layout: post
title: "药物生物信息学笔记"
data: 2017-05-31 22:15:11
categories: pharmaceutial bioinformatics
tags: pharmaceutial bioinformatics
---

* content
{:toc}


药物生物信息学，大概，是一门神学。

![hh](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFzic25VhvzJMdm5uY4DVue18r8DGeAibdOQWyacfsibS8nUthvjy70qNH3ZordtxRnBuARpP6icnlLaSw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)











# 绪论
全基因组水平分析药物效应和毒性的遗传标记
个体遗传差异对药物反应不同

单核苷酸多态性：SNP 基因组上单个核苷酸的变异包括转换、颠换、缺失或插入 形成的遗传标记分子

## 遗传基础
遗传多态性      

基因缺失变异和基因重复序列变异
                         
表观遗传

## 药物设计：确定靶标（疾病、药靶、适当的检测方法）    寻找先导化合物    优化先导化合物     临床前试验    临床试验

合理药物设计：对已知药物进行化学改进、基于受体的药物设计、基于配体的药物设计

**基于受体的药物设计
 
**基于配体的药物设计
 

## 生物信息学在药物研发中的作用
①先导化合物筛选 ②靶蛋白发现 ③药物作用机制 ④临床统计分析

# 药靶发现分析技术

药靶：药物与机体生物大分子的结合部位

药靶发现相关数据库 HMDB（human metabolome data   pathway analysis drugbank pdb pharmgkb）
 

# 药物作用与基因表达分析1（基于基因芯片技术）

先导化合物的筛选

高通量筛选法（HTS）：首先选择分子靶、建立表达体系、快捷、高效率的生物检测

药物相关基因表达谱分析

Connectivity map (cmap) the discoverty of functional connections between drugs,genes and diseases through the transitory feature of gene-expression changes
![](https://d2ufo47lrtsv5s.cloudfront.net/content/sci/313/5795/1929/F1.medium.gif)
[link](https://portals.broadinstitute.org/cmap/permuted_result.jsp?resultDescId=154043s)
# 药物作用与基因表达分析（基于测序技术）
HTS：自动操作系统（微孔板 堆栈） 样品库 检测系统（荧光分析，放射性同位素） 数据处理设备（数据库管理系统） 其他处理设备


## 筛选模型   

基于分子水平模型（受体筛选模型 离子通道筛选模型 酶筛选模型）

基于细胞水平模型（报告基因测定）
 
活性物Activies

活性样品Hits

先导化合物Leads

HTS具体过程：初筛 复筛 深入筛选 确证筛选 

虚拟HTS（vHTS）：根据分子识别模型：基于配体的方法 基于受体的方法

# 基于测序技术的药物作用基因表达分析

基因表达谱在药物研发中的作用：寻找具有治疗作用的基因产物或能够作为靶点的基因或基因产物

组织表达谱：进行全部或者部分组织的表达谱分析  获得的表达谱信息可以用于药物研发多个领域（如 确定靶点是否具有组织特异性）

副作用谱 （从转录层面理解某一化学实体是否有潜在毒理学属性）

药物基因组学（特征性的差异表达基因）

生物标志物（客观评价用于评价是否某种治疗是否能够影响相应的靶点并治疗了症状和疾病）


 
 

# 药物作用与SNP关联分析1

SNP：不同个体DNA序列上的单个碱基差异称为单核苷酸多态性（SNP）

锁链块：染色体上距离越近的SNP越倾向于以一个整体遗传给后代，这样位于某一区域的一组相互关联的SNP成为一个锁链块

关联研究通过检验某个特定的等位在疾病组和对照组中出现的频率差异来判断此等位是否是疾病的易感等位，发现与该等位呈链锁不平衡的致病基因。

##	SNP关联分析在药物研发中的作用
	高新型药物筛选的准确性  在个体化治疗中的应用 （用药剂量）
## 相关工具
通用生物学数据库（dbSNP 癌症SNP数据库（CaSNP））

		  药物相关数据库：DrugBank PharmGKB VnD Open Target


		  药物研发中的计算方法 没有靶的结构信息且不能预测其单位结构的情况下 基于配体的药物发现方法（QSAR）

		  反之使用基于结构的药物发现方法：蛋白质结构确定（实验方法：X射线晶体衍射、NMR、cryoEM(冷冻电镜术)、计算机模拟预测（同源建模（Modeller）、折叠识别(MUSTER)、从头建模(基于序列进行结构建模（QUARK）)、蛋白质和小分子数据库(ZINC)））、分子对接、从头配体设计


# 药物作用与SNP关联分析2（SNP关联分析在药物评价和开发中的应用和相关工具介绍）
药物研发中的计算方法：分子对接：预测两个或多个分子如何结合到一起形成稳定复合物（原则：几何互补、能量互补及化学环境互补来评价药物和受体相互作用的好坏，并找出两个分子之间最佳的结合模式）

种类：刚性对接 半柔性对接 柔性对接
 从头配体设计 

打分
   
## 常用分子对接工具：DOCK
Glide
GOLD
AutoDock
AutoDock Vina
FlexX
MOE
从头配体设计
相对于vHTS方法 从头配体设计对于较大的库产生候选药物的成功率低
常用工具：
BOMB(Biochemical and Organic Modelling Builder)
SPROUT
LigMerge
AutoGrow
SilcsBio
LigBuilder

# SNP相关工具2 ：HGMD FASTSNP SNP3D
## SNP选取原则 
优先功能性SNPs
   	外显子上的错义或同义突变位点
每一个外显子两端和内含子中潜在的剪切位点
基因3'或5'端
优先标志性SNPs
可包含多个SNP信息
优先常见的SNPs
弱势等位基因频率>5%

P53上所有SNP 根据SNP 推测其生物学功能
SNP功能注释工具：GoldenPath基因组浏览器 
Ensembl 基因组注释系统集成了SNP注释子系统 用BioMart系统对感兴趣的SNP数据进行数据挖掘  nsSNP功能注释 
人类SNP、突变数据库 （dbSNP）HumanGenome(人类基因组突变数据库) HGMD（人类基因突变数据库）、其他SNP数据库 （SWISS-PORT蛋白质数据库中包含了丰富的与蛋白质对应的SNP数据Swiss-Prot蛋白质数据库中包含了丰富的与蛋白质对应的SNP数据;基因组数据库(GDB)存放了大量中性突变数据;HapMapConsortium数据库是主要的SNP单倍型数据存放地;人类孟德尔遗传数据库(OMIM)存有丰富的关于人类基因和基因突变的数据）

# 药物作用通路分析1
通路分析：功能富集分析  检测与对照组比较的样本中相关的基因的变化
Omics 
 

## 通路分析方法：输入相（建立假设）--分析相--输出相 
通路分析方法 First generation :over representation analysis :如果在给定通路中差异表达的基因比例超过随机期望基因的比例 ，就能检测到相关的通路
 
## 常用统计学方法：超几何分布 卡方检验 二项概率 fisher精确检验 多重检验校正
适用于系统生物学研究
GoMiner(fisher exact test )  WebGestalt 



## Functional class scoring 
常用统计学方法：基准水平统计（差异倍数 t检验 对数似然比 信噪比）
通路水平统计（ks统计 wilcoxon sun rank statistic max-mean statistic 卡方检验）
GSEA 数据类型：micoarray data ks检验 Pathifier (PCA)

Third generation:pathway-topology based
通过拓扑结构中发现的相互作用，通过通路数据库进行注释，支撑了解释通路组分相关变化的信息
 




Pathway-express  SIGA PARADIGM

基于配体药物设计 依赖于结合到靶上的小分子化合物的了解
常用方法：药效集团(分子呈现特定生物活性必须具备的一种分子片段或形态特征  Catalyst软件包)建模 分子相似性方法 （结构相似的化合物倾向于有相似的结合属性）定量构效关系方法（QSAR）：通过数理统计方法建立一系列化合物的生理活性或某种性质
 
# 药物作用通路分析2

伊马替尼 治疗慢性髓性白血病
CML诊断标准：费城染色体（9号和22号染色体） BCR-ABL融合基因阳性确诊


蛋白酪氨酸激酶（PTK）抑制剂

# 药物代谢网络分析1
## 药物代谢网络的识别

药物代谢酶类来自细胞色素P450家族
代谢通路：在生化中，细胞中连接起来的一系列的化学反应被称为代谢通路

## 代谢网络的流分析和网络分析
代谢流：物质在代谢网络中沿网络进行的活动
代谢流分析：采用优化的过程来定量所有已知催化和转录互作的细胞内代谢流
代谢平衡：估计绝对流
同位素平衡：估计相对流
数学建模：基于C13流分析 基于约束的流分析
## 代谢网络分析：结合代谢平衡和标志实验来观察其特征
通路确定：流的量化描述依赖于初级代谢中活化的通路
代谢沟道效应
通路区域化
双向流


药物代谢网络在药物研发中的应用
# 药物代谢网络
## 隐马可夫模型：双重随机过程：马尔科夫链和一般随机过程
在生物信息学中应用：基因识别： HMMgene GENESCAN  GeneMark 
					序列比对：HMMER
					
##  petri网：可以用网状图形表示的系统模型 应用：代谢通路建模
##  细胞网络启发计算模型：细胞自动机 平行计算 局部的 一致性的 

人类代谢组数据库HMDB 

# 合成生物学
工程化三原则：标准化 抽象化 复杂系统区耦合

# 药物基因组学数据库

# 药物作用与表达、通路分析
处理探针芯片数据得到差异表达探针表达谱

CMAP 数据库 将差异表达探针分别形成两个列表，上调探针列表和下调探针列表，将列表中探针按照表达量的log2FC绝对值从高到低排序，进行quick query 

探针ID转换
方法1：直接下载两种芯片的CDF文件 将两者作交集，仅保留重叠部分的探针  工具：DAVID 或自写脚本
方法2：NetAffx
NetAffy analysis center 使用batch query HG-U133A 
---
layout: post
title: "novoBreak--sv detection"
date: 2017-06-06 22:22:22
categories: novoBreak tool
tags: tool 
---

* content
{:toc}

novoBreak简介

![](http://pic0.v4.cc/ps/mmbiz.qpic.cn/mmbiz_jpg/884XsHGHibP4LR1zibkXyMHp5gvhia2v9jLpqg3fEg9BezLHXXlMdEpic8f6bicJdhekspYxibbqdY4OtvibkOmo4JibPA/0?wx_fmt=jpeg)











# novoBreak
novobreak:检测全基因组测序数据中体细胞和生殖细胞结构突变breakpoint的全基因组局部组装算法 在体细胞识别方面表现突出 主要是因为它更充分利用跨breakpoints的reads,novobreak在识别小的缺失和插入（indel）方面的敏感性也更好

肿瘤中散发和复发的染色体畸变

通过测序技术，基于碱基对的方法检测全基因组范围的结构突变已经变得可能，但是计算方法有一定的灵敏度的限制 并且只能用于不扽SV类型

一种SV检测方法是把双末端短读段比对到参考基因组上识别 discordant read pairs ,read depth,split reads的信号

另一种检测方法依赖于 target local assembly 比对上的和在候选SV区域部分比对上的reads(在基因组序列中鉴定indel的新方法 Scalpel，能够在现有基因组数据中挖掘插入和缺失突变，该方法先将指定基因组区域的所有序列聚集起来，然后为这个区域生成新的序列比对，)这些方法都忽略了跨breakpoint 读段和完全和参考基因组不同的读段，但是全基因组的组装方法很少有偏向性，但是 组装基因组计算量比较大，结果通常被一些repeats,polyploidy,测序深度和测序覆盖度所影响

novobreak全基因组范围内breakpoints局部组装，组装基于共享一组k-mers在sujuect genome比如肿瘤基因组出现但不在参考基因组或对照数据里的 reads,

当被用于肿瘤组织和其相应的正常组织时，novobreak创建一个包含所有k-mer reads，以及host reads和k-mer出现频率的哈希表。接下来，过滤掉代表reference alleles或者测序错误的k-mer,保留那些不在参考基因组中出现代表variant or novel sequences 。然后搜索正常读段并且把这些k-mers分成germline k-mers和somatic k-mers。然后novobreaK识别跨越每个体细胞breakpoint的read pairs并且把每一个culster of reads组装成contig。将这些contig和基因组进行比较，novobreak识别breakpoints和相应的sv。最后，对每个breakpoint进行可信度分析。

相比于基于比对的方法,novobreak更有效地利用跨insertion breakpoints的reads，，对被DELLY和Manta检测忽略的SV位点的检测表明novobreak在对低覆盖区域的检测也比较好，

基于一个全基因组分类和过滤策略

## pipline
### 1. novoBreak begins with an indexing and filtering procedure to obtain 'novo k-mers; and associated short reads, described in the section “Indexing and filtering k-mers”

Given a sequence S of length L, a k-mer is a length k (k < L) substring of sequence S. We notice that if a read R contains a breakpoint of a structural change with respect to the reference or the normal genome of a cancer patient, there are at most k − 1 k-mers (k < |R|) covering the breakpoint. The default k is 31 (Supplementary Note 1) in novoBreak. We define these k-mers as 'novo k-mers' because they contain novel sequence information specific to the subject. In a tumor-normal paired cancer genome sequencing study, the novo k-mers contain the somatic breakpoints that specifically exist in the tumor but not in the paired normal sample. The first critical step of novoBreak is to obtain the novo k-mers. An effective approach for this is to implement a hash table that first indexes and loads all the k-mers in all the reads in the tumor sample into the memory and then eliminate k-mers that are present in the reference or the normal genome. The remaining high-frequency k-mers should contain genuine somatic breakpoints, including SNVs, small indels, and large SVs. This approach is computationally feasible for whole-exome or whole-transcriptome analysis. But for high-coverage whole-genome analysis, the memory cost is extremely high (usually a few hundred gigabytes) mainly because of the presence of sequencing errors. A critical component of novoBreak is the reduction of memory consumption. For whole-genome sequencing data, instead of indexing the sequenced reads, novoBreak starts from hashing all the k-mers in the reference genome. Then, it adopts a two-pass approach to calculate novo k-mers in the sequenced genomes. The first pass scans every reads and marks the status (presence or absence) of each constituent k-mer in the reference genome using the preconstructed hash table. In the process, novoBreak automatically trims off error-prone ends in low-quality reads (Supplementary Note 1). novoBreak uses a bit array data structure to mark a read. If a k-mer in a read is in the hash table, it will be marked as 1 (otherwise 0) in the corresponding bit in the bit array. When all the reads are processed, the hash table for the reference k-mers is released. Next, novoBreak goes through the reads containing at least one 0 bit to obtain the minimal occurrence of the nonreference k-mers. novoBreak adopts Bloom filter21, a probabilistic data structure that tests whether a given element is in a set. A Bloom filter is a bit array of m bits, initialized to be 0. k different hash functions are applied to an element and map the element to k different positions in the array. To add an element, these k positions will be set to 1. To test whether an element is in the set, each of the k positions will be examined. If there is a 0 at any of the k positions, the element is definitely not in the set. If all the k positions are 1, then either the element is in the set or the positions were coincidently set to 1 by other elements. Such false positive (FP) errors could happen because different elements could be coincidently hashed to same positions in the bit array. Fortunately, the chance of having an error is very small, less than

 ![image](https://www.nature.com/nmeth/journal/v14/n1/images/nmeth.4084-M1.gif)
 
where n is the total number of elements, m is the size of the bit array of the Bloom filter, and k is the number of hash functions. Note these rare FP errors do not hurt sensitivity and have negligible possibility of introducing FP breakpoints on account of the subsequent read clustering, assembly, alignment, and variant calling steps. We expand the above standard Bloom filter from one bit to two or more (default to three bits in novoBreak) to count if a k-mer has occurred more than a minimal number of times (default three in novoBreak; Supplementary Note 1) in the data set. Thus, k-mers introduced by sequencing errors will be automatically disregarded, and the remaining are novo k-mers from the variant alleles. For somatic analysis, novoBreak will further scan the normal control reads using a hash table and count the occurrence of these k-mers in the normal reads. Based on these counts, candidate somatic k-mers (i.e., k-mers only present in the tumor but not the normal sample) can be identified, with the effect of cross-contamination between the samples being accounted for. Finally, novoBreak loads read pairs containing the candidate somatic k-mers and automatically removes duplicated read pairs that have identical sequences in both reads.

### 含有相同novo k-mers集的双端读段被聚类在一起。每个簇包含相同的novo k-mers。然后将每个簇组装起来构成一个跨breakpoint序列。

### cluster local assembly

![image](https://www.nature.com/nmeth/journal/v14/n1/images_supplementary/nmeth.4084-SF2.jpg)

在每个breakpoint处，有k-1个 k-mers 覆盖了这个breakpoint，所以存在好几个覆盖这个断点的k-mer;另一方面，如果覆盖度足够深的话，也应该有几个有共同k-mers的read-pairs，用一个union-find的算法来完成这一步骤，可以在一个有reads和k-mers以及它们之间的联系所组成的无向图识别处所有的connected components。为了避免由于重复和测序错误导致的大的cluster,novobreak基于读段和k-mer的统计修剪connected components,为了减少计算时间，直接从bam文件读取，基于高质量的比对读段纠正碱基错误。

聚类以后，直接对每个簇记性组装，novobreak用SSAKE这个工具进行组装工作

SSAKE从每个簇产生多个contuig.每个contig是由BWA-MEM得到并且独立分析的。这样以后就产生了一些候选breakpoint,接下来合并它们创建一个唯一的SVs集

### 每个短读段在每个cluster中组装后，得到的contig用BWA-MEM比对到参考序列上，比对时加上-M参数mark shorter split hits as secondary允许二次比对，从比对结果中腿短断点和相关的SV，对于小的sv,比如indel，novobreak直接解析比对的contig的CIGAR字符串，对于大的SV，考虑两次contig比对的结果，目前novobreak可以检测 缺失，插入，转换，重复记忆碱基重定位。为了获得一个较高的精度，novobreak应用了一个打分过滤模型 

对检测到的breakpoint进行评分和排序，对于一个给定的locus（染色体位置），novobreaK计算一个统计学意义上的质量分数

![image](https://www.nature.com/nmeth/journal/v14/n1/images/nmeth.4084-M2.gif)

D值分别由支持来自肿瘤和正常样本数据的参考等位(reference alelle)和变异等位(vairiant alelle)的read pairs的counts值组成，G取0,1,2，分别表示 在肿瘤和正常样本中均没有SV突变 只在肿瘤样本中有SV突变 在肿瘤和正常样本中都有SV突变

计算数据的似然性，β二项分布估计似然值。





# C源代码
source code:源代码 需要自己编译成可执行软件
binary distribution:编译好的版本，可执行。

依靠SSAKE算法进行local 组装，bwa-mem进行contig mapping，samtools和picard(SamToFastq)来 
提取读段


Sam to fasta :cat *.sam | awk ‘{print “>”$1”\n”$10}’ > *.fasta
肿瘤样本的双末端读段被分割成k-mers.对k-mers进行索引，并与参考基因组序列和正常样本的读段序列进行比对

该k-mer
在正常样本读段序列上比对上，没有在参考序列中比对上的称为germline k-mers
在正常样本读段序列和参考序列上都不存在，称为somatic k-mers
过滤筛选：与参考基因组等位基因相同的、有测序错误的k-mers,germline k-mers


SSAKE：组装 将含有相同k-mers的读段聚类到一起，这些读段都包含了相同的断点i。这样，针对不同的读段就会形成多个不同的读段簇，然后对这些读段簇中的读段进行组装。

BWA-MEM比对：将组装好的长序列与参考序列进行比对，推断出准确的断点以及相应的SV，对每个断点进行评分、排序和输出。

模块化设计可以方便地更换不同的比对器和组装器

---
layout: post
title:  "high-throught-out sequences"
date:   2017-04-29 10:48:54
categories: 高通量测序技术与原理
tags: sequences
excerpt: 测序
---

##NGS生物信息学分析流程标准
质控分析（质量评估，去接头序列，去低质量序列、去重复序列）、序列比对、变异鉴定


Phred改进正确率 标准评估
Corss_match识别并删除read末端载体序列
Phrap组装contig
Consed查看编辑phrap组装结果

测序策略：1.hierarchical Sequencing of Strategy (BAC-by-BAC)自上而下
Step1:杂交 2.DIGESTION 酶切 2.鸟枪法测序

##二代测序原理
文库制备（小片段末端添加接头序列） PCR乳化扩增： 测序（454焦磷酸测序仪  de novo从头测序）结果文件SFF文件（FLOWER读取）

Illumina测序 文库制备 桥式PCR扩增 测序边合成边测序 可逆终止物 输出文件：fastq文件（重测序）

Solid荧光测序

Lon Torrent半导体测序（半导体传感器记录PH变化判定核苷酸模型）
PacBio SMRT   OXford Nanopore测序文库不需要扩增 单分子测序(三代测序技术)
<img src="C:/Users/Administrator/Desktop/pic/1.png" alt="alt text">


##测序数据预处理及质量控制

Mate pair序列 
判断质量分数编码方式33？64 fastqc  fataq是基于文本的，保存生物序列和其测序质量信息的标准格式 用ASCII字符表示 将fasta文件与质量数据放到一起
测序文件格式：illumina  .fastq格式
三代测序错误率高 用illumina等高通量短序列来校正
测序数据预处理：1.碱基质量分数核苷酸分布情况可视化 （FastQC） 2.基于碱基质量分数及序列引物进行读段的修剪及过滤  
切除污染序列 去除低质量序列（平均phred分值）
修剪和过滤序列：trimmomatic Cutadapt(不支持paired end)

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```



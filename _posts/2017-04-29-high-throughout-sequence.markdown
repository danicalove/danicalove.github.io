---
layout: post
title:  "high-throught-out sequences"
date:   2017-04-29 10:48:54
categories: 高通量测序技术与原理
tags: sequences
excerpt: 测序
---

## NGS生物信息学分析流程标准
质控分析（质量评估，去接头序列，去低质量序列、去重复序列）、序列比对、变异鉴定


Phred改进正确率 标准评估
Corss_match识别并删除read末端载体序列
Phrap组装contig
Consed查看编辑phrap组装结果

测序策略：1.hierarchical Sequencing of Strategy (BAC-by-BAC)自上而下
Step1:杂交 2.DIGESTION 酶切 2.鸟枪法测序

## 二代测序原理
文库制备（小片段末端添加接头序列） PCR乳化扩增： 测序（454焦磷酸测序仪  de novo从头测序）结果文件SFF文件（FLOWER读取）

Illumina测序 文库制备 桥式PCR扩增 测序边合成边测序 可逆终止物 输出文件：fastq文件（重测序）

Solid荧光测序

Lon Torrent半导体测序（半导体传感器记录PH变化判定核苷酸模型）
PacBio SMRT   OXford Nanopore测序文库不需要扩增 单分子测序(三代测序技术)



## 测序数据预处理及质量控制

Mate pair序列 
判断质量分数编码方式33？64 fastqc  fataq是基于文本的，保存生物序列和其测序质量信息的标准格式 用ASCII字符表示 将fasta文件与质量数据放到一起
测序文件格式：illumina  .fastq格式
三代测序错误率高 用illumina等高通量短序列来校正
测序数据预处理：1.碱基质量分数核苷酸分布情况可视化 （FastQC） 2.基于碱基质量分数及序列引物进行读段的修剪及过滤  
切除污染序列 去除低质量序列（平均phred分值）
修剪和过滤序列：trimmomatic Cutadapt(不支持paired end)

## 基因组测序数据分析

从头测序，重测序（根据参考基因组是否已知）
全基因组测序，靶向测序（根据所测基因组范围）
测序内容：文库制备及测序 基因组组装及注释 基因组重测序分析 比较基因组及功能基因组

从头测序：用于序列未知（病毒，细菌及其他微生物，模式生物）
文库制备：paired +mate pair 高通量短序列+长序列
需要插入多种文库
从头组装算法：OLC（重叠-连接-一致性方法）哈密顿路径 对所有重叠片段做多序列比对
Phrap
K-mer图法（将每个读段分成若干个有重叠的长度为K的子串） 创建k-mer哈希表创建de Bruijn graph 欧拉通路
K-mer图的复杂性：骨刺 Spur 气泡Bubble 磨损的绳子frayed rope:路径合一再分裂：原因：重复性区域
K-mer图的组装：图规约问题 简化
组装结果评价：平均长度 中位长度N50长度 
组装结果注释：注释内容一般包括：
1. mask repetitive sequences including low-complexity regions and
transposable elements
• RepeatMasker工具： http://www.repeatmasker.org/
• Repbase数据库： http://www.girinst.org/server/RepBase/index.php
2. 基因结构注释： CDS或ORF鉴定
• Genscan： http://genes.mit.edu/GENSCAN.html
• Augustus： http://augustus.gobics.de/
3. 基因功能注释： Gene Ontology， Pathyway等
• Blast2GO： https://www.blast2go.com/ 
可视化：Tablet 


##靶向重测序
参考基因组已知 可检测：小插入缺失indel SNP SV CNV(拷贝数变异)
文库制备：全基因组DNA 富集部分DNA（）全基因组子集
DNA序列捕获技术：基于杂交捕获方法 基于扩增子捕获方法（多重PCR方法，在同一个反应体系中，多对引物与目标区域两端互补配对，同时进行PCR扩增）
环化方法：基于扩增子捕获方法
策略：有参考序列即可比对亦可组装 
      无参考序列则组装


重测序数据分析主要流程：读段比对（比对到参考基因组） 变异鉴定 （识别序列差异）变异注释（对变异位点功能注释） 结果可视化
                                                  

短序列比对结果：SAM （文本文件）

BAM（二进制文件）存储samtools工具处理
比对后数据处理：**候选indel位点附近读段重新比对
**基于错配率重新校正碱基分数（GATK****）
SNP calling：启发式方法 概率模型 SNP过滤
主要工具（GATK）  the genome analysis toolktis
Indel calling：paired end局部比对      
局部组装再序列比对
SNV鉴定方法：贝叶斯模型 

CNV/SV鉴定方法：READ PAIR
                SPLIT_READ
                READ_DEPTH:CNV-seq:主要用于全外显子测序
                ASSEMBLY：
序列变异可视化显示：CIRCOS

## 表观基因组测序数据分析

组蛋白修饰测序：chiq_seq 免疫共沉淀捕获DNA序列：chip与高通量测序技术结合 ：读段比对 峰识别 峰注释 峰可视化 结果注释：BEDtools
DNA甲基化测序：MeDIP-Seq (MEDIP：差异甲基化位点鉴定，定量归一化，评估DNA
BS-Seq：WGBS RRBS 甲基化水平提取：Bismark
甲基化水平)分析方法：免疫沉淀
核小体定位测序：atac-seq
染色质互作分析测序:Hi-C测序
非编码RNA测序

## 转录组测序数据分析
RNA-Seq技术测序对象：mRNA +Total RNA
应用：基因表达水平 SNV 基因融合 选择性剪切 RNA编辑
RNA组成：mrna占比少 丰度小
选择性剪切   人类的多外显子基因都具有选择性剪切
RNA-seq读段比对方法：Tophat方法
基于参考序列的RNA-seq读段组装
代表性工具：cufflinks  StringTie
RNA_seq读段从头组装：Trinity

表达水平定量
基于基因注释（RedSeq,UCSC Known Genes,Ensembl or Gencode）,估计基因或转录本的表达水平，基于比对到该基因或转录本的读段数目来定量
基因或转录本的表达水平定量工具：RSEM Cufflinks
RPKM/FPKM

GTF:gene transfer format contain gene annotations or other transcript data
GFF:general feature format 
attributes  transcript_id, gene_id, gene_name

Seqname      source     feature 
↓           
Chr1  phytozome9_0      exon  10274 10430 .     +     .     transcript_id "PAC:24118181"; gene_id "LOC_Os01g01010"; gene_name "LOC_Os01
Chr1  phytozome9_0      exon  10504 10817 .     +     .     transcript_id "PAC:24118181"; gene_id "LOC_Os01g01010"; gene_name "LOC_Os01
Chr1  phytozome9_0      CDS   3449  3616  .     +     0     transcript_id "PAC:24118181"; gene_id "LOC_Os01g01010"; gene_name "LOC_Os01
Chr1  phytozome9_0      CDS   4357  4455  .     +     0     transcript_id "PAC:24118181"; gene_id "LOC_Os01g01010"; gene_name "LOC_Os01
Chr1  phytozome9_0      CDS   5457  5560  .     +     0     transcript_id "PAC:24118181"; gene_id "LOC_Os01g01010"; gene_name "LOC_Os01

融合基因：在某些异常情况下将来自两个不同的基因的外显子重新构成新的基因
Icgc国际癌症基因组联盟 

http://54.84.12.177/PanCanFusV2/

融合部分的表达量相近，而未融合部分的表达量各异

Allelic expression analysis 
在单克隆细胞系，当两个等位位点差异表达时，RNA——seq会检测到等位不平衡


用STAR比对 

用GATK的ASEReadounter工具计算每个等位位点的count


Rna编辑
C to U  A to I (高发于Alu重复序列) 碱基插入删除

RNA编辑的生物学功能：改变氨基酸 影响选择性剪切 改变RNA结构，进而影响RNA的转录后修饰，包括RNA的稳定性、转运等

1、结合基因组测序，找到DNA测序和RNA测序读段之间的A/G or C/T SNP
2. 单独用RNA-seq读段，利用统计模型、 SNP数据库等鉴定RNA编辑 
RNA编辑鉴定工具：
2、
• RNAEditor
• RED(RNA Editing sites Detector)
• REDItools
• RDDpred
• RES-Scanner
• GIREMI 

## Charpter6 蛋白质与DNA、RNA结合测序及数据分析 
蛋白质-DNA结合
转录因子Chip-seq测序技术
转录因子结合位点motif识别：MEME包中的MEME-ChIP


蛋白质-RNA结合
CLIP-Seq

就没见过这么乱的笔记可不是  还没做完  可把我厉害死了 
![] (https://img3.doubanio.com/view/note/large/public/p28729793.jpg)


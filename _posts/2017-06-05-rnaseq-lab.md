---
layout: post
title: "测序实验课"
date: 2017-06-05 17:58:00
categories: 测序 lab
tags: 测序 lab
---

* content
{:toc}

实验课整理















# Lab1 数据获取及质量评估 
- 测序数据库 SRA数据库
- 服务器端下载数据：获取数据 sratoolkit prefetch
- 将SRA格式数据转为fastq格式:Fastq-dump --gzip --split-files(如为双末端测序，则输出两个文件)  file

**软链接 ln -s 源文件 目标文件**

## Fastqc工具 质控及可视化
- Fastqc file 
1. Basic Statistics
2. Per base sequence quality
3. Per tile sequence quality
4. Per sequence quality scores
5. Per base sequence content 若ATCG四条线纷乱交织，则提示有污染
6. Per sequence GC content
7. Per base N content
8. Sequence Length Distribution
9. Sequence Duplication Levels
10. Overrepresented sequences 若失败，则提示有污染，可提示接头序列
11. Adapter Content
12. Kmer Content 

项前面为绿色则通过；黄色为警告；红色则表示失败   

# Lab2 数据预处理
- 去接头接头序列:一段短的序列已知的核酸链，用于链接序列未知的目标测序片段

补充：barcode:又称index,是一段很短的寡聚核苷链，用于在多个样品混合测序时，标记不同的样品、

insert:用于测序的目标片段，因为是包含在两个adapter之间，所以被称为“插入”片段。

一个常见测序片段类似于：adapter-barcode-insert-adapter。测序开始时前几个碱基无法测得，第一个adapter在数据输出时前几个碱基无法测得，第一个adapter在数据输出时被去除；由于测序仪读长限制，第二个adapter通常无法测得，所以，通常得到类似barcode--部分insert的read.最后，把barcode去除，只保留测序insert的片段，即demultiplexing.但是有时测序时会测穿，也就是说会得到barcode-insert的read部分adapter，那么这里就包含接头了，也就是大家经常说去接头药去除的部分。
- 过滤低质量序列 根据read上某滑动窗内碱基的平均phred分值来裁剪低质量区域

- 查找某文件位置： find dir -name filename  Grep -c 1 （输出前num内容）
- 修剪和过滤序列：trimmomatic 

- 两种模式：Paired end mode and single end mode
- 输入文件：fastq文件 gzip 或bzip2


- 单端模式：

java -jar trimmomatic-0.36.jar SE -phred33 input.fq.gz output.fq.gz ILLUMINACLIP:TruSeq3-SE:2:30:10 2:mismatch number 30:阈值 sliding window :过滤低质量序列 slidingwindow:<windowSize>:<requiredQuality> LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
- 双末端模式：

java -jar trimmomatic-0.35.jar PE -phred33 input_forward.fq.gz input_reverse.fq.gz output_forward_paired.fq.gz output_forward_unpaired.fq.gz output_reverse_paired.fq.gz output_reverse_unpaired.fq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36

含义：
- Remove Illumina adapters provided in the TruSeq3-SE.fa/ TruSeq3-PE.fa file (provided). Initially Trimmomatic will look for seed matches (16 bases) allowing maximally 2 mismatches. These seeds will be extended and clipped if in the case of paired end reads a score of 30 is reached (about 50 bases), or in the case of single ended reads a score of 10, (about 17 bases). 
- Remove leading low quality or N bases (below quality 3) 
- Remove trailing low quality or N bases (below quality 3) 
- Scan the read with a 4-base wide sliding window, cutting when the average quality per base drops below 15 
- Drop reads which are less than 36 bases long after these steps 


- cutadpat
- Python 包管理工具 pip ：Pip install --user -upgrade cutadapt
- 关于adapter:http://blog.sciencenet.cn/blog-777771-612299.html
- 切除低质量序列:cutadapt --quality-base=33 -q 20 -o source file outputfile
- 切除接头污染序列：cutadapt -a 3p=  -b  sm1=  sm2=  -b pcr1= pcr2= -a trueindex -a trueunivers -o 

### Screen 
1. Screen -ls 列出当前所有的session 
2. Screen -r name 回到name session
3. Screen -d name 远程detach 某个session

# Lab3 基因组从头组装
 先做数据预处理

- 安装 velvet velvetoptimiser
- 编译设置：make color; make ’CATEGORIES=57’; make ’MAXKMERLENGTH=57’; make ’BIGASSEMBLY=1’; make ’LONGSEQUENCES=1’ 
- Velveth 
velveth directory hash_length {[-file_format][-read_type][-separate|-interleaved] filename1 [filename2 ...]} [options]

directory：velveth结果文件保存的路径

hash_length：奇数整型，k-mer的大小；也支持m,M,s的写法，其中m=<k<M，且每次步长为s，结果文件会保存在directory_k目录下。

文件格式：支持-fasta  -fastq  -raw    -fasta.gz       -fastq.gz       -raw.gz -sam    -bam    -fmtAuto

文件布局：-interleaved（默认，双末端在同一个文件）        -separate（在两个配对的fastq文件中）

读段类型：支持-short  -shortPaired -short2 -shortPaired2 -long   -longPaired -reference等，每个文库对应一个类型。

- Velveg
velvetg directory [options]

directory：velveth结果文件保存的路径

-ins_length：读段插入距离

-cov_cutoff：保留下来的node的最低覆盖深度

-exp_cov：期望深度，如高于该值，则为重复序列

主要输出结果：

directory/contigs.fa: fasta file of contigs longer than twice hash length

directory/stats.txt: stats file (tab-spaced) useful for determining appropriate coverage cutoff


### 组装结果评价：N50长度the length that splits the total bases in half, after
the lengths are ordered

- 基于德布鲁阴图的SOAPdenovo2 组装软件
SOAPdenovo-63mer/SOAPdenovo-127mer组装 

SOAPdenovo-fusion

- -s <string> 设定配置文件
- -o <string> 输出文件的前缀
- -K <int> k-mer大小，默认值为23
- -p <int> 线程数，默认值为8
- -D <int> 边的深度阈值edgeCovCutoff，默认值为1

Config 文件 手动配置

#maximal read length最长读段长度

max_rd_len=150

[LIB]

#average insert size平均插入长度

avg_ins=500

#if sequence needs to be reversed 双末端测序为0，mate-pair为1

reverse_seq=0（测序方向）

#in which part(s) the reads are used 

#各个数值的含义如下：1(only contig assembly), 2 (only scaffold assembly), 3(both contig and scaffold assembly), or 4 (only gap closure)

asm_flags=3      1 contig  2 scaffload  3 all  4. gap补齐

#use only first n bps of each read 仅使用每个读段的前n个碱基

rd_len_cutoff=150

#in which order the reads are used while scaffolding 组装过程中文库使用的顺序

rank=1

# cutoff of pair number for a reliable connection (at least 3 for short insert size) 组装scaffold时需要的最小支持读段数

pair_num_cutoff=3

#minimum aligned length to contigs for a reliable read location (at least 32 for short insert size) 读段比对的最短长度

map_len=32

#a pair of fastq file, read 1 file should always be followed by read 2 file 文件路径

q1=/storage/home/shaojf/NGS/class/Lab3/p_ERR261708_1.fastq.gz

q2=/storage/home/shaojf/NGS/class/Lab3/p_ERR261708_2.fastq.gz




- 思考
测试不同的参数，比较N50值

自行安装配置相关软件，完成组装结果的注释：

mask repetitive sequences including low-complexity regions and transposable elements 

RepeatMasker工具：http://www.repeatmasker.org/

Repbase数据库：http://www.girinst.org/server/RepBase/index.php

基因结构注释：CDS或ORF鉴定
1. Genscan：http://genes.mit.edu/GENSCAN.html
2. Augustus：http://augustus.gobics.de/

基因功能注释：Gene Ontology，Pathway等
Blast2GO：https://www.blast2go.com/

# Lab4 基因组重测序-读段比对
## Bwa 
先用bwa index 构建参考序列 FM-index –t修改前缀

随后比对 sln比对/samse/sampe for BWA-backtrack, bwasw for BWA-SW and mem for the BWA-MEM algorithm

读长<100bp 用bwa aln  samse转成sam格式

读长>100bp 用bwa mem 

 bwa  mem ref.fa read1.fq read2.fq > mem-pe.sam

 bowtie2 end-to-end read alignment 搜索读段全长 加上—local 则进行local read alingnment 

 bowtie2比对分数

 通过控制match mismatch gap 得分

 Bowtie2使用及选项

•用法：bowtie2 [options]* -x <bt2-idx> {-1 <m1> -2 <m2> | -U <r>} -S [<hit>] 

•主要选项： 

•-x <bt2-idx> 参考基因组索引的前缀名。 

•-1 <m1> 双末端测序的第1端文件。如有多个文件，可以用逗号分隔。 

•-2 <m2> 双末端测序的第2端文件。 

•-U <r> 单端测序文件。 

•-S <hit>输出的SAM格式文件名。 

ma pping quality MAPQ	 

SAM文件的第五列

# Class5 基因组重测序 snv calling  


## Lab5 SNVcalling 

Bcftools可直接将BED格式进行VCF的查找  

Samtools piccard GATK 对

SAM格式：可选项格式：TAG：TYPE（i,Z,A(character)）AS:比对分数  MD：错配位置 SA:同一个读段的其他比对位置情况
Samtools view

Samtools sort 

Samtools rmdup

Samtools index

Samtools depth -r regoin -b bed 

Samtools bedcov 测序覆盖度 

Samtools flagstat 比对质量

Samtools mpileup multi-way pileup 鉴定snv 

Samtools mplieup -f reference seq -o outfilename -u(unpressed )v(vcf) raw file 

Bcftools call  snp /indel calling

Bcftools filter 过滤snp

Picard 

Readgroup a set of reads that were generated form a single run of a sequencing instrument 


GATK所需输入文件要求包含RG

编辑ReadGroup信息

Samtools addreplacerg 
 
Picard 

java -jar $PICARD SortSam  标记重复之前coordinate sort 

Java -jar $PICARD MarkDuplicates 

Java -jar $PICARD FilterVcf 对VCF文件过滤 

GATK鉴定SNV及SNV 

输入文件：BAM、CRMA

必须按坐标排序 samtools sort /picard sortsam

必须包括read group信息

## GATK预处理

BQSR（baseRecalibrator ）:delect systematic errors in base quality scores

AnalyzeCovarites:create plots to visualize base recalibration results 

printReads:write out sequence read data 

Java -jar $PICARD ValidateSamFile 验证bam文件 

## SNV鉴定 
Java -jar $GATK -T HaplotypeCaller 

## SNP过滤 
Recalibrate variant quality scores：校正变异质量分数

Java -jar $GATK -T VariantRecalibrator 

Java -jar $GATK -T ApplyRecalibration 

SNV hard filters 

Java -jar $GATK -T SelectVariants 

Java -jar $GATK -T VariantFiltration 

构建一套从fastq到vcf的自动化分析流程

# Lab6  表观基因组测序Chip-Seq（免疫沉淀技术捕获特定蛋白修饰区域的DNA序列）

组蛋白修饰检测：蛋白质在相关酶作用下发生甲基化、乙酰化、磷酸化等修饰的过程

蛋白修饰可能通过改变组蛋白的电荷，从而影响组蛋白与DNA的结合，改变染色质状态

分析方法：CHIP（染色质免疫共沉淀吗）-X（seq高通量方法 SAGE  chip微阵列）技术

对照样本：input DNA(经过交联处理后超声打碎的全基因组DNA 未经免疫沉淀处理) 

流程：1.读段比对 短序列比对工具将测序读段定位到参考基因组
      2.samtools或picard工具删除PCR duplicates

	  峰识别

	  三类峰：窄峰 几百碱基 大多数转录因子和部分组蛋白修饰的峰
        宽峰：宽度可达数万个碱基 大多数组蛋白修饰的峰

		混合峰：宽峰窄峰交替出现 
- Bowtie->bamtofastq 
- Bedtools bamtofastq工具
- MACS model-based analysis of chip-seq 峰识别工具
- Macs2 callpeak （默认窄峰）-t 输入文件 -c 对照文件 -f 输入文件格式 -B 生成bedGraph文件 用于可视化 -n NAME（输出文件名前缀）-fix-biomodel --extsize (tag延伸的长度) -broad (broad peaks如组蛋白修饰)  --call-summits(识别peak最高峰位置） 将Chip富集的区域识别出来 识别目标蛋白质在DNA上的结合位置 识别方法（read shift 设定滑窗 找到读段深度超过阈值或设定超过背景的区域）
- 峰注释（基于基因注释（NCBI RefSeq Ensembl UCSC等），计算每个峰中心位置与最近的基因转录起始位点（TSS）的距离 ，最近的基因一般被认为受该目标蛋白调控的基因）

- ChiPpeakAnno 用于peak注释 

```
Library(ChIPpeakAnno)
Import data and obtain overlapping peaks from replicates
toGRanges(sourcefile,format(MACS2,MACS,narrowPeak)) 转成genomics Ranges型 关于染色体坐标数据格式
Prepare annotation data 
Data(annotation data)
Visualize binding site distribution relatives to features
annotePeakInBatch(,AnnotationData=annotation data)
添加gene name
Library(org.Hs.eg.db) 
addGeneIDs(annotatedPeak=..,orgAnn=org.Hs.eg.db,IDs2Add=”symbol”
Library(GenomicFeatures))
Library(knownGene)
导出序列
getAllPeakSequence
Write2FASTA 写到fasta文件中
MEME
Meme-chip -o(output dir)c(覆盖)  
```

Bedtools

Intersect 找交集

Closest 找最近的区域

Sort 排序

Getfasta 获取基因组区间的序列



Free –g(人类可视化)

# 转录组测序 RNA-seq
- 创建基因组索引 --runMode genomeGenerate  
                        alignReads map reads


Regular:
alignReads map reads

STAR –runThreadN 10 –runMode alignReads –genomeDir STAR/ --readFilesIn  1.fastq 2.fastq –readFilesCommand  gunzip-c(如为压缩文件 则所需解压命令) –outFileNamePrefix (输出文件加前缀)  --outSAMtype(输出文件格式) BAM SortedByCoordinate

```
Other alternative :
2-pass 比对
--twopassMode  Basic可信的选择性剪切 2-pass mapping 
--quantMode  定量模式 TranscriptomeSAM 将产生一个额外的bam文件（可用于RSEM\express等软件的输入文件）
--chimSegmentMin(嵌合体比对最短长度)  可用STAR-FuSion鉴定融合位点)

HTse	q 定量 RSEM import htseq包
分析高通量测序数据的Python包 可用于读段计数
Htseq-qa 对fastq和sam文件进行质量检测 生成pdf文件
Htseq-count 基于sam和gff文件，生成每个特征的count结果
Htseq-count -f (bam or sam ) -r (pos or name)  -t (按照神农特征计数) -i (default”gene-ID)（exon or gene）-o 

RSEM 基因及转录本定量工具 
Rsem-prepare-reference 准备参考序列 --gtf .gtf --star fa .star
Rsem-calculate-expression 计算基因及转录本表达值 [options] reads file refernece suffix 

从读段开始 用STAR比对并确定表达值 
Rsem-calculate-expression --star --paired-end -p 2 --star-gzipped-read-file E
```










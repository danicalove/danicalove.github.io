---
layout: post
title: "甲基化芯片数据处理"
date: 2017-07-30 21:21:21
categories: 数据分析
tags: 甲基化芯片数据处理
---

* content
{:toc}


甲基化芯片数据处理















基于甲基化芯片识别复杂疾病相关的分子标记-数据处理流程
# 分析流程
## 数据标准化
## 缺失值处理
## 假设检验（t检验，秩和检验）
## 多重检验校正
## 差异甲基化位点的识别
差异甲基化位点（Methylation variable positions）:判断CPG位点在两个条件下的甲基化差异是否显著：T检验，WILCOXON秩和检验，FOLD Change

差异甲基化区域（differentially methylation region DMR区域）：CHAMP包提供两种方法寻找DMR，Bumphunter and ProbeLasso(Bumphunter将所有探针聚集成小的clusters，删除包含CpGs少的cluster（默认阈值为7），通过差异p值筛选候选的DMRs，经过permutation确定DMR.结果产生一个dataframe，包含了所有检测到的DMR，长度，clusters以及注释到的CpG位点的数目。ProbeLasso方法依赖于limma包的输出结果。首先计算probe spacing距离，这些有效的探针数据被划分成28个不同的类别，组成genomic feature,包括：
1st exon,3’UTR,5’UTR,body,intergenic region,TSS200,TSS1500
以及这些类别与邻近CpG岛的关系（island,shelf,shore or not associated）。用户通过制定一个最大或最小的lasso size，Probe Lasso方法决定哪个组别首先满足条件，以及相关的feature对应于哪个分位数。接着，设定一个适当大小的lasso，对数据集中的每一个显著的探针，如果lasso捕获到用户指定大小的显著性性特征，就把这个探针设为mini-DMR。Lasso边界重叠或者小于用户设定的距离的邻近mini_DMR之间的gap可以消除。)
## 差异基因的识别
## 差异基因的功能分析（GO，KEGG，PPI网络分析）

analysis proess
```bash
#####写在前面
### 核心命令都来自ChAMP包安装好后，doc下ChAMP.R

### linux cp数据
cp -r /home/software/lib/R/library/ChAMPdata/ ./ChAMP_test/
####

## 安装----------------------------------------------------------
#  source("http://bioconductor.org/biocLite.R")
#  biocLite(c("minfi","ChAMPdata","Illumina450ProbeVariants.db","sva","IlluminaHumanMethylation450kmanifest","limma","RPMM","DNAcopy","preprocessCore","impute","marray","wateRmelon","plyr","GenomicRanges","RefFreeEWAS","qvalue","isva","doParallel","bumphunter","quadprog","shiny","shinythemes","plotly","RColorBrewer","DMRcate","dendextend","IlluminaHumanMethylationEPICmanifest","FEM","matrixStats"))
 R 
setwd("/home/lipp/ChAMP_test") ###新建一个ChAMP_test文件夹，后续结果都会储存在这
library("ChAMP")
testDir="/home/lipp/ChAMP_test/ChAMPdata/extdata"#IDAT数据路径
  
  # 1. 加载原始idat数据
myLoad <- champ.load(testDir,arraytype="450K") #载入数据 epic illumina 850k甲基化芯片
               #### CpG.GUI()  #图形化展示，先别试
              ###champ.QC()
 			  # Alternatively: QC.GUI() #图形化展示，先别试
class(myLoad$beta) #

# [1] "matrix"


   #2. 数据标准化
myNorm <- champ.norm()  #对数据标准化，默认数据为 myLoad$beta

> ls()
# [1] "myLoad"          "myNorm"          "probe.features"  "probeInfoALL.lv"
# [5] "testDir"  
 class(myNorm)
 ##  [1] "matrix"

#myNorm[1:10,]
    > myNorm[1:10,]
#                    C1          C2         C3         C4         T1         T2
#cg00001349 0.673345050 0.627500704 0.67697354 0.70975670 0.46656322 0.76167976
#cg00001583 0.079408284 0.072519492 0.37411396 0.15932854 0.44702168 0.14756981
#cg00002028 0.029779631 0.023208291 0.04002213 0.02730524 0.03504087 0.09662921
#cg00002719 0.036078804 0.045309821 0.07608838 0.02276532 0.23375170 0.34626219
#cg00002837 0.211052139 0.270235408 0.34336824 0.43784615 0.13593510 0.41579813
#cg00003202 0.003925417 0.009918043 0.01053877 0.01030029 0.01601402 0.02635150
#cg00003287 0.047249550 0.044585147 0.14135883 0.11340561 0.06786954 0.11984857
#cg00004121 0.490938643 0.545759234 0.59120394 0.60116360 0.67871104 0.69365282
#cg00007036 0.953215788 0.965193370 0.95401975 0.95718437 0.95258178 0.95952565
#cg00007898 0.026586906 0.037847222 0.02922711 0.02362977 0.02231650 0.03055865
#                   T3          T4
#cg00001349 0.24434991 0.455701049
#cg00001583 0.39635270 0.391444617
#cg00002028 0.02083333 0.012018027
#cg00002719 0.07642563 0.302552553
#cg00002837 0.18168243 0.245981308
#cg00003202 0.01224682 0.005434079
#cg00003287 0.07895171 0.034338747
#cg00004121 0.67361349 0.589274795
#cg00007036 0.95110079 0.944112628
#cg00007898 0.05522828 0.038575668



    #3. 批次效应
champ.SVD()  # If Batch detected, run champ.runCombat() here.
 
    #4 差异甲基化位点 Differentially Methylated Position
myDMP <- champ.DMP() # myDMP <- champ.DMP( myNorm)
        ######DMP.GUI() #图形化展示，先别试
    
	#5 差异甲基化区域 Differentially Methylated Region
myDMR <- champ.DMR()  ###这一步有点慢
        ##### DMR.GUI()   #图形化展示，先别试
 class(myDMR)
 data.frame
 length(myDMR)
 1

   #6 寻找CNA
 myCNA <- champ.CNA()
  
  #8 GSEA分析
  myGSEA <- champ.GSEA()
class(myGSEA )
myGSEA[[1]][[1]][1:6,]
 
 ###练习1,提取chr11和chr1的DMR结果
```
Test2:对标准化后的beta用wilcox rank test 寻找差异甲基化位点，并和champ.DMP结果比较
```bash
>wilcox<-apply(myNorm,1,function(x){wilcox.test(x[1:4],x[5:8])$p.value})
>wilcox_p<-wilcox[wilcox<0.05]
>length(wilcox_p)
[1] 45874
> dim(myDMP)
[1] 4177   23
> dmp_wilcox<-myDMP[rownames(myDMP) %in% names(wilcox_p),]
> dim(dmp_wilcox)
[1] 4177   23
```


ps:screen 用法
- 创建screen回话： screen -S name
- 暂时离开保留会话 ctrl+ad
- 恢复会话 screen -r name
- 查看所有会话 screen -ls

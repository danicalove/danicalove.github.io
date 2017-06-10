---
layout: post
title: "affy芯片数据处理"
data: 2017-05-31 20:01:11
categories: R affy芯片 
tags: R 
---

* content 
{:toc}


虽说现在测序技术用的比较多，但是芯片毕竟是老大哥，还是有很大用武之地的。
![a](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFwwdFxBJ3zCZokATcEsaK6amHpQ1InNMb3PllXpJo2F6gGXMicJBJLWBl8NS52pxX23JXNaJcu1EYw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)












数据来源:GEO GSE65496 T细胞受体激活作为淋巴细胞白血病的肿瘤抑制子作用分机制 芯片表达谱数据

samples:
         
GSM1598820	ALL-SIL_cell_line_without_TCR_activation_rep1

GSM1598821	ALL-SIL_cell_line_without_TCR_activation_rep2

GSM1598822	ALL-SIL_cell_line_after_TCR_activation_rep1

GSM1598823	ALL-SIL_cell_line_after_TCR_activation_rep2

GSM1598824	ALL-SIL_cell_line_with_shRNA_control_vector_rep1

GSM1598825	ALL-SIL_cell_line_with_shRNA_control_vector_rep2

GSM1598826	ALL-SIL_cell_line_with_shRNA_TLX1_rep1

GSM1598827	ALL-SIL_cell_line_with_shRNA_TLX1_rep2

# 包的安装
```
source("http://bioconductor.org/biocLite.R")
biocLite(c("affy","pheatmap","limma"))
```

# 数据读取
## 读入cel文件
```
library("affy")
data.raw<-ReadAffy()
sampleNames(data.raw)
pm.data.raw<-pm(data.raw) #用pm和mm函数可以查看每个探针的检测状况
head(pm.data.raw)
```
## 查看基本信息
```
pData(data.raw)
```
# 对原始数据画图
## 显示芯片扫描图像
```
n.cel<-length(data.raw)
par(mfrow=c(ceiling(n.cel/2),2)) #ceiing 返回不小于参数x的最小整数 mfrow=c(3,2):一个图版显示3行2列
par(mar=c(0.5,0.5,2,0.5))绘出图的边界距离 c(bottom,left,top,right)
for (i in 1:n.cel){image(data.raw[,i])}
```

## 箱线图 
```
par(mforw=c(1,1))
par(mar=c(4,4,3,0.5))
par(cex=0.7) #缩放绘图文本和符号
if (n.cel>40) par(cex=0.5)
cols<-rainbow(n.cel*1.2)
boxplot(data.raw,col=cols,xlab="Sample",ylab="Log intensity")
```

## histogram 曲线
```
par(mar=c(4,4,0.5,0.5))
hist(data.raw,lty=1:3,col=cols)
legend("topright",legend=sampleNames(data.raw),lty=1:3,col=cols,box.col="transparent",xpd=TRUE)
```
## MA plot
```
par(mfrow = c(ceiling(n.cel/2), 2))
par(mar = c(3, 3, 2, 0.5))
par(tcl = 0.2)
par(mgp = c(2, 0.5, 0))
require(scales)
col <- alpha("seagreen", alpha = 0.01)
MAplot(data.raw, col = col, loess.col = "red", cex = 0.9)
```

## RNA降解分析
```
par(mfrow = c(1, 1))
par(mar = c(4, 4, 3, 0.5))
RNAdeg <- AffyRNAdeg(data.raw)
summaryAffyRNAdeg(RNAdeg)
plotAffyRNAdeg(RNAdeg, cols = cols)
legend("topleft", legend = sampleNames(data.raw), lty = 1, col = cols, box.col = "transparent",xpd = TRUE)
box()
```

# 数据预处理
#2.Background correcting , Normalizing（默认quantile normalization） and Calculating Expression

#背景校正方法（MAS方法，RMA方法，GCRMA方法），归一化方法（分位数标准化）可以集成到一个函数中即expresso中
#其中expresso的相应参数对应具体的rma函数和mas5函数
#mas5结果没有log2转换，转换后可以和rma的结果比较下，都在一个尺度


## 背景校正
```
data.rma <- bg.correct(data.raw, method="rma") 
data.mas <- bg.correct(data.raw, method="mas") 
class(data.rma)
head(pm(data.raw)-pm(data.mas),2)
head((data.raw)-mm(data.rma),2)#######差值为全部为0，说明rma方法没有对mm数值进行处理
head(pm(data.raw)-pm(data.rma),2)
head(mm(data.raw)-mm(data.mas),2)
```

## 归一化
```
##################  1) rma方法
data.rma <- rma(data.raw)   #在(可以对归一化的数据再画一个MA plot，比较下)已经log处理，主流用rma()   
         #> eset <- rma(data.raw) 
         #Background correcting:RMA只用PM信号
         #Normalizing：分位数标准化
          #Calculating Expression：log2处理
		  
exp.rma <- exprs(data.rma) ##提取表达值
write.table(exp.rma,file="../array_data/GSE65496_rma.txt",sep='\t',row.names=NA)

#################   2) mas5方法，可以分为三个步骤
############################ a: 提取表达值
data.mas5 <- mas5(data.raw)
exp.mas5 <- exprs(data.mas5)
     #background correction: mas 
     #PM/MM correction : mas 
     #expression values: mas 
     #background correcting...done.
     #54675 ids to be processed
     #|                    |
      #|####################|
	  dim(exp.mas5)
############################# b. mas5calls 选取”表达“的gene
data.mas5calls <- mas5calls(data.raw) #芯片可以检测的基因不一定在样品中都有表达，选取芯片上表达的gene
         #Getting probe level data...
         #Computing p-values
         #Making P/M/A Calls
exp.mas5calls <- exprs(data.mas5calls)

############################# c. 将至少在一个芯片中有表达的基因挑选出来
     source("pcount.R")
	 pcount(exp.mas5calls)
	 #pcount<-function{
     #for (i in 1:dim(exp.mas5calls[,1])){
	 #value<-exp.mas5calls[i,]
	 #values<-paste(value,collapse="")
	 #library(stringr)
	 #count<-str_count(values,pattern="P")
	 #if (count>1)
	 #read.table(rownames[i,],"pprobe.txt",sep="\t",quote="",row.names=FALSE,col.names=FALSE)
	 #	}
     #}
	
	
##################  3) expresso 集成了rma方法和mas5方法
	data.rma.expresso <- expresso(data.raw, bgcorrect.method="rma", normalize.method="quantiles", pmcorrect.method="pmonly", summary.method="medianpolish") 
    exp.rma.expresso <- exprs(data.rma.expresso)  #数据提取
     data.mas5.expresso=expresso(data.raw, bgcorrect.method="mas", normalize=FALSE, pmcorrect.method="mas", summary.method="mas") 
     exp.mas5.expresso <- exprs(data.mas5.expresso)
#########
```

# 后续数据分析---以rma的结果做分析
## 聚类方法
```
pearson_cor=cor(exp.rma)  ###样本间的pearson 相关系数
 scale_exp<-scale(pearson_cor)
 d<-dist(scale_exp,"euclidean")
 hcl<-hclust(d)
 plot(hcl)
 pdf("clust.pdf")
```

## 热图
```
library(pheatmap)
pheatmap(exp.rma[1:20,])
```

# 3. 差异gene ,差异基因筛选时用log2 值. (因此mas5call的方法需要自己log2转换)
#1)差异倍数
```
#2)t test (log2(value)后，做t test)
p.value <- apply(exp.rma, 1, function(x){t.test(x[1:4], x[5:8])$p.value})
p.exp.ram=exp.rma[p.value<0.01,]
```

##limma包使用经验贝叶斯方法求差异表达基因，要求数据经过log2处理
```
library(limma)
##samples=factor(c(rep("TCR"),4),rep("shRNA",4)) ## the function of factor is to encode a vector as a factor ##usage:factor(x=character(),levels,labels=levels,exclude=NA,ordered=is.ordered(x),nmax=NA)
design=model.matrix(~-1+factor(c(rep(1,4),rep(2,4))))) #实验设计矩阵，一个因素两个水平，作为下游lmfit的参数 #model.matrix creates a design matrix
colnames(design)=c("TCR","shRNA")
contrast.matrix=makeContrasts(contrast="TCR-shRNA",levels=design)#limma包的一个函数 construct the contrast matrix corresponding to specified contrast of a set of parametres 表明用户要对实验矩阵design的哪几列矩阵进行比较以得到差异
fit=lmfit(exp.rma,design) 
fit1=contrasts.fit(fit,contrast,matrix)#对基因表达矩阵做线性拟合 #give a linear model fit to micoarray data,conpute estimated cofficients and standard errors for a given set of contrasts
fit2=eBayes(fit) #given a micarray #extract a table of the top-ranked genes from a linear model fit 
dif=topTable(fit2,n=nrow(fit2))
dif_up=dif[dif[,"logFC"]>1 & dif[,"P.value"]<0.01 & dif[,"adj.P.Val"]<0.01,]
dif_down=dif[dif[,"logFC"]<-1 & dif[,"P.value"]<0.01 & dif[,"adj.P.Val"]<0.01,]
```


大功告成
![hh](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFwwdFxBJ3zCZokATcEsaK6anFpmO1JbSFHEoyt36ExobDibM09icYzLUww4tsn80G3ClJsBLKgejQwQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
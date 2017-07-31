---
layout: post
title: "表达谱数据处理"
date: 2017-07-30 21:21:21
categories: 数据分析
tags: 表达谱数据处理
---

* content
{:toc}


这里的表达谱数据经过前期处理，行名是基因名，列名是各样本，对应的表达值数据
处理包括















# 差异表达分析
gene_exp=read.table("file.txt",header=TRUE,row.names=1) ##读取文件的时候header表示的即是列名要不要保留
gene=as.matrix(gene_exp) ##转为矩阵格式

mean_gene=rbind(apply(gene_exp[,1:n],1,mean),apply(gene_exp[,n+1,m],1,mean))
##apply函数：对数组（array,可以是多维）或者列表（list）按照元素或元素构成的子集合进行迭代，并将当前元素或子集合作为参数调用某个指定函数apply(array,margin,FUN),margin:1(行)，2（列），c(1,2)表示行列

- tapply(array,indices,margin,FUN=NULL),按indices中的值分组，把相同值对应下标的array中的元素形成一个集合，应用到FUN，即group by indices,如果FUN返回的是一个值,tapply返回向量；若FUN返回多个值，tapply返回list,FUN为NULL时，返回一个长度和array中元素个数相等的向量
- lapply(list,FUN)在list上逐个元素调用FUN
- sapply 比lapply多了一个simplify参数，将lapply输出的list简化为vector或matrix

mean_gene=t(mean_gene)

for (i in 1:length(rownames(gene)))
{
    gene_t1=t.test(gene[i,1:n],gene[i,n+1:m]) 两样本做t检验，t检验比较两个平均数的差异是否显著，t检验分为单总体检验（一个样本平均数与一个已知的总体平均数的差异是否显著）和双总体检验（两个样本平均数与其各自所代表的总体差异是否显著）
    
mean_ratio=rbind(2^(mean_gene[i,1]-mean))
    
}

```shell
 setwd("E:/教学/2016-2017-第一学期/生物芯片及其数据分析/作业和实验/实验1/program")
#读入gene文件
gene_exp = read.table("D:/实验1/data/pancreas-mrna-expression", sep="\t", header=TRUE, row.names=1)
gene = as.matrix(gene_exp)
write.table("GeneName\tSPN\tPCA\tNET\tNormal\tSPN/Normal\tPCA/Normal\tNET/Normal\tPvalue for PCA to Normal","D:/实验1/result/PCA-normal-test.txt",row.names=FALSE,col.names =FALSE, sep="\t",quote=FALSE,append=TRUE)   
 
 mean_gene=rbind(apply(gene[,1:14],1,mean),apply(gene[,15:20],1,mean) ,apply(gene[,21:26],1,mean),apply(gene[,27:31],1,mean))
 mean_gene=t(mean_gene)
 for (i in  1:length(rownames(gene))) #得到各个gene的 t-test pvalue
  {
   gene_t1=t.test(gene[i,1:14],gene[i,27:31]) #SPN vs Normal
   gene_t2=t.test(gene[i,15:20],gene[i,27:31]) #PCA vs Normal
    mean_ratio=rbind(2^( mean_gene[i,1]- mean_gene[i,4]),2^( mean_gene[i,2]- mean_gene[i,4]),2^( mean_gene[i,3]- mean_gene[i,4]))
    #得到各个gene的 mean fold change
   #gt=cbind(rownames(gene)[i],mi_t[[3]],mean_ratio)
  gt=c(rownames(gene)[i],mean_gene[i,],as.vector(mean_ratio),gene_t2[[3]])
   write.table(t(gt),"D:/实验1/result/PCA-normal-test.txt",row.names=FALSE,col.names =FALSE, sep="\t",quote=FALSE,append=TRUE)  #每次重新运行时提前删除该文件 
  }
  
 #####fdr矫正后的p value
 result_test<-read.table("F:/大三上/生物芯片/实验课/实验1/result/PCA-normal-test.txt",sep="\t",header=TRUE,row.names=1)
p_value<-result_test[[8]]
result_test=as.matrix(result_test)
fdr<-p.adjust(p_value,method="BH",length(p_value))
 result_fdr=cbind(result_test,fdr)
 test_fdr=result_fdr[result_fdr[,9]<(1e-03),] #fdr控制在0.001
 test_fdr2=test_fdr[test_fdr[,8]<(5e-02),]#p_value控制在0.05
 #test_fdr3=result_fdr[result_fdr[,9]<(1e-03)&result_fdr[,8]<(5e-02),]
#colnames( test_fdr)=c(col_names,"fdr")
write.table(test_fdr,"D:/实验1/result/PCA-normal-test-fdr.txt",row.names=TRUE,col.names=NA, sep="\t",quote=FALSE)
```
FDR多重检验校正：
```
fdr<-p.adjust(p_value,method="BH",length(p_value)),##fdr一般控制在0.001，p_value控制在0.05
```
聚类计算

```bash
# 对所有基因聚类
setwd("E:/教学/2016-2017-第一学期/生物芯片及其数据分析/作业和实验/实验1/program")
gene_exp = read.table("../data/pancreas-mrna-expression", sep="\t", header=TRUE, row.names=1)
t_exp<-t(gene_exp)   #默认对行聚类，所以如果要对样本聚类，必须转秩，使得样本变成矩阵的行
scale_exp<-scale(t_exp) #scale()将一组数进行处理，默认情况下将一组数的每个数减去这组数的平均值后再除以这组数的均方根
# 距离计算 （矩阵行之间的距离）
d<-dist(scale_exp,"euclidean") 
hc1<-hclust(d)  #默认complete 。find similar clusters
plot(hc1)
re1<-rect.hclust(hc1, k=3, border="red") #在hc1图的branches上画k个矩形，各个矩形对应各个簇


hc2<-hclust(d,"average") 
hc3<-hclust(d,"centroid") 

#存成pdf文件
pdf("../result/cluster.pdf") #可以设置宽度和高度(12/13)
opar<-par(mfrow=c(2,1), mar=c(5.2,4,4.2,4.2)) 
plot(hc1, hang=-1,); 
re1<-rect.hclust(hc1, k=3, border="red") #在hc1图的branches上画k个矩形，各个矩形对应各个簇
hcc = as.dendrogram(hc1)  #将系统聚类的对象强制转化为谱系图。为什么要转
plot(hcc,horiz=TRUE) #测
dev.off()

#######用PCA 和 normal 做了fdr矫正后的差异表达基因，对PCA和normal样本聚类
#####heatmap
gene_exp1=as.matrix(gene_exp)
diff_exp = read.table("../result/PCA-normal-test-fdr.txt", sep="\t", header=TRUE, row.names=1)
hp_exp=gene_exp1[rownames(gene_exp1) %in% rownames(diff_exp),] 
hp_exp1=cbind(hp_exp[,15:20],hp_exp[,27:31])
heatmap(hp_exp1[1:100,],scale="row")
######heatmap调颜色
hmcols <- colorRampPalette(c("green","black","red"))(100) #do not needs glots
heatmap(hp_exp1, scale = "row", col = hmcols,main="heatmap for expression data")

```

数据分析之hclust
```
> x=runif(10)            
> y=runif(10)
> S=cbind(x,y)                                  #得到2维的数组
> rownames(S)=paste("Name",1:10,"")             #赋予名称，便于识别分类
> out.dist=dist(S,method="euclidean")           #数值变距离
注释：在聚类中求两点的距离有：
1，绝对距离：manhattan
2，欧氏距离：euclidean 默认
3，闵科夫斯基距离：minkowski
4，切比雪夫距离：chebyshev
5，马氏距离：mahalanobis
6，蓝氏距离：canberra
> out.hclust=hclust(out.dist,method="complete") #根据距离聚类
注释：聚类中集合之间的距离：
1，类平均法：average
2，重心法：centroid
3，中间距离法:median
4，最长距离法：complete 默认
5，最短距离法：single
6，离差平方和法：ward
7，密度估计法：density
> plclust(out.hclust)                           #对结果画图 如图1
> rect.hclust(out.hclust,k=3)                   #用矩形画出分为3类的区域 如图2
> out.id=cutree(out.hclust,k=3)                 #得到分为3类的数值
> out.id
 Name 1   Name 2   Name 3   Name 4   Name 5   Name 6   Name 7   Name 8  
       1        2        3        2        2        2        1        3 
 Name 9  Name 10  
       1        2 
> table(out.id,paste("Name",1:10,""))           #以向量的方式分辨名称对应的类
      
out.id Name 1  Name 10  Name 2  Name 3  Name 4  Name 5  Name 6  Name 7 
     1       1        0       0       0       0       0       0       1
     2       0        1       1       0       1       1       1       0
     3       0        0       0       1       0       0       0       0
      
out.id Name 8  Name 9 
     1       0       1
     2       0       0
     3       1       0R：数据分析之聚类分析hclust
```


# 相关系数
```bash
setwd("E:/教学/2016-2017-第一学期/生物芯片及其数据分析/作业和实验/实验2/相关系数/data")
tf2mi = read.table("tf2mirna", sep="\t", row.names=NULL, quote="",stringsAsFactors=FALSE)
tf2mi = tf2mi[,1:2]  
colnames(tf2mi) = c("TF", "miRNA") 
tf2mi$TF = as.vector(tf2mi$TF)
tf2mi$miRNA = as.vector(tf2mi$miRNA)
tflist =tf2mi$TF 
milist = tf2mi$miRNA 
gene=read.table("pancreas-mrna-expression", sep="\t", header=TRUE, row.names=1)
miRNA=read.table("pancreas-mirna-nor-biomarker", sep="\t", header=TRUE, row.names=1)
tf.exp = gene[rownames(gene) %in% tflist, ] 
mi.exp = miRNA[rownames(miRNA) %in% milist, ]   
	# 首先是tf和mi的相关性
	r1 = dim(tf.exp)[1]
	c1 = dim(mi.exp)[1]
	#if(!is.null(candidate)) {
		#c = c[c %in% candidate]
	#}
	cor.tf2mi = matrix(0, nrow=r1, ncol=c1)

	
	for(j in 1:c1) 
	{
		for(i in 1:r1)
			{
				rc = cor(t(tf.exp[i, ]), t(mi.exp[j, ]))
				#robust_cor后，相关系数值变大了
				print(rc)
				#p = rc$test$p.value
				cor.cutoff=0.2
				if(abs(rc) >= cor.cutoff) {
					cor.tf2mi[i,j] = rc
					#plot_cor(tf.exp[tflist==r[i], ], mi.exp[milist==c[j], ], outlier.index = outlier.index, xlab=r[i], ylab=c[j], main=v)
				}else {
					cor.tf2mi[i,j] = 0
				}
			
			}
  }

```

# 生存分析
install.package("survival")
```
#fit a Kaplan-Meier and plot it 
setwd("E:/实验2/km")

#数据统计
pdac<-read.table("data/pdac-6.txt")

##KM图和cox回归
library(survival)
#layout(rbind(1:2))   # 将绘图区分为2个部分

##KM图
 pdac["Time"]=pdac["Time"]*30
fit <- survfit(Surv(Time, Status) ~ Exp, data = pdac) 

#plot(fit, lty = 2:1, col=1:2,ylab="Estimated survival function",xlab="Survival Time (months)",xlim=c(0,90)) 
plot(fit, col=c("green","red"),ylab="Estimated survival function",xlab="Survival Time (days)")
##plot(1:26,pch=letters,col=1:26)

#axis(1, seq(0,100,by=10))#很赞！
text(2150,0.75,"log-rank P<0.0001")
legend(1800, .95, c("SAV1_high", "SAV1_low"),lty = 1:1, col=c("red","green")) 



##cox回归得到p value 
#单因素
fit=coxph(Surv(Time,Status) ~ Exp,data = pdac)  ##coxph:构建COX回归模型 Surv:用于创建生存数据对象
summary(fit)
#确认 哪个 VS 哪个
fit <- survfit(Surv(Time, Status) ~ Exp, data = pdac) ##survfit：创建KM生存曲线或是COX调整生存曲线
summary(fit)

fit=coxph(Surv(Time,Status) ~ Sex,data = pdac)
summary(fit)

fit=coxph(Surv(Time,Status) ~ Age,data = pdac) #HR=1.857;P=0.018
summary(fit)

fit=coxph(Surv(Time,Status) ~ DifferR,data = pdac) #HR=1.857;P=0.018
summary(fit)

####多因素
#多因素
fit=coxph(Surv(Time,Status) ~ Exp+NC+DifferR,data = pdac)
#fit=coxph(Surv(Time,Status) ~ Age+Exp+Sex+DifferR+TumorS+TC+NC+MC+TMN+TumorL+NervousI,data = pdac)
summary(fit)
```


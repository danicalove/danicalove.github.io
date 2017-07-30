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


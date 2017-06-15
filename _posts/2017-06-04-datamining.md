---
layout: post
title: "datamining"
date: 2017-06-04 11:11:11
categories: datamining 算法
tags: datamining 
---

* content
{:toc}

数据挖掘，大概也是一门神学。

![](http://pic0.v4.cc/ps/mmbiz.qpic.cn/mmbiz_jpg/884XsHGHibP79Z38cTLk4THrI6iahS0rLkuaiakJfumSAOgAMyBiaMyAo1cpBUpbticNorFp8rX8vluk0CGvzRDYdog/0?wx_fmt=jpeg)








- 频繁模式：数据中频繁出现的模式
- 频繁项集：频繁地在事务数据集中一起出现的集合
- 关联规则的两个属性：最小支持度阈值和最小置信度阈值
- 分类模型的表达形式：分类规则、决策树、数学公式、或神经网络，朴素贝叶斯分类、支持向量机、k最近邻分类
- 回归分析：回归建立连续值函数模型，用来预测缺失的或难以获得的数值数据值
- 相关分析：在分类和回归之前进行，识别与分类和回归过程显著相关的属性，选取这些属性用于回归和分类。
- 相关系数：描述变量间相关程度与变化方向的量数，总体相关系数用ρ表示，样本相关系数用r表示，|r|越接近1，表明两变量相关程度越高，它们之间的关系越密切
- 聚类(cluster):对象根据最大化类内相似性、最小化类间相似性的原则进行聚类或分组
作为预处理工具：
- 回归分析，PCA（主成分分析,可以作为多元回归和聚类分析的输入），分类(classification),关联分析(accociation analysis)等的预处理
- 压缩：图像压缩：向量矢量化(vector quantization)
- finding k-nearest neighbors:寻找k-近邻中心
- putlier detection
## 数据预处理
- 属性子集选择：逐步向前选择 逐步向后选择 决策树归纳（信息增益度量）
- 数据规范化：最小最大规范化，z分数规范化 小数定标规范化
- 无监督的学习：1. the class labels of training data is unknown 2.given a set of measurements,observations,etc.with the aim of establishing the existence of calsses or clusters in data
# 分类
数据分类两步：1.建立一个模型，描述预先定义的数据类或概念集的分类器 2.使用模型，对未知样本分类
## 决策树：
1. ID3
2. C4.5：多重分支和剪枝
3. CART：Classification and Regression Trees
### 算法
自顶向下递归的分治方式
#### 伪代码
```
算法： Generate_decision_tree

输入：

- 数据分区D，训练元组和它们对应类标号集合
- attribute_list,候选属性的集合
- Attribute_selection_method,一个确定“最好地”划分数据元组为个体类的分裂准则的过程。这个准则由分裂属性（splitting_attribute）和分裂点或划分子集组成。
输出：一棵决策树

方法：
1. 创建一个结点N
2. if D中的元组都在同一类c中then
    
    返回N作为叶节点，以类C标记；
    
   if attribute_list为空then

    返回N作为叶节点，标记为D中的多数类
3. 使用Attribute_selection_method(D,attribute_list),找出“最好地”splitting_criterion
4. 用splitting_criterion标记结点N
5. if splitting_criterion是离散值的，并且允许多路划分then //不限于二叉树
    attribute_list<-attribute_list-splitting——attribute;//删除分裂属性
    for splitting_criterion的每个输出j 
    //划分元组并对每个分区产生子树
        设Dj是D中满足输出j的数据元组的集合；
        if Dj为空 then
            加一个树叶到结点N，标记为D中的集合
        else加一个由Generate_decision_tree（Dj,attribute_list）返回的结点到N；
    endfor
6. 返回N；
```

## 分类算法
### knn（k近邻分类）
```
library(class)
data(iris)
index<-sample(1:nrow(iris),as.integer(0.7*nrow(iris)));index ##sample(x,size,replace=F,prob=NULL)
table(iris[index,5])
#knn
train<-iris[index,-5]
test<-iris[-index,-5]
cl<-iris[index,5]#set train data classified column
k<-3 #紧邻样本数

pre<-knn(train = train,test = test,cl=cl,k=k)
pre<-knn(train = train,test = test,cl=cl,k=k,prob=T)

table(iris[-index,5],pre)



###交叉验证
library(class)
vknn<-function(v,data,cl,k){
	grps=cut(1:nrow(data),v,labels=FALSE)[sample(1:nrow(data))]
	print(grps)
	pred=lapply(1:10,function(i,data,cl,k){
		omit=which(grps == i)
		pcl = knn(data[-omit,],data[omit,],cl[-omit],k=k)
	},data,cl,k)
	wh = unlist(pred)
	

}
vknn(5,iris[1:4],iris$Species,5)
	
	}

}
```
## 朴素贝叶斯
```
naiveBayes(Species~.,iris[1:75,])->model #建立模型
predict(model,iris[76:100,-5])->pre#预测
table(iris[76:100,]$Species,pre)



score<-c(0.9,0.8,0.7,0.6,0.55,0.54,0.53,0.51,0.50,0.40)
label<-c(1,1,-1,1,1,-1,-1,-1,1,-1)

#ROC曲线：横坐标：假阳性率 纵坐标：真阳性率
pred<-prediction(score,label)#得到tp,fp,tn,fn对应的数目
perf<-performance(pred,"tpr","fpr")#得到tpr(真阳性率)和fpr(假阳性率)
##perf<-performance(pred,"prec","rec")
# > perf
# An object of class "performance"
# Slot "x.name":
# [1] "False positive rate"

# Slot "y.name":
# [1] "True positive rate"

# Slot "alpha.name":
# [1] "Cutoff"

# Slot "x.values":
# [[1]]
 # [1] 0.0 0.0 0.0 0.2 0.2 0.2 0.4 0.6 0.8 0.8 1.0


# Slot "y.values":
# [[1]]
 # [1] 0.0 0.2 0.4 0.4 0.6 0.8 0.8 0.8 0.8 1.0 1.0


# Slot "alpha.values":
# [[1]]
 # [1]  Inf 0.90 0.80 0.70 0.60 0.55 0.54 0.53 0.51 0.50 0.40


AUC<-performance(pred,"auc")#计算曲线下面积
# > AUC
# An object of class "performance"
# Slot "x.name":
# [1] "None"

# Slot "y.name":
# [1] "Area under the ROC curve"

# Slot "alpha.name":
# [1] "none"

# Slot "x.values":
# list()

# Slot "y.values":
# [[1]]
# [1] 0.76


# Slot "alpha.values":
# list()

AUCValue<-AUC@y.values[[1]]
# > AUCValue
# [1] 0.76

plot(perf,col="blue")#或者plot(perf,colorize=T)
plot(perf,add=TRUE, col="green")  #在原来的图上再继续画ROC曲线



score<-rnorm(100,0,1)
label<-sample(c(-1,1),100,replace=T)
pred<-prediction(score,label)
perf<-performance(pred,"tpr","fpr")
AUC<-performance(pred,"auc")
plot(perf,col="blue")
plot(perf,add=TRUE, col="green") 

分区
op <- par(mfrow = c(2, 2), # 2 x 2 pictures on one plot
          pty = "s")       # square plotting region,
                           # independent of device size

#通过绘制ROC曲线比较kknn/tree/nnet三种分类器的分类效能。
library(ROCR)
library(gplots)
iris_100<-iris[1:100,]
ind<-sample(2,nrow(iris_100),replace=T,prob=c(0.7,0.3))  
trainSet<-iris_100[ind==1,]#ind是1的行号，找在iris中的行
testSet<-iris_100[ind==2,]


library(kknn)
iris.kknn <- kknn(Species~.,trainSet,testSet[,-5],k=5 

,distance = 5,kernel= "triangular")  
score_kknn<-apply(iris.kknn[["W"]],1,mean)
label_kknn<-as.character(testSet$Species)
pred_kknn<-prediction(score_kknn,label_kknn)
perf_kknn<-performance(pred_kknn,"tpr","fpr")
AUC_kknn<-performance(pred_kknn,"auc")#计算曲线下面积
AUCValue_kknn<-AUC_kknn@y.values[[1]]
plot(perf_kknn,col="green")

# data<-iris[100,]
# ind<-sample(2,nrow(data),replace=T,prob=c(0.7,0.3))  
# trainSet<-data[ind==1,]
# testSet<-data[ind==2,]
# library(kknn)
# kknn(Species~.,trainSet,testSet[,-5],k=5,distance=5,kernel="triangular")->knnc
# attributes(knnc)
# # for (i in 1:nrow(knnc[[3]]))
# # { 
	# # cl<-mean(knnc[[3]][i,])
	# # score[i]<-cl
# # }
# # score<-c(score)
# score<-apply(knnc[["W"]],1,mean)
# label<-as.character(testSet[,5])
# library(e1071)
# pred<-prediction(score,label)
# perf<-performance(pred,"tpr","fpr")
# AUC<-performance(pred,"auc")
# plot(perf,col="blue")
# plot(perf,add=TRUE, col="green")


library(tree)
#tree构建决策树(CART)
myFormula<-Species~Petal.Width+Petal.Length+Sepal.Width

+Sepal.Length#用哪些属性建树
treeCART<-tree(myFormula,data=trainSet)
score_tree<-apply(predict(treeCART,newdata=testSet),1,max)
label_tree<-as.character(testSet$Species)
pred_tree<-prediction(score_tree,label_tree)
perf_tree<-performance(pred_tree,"tpr","fpr")
AUC_tree<-performance(pred_tree,"auc")#计算曲线下面积
AUCValue_tree<-AUC_tree@y.values[[1]]
plot(perf_tree,add=TRUE, col="blue")




# nnet
# n<-nnet(trainSet[,-5],class.ind(trainSet$Species),size=1,rang=0.1,decy=5e-4,maxit=300)
# predict(n,newdata=testSet[,-5])->p
# perf<-performance(pred,"tpr","fpr")
# AUC<-performance(pred,"auc")
# plot(perf,col="blue")
# plot(perf,add=TRUE, col="green") 



library(nnet)
######BP实现  
bp<-nnet(trainSet[,-5],class.ind(trainSet$Species),size 

=2, rang = 0.1,decay = 5e-4, maxit = 300)
score_nnet<-apply(predict(bp,newdata=testSet[,-5]),1,max)
label_nnet<-as.character(testSet$Species)
pred_nnet<-prediction(score_nnet,label_nnet)
perf_nnet<-performance(pred_nnet,"tpr","fpr")
AUC_nnet<-performance(pred_nnet,"auc")#计算曲线下面积
AUCValue_nnet<-AUC_nnet@y.values[[1]]
plot(perf_nnet,add=TRUE, col="red")
```
## CARTforest
```
CARTForest<-function(dataset,repeat_r,kcv_k,prob){
	ind<-sample(2,nrow(dataset),replace=T,prob=prob)
	trainSet<-dataset[ind==1,]
	testSet<-dataset[ind==2,] 
	len<-nrow(trainSet)
	num<-1
	acc_train_test<-integer()
	acc_test<-integer()
	for(i in 1:repeat_r) {
		samp_ind<-sample(len)
		for(j in 1:kcv_k){
			tp_tn<-0
			val <-samp_ind[((j-1)*len/kcv_k+1):(j*len/kcv_k)]
			trainSet.train <- trainSet[-val,]  
			trainSet.test <- trainSet[val,]  
			assign(paste("treeCART",num,sep=""),tree(Species~.,data=trainSet.train ))
			train_table<-table(apply(predict(get(paste("treeCART",num,sep="")),newdata=trainSet.test ),1,function(x){names(x)[which.max(x)]}),trainSet.test$Species)
			for(k in 1:dim(train_table)[1]){
				tp_tn<-tp_tn+train_table[k+dim(train_table)[1]*(k-1)]
			}
			acc_train_test[num]<-(tp_tn/sum(train_table))
			num<-num+1
		}
	}
	acc_train_test_mean<-mean(acc_train_test)
	for(i in 1:(num-1)) {
		tp_tn<-0
		test_table<-table(apply(predict(get(paste("treeCART",i,sep="")),newdata=testSet),1,function(x){names(x)[which.max(x)]}),testSet$Species)
		for(k in 1:dim(test_table)[1]){
				tp_tn<-tp_tn+test_table[k+dim(test_table)[1]*(k-1)]
			}
		acc_test[i]<-(tp_tn/sum(test_table))
	}
	acc_test_mean<-mean(acc_test)
	acc<-matrix(c(acc_train_test_mean,acc_test_mean),nrow=2)
	rownames(acc)<-c("mean of train_test's acc","mean of test's acc")
	return(acc)
}
source("C:\\Users\\samsung\\Desktop\\CARTForest.r")
#source("C:\\Users\\Administrator\\Desktop\\CARTForest.r")
CARTForest(dataset=iris,repeat_r=10,kcv_k=10,prob=c(0.7,0.3))
```

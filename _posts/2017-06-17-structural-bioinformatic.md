---
layout: post
title: "structural-bioinformatics"
date: 2017-06-17 23:11:11
categories: bioinformatics
tags: bioinformatics
---

* content
{:toc}

前段时间github一直同步不了，为了期末考试做的笔记一直没有同步到网页上，昨天才收到github发来的邮件说是这个文件的编码不是UTF-8编码错误，现在终于又可以上传了，又有了写博客，整理笔记的心情了。
















# 考试内容
2 3 不考
简介：研究内容 （生物大分子结构） psi结构基因组计划 
PDB数据库 掌握pdb mmcif格式 解释
可视化：空间填充图 Jmol可视化

核酸二级结构元件  mfold 最小自由能算法 能量模型 计算自由能
随机上下文无关语法模型 符号及规则 预测二级结构 
蛋白质二级机构预测基本依据 倾向性  PSSM打分矩阵 jpred pyspred 神经网络 
DSSP三级结构基础上预测二级结构

测定生物大分子二级结构方法：

SCOP数据库层次及含义
生物大分子互作依据：结合结构域 蛋白质-蛋白质互作原理

染色质结构及表观遗传

染色质层级结构 HI-C,CHIA-PET测定方法 可视化方法 heatmap图看懂含义



# 01 简介
## 研究内容：生物大分子如蛋白质，RNA和DNA及相互作用的三维结构的预测和分析，由结构到功能理解生物大分子的作用机制。
## DNA结构
### 一级结构
构成核酸分子的核苷酸或碱基的排列顺序
### 二级结构
两条脱氧多核苷酸反向平行盘绕所形成的双螺旋结构
### 三级结构
DNA单链与双链相互作用形成的三链结构，即超螺旋结构，两条链过度缠绕产生正超螺旋，缠绕不足产生负超螺旋
### 四链结构
DNA单链或双链之间的相互作用形成的四链结构
## RNA结构
### 一级结构
核苷酸碱基排列成单链
### 二级结构
RNA主链在局部形成的有规律的折叠。双链RNA形成双螺旋，单链RNA在建立在链内碱基互补配对的基础上，形成各自不完美的局部双螺不完美的 局部旋，包括单核苷酸突起、三核苷酸突起、发夹环、对称的内部环和不对称的内部环等。
### 三级结构
RNA所有院子在三维空间上的排布。（倒L型）
## 蛋白质结构
### 一级结构
蛋白质多肽链中氨基酸残基排列顺序，是蛋白质空间构象和特异生物学功能的基础。
### 二级结构
蛋白质分子中某一肽链的**局部空间结构**（即该段肽链中**主链骨架**原子的相对空间排布，不涉及氨基酸侧链的构象）
### 三级结构
二级机构在氨基酸侧链R基的相互作用进一步盘曲或折叠形成的特定构象，整条多肽链中所有原子或基团在三维空间的排布位置。
### 四级结构
两个或两个以上亚基之间以非共价键相互作用形成的
## 结构基因组计划（PSI）目标
致力于加快结构基因组研究，进而理解其生物学功能。

- 规模化测定蛋白质的三维结构：(1)实验方法测定（2）基于序列相似性提供模型（3）用实验和计算方法提供蛋白质功能信息。
- 发展结构基因组的方法学
## 结构数据库
- 一级结构数据库：核酸序列数据库（EMBL,GenBank,DDBJ） 蛋白质序列数据库（Swiss-Port,PIR）
- 高级结构数据库：核酸结构数据库（NDB） 蛋白质结构数据库（PDB）蛋白质分类数据库（SCOP、CATH）
## 在药物研发中的作用
- 靶标估计
在药物发现过程的初期，如果获得了蛋白结构，结构生物信息学就可以对
该蛋白靶点是否可以被类药小分子结合做出评估。
- 虚拟筛选
已知蛋白质结构，通过两种方式对化合物数据库进行筛选。一是产生药效
团来描述配体结合位点的重要性质，然后用药效团对小分子化合物进行匹
配计算；二是分子对接。
- 先导化合物优化
基于结构信息或者晶体复合物信息对先导化合物进行优化，还可以利用分
子动力学来模拟先导化合物和靶蛋白的结合细节和动态变化，进一步利用
这些知识改造修饰先导化合物。

计算机辅助药物设计：基于结构的药物设计（Structure-based drug design） 根据药物靶点结
构，研究受体和小分子之间的相互作用，设计与活性口袋互补的新分
子或寻找新型先导化合物的技术。

# 结构数据库及结构可视化
NDB：DNA、RNA

PDB：A crystallographic database for the three-dimensional structural
data of large biological molecules, such as proteins, nucleic acids
and complex assemblies.
- 数据来源：X-射线晶体衍射技术，NMR光谱技术。沉淀电子显微镜
- PDB格式说明 

DBREF：提供了PDB序列和相应的数据库记录的链接；

SEQRES：残基序列，包括所研究的大分子每条链的氨基酸或核酸残基
序列（用3个字母的缩写来作为氨基酸名称，用一个或两个字母表示标准核苷酸）。

HELIX：用来标识蛋白质分子中螺旋结构的部分，并且在这部分对
分子中的螺旋结构都进行了编号和命名，也注释了各个螺旋的起始
和终止氨基酸残基以及螺旋的长度；

螺旋的说明

HELIX 1 1 GLY A 9 ASN A 22 1 
- HELIX：螺旋
- 1：螺旋的次序号（Serial_No）。从1开始顺次增加编号
- 1：螺旋标识(Helix_id)
- GLY：起始残基的名称
- A：螺旋所在的链
- 9：起始残基在序列中的排号
- ASN、 A、 22：末端残基名称、所在的链、末端残基在序列中的排号
- 1：螺旋分类号（1表示右手α-螺旋）
- 14：螺旋长度


SHEET：折叠片记录项标识了折叠片在分子中的位置，以及起始和
终止残基，并对每个折叠片进行了命名和编号。

折叠片的说明：

SHEET 1 AA 5 LYS A 60 VAL A 63 0

- SHEET：折叠
- 1：折叠片的分支链（strand）的编号，从1开始顺次增加编号
- AA： 折叠片的标识符(Sheet_id )
-  5：折叠片中分支链的数目
- LYS：起始残基的名称
- A：分支链中起始残基的主链标识符
- 60：分支链中起始残基在主链中的排序号
- VAL、 A、 63：末端残基名称、主链标识符、末端残基在主链中的排序号
- 0：相对于前导链的意义（0表示第一分支链， 1代表相对为顺式， -1代表相对为反式）

折叠片中氢键的说明（第二行以后各行从左到右）

SHEET 2 AA 5 PHE A 3 ASP A 8 - O GLU A 4 N GLU A 62
- 0：当前分支链中原子的名称（氧原子)
- GLU：当前分支链中的残基名称
- A：分支链的主链标识符
- 4：当前分支链残基的主链序列排号
- N：前导分支链中原子的名称（氮原子）
- GLU：前导分支链的残基名称
- A：前导分支链的主链标识符
- 62：前导分支链残基的主链序列排号

SHEET 1 AA 5 LYS A 60 VAL 63 0

SHEET 2 AA 5 PHE A 3 ASP A 8 -1 O GLU A 4 N GLU A 62

分支链1是从主链A的60位LYS到主链A的63位VAL，该分支链是首链。

分支链2是从主链A的3位PHE到主链A的8位ASP，该分支链与前导链关系是反式，主
链A的4位GLU上的氧与前导链A62位的GLU上的氮之间形成氢键。

- CRYST1：用来描述晶胞特征，包含了晶胞结构参数（三条棱的长
度、三条棱之间的夹角、空间聚合程度以及单位结构中聚合链的个
数）；

- ORIGX1：直角-PDB坐标转换；
- SCALE1：直角-部分结晶学坐标的转换。
- ATOM：标准基团的原子坐标；
- TER：在一个链中一个ATOM/HETATM记录的结束，指示出每个
多肽或者核酸链的最后一个残基；
- MASTER：列出了相应记录或所选数据类型文件的数目， 给出了所
有记录数目行数的验证数字；
- END：结束标志。

[PDB文件格式说明]（http://www.cnblogs.com/mingyongcheng/p/3521488.html）

[PDB Data Records](http://mmcif.wwpdb.org/docs/pdb_to_pdbx_correspondences.html)

## mmCIF格式：大分子晶体学信息文件

[标准mmCIF 格式](http://mmcif.wwpdb.org/dictionaries/ascii/mmcif_std.dic)

占位符：问号“？”表示某一个数据项的数值已经丢失；点号“.”表示数据
项没有合适的值或者这个值已经被有意忽略了。

多行文本字符串使用分号进行标注。一个分号放在第一行的开头，另一个分号放在最后一行的结尾。

数据表和向量可以通过“loop_”标识符来编码

“data_”标识符表示数据块

## 三类可视化方式
线框图，球棍图（蛋白质结构细节），空间填充图（蛋白质之间互作差异）

Backbone（骨架） and Ribbon（带状）（蛋白质折叠）

蛋白质链折叠方式

## 可视化工具
PyMol,Jmol可视化

# 核酸结构预测
DNA结构更有可能是双链互补的复合体结构，RNA结构则折叠为二级或三级结构

## RNA二级结构的结构元件
内环：当部分碱基变异，导致茎区的两个子串中各有部分碱基不能配对。

凸环：如果茎区的一个子串中有部分核苷酸在进化过程中消失或者加入，则原茎区中的
一个子串中会存在一段碱基不能形成配对，这种结构称为凸环。

RNA二级结构中的碱基配对通常以嵌套的模式出现，即对于二级结构
中的任意两对配对碱基i、 j和ݐҠ、 ݐҠ,它们的位置关系应当满足
ݐ)< i'ݐҠ<j' ݐҠ<j ݐ&Ȗݐ)< jݐ< ݐҠi'< ݐҪ'（假设ݐ)< i'ݐү܉,当**非嵌套的情形或者交叉出现**时，此时 ݐ)< i'ݐҠ< jݐ< j'ݐү܌那么就称为假结。

至少由两个由单链区域或者环连接的螺旋结构。the best characterized is the H type,因为环和茎区的长度不同，还有环和茎区之间的交互作用，假结代表了结构上的多样性。
假结的预测：Mfold 

## RNA二级结构的预测方法
### 动态规划算法（递归的过程：先定出一小段序列
的最好二级结构，再用相同的法则将序列扩展，找到相应的最好二级
结构；这种方法不断进行，直到全长序列。）：碱基最大配对算法，最小自由能算法（*）

碱基最大配对算法：主要思想: 要在更短序列的最好二级结构基础上获得序列 i 到 j 的最好二级结构，
只有4种可能的途经：
1. 原有结构两端各延伸一个残基并将它们配对；
2. 向5’端延伸一个不配对的碱基;
3. 向3’端延伸一个不配对的碱基;
4. 将已存在最好二级结构的两段合并起来;

最小自由能原理：在等温等压下，体系的自由能永不增大。任何自发
（不可逆）过程都是力图使体系自由能减小；当自由能减小到极小值
时，体系达到平衡状态。自由能最小的时候为RNA真实的二级结构

最小自由能算法的中心思想： RNA的二级结构在最优化组合时，通过
分子动力学方法获取的各种茎环结构的总能量最小，结构也最稳定。

基于矩阵的动态规划算法：将求解长序列二级结构的自由能转化为求若干个短序列二级结构自
由能的情形，再根据递归算法来寻找最小自由能路径。（**计算）

RNA二姐结构能量模型：最近邻能力原则，最近邻：碱基对的能力只依赖于它前面或后面的碱基或碱基对。以子环为基础

子环
- The term k-loop denotes a loop containing k-1 base pairs, making a total of k base pairs by including the closing base pair.
- A 1-loop is called a hairpin loop（发夹环） .
- 2-loop是堆积配对、凸环、内环。
- K-loop（k>2） 是多分支环。

发夹环部分终端错配自由能
- 该能量与封闭发夹环的碱基对以及与该碱基对相邻的两个未配对的碱基构成的整体相关。
- 发夹环中碱基的个数大于3才考虑，否则为0

相邻配对碱基间的堆积自由能

单碱基的堆积自由能

最大碱基配对算法由于假设前提过于简单，因此预测的效果不理想，
精度较低。
最小自由能算法是一种常用的算法，使用方便简单、发展比较成熟，
尤其是针对小分子的预测比较准确、可靠。但是，随着RNA序列长度
的增加，结构预测将变得不那么精确。
无论是最大碱基配对算法还是最小自由能算法，它们都只是考虑以嵌
套模式配对的情况，因此它们都有一个最大的缺陷就是**不能预测假结**。

### 比较序列分析法
对多条序列进行互补碱基的共变联配（Covariantalignment），查找与需要预测的RNA序列的高近似度序列，以一定的
相关数学模型为依托共同研究推算所给RNA序列的二级结构。

比较序列分析方法有随机上下文无关语法模型（Stochastic Contextfree Grammars Model， SCFG）

把RNA序列看成是具有一定语法规则的语
句，通过这些语法规则来分析RNA序列中存在的碱基配对关系，也就
是它的语义，从而得到该序列的二级结构。

人们为了计算方便，常常将蛋白质、 DNA、 RNA的残基或碱基看作是
一维不相关的字符串，但序列在二级结构配对时又涉及长程的相互关
联。为了处理这种符号语言以及这种长程相互关联， Chomsky于1956
年1959年提出了Chomsky hierarchy of transformational grammars理
论（简称Chomsky转换理论）。

Chomsky转换语法是由一系列的符号和重写规则组成的。

如果在上面所描述的上下文无关语法中，给每一个重写规则赋予一个概率
值，则构成了随机上下文无关语法。
 随机上下文无关语法的构成要素除了符号和重写规则外，还包括非终止状
态和非终止状态间的转换概率矩阵以及非终止状态向终止状态的生成概率
矩阵。
SCFG模型忽略了序列中的空位，相当于只含匹配状态不含插入/删除
状态的隐马尔可夫模型。

和共变模型（Covariance Model， CM）通过对SCFG模型进行扩展，使
之能处理插入和删除。

较序列分析法预测的结果比较可靠，其预测的结果也仅次于实验上
用x射线晶体衍射或核磁共振测定的结构。 但这种方法运用多序列比对，
需要基于一定数量的同源序列来做比对，而且比对的好坏直接影响预
测的结果，因此，该方法对同源序列之间的相似程度要求较高。
这种方法不适合对一条或较少序列以及同源性不高的序列进行预测，
这也是限制它进一步深入发展的客观因素。

### 组合优化方法
根据碱基配对可以构
成各式各样不同种类的茎区，只要知道了这些茎区怎样进行选择以及
如何组合，就能够有效地预测出RNA的二级结构。

螺旋区堆积算法
-螺旋区就是指的茎区，碱基配对会形成许多大小不一的茎区。已知一
条RNA序列，如果明确了其碱基配对关系，即确定了其二级结构。
-螺旋区堆积算法的主要操作方法就是尽最大可能列出所有螺旋区的所
有组合情况， 并找到其最小自由能的值为最小的组合结构，这个结构
就是最稳定的结构状态。无法预测假结

### 最大权重匹配算法
- 绘制折叠图：将RNA序列中的每一个碱基代表一个顶点，由这些顶点组成一个环的形状，
并根据Watson-Crick配对规则和G-U配对规则，在各碱基对中间，用一条线
把已构成配对的碱基连接起来。
 在整个匹配的过程中， 不允许出现一对多的碱基配对关系，只可以在两两
碱基之间形成一对一配对。
- 赋予权重
 利用合适的碱基配对权重打分的方法对各个碱基对赋予一定的权重，权重
的大小代表两个碱基间配对的概率。内边的权重为-2w，外边的权重为0。
 内边连接内点，外边连接外点和内点。
 算法的核心和难点问题是，怎样准确设置碱基配对的权重分值。

- 组合优化查找具有最大权重的结构：折叠图中每一种匹配线组合都是一个RNA的可能存在的结构， 选取总权重
值最大的那个匹配组合就是RNA序列最优的二级结构。
 具体实现：首先找到权重最大的边，并将边连结的两个顶点（两个碱基）
放入已匹配的集合，不参与下一步的寻找（一个碱基只参与一次配对），
利用同样的方法寻找其他的边，直到增加其他的边不再增加整个结构的权
重。

- 对最大权重匹配算法进行扩展可用于预测三联配
从所有内点组成的折叠图开始，依次利用外边（权重为0）替换最小的内边
（权重为负值） ， 使总权重达到最大值。
 被替换的内边构成了最大权重的匹配组合。
- 2-matching
 A graph in which no vertex is connected to more than two other
vertices.

- A derived graph for maximum weighted 2-matching

- 计算权重得分值
碱基配对权重的得分值是整个算法的核心和瓶颈所在。一张合理的得
分表应该尽可能考虑各种理论或实验上的因素，使得依据它预测出来
的最优结构应与实际尽量一致。
三种计算得分值的方法：
 采用比较序列中的互信息值，
 利用最小自由能算法中的能量参数，螺旋区作图得分


# 蛋白质三级结构预测方法
## 同源建模
与目标序列相似度>30%的已知结构模版
建立结构模型步骤：
1. 搜索结构模型的模板（T）：利用序列数据库搜索工具（BLAST,PSI-BLAST,FASTA）得到与目标蛋白相近的序列集，进而得到这些序列的结构集
2. 序列比对：CLUSTAL Omega(多重比对工具)，发现目标序列中与所有模板结构高度保守的区域以及保守性不高的区域
3. 建立主链结构：将模板结构叠加起来，找到**结构上保守的区域**。根据结构保守区将模板结构的坐标**拷贝**到目标蛋白中，从而构建目标蛋白的主链结构。最后，调整目标结构中主链各个原子的位置，使主链骨架构象符合立体化学原则，先建立模型的主链结构，再为loop区建模：1.**从头预测**，根据物理化学和量子化学原理来预测能量最低的稳定结构；2.基于已知环区结构的**同源建模**
4. 构建目标蛋白质的环区：loop区建模：1.**从头预测**，根据物理化学和量子化学原理来预测能量最低的稳定结构；2.基于已知环区结构的**同源建模**
5. 构建目标蛋白质的侧链：首先用目标序列的片段搜索**旋转异构体数据库**得到相似的片段，再从数据库中提取**侧链的空间取向**，在此基础上构建这段片段的侧链结构，利用**能量最小化原理进行优化**，使目标蛋白质的侧链基团处于能量最小的位置，具有稳定的构象
6. 优化模型：对预测出的蛋白机构质量评估，寻找结构中的一些异常构象,QMEAN,composite scoring function for the estimation for the global and local model quality.	QMQE(Global Model Quality Estimation) GMQE值越大，模可靠性越高；TM-align ：与PDB实验中测定的结构比对

工具1：SWISS-MODEL  分为四步：identification of structural templte,alignment of target sequence and template structure,model building ,and model quality evaluation.

第一模式mode中，只需要氨基酸序列作为输入数据，服务器自动选择合适的模版；第二种模式，the server will build the model bases on the given alignment；第三种模式：allows the user to submit a manually optimized modeling request to the SWISS-MODEL server,the model give the user control over a wide range of parameters

工具2：MODELLER:a popular software tools for producing homology models by satisfaction of spatial restraints using methoodology derived from NMR spectroscopy data processing.the method relies on an input sequence alignment between the targer amino acid sequence to be modeled and a template protein which structure has been solved.

## 折叠识别
当**缺乏同源性较高的模板**时，就需要用远程同源建模、折叠识别：fold recognition是为目标序列指定folds,通常该序列和已知结构的挡不住序列有**较低的序列相似度**，fold recognition 使用能量函数或其他相似分数的方法来比较目标序列和fold templates,具有最低能量函数或者相似分数高的模板将作为目标序列的fold

## 蛋白质分类:α：由少量α螺旋组成；β：由少量β折叠组成；α+β：包括α和β折叠，其中β折叠大部分是反平行的；α/β：该类蛋白质包括α和β折叠，其中β折叠大部分是平行的
### 分类数据库：对已知三维结构的蛋白质进行分类，并描述其结构和进化关系。首先用自动程序进行结构比较，然后人工检查来建立分类。在SCOP数据库中，蛋白质结构被分为折叠类CLASS（折叠类别：β折叠,把有相似二级结构组成的结构，但三界结构不同进化起源不同的成组）、折叠FOLD（the different shapes of domains within a class，折叠类中域的不同形状,如果具有相同折叠子的蛋白质具有相同的主要二级结构和拓扑连接，这些蛋白质就被认为共享一个相同的fold（折叠子），具有相同折叠子的蛋白质具有相同的二级结构、拓扑连接和空间分布，但进化上不一定有联系）、超家族（the domains in a fold are grouped into superfamilies,which hace at least a distant commmno ancestor ，如果蛋白质序列相似性低，但结构相似，具有同样的折叠子，往往执行相似的功能或者功能在进化上有联系，这类蛋白质视作属于同一超家族。通常，一个折叠子可以分为若干个超家族）和家族(the domains ina superfamily are grouped into familes,which have a more recent common ancestor，通常将相似性程度在30%以上的蛋白质归为“蛋白质家族”，同一蛋白质家族的成员称为“同源蛋白质”)四个层次

步骤：1.结构模体数据库的构建：从蛋白质结构数据库（PDB,FSSP,SCOP,CATH）中选取蛋白质结构。2.设计打分规则，基于已知结构和序列之间的关系为靶序列和模体之间的fitness打分 3、threading alignment:将靶序列与每一个结构模板比对优化设计的打分规则 4、Threading prediction:select the threading alignment that is statistically most probable as the threading prediction.Then construct a structure  model for the target by placing the backbone atoms of the target seuence at their aligned backone positions of the selected structural template

工具：Phyre 和 phyre2

Stage1:收集同源序列，HHblits筛选后，the resulting 多序列比对用PSIPRED预测二级结构，序列比对和二级结构预测隐马尔科夫模型

stage2：fold library scanning,this is scanned against a database of HMMs of proteins of known structure.The top-scoring alignments from this search are used to construct ceude backone-only models

Stage2:loop modeling:indels in these models are corrected by loop modeling

stap4:side-chain-alignment:amino acid chains are added to generate the final Phyre2 model

## 从头预测
基于分子动力学，寻找能量最低的构象，计算量大，只能做小分子预测
不直接使用已知模板的方法，蛋白质三级结构是从原始序列氨基酸预测来的

基本思想：De novo conformation predictors usually function by
producing candidate conformations (decoys) and then choosing
amongst them based on their thermodynamic stability and energy
state.

Most successful predictors will have the following three factors in
common:
- An accurate energy function that corresponds the most
thermodynamically stable state to the native structure of a protein.
- An efficient search method capable of quickly identify lowenergy states through conformational search.
- The ability to select native-like models from a collection of
decoy structures.

工具：I-TASSER  QUARK

Zhang’s lab has developed a number of algorithms for protein 3D structure
prediction, including I-TASSER for iterative protein structure
assembly, QUARK for ab initio protein folding,
and MUSTER and LOMETS for protein template structure identification,
some of which have been recognized as the world's best and widely used by
the community.

QUARK:three steps:
-The first step is for multiple feature predictions and fragment
generation starting from one query sequence.
-The second step is structural constructions using REMC(replicaexchange Monte Carlo) simulation based on the semi-reduced protein
model.
-The third step is for decoy structure clustering and full-atomic
refinement.

## 蛋白质功能预测
CAFACTOR工具：protein function prediction,based on the sequence-to-structure-to-function paradigm

# 蛋白质-核酸互作检测

按照DNA,RNA结合蛋白质在遗传信息传递过程中发挥的作用，将其分为DNA结合蛋白质（在基因转录过程中，包括转录位点识别、对外界和细胞内信号刺激做出回应），RNA结合蛋白质（基因翻译过程）

转录因子：DBD（DNA-binding domains）,TAD(trans-activing domain),SSD(signal sensing domain)

DNA结合结构域：转录因子至少包含一个DNA结合结构域

bHLH is a protein structural motif that characterizes one of the largest families of 转录因子二聚体，这个模体由两个α螺旋构成,有loop连接，通常是较大的那个螺旋半汉DNA结合结构域，一般来说有这个结构域的转录因子是二聚体，都包含可以促进DNA结合的氨基酸残基

HTH(helix-turn-helix)是DNA结合的一个主要的结构motif
由两个α螺旋由短的转角组成，在许多调控基因表达的蛋白质中发现

bZIP (Basic-region leucine zipper) is a common three-dimensional structural
motif in proteins. The bZIP domain is 60 to 80 amino acids in length with a
highly conserved DNA binding basic region and a more diversified leucine
zipper (亮氨酸拉链) dimerization region.

亮氨酸拉链：两组平行走向，带氨基酸的α-螺旋形成的对称二聚体

The bZIP interacts with the DNA via its N-terminal, where the lysines(赖氨
酸) and arginines（精氨酸） are located; these basic residues interact in
the major groove of the DNA.

The leucine zipper is located in the C-terminal region of the bZIP and it forms
an amphipathic（两亲性） alpha helix. When that alpha helix dimerizes, the
zipper is formed.

The hydrophobic side（疏水端） of the helix forms a dimer with itself or
another similar helix, burying the non-polar amino acids away from
the solvent（溶剂） . The hydrophilic side（亲水端） of the helix interacts
with the water in the solvent.

锌指结构存在于类固醇激素受体超家族和一些转录因子中，属DNA结合蛋白。
锌指结构约由30个氨基酸残基组成。 4个半胱氨酸残基（C）或2个半胱氨酸残
基和2个组氨酸残基（H）与1个锌离子配价结合。其中前一对半胱氨酸残基与
后一对半胱氨酸（或组氨酸）残基之间相隔12个氨基酸残基。
锌指结构的-螺旋部分是与DNA结合的部位

核糖体：Ribosomes consist of two subunits that fit together and work as one to
translate the mRNA into a polypeptide chain during protein synthesis.

RNA结合蛋白（RBPs）：结合到RNA的单链或双链上参与核糖核蛋白的生成。包含（RRM:RNArecognition motif，包含四个反向平行的β链（和RNA互作的部分）和两个α螺旋结构组成，有很强的可塑性在各种生物学功能中有重要的作用）,dsRBM（在RNA定位，RNA干扰，RNA编辑和转录表达中有重要作用）,锌指结构
interact along the RNA duplex via both α-helices and β1-β2 loop

查询结构域：UniPort Knowledgebase(UnitProtKB)数据库，Pfam(protein families )

## 生物大分子结构的测定方法
### 一级结构
- 核酸：生物芯片或测序技术
- 蛋白质：质谱等
### 高级结构
- X射线晶体衍射技术：基于衍射线的方向和衍射线的强度
- 多维核磁共振技术（NMR）：在原子分辨率下测定溶液中生物大分子三维结构的唯一方法

## 蛋白质-核酸互作的检测：给定的蛋白质是否结合于RNA，如果结合的话，蛋白质序列中的哪个残基部分和RNA连接，蛋白质-RNA结合复合体的结构是怎么样的
机器学习方法：序列特征（PSSM）+结构特征预测蛋白质与核酸结合相关信息

两类预测算法：一些方法使用结构信息预测DNA结合位点，需要蛋白质三维结构信息，另外的仅需要序列细腻洗，不需要蛋白质结构信息。

用来预测结合蛋白质和结合残基的特征分为两类：结构特征和序列特征
- sequence features:1,氨基酸序列，为预测提供基本信息，原始的序列用2--dimensional binary vactor:0101编码，二进制，2.PSSM矩阵使用sequence profiles，profiles are usually generated using PSI-BLAST, 3、Global composition pf amino acids:临近残基也被作为组成feature.   4.残基类别
- structural features:1.structural motifs  2.surface curvature（表面弯曲程度） 3.secondary structure:每个残基的二级结构分为三类：螺旋，折叠，无规卷曲，一组三个数表示残基的构象（1,0,0）表示螺旋结构。4.SASA(solvent accessible surface area)(溶剂可及面积)

DISPLAR: DNA Interaction-Site Prediction from a List of Adjacent Residues.基于蛋白质结构信息预测，训练并检验一个神经网络预测
SASA(溶液可达表面积)+PSSM(位置权重矩阵)

# 蛋白质-蛋白质互作
结构域：给定蛋白质保守区域的部分，可以发展，发挥功能，独立于蛋白质链的其他部分

通常，有一个由亲水性残基包围而成的疏水性残基

结构域的类别： SH2：在肿瘤蛋白和其他细胞内信号转导蛋白中结构上保守的蛋白质域，绑定与磷酸化的酪氨酸残基上

SH3：SRC Homology 3 Domain(SH3 domain):大约由60个氨基酸残基组成，在信号转导通路中负责控制蛋白质-蛋白质互作，调控参与细胞质信号的蛋白质互作

PDZ domain:在真核细胞中广泛分布，由80-90个氨基酸组成的在细菌，酵母菌，植物，病毒和动物信号蛋白中肺癌吸纳的普通的结构域，含有这些域的蛋白质互相协作组织信号复合物
，在信号转导复合物的形成和功能中有重要作用，由疏水氨基酸组成，许多PDZ域只在β链和长的α-链中有一个结合位点

检测PPI的方法：最通用的高通量方法是酵母双杂交系统，亲和纯化和质谱的连用

PPT的预测是一个结合了生物信息学和结构生物学的领域，目的在于鉴别和分类蛋白质组或对之间的交互作用。

## PPI数据库
### primary database:通过小范围或大范围的实验手段测定的公开的PPI：DIP,BIND,BioGRID,HPRD(human protein reference database)

### meta-database:通过对原始数据库信息的整合承德数据库，也收集部分原始数据，APID,MPIDB,PINA,GPS-Prot

### 预测数据域：PIPs(protein-protein interaction prediction),I2D(interlogous interaction database),UniHI，Predictome(预测蛋白质功能联系的数据库) OPHID（在线预测人类蛋白相互作用数据库）

综合数据库：STRING 数据库收纳了已知或预测的蛋白质通路，这些交互包括物理上的和功能上的关联，收集于计算机预测，

参与互作的蛋白质偏向于协同进化，基于蛋白质对的系统发生距离寻找互作的差异，研究

## ppi预测
interology mapping:直系同源互作对（不同物种间一对直系同源蛋白间保守的互作关系）映射法，只依靠蛋白序列的比对

domain-domain interaction mapping:结构域-结构域互作映射法，查找保守的互作结构域预测一对蛋白的互作关系:只需要蛋白质序列信息就可以进行预测，保守的互作结构域常有相关的蛋白质复合体结构信息的支持

phylogenetic profile:系统发育谱法,通过检验一对蛋白是否在不同生物基因组间共同出现或消失，为未知功能蛋白质注释

simailarity of phylogentic trees:系统发育树相似性法 mirror tree(镜像树方法)。基于蛋白质的共进化

Gene neighborhood:基因邻居法，功能相关的基因倾向于在基因组中连在一起，构成一个操纵子

Gene fusion:基因融合法，蛋白质功能相关或相互作用的指示，代谢蛋白中普遍存在基因融合现象

关联法：两个蛋白质间除了直接物理互作外存在其他功能关联，如基因表达、蛋白质定位、参与到同一生物学过程中

PPI预测工具

iLoop:输入包括要查找的蛋白质的结构和要检测的蛋白质对,structural features根据序列相似性比对到要查找的蛋白质上。结构特征根据相似度分类 ，使用随机森林分类器

蛋白质-蛋白质对接：第一步：尽可能构建大量的复合物构象；第二步用一种评分函数对构象进行打分排序，最后一步，对构象进行能量优化，使之接近天然复合物的结构

蛋白质对接算法：刚性对接，复合体的键角，键长，扭转角在复合物形成的任一阶段都没有被修改，替换吗，，，，蛋白质对接软件：ClusPro,3D-Dock Suite,3D-Garden

# 染色质结构
DNA压缩：147bp的DNA缠绕组蛋白把具体近两圈形成核小体，核小体呈螺旋状排列构成纤丝状，纤丝再进一步压缩成常染色质，有丝分裂时染色质进一步压缩为染色体，DNA长度被压缩了7000倍

核小体时染色质的基本结构单位，结构呈串珠样

组蛋白的五个主要家族：H1/H5（连接组蛋白）,H2A,H2B,H3,H4（核心组蛋白，以二聚体的形式存在，两个H2A-H2B二聚体组成一个H3-H4四聚体，高度保守结构相似，all featuring a 'helix loop helix loop helix' motif）,组蛋白末端tail-组蛋白修饰（表现）组蛋白编码：用组蛋白修饰预测基因表达水平

染色质：DNA组装成核小体，并进一步组装成染色质

表观遗传相关修饰：DNA甲基化，组蛋白乙酰化，染色质重塑

## 染色质层级结构
TADs:拓扑相关结构域：在不同的细胞类型中连续，大部分保守的启动子和增强子间的交互不经过TAD边界。TAD边界区的破坏可能印象附近基因的表达，从而引起疾病的发生。

## 染色质结构的测定
Chromosome confirmation capture(3C,染色体构象捕获技术)，通过分析基因组座之间的interaction 频率决定染色质构象

3C-based方法：甲醛处理、交联固定，酶切或超声打断、加入连接酶、蛋白酶解交联、测定
，通过PCR产物的有无，产量的高低，对是否存在相互作用进行判断

4C：反向PCR：PCR只能扩增两端序列已知的基因片
段，反向PCR可扩增一段已知序列的
两端未知序列。 反向PCR的目的在
于扩增一段已知序列旁侧的DNA，
也就是说这一反应体系不是在一对
引物之间而是在引物外侧合成DNA。

HI-C 使用高通量测序技术寻找片段的核苷酸序列，使用双末端测序

技术原理：
1. 甲醛交联：首先用甲醛秃顶细胞内自然状态下交联在一起或空间距离非（1）甲醛交联：首先用甲醛秃顶细胞内自然状态下交联在一起或空间距离非
常接近的DNA-蛋白质或蛋白质-蛋白质复合物。
2. 酶切和末端标记：用限制性内切酶消化和分离染色质，并用生物素标记
片段末端。
3. 片段末端连接：利用DNA连接酶把不同切口末端连接起来，形成了一个
环状的嵌合分子。
4. 纯化和剪切：将这些环状分子进行纯化，并剪切打碎成DNA片段。
5. 建立Hi-C文库：使用生物素标记沉淀技术获取有标记的目标DNA片段，
并选择合适大小的DNA片段，建立Hi-C文库。
6. 测序分析：利用高通量测序技术对Hi-C文库包含样品进行双末端测序，
产生大量可供分析的配对末端片段。

CHIA-PET技术结合Hi-C和CHIP-seq技术检测交互

HI-C数据处理
- 序列比对：将测序获得的reads比对到参考基因组上，获得有效的相互作用数据。
- 数据过滤： 由于测序过程中会存在测序误差，或者比对不匹配，质量不合格，以及数
据偏向性等问题，因此需要进行数据过滤和平衡处理。
- 构建染色质交互矩阵：构建染色质交互矩阵，通过图形化处理检测数据质量以及分析染色质构象
间具体的相互作用情况。
- 标准化：对交互矩阵进行标准化或平衡处理。

实验部分：可视化及看懂heatmap图



# Paper-reading-notes
This repository is used to save my reading notes and some codes from github.

一、ASPEM:Embedding Learning by Aspect in Heterogeneoous Information Networks
https://github.com/ysyushi/aspem.git related code and database

·ABSTRACT

HIN(Heterogeneous information networks)是一种现实世界应用中无处不在的网络类型。本文提出‘aspect’的概念，每一个HIN的aspect是一个单元，表示一种特定的语义，并且提出一种全新的embedding learning framework——‘ASPEM’，这种framework可以基于多种aspect的分类来尽可能的多保留HIN中的语义信息。

·1 INTRODUCTION

文章首先引出HIN的概念。其中，IMDB是一种典型的HIN类型，包含着‘用户对于电影的偏好’的相关信息。IMDB有五种不同的结点类型：user,movie,actor,director,and genre.随后，作者还介绍了network embedding的概念，也是我们在ASPEM中需要做的：projects the network into low-dimensional space,where each node is represented using a corresponding embedding vector,and the relativity among nodes is preserved.这里的embedding vectors可以直接当做node feature在接下来的不同的应用中使用，比如node classification。

问题是，如果我们直接embed all the nodes into one low-dimensional space,可能会导致信息丢失。这里作者以IMDB为例说明information loss。所以，为了解决这个问题，可以将IMDB embedding into 2 distinct space.

所以ASPEM framework有几个优点：①通过将网络分为不同的aspect，可以减少aspect间的incompatibility；②不需要addition supervision；③can be extended.

·2 RELATED WORK

这里主要介绍了运用ASPEM framework需要了解的一些概念和算法：HIN,Network embedding（许多的homogeneous network embedding一般使用word2vec中的skip-gram模型）,HIN embedding（一般learning embedding by traversing all edge types and sampling one dege at a time for each edge type.同时介绍了几种HIN embedding算法）。

·3 PROBLEM DEFINITION

(1)HIN

假设一个information network是一个有向图G=(\nu ,\varepsilon )，其中node mapping\phi :\nu →T,edge type mapping\psi :\varepsilon →R.当结点的数量大于一或者边的数量大于一时，我们把这种网络叫做HIN。HIN can be abstracted using a network schema\tilde{G} =(\nu ,\varepsilon )。
(2)ASPECT OF HIN

对于一个给定的HIN\tilde{G} =(\nu ,\varepsilon )的一个aspect就是G的一个子图。T^a \subseteq  T,R^a \subseteq  R.

(3)HIN EMBEDDING FROM ASPECT

对于一个HIN的其中一个aspect，embedding learning in HIN from one aspect a is to learn node embedding mapping.也就是将该aspect内的所有node逐一嵌入到一个d维的向量空间中，即每个node都有一个d-dimensional vector作为其embedding.所有的embedding learning in aspects简单级联就是整个HIN的embedding learning.

·4 THE ASPEM FRAMWORK

作者这里把ASPEM framework分为了三步。第一步是根据不同的要求在HIN中选择a set of representative aspects构成层次集合；第二步是对每个aspect分别做embedding；第三部是将不同aspect的embedding做整合。

4.1 HINs的层次选择

这里作者引入了一个Inc(·)来描述每个层次的不兼容性。这里的Inc(·)必须满足三个特性：非负性、单调性和凸性。

作者认为，不兼容性来自于存在不同类型的边。所以，为了计算不兼容性，我们可以从最简单的场景开始：三种点由两种不同类型的边连接：


这时，我们可以计算由这三种点和两种边组成的一个层次的不兼容性：


其中


表示对a层次的所有子层的不兼容性的遍历结果求和。

这里作者利用Jaccard系数来衡量两种类型的边的不一致性，也就是前面讨论的不兼容性。


这里计算的是当\phi_{c} 作为中心结点以\psi_{r} 和\psi_{l} 为连接其他两种结点的边的不兼容度。也就是子层次中的一个不兼容度。
这里解释两个概念：

①Jaccard系数：用于比较有限样本集之间的相似性与差异性。Jaccard系数值越大，样本相似度越高。给定两个集合A,B，Jaccard 系数定义为A与B交集的大小与A与B并集的大小的比值，定义如下：


所以，如果我们要计算的是不兼容度，我们应该取Jaccard系数的倒数。所以4.2式的结果越小，证明两种边的不兼容度越低，相似性越高。当结果为0时，证明两种边是相同的。其中， 差距越大说明不一致性越高，当相似性完全一样时则算出来为0，这就是后面减1的作用。

②邻接矩阵：逻辑结构分为两部分：V和E集合。因此，用一个一维数组存放图中所有顶点数据；用一个二维数组存放顶点间关系（边或弧）的数据，这个二维数组称为邻接矩阵。


这里作者将不一致性的平均值作为不兼容性的衡量，也就是将该层次中每个结点作为中心结点计算出的不兼容度相加，再除以结点的个数。

为了选出一个由合适的层次组成的集合，作者认为应该设置一个阈值\theta ，当一个层次满足以下规则的时候，将该层次选入集合：
①一个子层的不兼容性如果大于\theta ，那么不被选入子层集合；

②如果两个子层a_{1} 、a_{2} 同时都满足不兼容性的要求，但是a_{1} \subset a_{2} ，那么我们将a_{2} 选入子层集合，这样是为了确保子层集合的简洁性。（即，若入选的子层互相包含，只选择覆盖范围最高的子层进入集合）
4.2 Embedding Learning from One Aspect

这里作者使用skip-gram模型作为单个层次的embedding algorithm进行学习。同时，单个层次也可以直接整合应用其他homogeneous network的嵌入方法。

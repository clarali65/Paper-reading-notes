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
假设一个information network是一个有向图G=(V,E)，其中node mapping:edge type mapping:.当结点的数量大于一或者边的数量大于一时，我们把这种网络叫做HIN。

# Open BERT IR

本项目为大家提供了多个开源的中文文本排序模型。

NDCG评价指标定义及价值：

标准化折扣累积增益 (nDCG) 是最常用于衡量网络搜索结果质量的指标。
与上述其他指标不同，nDCG 是专门为分级相关性判断而设计的。 
例如，如果相关性以五分制衡量，rel(q, d) 将返回 r ∈ {0, 1, 2, 3, 4}。

首先，我们定义折扣累积收益（DCG）：

DCG(q, d)=Sigma\[(2^r-1)/log(i+1)\]

这里使用的增益是指效用，即用户从特定结果中获得多少价值。
此计算涉及两个因素：(1) 相关性等级（即，高度相关的结果比相关结果“更有价值”）和 (2) 结果出现的排名（相关结果接近顶部） 排名列表更“有价值”）。
折扣是指当用户消费排名列表中越来越低的结果时增益（效用）的衰减，即因子（2）。

最后，我们引入标准化：
nDCG=DCG/IDCG

其中 IDCG 表示“理想”排名列表的 DCG：这将是一个排名列表，从最高相关等级的所有文档开始，然后是具有下一个最高相关等级的文档，依此类推。
因此，nDCG 表示归一化的 DCG 相对于最佳可能排名列表的 \[0, 1\] 范围。 
通常，nDCG 与等级截止相关； 10 或 20 的值很常见。 
由于大多数商业网络搜索引擎在一个页面上显示十个结果（至少在桌面上），因此这两个设置代表相对于结果的第一页或前两页的 nDCG。 
出于类似的原因，考虑到手机的屏幕尺寸要小得多，nDCG@3 或 nDCG@5 经常用于移动搜索环境中。

该指标在评估网络搜索结果时很受欢迎，原因有很多：
首先，nDCG 可以利用分级相关性判断，从而对输出质量提供更精细的区分。 
其次，正如眼球追踪研究所揭示的那样，折扣和截止代表了现实世界用户行为的相当准确（尽管经过简化）的模型； 例如，参见 Joachims 等人\[2007\]。
用户确实倾向于线性地扫描结果，随着他们消耗越来越多的结果（即，在排名列表中进一步向下移动），“放弃”和“失去兴趣”的可能性越来越大。
这是在折扣中建模的，并且 nDCG 的变体应用不同的折扣方案来对用户行为的这方面进行建模。
当用户停止阅读（即放弃）时，截止值会模拟硬停止。
例如，nDCG@10 量化浏览器中搜索结果第一页的结果质量，假设用户从未单击“下一页”（这种情况很常见）。

# 模型评价

在scifact数据集下，对比不同的SentenceTransformer模型的nDCG值。


| 数据集      | Method                | model                                                                | 语言 | NDCG@1 |NDCG@10 |NDCG@100 |NDCG@1000 |
|----------|-----------------------|----------------------------------------------------------------------|----|--------|--------|--------|--------|
| scifact  | Dense Retrivel        | msmarco-distilbert-base-tas-b                                        | en | 0.5333 |0.6428|0.6698|0.6811|
| scifact  | Dense Retrivel        | multi-qa-distilbert-cos-v1                                           | en | 0.4700 | 0.5957| 0.6322| 0.6425|
| scifact  | Sparse Retrivel       | BeIR/sparta-msmarco-distilbert-base-v1                               | en | 0.5033 | 0.5978| 0.6311| 0.6450|
| scifact  | Dense Retrivel/ReRank | msmarco-distilbert-base-tas-b && cross-encoder/ms-marco-electra-base | en | 0.5767 | 0.6643| 0.6958| 0.6958|
| scifact  | Dense Retrivel        | msmarco-distilbert-base-v3                                           | en | 0.4000 |0.5082|0.5500|0.5671|
| scifact  | Dense Retrivel        | GPL/scifact-msmarco-distilbert-gpl                                   | en | 0.5467 |0.6641|0.6880|0.6970|
| scifact  | Dense Retrivel        | GPL/scifact-distilbert-tas-b-gpl-self_miner                          | en | 0.5667 |0.6736|0.7043|0.7123|
| scifact  | Dense Retrivel        | pritamdeka/S-PubMedBert-MS-MARCO-SCIFACT                             | en | 0.7900 |0.8621|0.8718|0.8718|
| scifact  | Dense Retrivel        | income/contriever-gpl-scifact                                        | en | 0.5667 |0.6663|0.6983|0.7065|


# 测评日志

[sentence_bert_evaluation.ipynb](colab%2Fsentence_bert_evaluation.ipynb)包含Dense、Sparse以及ReRank测评脚本

[sentence_bert_evaluation_dense.ipynb](colab%2Fsentence_bert_evaluation_dense.ipynb)使用Dense模型，测试了尽可能多的模型的NDCG值。


## 关注我们
欢迎关注知乎专栏号。

[深度学习兴趣小组](https://www.zhihu.com/column/thuil)


## 问题反馈 & 贡献
如有问题，请在GitHub Issue中提交。  
我们没有运营，鼓励网友互相帮助解决问题。  
如果发现实现上的问题或愿意共同建设该项目，请提交Pull Request。


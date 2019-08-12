# 【Task7】实践

1. 计算每个content的CTR。

数据集下载：链接：https://pan.baidu.com/s/1YDvBWp35xKLg5zsysEjDGA 提取码：rpgs 

2. 【选做】 使用Spark实现ALS矩阵分解算法

movielen 数据集：http://files.grouplens.org/datasets/movielens/ml-100k.zip

 [基于ALS矩阵分解算法的Spark推荐引擎实现](https://www.cnblogs.com/muchen/p/6882465.html)

3. 使用Spark分析Amazon DataSet(实现 Spark LR、Spark TFIDF)

数据集：[http://jmcauley.ucsd.edu/data/amazon/](http://jmcauley.ucsd.edu/data/amazon/)

**preprocess**
	
	* Spark LR
	
	* Spark TFIDF
---
## 计算每个content的CTR  
```python
import pandas as pd
df = pd.read_csv("file:///usr/local/lily/task7/content_list_id.txt", sep="\t")
df['content_list_len']=df['content_list'].apply(lambda x:len(x.split(',')))
df['content_id_len']=df['content_id'].apply(lambda x:len(x.split(',')))
df['ctr'] = df['content_id_len'] / df['content_list_len']
```
## Spark实现ALS矩阵分解算法
[pyspark实现ALS矩阵分解算法](https://blog.csdn.net/qq_39315740/article/details/99298947)  
## 使用Spark分析Amazon DataSet(实现 Spark LR、Spark TFIDF)
[Spark分析Amazon DataSet(实现Spark TF-IDF)](https://blog.csdn.net/qq_39315740/article/details/99304266)  

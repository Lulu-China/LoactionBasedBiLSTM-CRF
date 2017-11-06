# NER-LSTM-CRF
An easy-to-use named entity recognition (NER) toolkit, implemented the LSTM+CRF model in tensorflow.

## 1. Model
Bi-LSTM/Bi-GRU + CRF.

## 2. Usage
### 2.1 数据准备
训练数据处理成下列形式，特征之间用制表符(或空格)隔开，每行共n列，1至n-1列为特征，最后一列为label。

地	地址	D_N	N	N	N	N	O
址	地址	D_N	N	N	N	N	O
：	：	D_W	N	N	N	N	O
北	北京市	A_NS	N	N	Y	N	B_NS
京	北京市	A_NS	N	N	N	N	I_NS
市	北京市	A_NS	Y	N	N	N	I_NS
海	海淀区	A_NS	Y	N	N	N	B_NS
淀	海淀区	A_NS	N	N	N	N	I_NS
区	海淀区	A_NS	N	N	N	N	I_NS
首	首体南路	A_NS	N	N	N	N	B_NS
体	首体南路	A_NS	N	N	N	N	I_NS
南	首体南路	A_NS	N	N	Y	N	I_NS
路	首体南路	A_NS	Y	N	N	N	I_NS
2	22号	D_MQ	N	N	N	N	O
2	22号	D_MQ	N	N	N	N	O
号	22号	D_MQ	N	N	N	N	O
国	国兴大厦	A_NS	Y	N	N	N	B_NS
兴	国兴大厦	A_NS	N	N	N	N	I_NS
大	国兴大厦	A_NS	N	Y	N	N	I_NS
厦	国兴大厦	A_NS	N	N	N	N	I_NS
1	10层	D_MQ	N	N	N	N	O
0	10层	D_MQ	N	N	N	N	O
层	10层	D_MQ	N	N	N	N	O
&	&	D_W	N	N	N	N	O
#	#	D_W	N	N	N	N	O
1	13	D_MQ	N	N	N	N	O
3	13	D_MQ	N	N	N	N	O
;	;	D_W	N	N	N	N	O
### 2.2 修改配置文件
- Step 1: 将上述训练文件的路径写入配置文件`config.yml`中的`data_params/path_train`参数里；
- Step 2: 以上样例数据中每行包含三列，分别称为`f1`、`f2`和`label`，首先需要将需要将`model_params/feature_names`设置为`['f1', 'f2']`，并将`embed_params`下的名称改为相应的feature name，其中的`shape`参数需要通过预处理之后才能得到(Step 3)，`path_pre_train`为预训练的词向量路径，格式同gensim生成的txt文件格式；
- Step 3: 修改`data_params`下的参数：该参数存放特征和label的voc(即名称到编号id的映射字典)，改为相应的路径。

### 2.3 预处理
- 运行`python/python3 preprocessing.py`进行预处理，会得到各个特征的item数以及label数，并自动修改`config.yml`文件中各个feature的`shape`参数，以及`nb_classes`参数。
- 需要注意的是，若提供了预训练的embedding向量，则特征embedding的维度以预训练的向量维度为准，若没有提供预训练的向量，则第一列的特征向量维度默认为64，其余特征为32，这里可以根据实际情况进行调整。

### 2.４ 训练模型
- 训练模型：根据需要调整其余参数，其中dev_size表示开发集占训练集的比例（默认值为0.1），并运行`python/python3 train.py`。

### 2.５ 标记数据
- 标记数据：`config.yml`中修改相应的`path_test`和`path_result`，并运行`python/python3 test.py`。

### 2.6 参数说明
- model_params/bilstm_params:


```
num_units: bilstm/bigru单元数，默认256;
num_layers: bilstm/bigru层数；
use_crf: 是否使用crf层；
rnn_unit: lstm or gru，模型中使用哪种单元；
learning_rate: 学习率；
clip: None or int, 梯度裁剪；
dev_size: 训练集中划分出的开发集的比例，shuffle之后再划分；
dropout_rate: bilstm/bigru输出与全连接层之间；
l2_rate: 加在全连接层权重上；
nb_classes: 标签数（preprocessing.py输出结果上+1）;
sequence_length: 句子最大长度（preprocessing.py输出结果中有句子分布）；
batch_size: batch size；
nb_epoch: 迭代次数；
max_patience: 最大耐心值，即在开发集上的表现累计max_patience次没有提升时，训练即终止；
path_model: 模型存放路径；
sep: 'table' or 'space'，表示特征之间的分隔符；
some other parameters...
```

## 3. Requirements
- numpy
- tensorflow 1.2.0+
- pickle
- tqdm
- pyyaml

## 4. References
- 参考论文：[http://www.aclweb.org/anthology/N16-1030](http://www.aclweb.org/anthology/N16-1030 "http://www.aclweb.org/anthology/N16-1030")
- 参考项目：[https://github.com/koth/kcws](https://github.com/koth/kcws "https://github.com/koth/kcws") ; [https://github.com/chilynn/sequence-labeling](https://github.com/chilynn/sequence-labeling "https://github.com/chilynn/sequence-labeling")

Updating...

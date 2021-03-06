# RandomForest

## 1.概念
- 随机森林是通过集成学习的思想将多棵树集成的一种算法，基本单元是决策树，本质属于集成学习
- 随机体现在随机样本，随机特征，使得不易过拟合，良好的抗噪能力
- 随机森林中的树如果生成？
	- 训练集为N，每颗树随机且有放回的抽取N个训练样本(bootstrap sampling),约1/3没有采到
	- 特质维度为M，指定一个常数m < M, 随机从M个特征中选取m个特征子集
	- 没有剪枝过程
- 随机森林分类效果（错误率）与什么有关？
    - 森林中任意两棵树的相关性，相关性越大，错误率越大
    - 每课树的分类能力，能力越强，森林错误率越低
    - m与上述两者呈正相关 


## 2. scikit-learn中的参数
- 决策树部分
	- criterion: ”gini” or “entropy”(default=”gini”)是计算属性的gini(基尼不纯度)还是entropy(信息增益)
	- max_features: RF划分时考虑的最大特征数,一般在特征不多时默认为None，max_features＝n_features，如果是分类问题则max_features＝sqrt(n_features)
	- max_depth: 最大深度，一般不设置，特征多样本多时，限制在10-100
	- min_samples_split: 内部节点再划分所需最小样本数,默认为2,一般不设置,特征多样本多时,增大此值
	- min_samples_leaf: 叶子节点最少样本数,默认为1,一般不设置,特征多样本多时,增大此值
- 随机森林特有参数
	- n_estimators: 决策树个数，默认100，与learning_rate一起考虑
	- oob_score: 即是否采用袋外样本来评估模型的好坏,建议开启，用于评估泛化能力
	- n_jobs：并行job个数；1=不并行；n：n个并行；-1：CPU有多少core，就启动多少job

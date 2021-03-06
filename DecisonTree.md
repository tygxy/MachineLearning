# 决策树

## 1.基本流程
- 递归进行划分
- 三种情况导致递归返回
	- 当前节点样本属于同一类别
	- 当前节点可用属性为空，或者所有样本在所有属性上类别相同
	- 当前节点包含的样本集合为空

## 2.划分选择（如何选择最优属性）

### 2.1 信息增益（ID3决策树）
- 集合为D，共m个标识，其中第k个标识所占比例为pk(k=1,2,...,m)
- 离散属性a有V个取值，第V个分支节点包含属性a上取值为av的样本记为Dv
- 信息熵的计算 
- 信息增益的计算
- 计算不同属性的信息增益，找到max,即选为最优属性

### 2.2 信息增益率（C4.5决策树）
- 为什么不用ID3？ 信息增益会天然对可取数目较多的属性有偏好
- 信息增益率的计算
- 属性a可能取值数目越大，IV(a)则越大，即分母越大
- 但是信息增益率会天然对可取数目小的属性有偏好，所以一般的步骤是先找出信息增益高于平均水平的属性，再从中选择增益率最高的

### 2.3 基尼指数（CART决策树）
- 基尼值的计算
- 基尼指数的计算
- 计算不同属性的基尼指数，找到min,即选为最优属性

## 3.剪枝处理

### 3.1 概念
- 为何要剪枝？ 防过拟合，通过剪枝提高泛化能力
- 如何评价泛化能力的提高？CV交叉验证

### 3.2 预剪枝
- 对每个结点进行评估，如果不能带来泛化性能的提升，则停止划分
- 根据划分前后，验证集的准确率高低，判断是否进行划分
- 预剪枝可能会带来欠拟合

### 3.3 后剪枝
- 训练完一颗决策树，从底向上判断非叶结点，如果将该结点替换成叶子结点能提升泛化能力，则替换
- 保留更多分支，欠拟合风险小，但训练开销大

## 4. 连续与缺省值

### 4.1 连续值怎么处理
- 二分法：将连续属性a的所有值排序，从a1-an；取两两中点ti，共找到n-1个t;使用不同的t,计算最大增益，则确定此时的t为划分点
- 可以同一属性划分多次

### 4.2 缺省值怎么处理


## 5. 多变量决策树
- 非叶节点不再仅针对一个属性，还是对属性的线性组合





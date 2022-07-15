# QCNN模型自验报告

<center>白琪 &nbsp qi.bai@zju.edu.cn</center>



[toc]

## 1. 模型简介

该模型使用QCNN对多体波函数进行分类。

抽象到具体的分类问题，就是对波函数进行二分类。量子比特数目确定的情况下，编码线路是给定的。每一个样本输入是encoder中的参数，输出是-1或1，分别代表顺磁相和铁磁相。分类网络使用的是QCNN。

### 1.1. 网络模型结构简介

QCNN主要包含卷积层和池化层，如果把conv和pooling看成是一个模块的话，每个模块共包含21个参数。下面分别介绍。

### 1.1.1 Conv层

结构如图：

![image-20220715104914171](C:\Users\richybai\AppData\Roaming\Typora\typora-user-images\image-20220715104914171.png)

1. 分别对qubits按顺序作用RX，RY，RZ门
2. 在qubits上按顺序作用ZZ，YY，XX门
3. 再分别对qubits按顺序作用RX，RY，RZ门

其中每个门的参数各不相同。有15个参数门，15个参数。

### 1.1.2 Pooling层

结构如图：

![image-20220715105336377](C:\Users\richybai\AppData\Roaming\Typora\typora-user-images\image-20220715105336377.png)

1. 分别对qubits按顺序作用RX，RY，RZ门
2. 在两个qubits上作用CX门，确定哪一个qubit在pooling后继续前向传播
3. 在保留的qubits上按顺序作用RZ，RY，RX门，注意此时参数与前面作用在这个qubit上的互为相反数

其中有些参数门公用同一个参数。有9个参数门，6个参数。



### 1.2. 数据集

### 1.2.1 原始数据集

数据集使用的是tensorflow-quantum中内置的[tfi_chain](https://tensorflow.google.cn/quantum/api_docs/python/tfq/datasets/tfi_chain?hl=zh-cn)数据集。数据集中共包含81个数据，每条数据对本任务有用的数据为

1. gamma：取值范围[0.2, 1.8]。确定样本为顺磁相和铁磁相，在训练过程中被二值化为-1和1
2. params：是encoder中的参数。不同qubits数目对应的参数数目不同

以4-qubit系统为例，展示样本encoder的线路：

![image-20220715110434933](C:\Users\richybai\AppData\Roaming\Typora\typora-user-images\image-20220715110434933.png)

1. 每个qubits作用H门
2. ZZ门按顺序两两作用在所有qubits上形成环，之后在每个qubits上作用RX门。注意此时的参数ZZ门都相同，RX门也都相同
3. 作用2中相同的门，此时参数采用新的参数

### 1.2.2 数据集增强

使用线性插值方法对样本中的gamma和params做了插值，增加样本个数。相邻的两个样本点中再插入11个点，这样样本数量增加到了961。



### 1.3. 代码提交地址

https://gitee.com/richybai/qcnn



## 2.   代码目录结构说明

（目录结构及文件说明：[需遵从model_zoo代码目录规范](https://gitee.com/mindspore/models/blob/master/how_to_contribute/CONTRIBUTING_ATTENTION_CN.md#目录结构)）

## 3.   自验结果

（包括所用MindSpore版本、自验环境、自验精度结果、论文精度等，推荐多提供截图参考。提供自验结果截图或日志文件）

### 3.1. 自验环境：

（所用硬件环境、MindSpore版本、python第三方库等说明）

### 3.2. 训练超参数：

（batch_size, epoch, learning rate, loss function, optimizer, 并行度等超参）

### 3.3. 训练：

模型训练

#### 3.3.1. 如何启动训练脚本：

训练如何启动：

（运行命令实例，建议附截图）

#### 3.3.2. 训练精度结果：

（训练精度结果，需附截图或日志文件）

## \4.   参考资料

### 4.1. 参考论文：

参考论文

### 4.2. 参考git项目：



参考Tensorflow, pytorch, caffe等项目网址
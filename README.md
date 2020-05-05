## Instance-Batch Normalization Network

这个仓库是fork from论文作者官方代码仓库的基础上再进行修改的,详情可以看原来的README.md([README_official.md](https://github.com/XingangPan/IBN-Net/blob/master/README.md))文档。

### Paper

Xingang Pan, Ping Luo, Jianping Shi, Xiaoou Tang. ["Two at Once: Enhancing Learning and Generalization Capacities via IBN-Net"](https://arxiv.org/abs/1807.09441), ECCV2018.

### Introduction
<img align="middle" width="500" height="280" src="https://github.com/XingangPan/IBN-Net/blob/master/utils/IBNNet.png">

- IBN-Net carefully unifies instance normalization and batch normalization in a single deep network.
- It provides an extremely simple way to increase both modeling and generalization capacity without adding model complexity.
- IBN-Net works surprisingly well for Person Re-identification task, see [michuanhaohao/reid-strong-baseline](https://github.com/michuanhaohao/reid-strong-baseline) and [strong baseline for ReID](https://arxiv.org/pdf/1906.08332.pdf) for more details.

### Requirements
- torch >= 1.0.0 (master branch)
- torchvision >= 0.2.4

### Modify
原作者虽然在论文中提及到IBN-b的结构,但这个结构只针对残差网络进行的：

<img align="middle" width="486" height="333" src="https://github.com/BohriumKwong/IBN-Net/blob/master/utils/images/IBN-b.jpg">

在官方代码仓库中,作者只有`resnet`和`se_resnet`同时实现了INB-a和IBN-b,而`densenet`和`resnext`只有IBN-a而没有IBN-b,不排除原作者觉得IBN-b的结构并不适用于这两个网络的可能。

尽管如此,我还是在官方代码的基础上,结合论文中IBN-b结构的描述,将在残差block中**{addition}**之后进行IN替换BN的处理,改为在**dense block**中**{transition}**层刚开始的时候(结合所有的dense concat等价于残差block中的addition),将IN替换BN,实现了自己构想中的`densenet-IBN-b`,我也没和作者进行沟通确定我写的这个结构是否合理,各位可以按需使用。以下是构建思路：

<img align="middle" width="827" height="500" src="https://github.com/BohriumKwong/IBN-Net/blob/master/utils/images/densenet-IBN-b.jpg">

然后是根据论文原文所述 "only add three IN layers after **the first convolution layer (conv1)** and **the first two convolution groups(conv2x, conv3x)**":

<img align="middle" width="867" height="423" src="https://github.com/BohriumKwong/IBN-Net/blob/master/utils/images/densenet-structure.jpg">

### Results
这里用的是我们自己使用的结直肠癌九分类的公开数据集CRC_9_CLASS进行的测试。其中我们只取颜色深浅筛选后的NO-NORM-100K数据集以及加入一半的COLOR-NORM-100K数据集作为训练集,剩下不同颜色深浅的NO-NORM-100K作为测试集;而从没参与任何训练过程的CRC-VAL-HE-7K数据集则作为验证集。训练时数据加载及增强使用到的我本人修改的`dataloader`的方法(详见 https://github.com/BohriumKwong/pytorch_data_augmention_loader )。
以下是测试的结果：

acc/loss (base on focal loss,the weight of 'TUM' is 1.1 others are 1) on the CRC_9_CLASS test dataset(CRC-VAL-HE-7K) are reported.

| Model                     | origin         |  IBN-a Net      | IBN-b Net     |
| -------------------       | ------------------ | ------------------ | ------------------ |
| DenseNet-121          | 84.8%/0.943             | 92%/0.86       | 95.9%/0.814              |



### Citing IBN-Net
```
@inproceedings{pan2018IBN-Net,
  author = {Xingang Pan, Ping Luo, Jianping Shi, and Xiaoou Tang},
  title = {Two at Once: Enhancing Learning and Generalization Capacities via IBN-Net},
  booktitle = {ECCV},
  year = {2018}
}
```

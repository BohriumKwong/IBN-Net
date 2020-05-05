## Instance-Batch Normalization Network

这个仓库fork from论文作者官方代码仓库再进行修改的,详情可以看原来的README.md([README_official.md](https://github.com/XingangPan/IBN-Net/blob/master/README.md))文档。

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
原作者需要在论文中提及到IBN-b的结构,但这个结构只针对残差网络进行的：

<img align="middle" width="486" height="333" src="https://github.com/BohriumKwong/IBN-Net/tree/master/utils/images/IBN-b.jpg">

我在基于官方代码的基础上,结合论文中IBN-b结构的描述,将在残差block中**{addition}**之后进行IN替换BN的处理,改为在**dense block中**{transition}**层刚开始的时候(结合所有的dense concat等价于残差block中的addition),将IN替换BN：

<img align="middle" width="827" height="500" src="https://github.com/BohriumKwong/IBN-Net/tree/master/utils/images/densenet-IBN-b.jpg">

然后是根据论文所述,only add three IN layers after the first convolution layer (conv1) and the first two convolutiongroups (conv2 x, conv3 x):

<img align="middle" width="867" height="423" src="https://github.com/BohriumKwong/IBN-Net/tree/master/utils/images/densenet-structure.jpg">

### Results

acc/error(base on focal loss,the weight of 'TUM' is 1.1 others are 1) on the CRC_9_CLASS test dataset(CRC-VAL-HE-7K) are reported. You may get different results when training your models with different random seed.

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

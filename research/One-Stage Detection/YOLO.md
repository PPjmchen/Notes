YOLOv3论文精读

![](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504115334.jpg)

![A road map of object detection. Milestone detectors in this figure: VJ... |  Download Scientific Diagram](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504115620.ppm)

## 一、实现细节

### 1. 先验框配置

使用k-means对数据集的所有目标框作聚类，在YOLOv3中，一共选取了9个聚类中心，并将它们均分给三个特征尺度。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504162228.jpg" alt="16-22-24" style="zoom:50%;" />

也就是说在三种尺度的特征图上，每个特征点都根据聚类结果放置三个anchor box，对于256的输入图片大小，一共有$8 × 8 × 3 + 16 × 16 × 3 + 32 × 32 × 3 = 4032$个anchor box。

### 2. 边框回归预测

网络预测对每个anchor预测其回归值$(t_x, t_y, t_w, t_h)$，$（c_x, c_y）$是特征点对应原图网格左上顶点的坐标，$(p_w, p_h)$是先验框的宽高。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504163434.jpg" alt="16-34-24" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504164740.jpg" alt="16-47-34" style="zoom:50%;" />

YOLOv3使用MSE来作边框回归的loss，单个特征点的损失使用简单的相减即可，且ground truth边框可以通过上式的逆过程得到。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504164958.jpg" alt="16-48-20" style="zoom:50%;" />

### 3. 是否含有目标（objectness score）的预测

使用逻辑回归（sigmoid function）来预测某一个anchor是否含有目标，其输出值在$[0,1]$之间。对于ground truth，如果有一个anchor，与gt box的iou比其他anchor都大，且高于设定阈值（0.5），则这个anchor的objectness score为1，其他的anchor均为0。对于非objectness anchor，不作边框回归loss和目标分类loss，只作objectness的loss。

### 4. 分类预测

每个anchor都需要预测目标类别，YOLOv3不使用Softmax，原因是每个目标可能是多标签的（eg. woman & person），所以使用Sigmoid，可以同时预测多个标签类别。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504171445.jpg" alt="17-14-19" style="zoom:50%;" />

### 5. 跨尺度预测

三种特征图尺度来预测大、中、小目标。使用类似特征金字塔的方式提取三个尺度的特征。每个特征图上的每个特征点，放置三个预设anchor box，并对它们作边框回归、objectness分类，目标分类。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504172052.jpg" alt="17-20-28" style="zoom:50%;" />



## 二、模块介绍

- 特征提取网络（Backbone）

  使用Darknet53对输入进行特征提取，假设输入图片大小为256*256，提取得到的三个尺度特征将分别缩小8倍（32\*32）、16倍（16\*16）、32倍（8\*8）。

  **Darknet53网络：**

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504143210.jpg" alt="preview" style="zoom: 33%;" />

- 特征融合模块（Neck）

  小尺度特征，相当于将原图划分为13*13的网格图，用于检测大目标；同理中尺特征检测中目标，大尺度特征检测小目标。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504135213.jpg" alt="13-52-06" style="zoom: 33%;" />

- 输出预测模块（Head）

  ![14-28-29](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504142853.jpg)

- 模型总览

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504202952.jpg" alt="preview" style="zoom:150%;" />

特征融合中，早期网络特征能够提供更加具有细粒度的信息，上采样特征能够提供更丰富的语义信息，使用卷积层进一步融合特征。

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504202943.jpg" alt="17-25-18" style="zoom:50%;" />

## 三、训练策略及推理过程

![20-27-30](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504202934.jpg)

![20-28-50](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504202938.jpg)

## 四、实验结果比较



<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210504134416.png" alt="img" style="zoom:33%;" />

Darknet-53精度与ResNet-152相当，且速度超过极多，能够达到实时检测。

<img src="/var/folders/2c/1pbc_qsd1bnf381d4g97w51m0000gp/T/com.monosnap.monosnap/19-44-31.jpg" alt="19-44-31" style="zoom:50%;" />



<img src="/var/folders/2c/1pbc_qsd1bnf381d4g97w51m0000gp/T/com.monosnap.monosnap/19-46-14.jpg" alt="19-46-14" style="zoom:50%;" />
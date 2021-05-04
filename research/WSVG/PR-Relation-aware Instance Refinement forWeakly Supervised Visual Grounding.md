CVPR2021

![11-34-34](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210501113436.jpg)



- [x] Language prior

- [x] Corse-to-fine matching process

- [x] Multi-task loss


## 0. 符号定义

**grounding model: ** $M$

**input image:** $I$

**description: **$D$

**noun phrases: **$Q = \{q_i\}_{i=1}^N$ , $Q \in D$

**pridict locations: **$B = \{b_i\}_{i=1}^N$, $B = M(I, D, Q)$

**training dataset: ** $X = \{(I^l, D^l, Q^l)\}_{l=1}^L$, 即训练集不包含phrase-location标注

**proposals: **$O = \{<o_m, c_m>\}_{m=1}^M$，$o_m \in R^4, c_m \in \{1, 2,...,C\}$,从外部detector检测得到一系列候选框

**feature dimension: ** $d$

## 1. First stage

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502112611.jpg" style="zoom:67%;" />

- **Backbone Network**

  **视觉特征：**首先使用CNN提取视觉特征（eg. ResNet），再使用外部检测器（eg. Faster R-CNN）获取一系列候选框$O$，对每个$o_m$，使用==RoI Align==以及GAP提取对应的conv-feature，将其编码为特征向量$X_{o_m}\in R^d$

  **文字特征：**

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502142451.jpg" alt="14-24-35" style="zoom:50%;" />

- **Coarse-level Matching Network**

  作用：挑选出部分相关候选框，并做初步的refine

  做法：对每个phrase-box pair，即$\{q_i, o_m\}$计算相似度，并回归box offset

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502143730.jpg" alt="14-37-29" style="zoom:50%;" />

  其中$F_{cls},F_{reg}$是两层全连接层。

  同时，由于每个候选框可以得到一个预测类别$c_m$，因此可以计算另一个相似度$s_{i,m}^a$，并fuse两个相似度，作softmax得到最终$\{q_i, o_m\}$相似度：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502144622.jpg" alt="14-46-19" style="zoom:50%;" />

  先对所有的候选框进行box offset的调整，并且每个$q_i$都通过上述计算得到$topK$个候选区域，因此对每个$q_i$都得到了$K$个refine后的候选区域：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502145223.jpg" alt="14-52-18" style="zoom:50%;" />

  

## 2. Second stage

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502112730.jpg" alt="11-27-20" style="zoom:67%;" />

- **Visual Object Graph Network**

  **作用**：根据language structure来捕捉视觉上下文关系

  首先，使用[external language parser](https://github.com/vacancy/SceneGraphParser)提取文字关系短语。搭建一个具有N个节点的图网络，其中第i个节点$z_i$，编码了短语$q_i$的视觉特征。如果短语$q_i,q_j$之间存在关系词，则节点$z_i, z_j$之间进行连接。

  节点$z_i$的表示：$q_i$对应的k个候选框的特征向量的加权求和

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502153104.jpg" alt="15-30-59" style="zoom:50%;" />

  节点间进行信息传递：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502160056.jpg" alt="16-00-54" style="zoom:50%;" />

  这里的$X_{o_{i,k}}^{,}$就是文章所谓的context-aware feature

  

- **Fine-level Matching Network**

  对每个phrase $q_i$以及其对应的K个候选框，每个候选框将得到一个分数，并进一步计算该候选框的box offset，同样采用两个全连接层完成：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502163348.jpg" alt="16-30-26" style="zoom:50%;" />

## 3. 模型推理

首先对每个$q_i$计算其每个候选框$o_{i,k}$的相似度分数$\{s_{i,k}\}_{k=1}^K$, 其中$s_{i,k} = s_{i,k}^c · s_{i,k}^f$，即corse-level 和 fine-level的相似度乘积，并对这些候选框进行refine，最后选择相似度最大值的候选框作为最终结果。

## 4. 损失函数

损失函数的监督信号来自两方面：（1）部分已训练的模型本身 （2）实体关系中的先验文字信息，eg. language prior

完整的损失函数由四部分组成：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502165154.jpg" alt="16-51-49" style="zoom:50%;" />

- Phrase reconstruction loss $L_{res}$

  在corse阶段和fine阶段，都把一系列候选框的特征解码重构为phrase，用实际的phrase作为监督。

  以corse阶段为例，首先被detector提取出的$M$个候选框，对应$q_i$有M个相似度分数，将他们加权求和

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502170742.jpg" style="zoom:50%;" />
  
  再使用一个LSTM解码器将它解码回到phrase：
  
  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502171013.jpg" alt="17-10-08" style="zoom:50%;" />
  
  同理在fine阶段也对每个phrase对应的$topK$个候选框特征进行加权求和和解码，同时解码使用的LSTM将共享参数。
  
  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502171212.jpg" alt="17-11-59" style="zoom:50%;" />

- Self-taught regression loss $L_{reg}$

  观察得到，在没有$L_{reg}$的条件下，经过几轮训练后，相似度分数较高的候选框往往定位较为准确，因此可以利用被部分训练的模型生成的分数较高的候选框作为伪标签来监督候选框的refine，如图所示：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502171640.jpg" alt="17-16-32" style="zoom:50%;" />

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502172957.png" alt="17-25-26" style="zoom:50%;" />

- Relation classification loss $L_{rel}$

  从数据集中提取全部的关系词，并选择其中最常见的$C_r$种关系组成关系集合$R = \{0,1,2,...,C_r\}$，对fine阶段经过图网络传递信息后的节点特征向量，经过全连接层预测词关系，并用交叉熵作为损失：

  <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/20210502173332.jpg" alt="17-33-30" style="zoom:50%;" />


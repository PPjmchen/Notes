# pytorch中的常用函数总结

## 一、新建张量函数

|                函数                |           功能            |
| :--------------------------------: | :-----------------------: |
|           Tensor(*sizes)           |       基础构造函数        |
|           tensor(data,)            |  类似np.array的构造函数   |
|            ones(*sizes)            |         全1Tensor         |
|           zeros(*sizes)            |         全0Tensor         |
|            eye(*sizes)             |    对角线为1，其他为0     |
|          arange(s,e,step           |    从s到e，步长为step     |
|        linspace(s,e,steps)         | 从s到e，均匀切分成steps份 |
|         rand/randn(*sizes)         |       均匀/标准分布       |
| normal(mean,std)/uniform_(from,to) |     正态分布/均匀分布     |
|            randperm(m)             |         随机排列          |

注意：

1. uniform函数是对张量的函数

```python
a = t.Tensor(3, 4).uniform_(3, 9)

tensor([[7.5372, 4.2334, 8.4022, 6.9240],
        [7.1781, 4.9170, 7.0675, 7.9179],
        [4.8778, 4.0774, 6.7495, 5.8901]])
```

2. rand 是[0,1]之间的均匀分布，randn是标准正态分布（N(0，1)）

## 二、对张量的计算

​	求范数（参考https://blog.csdn.net/shijing_0214/article/details/51757564）
$$
Lp=\sqrt[p]{\sum\limits_{1}^n  x_i^p}，x=(x_1,x_2,\cdots,x_n)
$$
常用的范数：

L1范数，表示向量元素绝对值之和
$$
||x||_1=\sum_i|x_i|
$$
L2范数，表示向量元素平方和之和的开根号
$$
||x||_2=\sqrt{\sum_ix_i^2}
$$


```python
torch.norm(input, p) # 默认求L2范数
```

$$
 P(x) = \dfrac{1}{\text{to} - \text{from}}
$$

## 三、张量的变形

1. tensor.view()，参数为变形后的size

   **<u>注意</u>**：view不改变原tensor的数据，且通过tensor.view()得到的新tensor和原tensor共享内存，即其中一个的数据发生改变，另一个也发生改变。

   ```python
   a = torch.tensor([[1,2,3],[4,5,6]])
   a = 
   tensor([[1, 2, 3],
           [4, 5, 6]])
   
   b = a.view(3, 2)
   b = 
   tensor([[1, 2],
           [3, 4],
           [5, 6]])
   
   a[0][0] = 100
   a = 
   tensor([[100,   2,   3],
           [  4,   5,   6]])
   
   b = 
   tensor([[100,   2],
           [  3,   4],
           [  5,   6]])
   ```

   

2. tensor.squeeze(dim) & tensor.unsqueeze(dim)

   用于在指定维度上降维和升维

3. tensor.resize() & tensor.resize_()

   作用和用法类似于tensor.view()，但区别在于，tensor.resize()可以改变元素的个数，若变形后元素个数减少，则直接删去多余元素，若变形后元素个数增多，则自动分配新的内存空间

## 四、张量的索引操作

类似于numpy的索引操作，不赘述。

**None的作用**：把None写在某一维度上，指的是在该维度上新增一个轴，类似于np.newaxis

```python
a = tensor.randn(3, 4)
a.shape
a[:, None].shape

torch.Size([3, 4])
torch.Size([3, 1, 4])
```

**使用比较式索引**：用于找出一个tensor中符合比较式的元素，并存入一个新的一维tensor中

```python
a = tensor.randn(3, 2, 5)
tensor([[[-0.0157,  0.1186,  0.6112, -0.3510, -0.1043],
         [-0.7188,  0.3150,  0.9442,  0.7519, -1.1767]],

        [[ 1.3607,  0.0920, -2.0861, -0.3337, -0.3140],
         [ 1.2725, -0.0086, -0.3577,  0.9032,  0.8876]],

        [[-1.0968, -1.1566,  0.1006, -0.4262, -0.7516],
         [ 0.3616,  0.8456, -0.3045,  1.4282, -0.0548]]])

a[a > 1]
tensor([1.3607, 1.2725, 1.4282])
```

**ByteTensor**：比较式的结果即ByteTensor

```python
a > 1
tensor([[[False, False, False, False, False],
         [False, False, False, False, False]],

        [[ True, False, False, False, False],
         [ True, False, False, False, False]],

        [[False, False, False, False, False],
         [False, False, False,  True, False]]])
```

**使用tensor作为索引**：

把tensor作为索引，写在在对应的维度上，可以选出该维度上被索引的元素

**其他常用选择函数**

|              函数               |                         功能                          |
| :-----------------------------: | :---------------------------------------------------: |
| index_select(input, dim, index) |      在指定维度dim上选取，比如选取某些行、某些列      |
|   masked_select(input, mask)    |       例子如上，a[a>0]，使用ByteTensor进行选取        |
|         non_zero(input)         |                     非0元素的下标                     |
|    gather(input, dim, index)    | 根据index，在dim维度上选取数据，输出的size与index一样 |

gather(input, dim, index)等价于input.gather(dim, index)。输出的结果与index形状一致

gather字面上为收集的意思，在实际使用中发现，其作用为根据index收集维度和指定dim上向量一致的向量，对于2维的tensor，若dim=0，则收集行向量，若dim=1，则收集列向量：

```python
a = t.arange(16).view(4, 4)
tensor([[ 0,  1,  2,  3],
        [ 4,  5,  6,  7],
        [ 8,  9, 10, 11],
        [12, 13, 14, 15]])

index = t.tensor([[0,1,2,3]])
a.gather(0, index)

tensor([[ 0,  5, 10, 15]])
```

```python
index = t.tensor([[0,1,2,3]]).t()
a.gather(1, index)

tensor([[ 0],
        [ 5],
        [10],
        [15]])
```

对于3维的tensor，假设input为[3,3,3]的tensor:

若dim=0，则在高度方向收集维度为[3,3]的tensor，即index形状为[x,3,3]

若dim=1，则在宽度方向收集维度为[3,3]的tensor，即index形状为[3,x,3]

若dim=2，则在长度方向收集维度为[3,3]的tensor，即index形状为[3,3,x]

```python
a = t.arange(27).view(3,3,3)
tensor([[[ 0,  1,  2],
         [ 3,  4,  5],
         [ 6,  7,  8]],

        [[ 9, 10, 11],
         [12, 13, 14],
         [15, 16, 17]],

        [[18, 19, 20],
         [21, 22, 23],
         [24, 25, 26]]])

a.gather(0, t.tensor([[[0,0,0],[0,0,0],[0,0,0]]]))
tensor([[[0, 1, 2],
         [3, 4, 5],
         [6, 7, 8]]])

a.gather(2, t.tensor([[[0,1],[0,1],[0,1]],[[0,1],[0,1],[0,1]],[[0,1],[0,1],[0,1]]]))
tensor([[[ 0,  1],
         [ 3,  4],
         [ 6,  7]],

        [[ 9, 10],
         [12, 13],
         [15, 16]],

        [[18, 19],
         [21, 22],
         [24, 25]]])
```

**高级索引**

PyTorch在0.2版本中完善了索引操作，目前已经支持绝大多数numpy的高级索引。高级索引可以看成是普通索引操作的扩展，但是高级索引操作的结果一般不和原始的Tensor共享内存。 https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#advanced-indexing

```
x = t.arange(0,27).view(3,3,3)
x
```

```
tensor([[[ 0,  1,  2],
         [ 3,  4,  5],
         [ 6,  7,  8]],

        [[ 9, 10, 11],
         [12, 13, 14],
         [15, 16, 17]],

        [[18, 19, 20],
         [21, 22, 23],
         [24, 25, 26]]])
```

```
x[[1, 2], [1, 2], [2, 0]] # x[1,1,2]和x[2,2,0]
```

```
tensor([14, 24])
```

```
x[[2, 1, 0], [0], [1]] # x[2,0,1],x[1,0,1],x[0,0,1]
```

```
tensor([19, 10,  1])
```

```
x[[0, 2], ...] # x[0] 和 x[2]
```

```
tensor([[[ 0,  1,  2],
         [ 3,  4,  5],
         [ 6,  7,  8]],

        [[18, 19, 20],
         [21, 22, 23],
         [24, 25, 26]]])
```

## 五、tensor的类型

**tensor数据类型总结**

| Data type                | dtype                             | CPU tensor                                                   | GPU tensor                |
| ------------------------ | --------------------------------- | ------------------------------------------------------------ | ------------------------- |
| 32-bit floating point    | `torch.float32` or `torch.float`  | `torch.FloatTensor`                                          | `torch.cuda.FloatTensor`  |
| 64-bit floating point    | `torch.float64` or `torch.double` | `torch.DoubleTensor`                                         | `torch.cuda.DoubleTensor` |
| 16-bit floating point    | `torch.float16` or `torch.half`   | `torch.HalfTensor`                                           | `torch.cuda.HalfTensor`   |
| 8-bit integer (unsigned) | `torch.uint8`                     | [`torch.ByteTensor`](https://pytorch.org/docs/stable/tensors.html#torch.ByteTensor) | `torch.cuda.ByteTensor`   |
| 8-bit integer (signed)   | `torch.int8`                      | `torch.CharTensor`                                           | `torch.cuda.CharTensor`   |
| 16-bit integer (signed)  | `torch.int16` or `torch.short`    | `torch.ShortTensor`                                          | `torch.cuda.ShortTensor`  |
| 32-bit integer (signed)  | `torch.int32` or `torch.int`      | `torch.IntTensor`                                            | `torch.cuda.IntTensor`    |
| 64-bit integer (signed)  | `torch.int64` or `torch.long`     | `torch.LongTensor`                                           | `torch.cuda.LongTensor`   |

**设置默认数据类型有两种方法：**

1. 设置默认浮点型dtype:

```python
torch.set_default_dtype(torch.float64)
```

2. 设置默认浮点型tensor数据类型：

```
torch.set_default_tensor_type(torch.DoubleTensor)
```

**CPU tensor与GPU tensor之间的互相转换**:

通过`tensor.cuda`和`tensor.cpu`方法实现，此外还可以使用`tensor.to(device)`

**查看tensor的数据类型**

使用tensor.dtype可以查看tensor的数据类型，**注意：**dtype和tensor在CPU上和在GPU上无关：

```
a = torch.Tensor(2, 3)
a.dtype

torch.float64
```

**改变tensor的数据类型：**

1. 使用tensor.type(new_type)改变tensor的数据类型：

```
torch.set_default_tensor_type('torch.FloatTensor')
b = a.type(torch.FloatTensor)
b.dtype

torch.float32
```

注意：a.type(torch.FloatTensor)有更为简便的替代方法——a.float()，同理tensor.long(),tensor.half()作用类似

2. 使用tensor_a.type_as(tensor_b)，可以得到一个形状和数据与tensor_a一致，但数据类型取自tensor_b的tensor

```python
a = torch.tensor([6,6,6])
b = torch.tensor([2,3]).float().cuda
c = a.type_as(b)
c.dtype

torch.float32

# 使用tensor.device查看tensor处于cpu还是gpu
c.device

device(type='cuda', index=0)
```

**使用原有tensor的属性来新建tensor**

1. torch.*_like(tensor_a)可以生成和tensor_a拥有同样属性(类型，形状，cpu/gpu)的新tensor

> torch.ones_like()
>
> torch.zeros_like()
>
> torch.rand_like()
>
> torch.randn_like()
>
> torch.empty_like()

2. tensor_a.new_*(size)可以生成和tensor_a拥有同样类型、cpu/gpu但形状不同的新tensor

> tensor.new_empty(size)
>
> tensor.new_full(size, 填充数值)
>
> tensor.new_ones(size)
>
> tensor.new_zeros(size)
>
> tensor.new_tensor([])  **用法同tensor.tensor([])，但类型和设备取自tensor_a**

## 六、逐元素操作

实现对tensor的element-wise操作，此类函数的输入输出形状一致

|                函数                 |                   功能                   |
| :---------------------------------: | :--------------------------------------: |
| abs/sqrt/mul/div/exp/fmod/log/pow.. | 绝对值/平方根/乘法/除法/指数/求余/求幂.. |
|      cos/sin/asin/atan2/cosh..      |               相关三角函数               |
|       ceil/round/floor/trunc        |  上取整/四舍五入/下取整/只保留整数部分   |
|       clamp(input, min, max)        |           超过min和max部分截断           |
|            sigmod/tanh..            |                 激活函数                 |

对于mul, div, pow, fmod等简单的计算函数，pytorch实现了运算符重载，如

`a ** 2` 等价于`torch.pow(a,2)`, `a * 2`等价于`torch.mul(a,2)`。

其中`clamp(x, min, max)`的输出满足以下公式：
$$
y_i =
\begin{cases}
min,  & \text{if  } x_i \lt min \\
x_i,  & \text{if  } min \le x_i \le max  \\
max,  & \text{if  } x_i \gt max\\
\end{cases}
$$
`clamp`常用在某些需要比较大小的地方，如取一个tensor的每个元素与另一个数的较大值，且可只定义min值或只定义max值。

## 七、归并操作

常用归并函数

|         函数         |        功能         |
| :------------------: | :-----------------: |
| mean/sum/median/mode | 均值/和/中位数/众数 |
|      norm/dist       |      范数/距离      |
|       std/var        |     标准差/方差     |
|    cumsum/cumprod    |      累加/累乘      |

**tensor.sum()**:

若函数不指定dim值，则默认对整个tensor内的所有元素求和

```python
b = torch.ones(2, 3)
b.sum()

tensor(6.)
```

若想要沿着某一个维度对tensor求和，则给函数传入dim参数，并指定keepdim=True/False

- 若keepdim=False，则保留输出中为1的维度
- 若keepdim=False，则不保留输出中为1的维度

```python
b.sum(dim=0, keepdim=True)
b.sum(dim=1, keepdim=True)
b.sum(dim=0, keepdim=False)
b.sum(dim=1, keepdim=False)

tensor([[2., 2., 2.]])
tensor([[3.],
        [3.]])
tensor([2., 2., 2.])
tensor([3., 3.])
```

**dim的解释：**

*对于二维tensor：*

假设输入的形状为(m, n)

- 如果指定dim=0，输出的形状为行向量(1, n)或者(n)
- 如果指定dim=1，输出的形状为列向量(m, 1)或者(m)

*对于三维tensor:*

假设输入的形状是(m, n, k)

- 如果指定dim=0，输出的形状就是(1, n, k)或者(n, k)
- 如果指定dim=1，输出的形状就是(m, 1, k)或者(m, k)
- 如果指定dim=2，输出的形状就是(m, n, 1)或者(m, n)

（当keepdim=True时，输出形状才含有1）

## 八、比较操作

常用的比较函数

|       函数        |                 功能                  |
| :---------------: | :-----------------------------------: |
| gt/lt/ge/le/eq/ne | 大于/小于/大于等于/小于等于/等于/不等 |
|       topk        |              最大的k个数              |
|       sort        |                 排序                  |
|      max/min      |       比较两个tensor最大最小值        |

其中，第一行的函数已经实现了运算符重载，可以直接使用>,<,=，!=,==.

第一行的函数为逐元素操作，输出为相同形状的bool值

topk类似于归并操作，需要指定比较的维度

max/min函数当不指定dim时，对所有的元素找最大/最小值，当指定dim时，返回指定dim上的最大/最小值组成的tensor以及对应的Index

```python
b = torch.arange(0, 19, 2).view(2,5)

tensor([[ 0,  2,  4,  6,  8],
        [10, 12, 14, 16, 18]])
        
b.max(dim=0) # 对两个行向量内的元素找最大值

torch.return_types.max(
values=tensor([10, 12, 14, 16, 18]),
indices=tensor([1, 1, 1, 1, 1]))

b.max(dim=1) # 对五个列向量内的元素找最大值

torch.return_types.max(
values=tensor([ 8, 18]),
indices=tensor([4, 4]))

```

## 九、线性代数

常用的线性代数函数

|               函数               |               功能                |
| :------------------------------: | :-------------------------------: |
|              trace               |     对角线元素之和(矩阵的迹)      |
|               diag               |            对角线元素             |
|            triu/tril             | 矩阵的上三角/下三角，可指定偏移量 |
|              mm/bmm              |     矩阵乘法，batch的矩阵乘法     |
| addmm/addbmm/addmv/addr/badbmm.. |             矩阵运算              |
|                t                 |               转置                |
|            dot/cross             |             内积/外积             |
|             inverse              |             求逆矩阵              |
|               svd                |            奇异值分解             |

**注意：**矩阵的转置会导致存储空间不连续，需调用它的`.contiguous`方法将其转为连续

```python
b = torch.arange(0, 19, 2).view(2,5)
tensor([[ 0,  2,  4,  6,  8],
        [10, 12, 14, 16, 18]])
        
b.t_()
tensor([[ 0, 10],
        [ 2, 12],
        [ 4, 14],
        [ 6, 16],
        [ 8, 18]])
        
b.is_contiguous()
False

b = b.contiguous()
b.is_contiguous()
True
```

## 十、tensor与numpy

pytorch中，tensor和numpy的数组共享内存，彼此之间的互操作很高效，转换开销小，所以遇到pytorch不支持的操作时，先考虑用numpy能否解决，若能则直接转为numpy处理后再转会torch

**tensor和array的互换：**

```python
import numpy as np

import torch

a = np.ones([2, 3],dtype = np.float64)

# 方法一：tensor.from_numpy(array)
b = torch.from_numpy(a)

# 方法二：tensor.Tensor(array)
b = torch.Tensor(a)

# 方法三：tensor.tensor(array)
b = torch.tensor(a)

b

tensor([[1., 1., 1.],
        [1., 1., 1.]], dtype=torch.float64)

```

**注意：**

- 使用tensor.from_numpy(array)得到的tensor数据类型和numpy一致

- 使用torch.Tensor(array)得到的tensor，若numpy的数据类型和torch一致，则互换的输出结果和输入共享内存，若numpy的数据类型和torch不一致，则只进行数据拷贝，不共享内存
- 使用torch.tensor(array)得到的tensor无论什么情况都只进行数据拷贝，不共享内存

## 十一、CPU & GPU

tensor可以在GPU和CPU上传输

1. GPU $$\rightarrow$$ CPU

   ```python
   tensor.cpu()
   ```

2. CPU $$\rightarrow$$ GPU

   ```python
   tensor.cuda(device_id)
   ```

3. 通用做法（推荐）

   ```
   device = torch.device('gpu')
   device = torch.device('cpu')
   tensor.to(device)
   ```

**注意：**tensor可以在定义时就指定存放在内存还是显存

```python
a = torch.randn(3, 4, device = torch.device('cuda'))
```

## 十二、tensor的保存和加载

可以把tensor中的数据保存在一个.pth文件中，根据需要进行读取

1. torch.save()

```python
a = a.cuda(0)
torch.save(a, 'a.pth') # t.save(your_tensor,'save_path')
```

2. torch.load()

```python
b = torhc.load('a.pth') # t.load('save_path')
```

3. 指定load到cpu上或gpu上

```python
# 加载为c, 存储于CPU
c = t.load('a.pth', map_location=lambda storage, loc: storage)
# 加载为d, 存储于GPU0上
d = t.load('a.pth', map_location={'cuda:1':'cuda:0'})
```

## 十三、其他

1. 大多数`t.function`都有一个参数`out`，这时候产生的结果将保存在out指定tensor之中。
2. `t.set_num_threads`可以设置PyTorch进行CPU多线程并行计算时候所占用的线程数，这个可以用来限制PyTorch所占用的CPU数目。
3. `t.set_printoptions`可以用来设置打印tensor时的数值精度和格式

```
a = t.randn(2,3)
a

tensor([[-0.2005,  0.5701,  0.3342],
        [-0.4209, -0.0814, -0.1660]])
        
t.set_printoptions(precision=10)
a

tensor([[-0.2005225718,  0.5700706840,  0.3341696262],
        [-0.4208618701, -0.0814390928, -0.1659913808]])
```

4. pytorch中的rand/randn函数是根据随机数种子生成的随机数的，可以人工设置随机数种子，用来保证每次生成的随机数均一致

   ```python
   torch.manual_seed(1000) # 1000为人工设置的随机数种子
   ```

   

# 注意

在科学计算程序中应当极力避免使用Python原生的`for循环`。

Python本身是一门高级语言，使用很方便，但这也意味着很多操作很低效，尤其是`for`循环。
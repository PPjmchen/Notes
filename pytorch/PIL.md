# PIL库的使用方法

## 读取图片

```python
from PIL import Image
lena = Image.open('imgs/lena.png') # 指定图片路径
```

## 把图片和tensor的互换

使用`torchvision.transforms`中的`ToTensor`和`ToPILImage`

```python
from torchvision.transforms import ToTensor, ToPILImage
to_tensor = ToTensor()
to_pil = ToPILImage()
```




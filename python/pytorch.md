


 https://pytorch.org/tutorials/beginner/basics/data_tutorial.html
## torch.utils.data.Dataset
包含要训练的数据；
pytroch 有很多自带的数据集：https://pytorch.org/vision/stable/datasets.html

自己实现的DataSet类继承torch.utils.data.Dataset类，
## PIL Image

PIL是一个python 图像处理库
使用方法
```python

pip install pillow

import PIL
from PIL import Image

image = Image.open("demo.jpg")
image.show()

```

## torch.utils.data.DataLoader

通过DataSet生成DataLoader，DataLoader支持遍历和批量的读取DataSet。

## nn
torch.nn是神经网络模块
nn.Relu, nn.Module, nn.Linear, nn.Conv2d


### nn.Linear
用于输入数据的线性计算，用法: `in_features`:输入的特征数量，`out_features`: 输出的特征数量。
```python
layer1 = torch.nn.Linear(in_features=28*28, out_features=20)
input = torch.rand(3, 28, 28)
hidden1 = layer1(input)
// hidden1.shape(3, 20)
```
### nn.ReLU
ReLU激活函数，大于等于0时输出原值，小于0时输出0。

### nn.Sequential
数据处理的有序集合
```python
flatten = nn.Flatten()

seq_modules = nn.Sequential(
	flatten,
	layer1,
	nn.ReLU(),
	nn.Linear(20, 10)
)
input_image = torch.rand(3, 28, 28)
logits = seq_modules(input_image)
// logits.shape(3, 10)

```

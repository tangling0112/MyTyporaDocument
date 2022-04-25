# :family_man_woman_girl_boy:GAN

## 前置

### 项目目标

通过使用`MINST`数据集对我的网络进行训练,并最终完成手写数字生成的任务

### :rescue_worker_helmet:项目环境依赖

- `pytorch 1.9.0`
- `matplotlib`
- `torch`必须正确配置可以使用`GPU`
  - 我们可以通过`print(torch.cuda.is available())`来判定是否可以使用`GPU`
- :red_circle: **注意**:我们的模型没有断点续传功能,以后再说吧.所以只能自己改改代码实现了.

### :small_red_triangle: 判别器介绍

- 我们使用`Lenet5`的改造形式作为我们的判别器
- 使用`torch.BCELoss`对判别器的输出进行损失计算
- 使用学习率为$0.0002$,$beta$为$(0.5,0.999)$的Adam优化器进行训练

### :black_heart: 生成器介绍

- 使用**反卷积神经网络**进行噪音的维度提升从而生成图片
- 使用`torch.BCELoss`对生成器的输出经过判别器后的`logit`输出进行损失计算

## :package: 编制判别器

### 基本原理

- 我们的判别器**直接使用了Lenet5的结构**,只不过将其最后一层的线性层的`Out_feather`进行了修改为`1`

- 使用`LeakyRelu`作为激活函数

- 每一个卷积之后做`BatchNorm`

- 基本输入到输出的流程如下

  ```
  (B,C,H,W)=(批量数,通道数,高,宽)
  
  [b,1,32,32]=>[b,6,28,28]    卷积,卷积核=5×5,步长=1,padding=0
  [b,6,28,28]=>[b,6,14,14]	最大池化,核=2×2,步长=2,padding=0
  [b,6,14,14]=>[b,16,10,10]	卷积,卷积核=5×5,步长=1,padding=0
  [b,16,10,10]=>[b,16,5,5]	最大池化,核=2×2,步长=2,padding=0
  [b,16,5,5]=>[b,400]			nn.Flatten打平
  [b,400]=[b,120]				线性层
  [b,120]=>[b,84]				线性层
  [b,84]=>[b,1]   			线性层
  ```

### 具体代码

```python
import torch.nn as nn
import torch


class Discriminater(nn.Module):
    def __init__(self) -> None:
        super(Discriminater, self).__init__()
        self.model = nn.Sequential(
            # *[b,1,32,32]=>[b,6,28,28]
            nn.Conv2d(1, 6, kernel_size=(5, 5), stride=(1, 1), padding=0),
            nn.BatchNorm2d(6),
            nn.LeakyReLU(0.2),

            # *[b,6,28,28]=>[b,6,14,14]
            nn.MaxPool2d(kernel_size=2, stride=2, padding=0),

            # *[b,6,14,14]=>[b,16,10,10]
            nn.Conv2d(in_channels=6, out_channels=16,
                      kernel_size=(5, 5), stride=(1, 1), padding=0),
            nn.BatchNorm2d(16),
            nn.LeakyReLU(0.2),

            # *[b,16,10,10]=>[b,16,5,5]
            nn.MaxPool2d(kernel_size=2, stride=2, padding=0),

            nn.Flatten(),  # [b,16,5,5]=>[b,400]

            # [b,400]=[b,120]
            nn.Linear(in_features=400, out_features=120),
            nn.LeakyReLU(0.2),

            # [b,120]=>[b,84]
            nn.Linear(in_features=120, out_features=84),
            nn.LeakyReLU(0.2),

            # [b,84]=>[b,1]
            nn.Linear(in_features=84, out_features=1),
            nn.Sigmoid()
        )

    # 前向传播
    def forward(self, x):
        x = self.model(x)
        return x

```

### 代码使用方法

- 创建文件名为`Discriminater`的`py文件`,将代码复制粘贴进该文件

## :blue_heart: 编制生成器

### 基本原理

- 使用`Relu`作为激活函数,最后一层反卷积之后使用`nn.Tanh`作为激活函数

- 每一次反卷积后做`BatchNorm`

- 使用`torch.nn.ConvTranspose2d`函数实现反卷积

- 基本流程如下

  ```
  (B,C,H,W)=(批量数,通道数,高,宽)
  
  (B,1,6,6)=>(B,128,10,10)		反卷积,卷积核=5×5,步长=1
  (B,128,10,10)=>(B,64,14,14)		反卷积,卷积核=5×5,步长=1
  (B,64,14,14)=>(B,32,18,18)		反卷积,卷积核=5×5,步长=1
  (B,32,18,18)=>(B,16,22,22)		反卷积,卷积核=5×5,步长=1
  (B,16,22,22)=>(B,8,26,26)		反卷积,卷积核=5×5,步长=1
  (B,8,26,26)=>(B,4,30,30)		反卷积,卷积核=5×5,步长=1
  (B,4,30,30)=>(B,1,32,32)		反卷积,卷积核=5×5,步长=1
  ```

### 具体代码

```python
import torch.nn as nn


class Generater(nn.Module):
    def __init__(self):
        super(Generater, self).__init__()
        self.net = nn.Sequential(
            # 输出长度=（输入长度-1）×步长+输出Padding-2×输入的Padding+核的大小
            # （B，C，H，W）=（B，1，6，6）
            nn.ConvTranspose2d(1, 128, kernel_size=(5, 5), stride=(1, 1)),
            nn.BatchNorm2d(128),  # (B,1,6,6)=>(B,128,10,10)
            nn.ReLU(),

            nn.ConvTranspose2d(128, 64, stride=(1, 1), kernel_size=(5, 5)),
            nn.BatchNorm2d(64),
            nn.ReLU(),  # (B,128,10,10)=>(B,64,14,14)

            nn.ConvTranspose2d(64, 32, kernel_size=(5, 5), stride=(1, 1)),
            nn.BatchNorm2d(32),
            nn.ReLU(),  # (B,64,14,14)=>(B,32,18,18)

            nn.ConvTranspose2d(32, 16, kernel_size=(5, 5), stride=(1, 1)),
            nn.BatchNorm2d(16),
            nn.ReLU(),  # (B,32,18,18)=>(B,16,22,22)

            nn.ConvTranspose2d(16, 8, kernel_size=(5, 5), stride=(1, 1)),
            nn.BatchNorm2d(8),
            nn.ReLU(),  # (B,16,22,22)=>(B,8,26,26)

            nn.ConvTranspose2d(8, 4, kernel_size=(5, 5), stride=(1, 1)),
            nn.BatchNorm2d(4),
            nn.ReLU(),  # (B,8,26,26)=>(B,4,30,30)

            nn.ConvTranspose2d(4, 1, kernel_size=(3, 3), stride=(1, 1)),
            nn.Tanh(),  # (B,4,30,30)=>(B,1,32,32)
        )

    def forward(self, z):
        z = self.net(z)
        return z

```

### 代码使用方法

- 创建名为`Generater`的`py文件`,将代码复制在其中

## :green_heart: 编制主函数(用于训练)

### 具体代码

```python
import torch
import torchvision
import torch.nn as nn
from torch.utils.data import DataLoader
import torch.optim as optim
from torchvision import transforms, datasets
from Generater import Generater
from Discriminater import Discriminater
import matplotlib.pyplot as plt
from PIL import Image

Batch = 32


def set_requires_grad(net: nn.Module, mode=True):
    for p in net.parameters():
        p.requires_grad_(mode)

#实例化数据集
MINST_Train = datasets.MNIST("./Train_image/", True, transforms.Compose([
    transforms.Resize((32, 32)),
    transforms.ToTensor()]), download=True)

#实例化数据加载器
DataLoader = DataLoader(MINST_Train, batch_size=Batch, shuffle=True)

device = torch.device("cuda")

# 实例化生成器与判别器
Generater = Generater().to(device)
Discriminater = Discriminater().to(device)
Loss_Function = torch.nn.BCELoss().to(device)

# 设置生成器标签与判别器标签
real_label = torch.ones((Batch, 1)).to(device)
fake_label = torch.zeros((Batch, 1)).to(device)

# 实例化优化器
G_optimizer = optim.Adam(Generater.parameters(), lr=0.0002, betas=(0.5, 0.999))
D_optimizer = optim.Adam(Discriminater.parameters(), lr=0.0002, betas=(0.5, 0.999))
k = 1

for epoch in range(5000):
    count=0
    for batchidx, (img, img_label) in enumerate(DataLoader):
    
##############'''判别器训练'''##########################
#************* 让判别器识别真图形，并希望其输出1************************
        D_optimizer.zero_grad()
        img = img.to(device)
        RD_logit = Discriminater(img)
        RD_Loss = Loss_Function(RD_logit, real_label)
        RD_Loss.backward(retain_graph=True)
        
#************ 让判别器识别假图片，希望其输出0**********************
        G_input = torch.nn.init.kaiming_normal(torch.Tensor(Batch, 1, 6, 6)).to(device)  # 生成噪音样本
        G_output = Generater(G_input)  # 获取生成的图片
        FD_logit = Discriminater(G_output)  # 获取生成图片的判别概率
        FD_Loss = Loss_Function(FD_logit, fake_label)
        FD_Loss.backward()
        
#***********合并Loss并使用Adam优化器进行优化********************
        D_Loss = RD_Loss + FD_Loss
        D_optimizer.step()

######################生成器训练#############################
        Generater.zero_grad()
        G_input = torch.nn.init.kaiming_normal(torch.Tensor(Batch, 1, 6, 6)).to(device)  # 生成噪音样本
        G_output = Generater(G_input)
        G_logit = Discriminater(G_output)
        G_Loss = Loss_Function(G_logit, real_label)
        G_Loss.backward(retain_graph=True)
        G_optimizer.step()
        #当Epoch大于5时没训练64个batch展现一次生成的图片
        if epoch > 5:
            count += 1
            if count%64==0:
                plt.imshow(G_output[1, :, :, :].squeeze(0).squeeze(1).cpu().detach().numpy(), cmap='binary')
                plt.show()


```

## 使用步骤

- 在`Pycharm`中新建项目
- 在项目的根工作目录下创建`Train_image`文件夹,并将下载并解压后的`MINST`数据集放置在该文件夹内
- 在项目的根工作目录下新建`Discriminater.py`文件,并将判别器代码复制粘贴进文件
- 在项目的根工作目录下新建`Generater.py`文件,并将生成器的代码复制粘贴进文件
- 在项目的根工作目录下新建`Train.py`文件,并将主函数的代码复制粘贴进文件
- 具体图示
  - <img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Gan.assets\image_56.png" alt="image_56" style="zoom:60%;" />

## 生成图片展示

### 前提

由于我的电脑比较菜,所以我只训练了几个`Epoch`,想要好的效果怎么也得上百次迭代吧,所以生成的图形就当看个乐子

### 图片

- <img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Gan.assets\image_57.png" alt="image_57" style="zoom:33%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Gan.assets\image_58.png" alt="image_58" style="zoom: 33%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Gan.assets\image-20220419170954164.png" alt="image-20220419170954164" style="zoom:27%;" />

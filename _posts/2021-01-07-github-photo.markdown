---
layout: post
title: Resnet笔记
date: 2021-01-07 12:19:23 +0900
category: sample
---
# resnet基础结构


Resnet-18和Resnet-34采用的是BasicBlock 残差块结构

![basicblock]({{ site.url }}/assets/basicblock.jpg)

```python
BasicBlock实现：
class BasicBlock(nn.Module):
​		expansion=1
​		def  __ init __(self,in_channel,out_channel, stride=1, downsample=None):
​				super(BasicBlock, self).__ init __()
​				self.conv1 = nn.Conv2d(in_channels=in_channel,out_channels=out_channel,
​										kernel_size=3, stride=stride, padding=1, bias=False)
​				self.bn1 = nn.BatchNorm2d(out_channel)
​				self.relu=nn.ReLU()
​				self.conv2 = nn.Conv2d(in_channels=out_channel, out_channels=out_channel,
​										kernel_size=3, stride=1, padding=1, bias=False)
​				self.bn2 = nn.BatchNorm2d(out_channel)
​				self.downsample = downsample
​		def forward(self,x):
​				identity = x
​				if self.downsample is not None:
​							identity = self.downsample(x)
​				out = self.conv1(x)
​				out = self.bn1(out)
​				out = self.relu(out)
​				out = self.conv2(out)
​				out = self.bn2(out)
​				out += identity
​				out = self.relu(out)
​				return out
```

 


resnet-50，resnet-101和resnet-152采用的是Bottleneck残差块结构

![bottleneck]({{ site.url }}/assets/bottleneck.jpg)



```python
class Bottleneck(nn.Module):
    expansion = 4

    def __init__(self, in_channel, out_channel, stride=1, downsample=None):
        super(Bottleneck, self).__init__()
        self.conv1 = nn.Conv2d(in_channels=in_channel, out_channels=out_channel,
                               kernel_size=1, stride=1, bias=False)  # squeeze channels
        self.bn1 = nn.BatchNorm2d(out_channel)
        # -----------------------------------------
        self.conv2 = nn.Conv2d(in_channels=out_channel, out_channels=out_channel,
                               kernel_size=3, stride=stride, bias=False, padding=1)
        self.bn2 = nn.BatchNorm2d(out_channel)
        # -----------------------------------------
        self.conv3 = nn.Conv2d(in_channels=out_channel, out_channels=out_channel*self.expansion,
                               kernel_size=1, stride=1, bias=False)  # unsqueeze channels
        self.bn3 = nn.BatchNorm2d(out_channel*self.expansion)
        self.relu = nn.ReLU(inplace=True)
        self.downsample = downsample

    def forward(self, x):
        identity = x
        if self.downsample is not None:
            identity = self.downsample(x)

        out = self.conv1(x)
        out = self.bn1(out)
        out = self.relu(out)

        out = self.conv2(out)
        out = self.bn2(out)
        out = self.relu(out)

        out = self.conv3(out)
        out = self.bn3(out)

        out += identity
        out = self.relu(out)

        return out
```


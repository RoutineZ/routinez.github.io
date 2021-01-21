# transforms的方法

------

## 一、剪裁——crop

### 1.随机剪裁：transforms.RandomCrop

transforms.RandomCrop(size,paddin=None,pad_if_needed=False,fill=0,padding_mode='constant')

参数：size-(sequence 或者 int)，若为sequence,则为(h,w)，若为int，则(size,size)。 padding-(sequence or int, optional)，此参数是设置填充多少个pixel。 当为int时，图像上下左右均填充int个，例如padding=4，则上下左右均填充4个pixel，若为32*32，则会变成 40×40

```
ex: img_crop=transforms.RandomCrop(1024,960)(img)
```



### 2.中心剪裁：transforms.CenterCrop

根据给定的size，从中心剪裁

```
ex:img_crop=transforms.CenterCrop(1024,960)(img)
```



## 3.随机长宽比裁剪 transforms.RandomResizedCrop

transforms.RandomResizedCrop(size, scale=(0.08, 1.0), ratio=(0.75, 1.3333333333333333), interpolation=2) 

参数：size-输出分辨率，scale-随机crop大小，ratio-随机长宽比设置，interpolation-插值方法，默认为双线性插值



## 二、翻转和旋转——Flip and Rotation



### 1.依概率p水平翻转transforms.RandomHorizontalFlip

transforms.RandomHorizontalFlip(p=0.5) ，功能：依据概率p对PIL图片进行水平翻转 参数： p- 概率，默认值为0.5



### 2.依概率p垂直翻转transforms.RandomVerticalFlip

transforms.RandomVerticalFlip(p=0.5) 功能：依据概率p对PIL图片进行垂直翻转 参数： p- 概率，默认值为0.5



### 3.随机旋转：transforms.RandomRotation

transforms.RandomRotation(degrees, resample=False, expand=False, center=None)  

功能：依degrees随机旋转一定角度，参数： degress- (sequence or float or int) ，若为单个数，如 30，则表示在（-30，+30）之间随机旋转 若为sequence，如(30，60)，则表示在30-60度之间随机旋转  resample- 重采样方法选择，可选 PIL.Image.NEAREST, PIL.Image.BILINEAR, PIL.Image.BICUBIC， center- 可选为中心旋转还是左上角旋转



## 三、图像变换

### 1.resize：transforms.Resize

transforms.Resize(size, interpolation=2)

功能：重置图像分辨率 参数：重置图像分辨率 参数： size- If size is an int, if height > width, then image will be rescaled to (size * height / width, size)，所以建议size设定为h*w interpolation- 插值方法选择，默认为PIL.Image.BILINEAR



### 2.标准化：transforms.Normalize

transforms.Normalize(mean, std)

功能：对数据按通道进行标准化，即先减均值，再除以标准差，注意是 h * w * c



### 3.转为tensor：transforms.ToTensor

transforms.ToTensor   

功能：将PIL Image或者 ndarray 转换为tensor，并且归一化至[0-1] 注意事项：归一化至[0-1]是直接除以255，若自己的ndarray数据尺度有变化，则需要自行修改
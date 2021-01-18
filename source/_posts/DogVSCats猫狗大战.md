# 迁移学习 VGGNet、VGG16、ResNet50
按照下载好的数据集运行时,会出现如下错误：
```python
RuntimeError: Found 0 files in subfolders of: ./DogsVSCats/valid
Supported extensions are: .jpg,.jpeg,.png,.ppm,.bmp,.pgm,.tif,.tiff,.webp
```
此时需要按照image/image/picture的排列方式，新建文件夹。
```python
data_dir = "./DogsVSCats/"
```

import torch
import torchvision
from torchvision import datasets,transforms
import os
import matplotlib.pyplot as plt 
import time

# data_dir = os.getcwd() 
# data_dir = os.path.join(data_dir,"DogsVSCats")
data_dir = "./DogsVSCats/"

data_transform = {
    x:transforms.Compose(
        [
            transforms.Scale([64,64]),    #Scale类将原始图片的大小统一缩放至64×64
            transforms.ToTensor()
        ]
    )
    for x in ["train","valid"]
}


image_datasets = {
    x:datasets.ImageFolder(
        root=os.path.join(data_dir,x),  
        #将输入参数中的两个名字拼接成一个完整的文件路径
        transform=data_transform[x]
    )
    for x in ["train","valid"]
}


dataloader = {
    x:torch.utils.data.DataLoader(
        dataset=image_datasets[x],
        batch_size=16,
        shuffle=True
    )
    for x in ["train","valid"]
}



X_example, Y_example = next(iter(dataloader['train']))
print('X_example个数{}'.format(len(X_example)))   #X_example个数16
print('Y_example个数{}'.format(len(Y_example)))   #Y_example个数16

index_classes = image_datasets['train'].class_to_idx   # 显示类别对应的独热编码
print(index_classes)     #{'cat': 0, 'dog': 1}


example_classes = image_datasets['train'].classes     # 将原始图像的类别保存起来
print(example_classes)       #['cat', 'dog']



img = torchvision.utils.make_grid(X_example)
img = img.numpy().transpose([1,2,0])
print([example_classes[i] for i in Y_example])
#['cat', 'cat', 'cat', 'cat', 'dog', 'cat', 'cat', 'dog', 'cat', 'cat', 'dog', 'dog', 'cat', 'dog', 'dog', 'cat']
plt.imshow(img)
plt.show()

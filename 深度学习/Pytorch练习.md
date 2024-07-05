# Pytorch练习





---



## Dataset类

```python
class Dataset:
    def __init__(self, x_list):
        pass
    
    def __getitem__(self, idx):
        pass
    
    def __len__(self):
        pass
```

​                     

 Pytorch的dataset类是一个抽象类，在 [PyTorch](https://so.csdn.net/so/search?q=PyTorch&spm=1001.2101.3001.7020) 中，任何继承自 `torch.utils.data.Dataset` 的自定义数据集**必须需要实现**它的`getitem() `方法和`len()`方法。

- `__len__(self)`： 这个方法返回数据集中的数据点的总数，即数据集的大小

- `__getitem__(self, idx)，`这个方法应该返回**一个索引处**的数据点和其对应的标签（所以参数为idx）



### Dataset类作用

- 机器学习项目中需要处理大量数据时，可以使用PyTorch的Dataset来组织和管理数据。
- 需要对数据进行预处理、增强或归一化等操作时，可以使用PyTorch的Dataset来方便地实现这些功能。

- 需要将数据集加载到内存中时，可以使用PyTorch的Dataset来实现高效的数据读取和缓存





### `__init__`方法

```python
def __init__(self, x_list):
        """ 初始化类实例
        参数:
            x_list: data with list type
       返回值:
            None
        """
        if not isinstance(x_list, list):
            raise ValueError("input x_list is not a list type")
        self.data = x_list
        print("intialize success")
```

`__init__`（）方法负责初始化，根据自己的经验，必须要对初始化函数的参数进行类型检查，避免不必要的麻烦。

- self表示这个方法是属于类实例的，而不是类的。函数中的**self.data**是非常重要的实例属性，**后面的两个方法的意义实际上就是来获取这个实例属性的信息。**

- 它是在对象创建时被解释器自动调用的







### `_getitem_`方法

```python
    def __getitem__(self, idx):
        return self.data[idx]
```

- 使用索引的方式获取这个类实例的属性值和标签



### `_len_`方法

```python
	def __len__(self):
        return len(self.data)
```

- 测量某个类实例属性的长度





### 具体代码实现

```python
from torch.utils.data import Dataset
from PIL import Image
import os
class Mydata(Dataset):
    
    def __init__(self,root_dir,label_dir):
        self.root_dir = root_dir
        self.label_dir = label_dir
        self.path = os.path.join(self.root_dir,self.label_dir)
        self.img_path = os.listdir(self.path)
        
    def __getitem__(self,idx):
        img_name = self.img_path[idx]
        img_item_path = os.path.join(self.root_dir,self.label_dir,img_name)
        img = Image.open(img_item_path) #调用Image库
        label = self.label.dir
        return img,label
    
    def __len__(self):
        return len(self.img_path)
```



这是一个基于深度学习下图像数据集创建的具体例子，在这个例子中，`Mydata` 类定义了一个图像数据集





```python
# 定义Dataset加载方式
class MyData(Dataset):
    def __init__(self, filepath, labels=None, transform=None):
        self.filepath = filepath
        self.labels = labels
        self.transform = transform

    def __getitem__(self, index):
        image = read_image(self.filepath[index]) # 读取后的图片维度是[channl, height, width]
        image = image.to(torch.float32) / 255. # 转换为float32类型，除以255进行归一化
        if self.transform is not None:
            image = self.transform(image)
        if self.labels is not None:
            return image, self.labels[index]
        return image

    def __len__(self):
        return self.filepath.shape[0]

```

这是深度学习实践中猫狗分类图像数据集创建的具体例子，在这个例子中，`Mydata` 类有三个参数。调用如下：

```python
ds1 = MyData(train_path, train_labels, transform)
ds2 = MyData(val_path, val_labels, transform)
ds3 = MyData(test_path, test_labels, transform)
```



#### 总结一下：

`__init__`（）方法负责初始化

- 一般在数据集中指输入的具体数据。一般就是定义所输入的数据所需的一些参数。并常在其中将数据划分为输入特征(x_data)和目标标签(y_data)。  同时，一般还在其中记录了数据集的长度。或者进行图像的变换

- 参数一般为self、x_list或者通过数据存放的相对路径使用os或其他函数调用



`_getitem_`（）方法负责的是通过索引去调用数据集中数据的值和标签。如果`__init__`中未划分标签及数据 就在这里划分

- 参数为self, idx



`_len_`（）方法负责返回数据集的长度

- 一般只有self

---







## Dataloader类

```python
from torch.utils.data import DataLoader
```



### 1. 定义

`DataLoader` 是一个迭代器，用于将 `Dataset` 封装成易于访问的数据流，支持批量加载和多进程数据加载等操作。



### 2. 参数

- `dataset`: 要加载的 `Dataset `对象。

- `batch_size`（可选）: 每个批次加载的样本数量。即对`Dataset`数据集进行等分，分成的份数(每份叫作一个batch)为len(dataset)/batch_size。`batch_size`通常是单次训练使用的数据量，默认为1。

- `shuffle`（可选）: 是否在每个训练周期开始时打乱数据。

- `num_workers`（可选）: 用于数据加载的进程数。



### 3.示例 ：如何使用Dataloader

```python
train_ds = DataLoader(ds1, batch_size=batch_size, shuffle=True)
val_ds = DataLoader(ds2, batch_size=batch_size, shuffle=True)
test_ds = DataLoader(ds3, batch_size=batch_size, shuffle=True)
batch_size = 128 # 批大小
```


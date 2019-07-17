# keras-frcnn-master理解

感谢！

> [B站楼主-Mike高](https://space.bilibili.com/45151802)

## 各种模块中部分函数的理解

### `__future__` 中的division

1. __在Python2.x中，对于除法有两种情况，如果是两个整数相除，结果仍是整数，余数会被扔掉(Truncating Division)这种除法叫“地板除”：__

- 在Python3.x中，所有除法都是精确除法，“地板除”用`//`表示：

  ```python
  >>> 3 / 4
  0.75
  >>> 3 // 4
  0
  ```

  

- 要想在Python2.x中使用Python3.x的除法，可以通过`__future__`模块的`division`实现：

### pprint模块

1. __pprint.pprint()`是“美观打印机”，用于生成数据结构的美观视图，如果一行大于等于8个对象，则会竖行表示__

- 说明：不是很理解，回头再说（但是我试了一下，`pprint.pprint()`函数会尽可能的和你构造的数据格式一样）

  ```python
  import pprint
  
  data = (
      'test',
      [1, 3, 3, 'test', 4, 5],
      'this is a string', 'hello world',
      {'age': 20, 'gender': 'F'}
  )
  
  data1 = ('test', [1, 3, 4, 5, 'test'], 'this is a string', {'age': 20, 'gender': 'F'} )
  
  
  print(data1)
  print()
  print('+' * 30)
  print()
  pprint.pprint(data1)
  ```

  

### pickel模块

说明：python3程序运行中得到了一些字符串，列表，字典等数据，想要永久的保存下来，方便以后使用，而不是简单的存放在内存中，关机断电数据会丢失，python的pickle模块提供了可以将对象转换为一种可以传输存储的数据格式。

1. __dump函数：需要指定两个参数，第三个参数可以不指定默认为0，第一个参数是需要序列化的python3对象，第二个是本地文件夹，第三个是存储的协议{0, 1, 2}__

   `pickle.dump(对象, 文件)`

   ```python
import pickle
   
   
   dict_a = {'name': 'tom', 'age': 21}
   
   with open('pickle模块/text.txt', 'wb') as f:
       pickle.dump(dict_a, f)
   
   with open('pickle模块/text.txt', 'rb') as f:
       dict_b = pickle.load(f)
   
   print(dict_b)
   ```

2. __dunps函数：将对象序列化，不存入文件中__

   `pickle.dumps(对象)`

   ```python
   import pickle
   
   
   list_a = [1, 3, 3, 4, 'I am tom']
   
   data_1 = pickle.dumps(list_a)
   
   data_2 = pickle.loads(data_1)
   
   print(data_1)
   print(data_2)
   ```


### numpy模块

说明：数组、矩阵的科学计算模块

1. __meshgrid()函数__

   说明：就是一个映射的关系，直角坐标系，X轴=x，Y轴=y:

   ```python
   >>> import numpy as np
   
   >>> x = np.arange(4)
   >>> y = np.array([6, 7, 8, 9])
   >>> y
   array([6, 7, 8, 9])
   >>> x
   array([0, 1, 2, 3])
   >>> x, y = np.meshgrid(x, y)
   >>> x
   array([[0, 1, 2, 3],
          [0, 1, 2, 3],
          [0, 1, 2, 3],
          [0, 1, 2, 3]])
   >>> y
   array([[6, 6, 6, 6],
          [7, 7, 7, 7],
          [8, 8, 8, 8],
          [9, 9, 9, 9]])
   
   >>> x.flatten()
   array([0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3])
   >>> y.flatten()
   array([6, 6, 6, 6, 7, 7, 7, 7, 8, 8, 8, 8, 9, 9, 9, 9])
   ```

2. __stack(), concatenate()函数__

   说明：将两个矩阵进行组合，stack()函数是将两个矩阵进行组合（堆积多维度），concatenate()函数是将两个矩阵进行组合（将第二个矩阵连接到第一矩阵豁免，矩阵维度不改变）,其中，还有个参数 axis是调整维度的顺序的，和组合方式的。

   参数： __axis__ 取值__[0, 1, 2...]__,实际就是代表矩阵的维度：比如矩阵A的__shape__为__（2， 3， 4）__，代表的是__3__维矩阵，在一个大矩阵中，__有2个，3行，4列的矩阵__，而__axis=0__代表在个数上添加，__axis=1__代表在行数上添加，__axis=2__代表在列数上添加

   `stack()` 增加一个维度

   ```python
   import numpy as np
   
   
   a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
   b = np.array([[0, 9, 8], [7, 6, 5], [3, 2, 1]])
   
   # a.shape
   # (3, 3)
   # b.shape
   # (3, 3)
   
   c = np.stack((a, b), axis=0)
   # c
   # array([[[1, 2, 3],
   #         [4, 5, 6],
   #         [7, 8, 9]],
   
   #        [[0, 9, 8],
   #         [7, 6, 5],
   #         [3, 2, 1]]])
   
   # c.shape
   # (2, 3, 3)
   
   d = np.stack((a, b), axis=1)
   
   # d.shape
   # (3, 2, 3)
    
   # d 的组合方式，axis=1时，就是先一行a元素， 在一行b元素
   # d(array)
   # [[[1 2 3]
   #   [0 9 8]]
   
   #  [[4 5 6]
   #   [7 6 5]]
   
   #  [[7 8 9]
   #   [3 2 1]]]
   
   e = np.stack((a, b), axis=2)
   # e.shape
   #  (3, 3, 2)
   
   # e的组合方式，axis=2时，就是一个a矩阵的元素，加b矩阵的元素
   # e
   # [[[1 0]
   #   [2 9]
   #   [3 8]]
   
   #  [[4 7]
   #   [5 6]
   #   [6 5]]
   
   #  [[7 3]
   #   [8 2]
   #   [9 1]]]
   ```
   
   concatenate() 函数 增加数量，但不增加维度
   
   ```python
   import numpy as np
   
   
   a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
   b = np.array([[0, 9, 8], [7, 6, 5], [3, 2, 1]])
   
   # a.shape
   # (3, 3)
   # b.shape
   # (3, 3)
   
   # axis=0，暂时理解：是竖向拼接
   c = np.concatenate((a, b), axis=0)
   
   # c
   # array([[1, 2, 3],
   #         [4, 5, 6],
   #         [7, 8, 9],
   #         [0, 9, 8],
   #         [7, 6, 5],
   #         [3, 2, 1]])
   
   # c.shape
   # (6, 3)
      
   
   # axis=1，暂时理解：横向拼接
   d = np.concatenate((a, b), axis=1)
   # d
   # array([[1, 2, 3, 0, 9, 8],
   #         [4, 5, 6, 7, 6, 5],
   #         [7, 8, 9, 3, 2, 1]])
   
   # d.shape
   # (3, 6)
   ```
   
   对concatenate()函数的深入理解
   
   ```python
   >>> import numpy as mp
   >>> import numpy as np
   >>> a = np.arange(4)
   >>> a
   array([0, 1, 2, 3])
   >>> b = np.arange(5, 9)
   >>> b
   array([5, 6, 7, 8])
   >>> a , b = np.meshgrid(a, b)
   >>> a
   array([[0, 1, 2, 3],
          [0, 1, 2, 3],
          [0, 1, 2, 3],
          [0, 1, 2, 3]])
   >>> b
   array([[5, 5, 5, 5],
          [6, 6, 6, 6],
          [7, 7, 7, 7],
          [8, 8, 8, 8]])
   >>> c = np.stack((a, b), axis=0)
   >>> c
   array([[[0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3]],
   
          [[5, 5, 5, 5],
           [6, 6, 6, 6],
           [7, 7, 7, 7],
           [8, 8, 8, 8]]])
   >>> c.shape
   (2, 4, 4)
   >>> d = np.concatenate((c, c), axis=2)
   >>> d
   array([[[0, 1, 2, 3, 0, 1, 2, 3],
           [0, 1, 2, 3, 0, 1, 2, 3],
           [0, 1, 2, 3, 0, 1, 2, 3],
           [0, 1, 2, 3, 0, 1, 2, 3]],
   
          [[5, 5, 5, 5, 5, 5, 5, 5],
           [6, 6, 6, 6, 6, 6, 6, 6],
           [7, 7, 7, 7, 7, 7, 7, 7],
           [8, 8, 8, 8, 8, 8, 8, 8]]])
   >>> d.shape
   (2, 4, 8)
   >>> e = np.concatenate((c, c), axis=0)
   >>> e
   array([[[0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3]],
   
          [[5, 5, 5, 5],
           [6, 6, 6, 6],
           [7, 7, 7, 7],
           [8, 8, 8, 8]],
   
          [[0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3]],
   
          [[5, 5, 5, 5],
           [6, 6, 6, 6],
           [7, 7, 7, 7],
           [8, 8, 8, 8]]])
   >>> e.shape
   (4, 4, 4)
   >>> f = np.concatenate((c, c), axis=1)
   >>> f
   array([[[0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3]],
   
          [[5, 5, 5, 5],
           [6, 6, 6, 6],
           [7, 7, 7, 7],
           [8, 8, 8, 8],
           [5, 5, 5, 5],
           [6, 6, 6, 6],
           [7, 7, 7, 7],
           [8, 8, 8, 8]]])
   >>> f.shape
   (2, 8, 4)
   ```
   
   

## anchor理解

说明：RPN网络的输入的数据是是共享层的feature map ，针对feature map 上的每一个像素点，映射回原图像的感受视野的中心点（基准点）。围绕这个基准点选取k=9个矩形框，这些矩形框就是“anchor”，它们一共有3种形状，长宽比分别为{1：1， 1：2， 2：1}，共9种。如图。假设feature map的尺寸为w*h，那么针对这个feature map 的anchor一共有           `9 * w * h `个anchor.

__anchor形状示意图：__

![anchor-形状](G:\git-项目\Notes\git_picture\anchor-形状.png)

### anchor scale (锚框的尺寸)

说明：x, y指锚框的长宽，scale为尺寸（好像是给好的），计算x， y 的大小。公式为：

```python
>>> import numpy as np
# 设x， y 的比例为1.5
# 实际中，不知道 x，y 的值
>>> x = 30
>>> y = 20
# scale为面积开平方
>>> scale = np.sqrt(x * y)
>>> scale
24.49489742783178
# 算出x值
>>> x = scale * np.sqrt(x / y)
>>> x
29.999999999999996
# 算出y值
>>> y = scale / np.sqrt(x / y)
>>> y
20.0
```

1. __anchor 大小计算，根据输入样本尺寸计算anchor大小，scale固定：__

   ```python
   import numpy as np
   import matplotlib.pyplot as plt
   import matplotlib.patches as patches
   from PIL import Image
   # %matplotlib inline
   
   
   # 简单原始图像输入 尺寸 128*128
   sample_raw_x = 128
   sample_raw_y = 128
   
   # rpn 8倍，下采样
   rpn_stride = 8
   
   Feature_size_x = sample_raw_x / rpn_stride
   Feature_size_y = sample_raw_y / rpn_stride
   # print('Feature map：X长度', Feature_size_x)
   # print('Feature map：Y长度', Feature_size_y)
   
   # 定义：scale的大小，3种锚框大小（面积不同），3种长宽比例（面积相同），共9中锚框
   scales = [1, 2, 4]
   
   # 定义：锚框比例的大小（x / y）
   ratios = [0.5, 1, 2]
   
   
   # 测试、预览
   # 组合feature map的坐标，feature map size (16 * 16)
   # fx = np.arange(Feature_size_x)
   # fy = np.arange(Feature_size_y)
   # print(fx)
   # print(fy)
   
   # F_X, F_Y = np.meshgrid(fx, fy)
   # print(F_X)
   # print('*' * 70)
   # print(F_Y)
   
   # 将 F_X ,F_Y 平铺
   # F_X, F_Y = F_X.flatten(), F_Y.flatten()
   
   # 处理anchor函数
   def anchor(Feature_size_x, Feature_size_y, rpn_stride, scales, ratios):
   
       # 组合anchor的尺寸和比例  scales ratios
       scales, ratios = np.meshgrid(scales, ratios)
       # 将 尺寸和比例打成一维向量
       scales, ratios = scales.flatten(), ratios.flatten()
   
       # 计算w，h的值
       w = scales * np.sqrt(ratios)  # 长度
       h = scales / np.sqrt(ratios)  # 宽度
       # print(w)
       # print('+' * 30)
       # print(h)
   
       # 映射回原始图像（feature map 映射回原始图像对应的点）
       Shift_X = np.arange(0, Feature_size_x) * rpn_stride
       Shift_Y = np.arange(0, Feature_size_y) * rpn_stride
   
       # anchor 在原始图像上的位置
       X, Y = np.meshgrid(Shift_X, Shift_Y)  # X, Y 是anchor在原始图像上的中心点
       # print(X.shape)
   
       # 每一个anchor点上有 9 个尺寸的锚框
       X, w = np.meshgrid(X, w)  # 将锚点的X ，和锚点对应的w长度，对应起来，X(9, 256),w(9, 256)
       Y, h = np.meshgrid(Y, h)
       # print(X.shape)
       # print(w.shape)
   
       # stack 组合 anchor中心点，和组合对应中心点的长宽
       anchor_center = np.stack((Y, X), axis=2).reshape(-1, 2)
       anchor_size = np.stack((h, w), axis=2).reshape(-1, 2)
   
       # 将左上、右下的坐标点输出
       boxes = np.concatenate((anchor_center - 0.5 * anchor_size,
                               anchor_center + 0.5 * anchor_size), axis=1)
   
       # print(boxes.shape)
       return boxes
   
   
   if __name__ == '__main__':
   
       boxes = anchor(Feature_size_x, Feature_size_y, rpn_stride, scales, ratios)
   
       plt.figure(figsize=(10, 10))
       image = Image.open('anchor/mv_1.jpg')
       plt.imshow(image)
      
   
       axs = plt.gca()
   
       for i in range(boxes.shape[0]):
           box = boxes[i]
           # print(box)
           rec = patches.Rectangle((box[0], box[1]), box[2] - box[0], box[3] - box[1],
                                   edgecolor='r', facecolor='none')
   
           axs.add_patch(rec)
       
       plt.show()
   ```




## 多任务的理解（多输入多输出）

说明：

> 多任务学习是一种归纳迁移机制，基本目标是提高泛化性能。多任务学习通过相关任务训练信号中领域特定信息来提高泛化能力，利用共享表示采用并行训练的方法学习多个任务。（我也读不懂）多任务就是同时学习多个任务的机器学习。深度网络的层级表示从语义上从底层到高层不断递进。深度网络强大的表现能力，使得多任务深度学习有了施展的空间。在多任务深度网络中，底层次语义信息共享有助于减少计算量，同时共享表示层可以使得几个有共性的任务更好的结合相关信息，任务特定层则可以单独建立模型处理特定信息，实现共享层信息和任务特定信息的统一。[引用地址](https://zhuanlan.zhihu.com/p/22190532)

### 预备知识

1. 关于`python`的匿名函数`lambda`

   说明：在编程语言中，`c++/java`是属于过程式编程，而匿名函数`python`的`lambda`一般用于函数式编程中。匿名函数使用规则：

   -  一般为一行表达式，必须有返回值
   - 不能有`return`
   - 可以没有参数，也可以有多个参数

   ```python
   # 使用方法一
   lam = lambda x:x * 3
   
   restual = lam(4)
   print(restual)
   
   
   # 等价于func函数定义
   def func(x):
       return x * 3
   
   restual = func(4)
   print(restual)
   
   
   # 匿名函数可以有多个参数
   lam = lambda x, y:x - y
   
   restual = lam(5, 3)
   print(restual)
   
   
   # 嵌套使用
   def func(x, y):
       # 匿名函数本身被当作 func函数 的return的返回值
       return lambda z: z + x + y
   
   f = func(3, 4)
   restual = f(2)
   print(restual)
   
   
   # 匿名函数 + def 混合使用
   def func(**key):
       print(key)
   
   lam = lambda z: func(**z)
   
   restual = lam({'a':1, 'b':2})
   print(restual)
   ```

2. yield惰性生成器

   说明：在`python`中。使用yield的函数被称为生成器（generator）。跟普通函数不同的是生成器时返回迭代器的函数，只能用于迭代操作，更简单的理解生成器就是一个迭代器。再调用生成器运行过程中，__每次遇到`yield`时函数会保存当前所有运行的信息，返回yield的值，并在下次执行next()方法时从当前位置继续执行，或者时调用生成器函数对象的方法__`__next__()`

   ```python
   import sys
    
   def fibonacci(n): # 生成器函数 - 斐波那契
       a, b, counter = 0, 1, 0
       while True:
           if (counter > n): 
               return
           yield a
           a, b = b, a + b
           counter += 1
   
   if __name__ == '__main__':
   
       # f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
       f = fibonacci(10)
       while True:
           try:
               # print (next(f), end=" ")
               print(f.__next__(), end=' ')
           except StopIteration:
               sys.exit()
   ```

   
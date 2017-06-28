---
title: Python
date: 2017-06-09 20:52:56
categories:
tags: Python
description: Python
mathjax: False
---

# 图像操作

## 关于PIL image和skimage的图像处理

### 对skimage图像

镜像处理：

```
from skimage import io,transform
import matplotlib.pyplot as plt
import cv2

img=io.imread("test.jpg")
#img=transform.rotate(img,180)
img=cv2.flip(img,1)
plt.figure('skimage')
plt.imshow(img)
plt.show()
print img.shape
print(img.dtype)
```

# 文本处理

## 提取两个文件中相同的部分

a.txt内容：

```
aaa 0
bbb 0
ccc 0
```

b.txt内容：

```
aaa 0
bbb 1
ccc 0
```

### 提取相同的部分

写入到c.txt

```
fa=open('a.txt','r')
a=fa.readlines()
fa.close()
fb=open('b.txt','r')
b=fb.readlines()
fb.close()
c= [i for i in a if i in b]
fc=open('c.txt','w')
fc.writelines(c)
fc.close()
print 'Done'
```

最后c.txt内容

```
aaa 0
ccc 0
```



# 读取文件并绘制图片

## 散点图

```
import matplotlib
import matplotlib.pyplot as plt

def loadData(fileName):
    inFile=open(fileName,'r')
    X = []
    y = []
    for line in inFile:
        trainingSet = line.split()  
        X.append(trainingSet[0])
        y.append(trainingSet[1])
    return (X, y)


def plotData(X, y):
    length = len(y)
    plt.figure(1)
    #plt.plot(X, y, 'rx')
    plt.scatter(X,y,c='r',marker='.')
    plt.xlabel('eye_width')
    plt.ylabel('eye_height')
    #plt.show()
    plt.savefig('dis.png')

if __name__ == '__main__':
    (X, y) = loadData('dis.txt')

    plotData(X, y)
```

## 折线图

```
import matplotlib.pyplot as plt 

y1=[14,3329,213675,451416,491919,728911,1379232,1287442,309026,85674,29481,9051,2894,932,279,86,14,6,0,0] 
x1=range(0,200,10) 

num = 0
for i in range(20):
    num+=y1[i]
print num
plt.plot(x1,y1,label='Frist line',linewidth=1,color='r',marker='o', 
markerfacecolor='blue',markersize=6) 
plt.xlabel('eye_width distribute') 
plt.ylabel('Num') 
plt.title('Eye\nCheck it out') 
plt.legend()
plt.savefig('figure.png') 
plt.show() 
```



# 统计文件中的数据分布

```
import matplotlib
import matplotlib.pyplot as plt
import numpy as np

def loadData(fileName):
    inFile=open(fileName,'r')
    x_lines=inFile.readlines()#x_lines为str的list
    x_distribute=[0]*20 #对列表元素进行重复复制
    for x_line in x_lines:
        x_point=x_line.split()[0]
        i=np.int(np.float32(x_point)/10)  #注意str要先转化为np.float才能转化为int型
        x_distribute[i]+=1
    print x_distribute

if __name__ == '__main__':
    loadData('dis.txt')
```

# windows下安装OpenCV for Python

1. Download Python, Numpy, OpenCV from their official sites.

2. Extract OpenCV (will be extracted to a folder opencv)

3. Copy ..\opencv\build\python\x86\2.7\cv2.pyd

4. Paste it in C:\Python27\Lib\site-packages

5. Open Python IDLE or terminal, and type

   ```
   >>> import cv2
   ```

# 读取文件，进行批量创建目录

存在一个image.txt里面每一行都是目录/文件名，要提取目录名，并由此创建新目录，内容如下：

```
xxx/0.jpg
yyy/0.jpg
zzz/0.jpg
```

创建xxx目录，yyy目录，zzz目录

```
import sys
import os
import numpy as np
import skimage
from skimage import io,transform
import matplotlib.pyplot as plt
import cv2

def read_file(path):
    with open(path) as f:
        return list(f)


def make_dir(image_path):   
    image_lines = read_file(image_path)
    if not image_lines:
        print 'empty file'
        return
    i = 0
    for image_line in image_lines:
        image_line = image_line.strip('\n')
        subdir_name = image_line.split('/')[0]
        print subdir_name
        isExists=os.path.exists(subdir_name)
        if not isExists:
            os.mkdir(subdir_name)
            print subdir_name+"created successfully!"
       
        i = i+1
        sys.stdout.write('\riter %d\n' %(i))
        sys.stdout.flush()

    print 'Done'

if __name__=='__main__': 
    image_path='./image.txt'
    make_dir(image_path)

```


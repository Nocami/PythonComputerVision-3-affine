# PythonComputerVision-3
本次我们介绍图像到图像的映射相关内容，主要内容为对图像块应用仿射变换，我们将其称为图像扭曲（或者放射扭曲）。这里会介绍放射扭曲的一个简单的例子————图像中的图像。  
## 单映性变换：  
单应性变换，简单来说，是将一个平面内的点映射到另一个平面内的二位投影变换，简称2D图像变换。  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/111.jpg)  
点的齐次坐标依赖于其尺度定义，因此单应性矩阵H也仅依赖尺度定义，所以，单应性矩阵具有8个独立的自由度。  
**两种重要映射类型：**  

![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/01bb.jpg)图-透视  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/04仿射.jpg)图-仿射  
### 直接线性变换算法：
单应性矩阵可以由两幅图像中对应点对计算出来，一个完全射影变换具有8个自由度。根据对应点约束，每个对应点对可以写出两个方程，分别对应于x和y坐标。因此，计算单应性矩阵H需要4个对应点对。  
DLT（Direct Linear Transformation，直接线性变换）是给定4个或者更多对应点对矩阵来计算单应性矩阵H的算法。将单应性矩阵H作用在对应点对上，重新写出该方程，我们可以得到下面方程：  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/333%E7%9B%B4%E6%8E%A5%E7%BA%BF%E6%80%A7.jpg)  
### 仿射变换：
因为仿射变换有六个自由度，因此我们需要三个对应点对来估计矩阵H。仿射变换可以用上面的DLT算法估计得出。  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/222仿射.jpg)  
## 图像扭曲
对图像块应用仿射变换，我们将其称为图像扭曲（或者放射扭曲）。  
transformed_im = ndimage.affine_transform(im,A,b,size)  
使用如上所示的一个线性变换A和一个平移向量b来对图像块应用仿射变换。选项size可以用来指定输出图像的大小。默认输出图像设置为和原始图像同样大小。  
### 图像中的图像
仿射扭曲的一个简单例子是，将图像或者图像的一部分放置在另一幅图像中，使得它们能够和指定的区域或标记物对齐。  
函数image_in_image()的输入参数为两幅图像和一个坐标。该坐标为将第一幅图像放置到第二幅图像中的角点坐标。  
详细代码如下：  
``
 # -*- coding: utf-8 -*-
from PCV.geometry import warp, homography
from PIL import  Image
from pylab import *
from scipy import ndimage

# example of affine warp of im1 onto im2

im1 = array(Image.open(r'e:\Study\pythonxyProject\sift\images/001b.jpg').convert('L'))
im2 = array(Image.open(r'e:\Study\pythonxyProject\sift\images/002.jpg').convert('L'))
# set to points
tp = array([[338,540,540,338],[372,372,696,696],[1,1,1,1]])
#tp = array([[675,826,826,677],[55,52,281,277],[1,1,1,1]])
im3 = warp.image_in_image(im1,im2,tp)
figure()
gray()
subplot(141)
axis('off')
imshow(im1)
subplot(142)
axis('off')
imshow(im2)
subplot(143)
axis('off')
imshow(im3)

# set from points to corners of im1
m,n = im1.shape[:2]
fp = array([[0,m,m,0],[0,0,n,n],[1,1,1,1]])
# first triangle
tp2 = tp[:,:3]
fp2 = fp[:,:3]
# compute H
H = homography.Haffine_from_points(tp2,fp2)
im1_t = ndimage.affine_transform(im1,H[:2,:2],
(H[0,2],H[1,2]),im2.shape[:2])
# alpha for triangle
alpha = warp.alpha_for_triangle(tp2,im2.shape[0],im2.shape[1])
im3 = (1-alpha)*im2 + alpha*im1_t
# second triangle
tp2 = tp[:,[0,2,3]]
fp2 = fp[:,[0,2,3]]
# compute H
H = homography.Haffine_from_points(tp2,fp2)
im1_t = ndimage.affine_transform(im1,H[:2,:2],
(H[0,2],H[1,2]),im2.shape[:2])
# alpha for triangle
alpha = warp.alpha_for_triangle(tp2,im2.shape[0],im2.shape[1])
im4 = (1-alpha)*im3 + alpha*im1_t
subplot(144)
imshow(im4)
axis('off')
show()
``
这里要特别说明，为了能准确的将第一幅图放在第二幅图中我们想要的位置，需要找出要放置在第二图中的角点坐标，这里我们可以使用PyLab类库中的ginput()函数获得，代码如下：  
``
from PIL import Image
from pylab import *

im = array(Image.open(r'e:\Study\pythonxyProject\sift\images/003.jpg'))
imshow(im)
print 'Please click 3 points'
x = ginput(3)
print 'you clicked:',x
show()
``
此段代码可以帮助你找到图片中的角点坐标，即鼠标所☞的位置，显示在左下角中。  
**还需特别说明**：函数参数中的输入分别为[y1,y2,y3,y4][x1,x2,x3,x4][1,1,1,1]，即图片的**左上角**开始，逆时针旋转。  
这里使用了我目前所在高校--JMU的部分图片来给大家做演示：  
图一和图二分别为JMU的**延奎图书馆**的日间景与夜景：  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/001b.jpg)
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/002.jpg)  
我们将图一放置在图二的中间部位，让图书馆的昼夜景象同处于同一画面之中：  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/test00.jpg)  
特写：  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/QQ%E6%88%AA%E5%9B%BE20190319153042.jpg)  

这就是使用仿射变换将一副图像放置到另一幅图像中的实例。

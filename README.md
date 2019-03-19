# PythonComputerVision-3
本次我们介绍图像到图像的映射相关内容，主要内容为对图像块应用仿射变换，我们将其称为图像扭曲（或者放射扭曲）。这里会介绍放射扭曲的一个简单的例子————图像中的图像。  
## 单映性变换：  
单应性变换，简单来说，是将一个平面内的点映射到另一个平面内的二位投影变换，简称2D图像变换。  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/111.jpg)  
点的齐次坐标依赖于其尺度定义，因此单应性矩阵H也仅依赖尺度定义，所以，单应性矩阵具有8个独立的自由度。  
**两种重要映射类型：**
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/01bb.jpg)  
图-透视  
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/04仿射.jpg)  
图-仿射  
### 
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/001b.jpg)
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/002.jpg)
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/test00.jpg)
![image](https://github.com/Nocami/PythonComputerVision-3/blob/master/images/QQ%E6%88%AA%E5%9B%BE20190319153042.jpg)

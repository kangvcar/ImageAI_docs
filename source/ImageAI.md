
# ImageAI

ImageAI是一个python库，旨在使开发人员能够使用简单的几行代码构建具有包含深度学习和计算机视觉功能的应用程序和系统。
这个**AI Commons**项目[](https://commons.specpal.science)[https://commons.specpal.science](https://commons.specpal.science) 由[Moses Olafenwa](https://twitter.com/OlafenwaMoses)和[John Olafenwa](https://twitter.com/johnolafenwa)开发和维护

* * *

**ImageAI**本着简洁的原则，支持最先进的机器学习算法，用于图像预测，自定义图像预测，物体检测，视频检测，视频对象跟踪和图像预测训练。**ImageAI**目前支持使用在ImageNet-1000数据集上训练的4种不同机器学习算法进行图像预测和训练。**ImageAI**还支持使用在COCO数据集上训练的RetinaNet进行对象检测，视频检测和对象跟踪。
最终，**ImageAI**将为计算机视觉提供更广泛和更专业化的支持，包括但不限于特殊环境和特殊领域的图像识别。

**新版本：ImageAI 2.0.1**

新功能：

- 添加了 SqueezeNet，ResNet50，InceptionV3 和 DenseNet121 模型进行自定义图像预测训练

- 添加了自定义训练模型和json文件进行导入和导出自定义图像

- 预览版：添加视频对象检测和视频自定义对象检测（对象跟踪）

- 为所有图像预测和对象检测任务添加文件，numpy数组和流输入类型（仅用于视频检测的文件输入）

- 添加文件和numpy数组输出类型，用于图像中的对象检测和自定义对象检测

- 引入4种速度模式（'normal', 'fast', 'faster' and 'fastest'）进行图像预测，在'fastest'速度模式下预测时间将缩短50％，同时保持预测精准度

- 为图像所有物体检测和视频物体检测任务引入5种速度模式（'normal', 'fast', 'faster', 'fastest' and 'flash'），在'flash'速度模式下预测时间将缩短80％以上并且精准度与`minimum_percentage_probability`保持一致，请将该值调至较低

- 引入帧检测率，允许开发人员调整视频中的检测间隔`frame_detection_interval`，有利于达到特定效果。

## 依赖

在应用程序开发中使用**ImageAI**之前必须安装以下依赖项：

- Python 3.5.1（及更高版本） [下载](https://www.python.org/downloads/)（即将推出支持Python 2.7）

- pip3 [安装](https://pypi.python.org/pypi/pip)

- Tensorflow 1.4.0（及更高版本）  [安装](https://www.tensorflow.org/install/install_windows) 或 通过pip安装

    ` pip3 install --upgrade tensorflow `

- Numpy 1.13.1（及更高版本） [安装](https://www.scipy.org/install.html)或 通过pip安装

    ` pip3 install numpy `

- SciPy 0.19.1（及更高版本） [安装](https://www.scipy.org/install.html)或 通过pip安装

    ` pip3 install scipy `

- OpenCV [安装](https://pypi.python.org/pypi/opencv-python)或 通过pip安装

    ` pip3 install opencv-python `

- pillow  [安装](https://pypi.org/project/Pillow/2.2.1/)或 通过pip安装

    ` pip3 install pillow  `

- Matplotlib [安装](https://matplotlib.org/users/installing.html)或 通过pip安装

    ` pip3 install matplotlib `

- h5py [安装](http://docs.h5py.org/en/latest/build.html)或 通过pip安装

    ` pip3 install h5py `

- Keras 2.x [安装](https://keras.io/#installation)或 通过pip安装

    ` pip3 install keras `

## 安装

请在命令行中运行如下命令来安装 **ImageAI**：

`pip3 install https://github.com/OlafenwaMoses/ImageAI/releases/download/2.0.1/imageai-2.0.1-py3-none-any.whl`

或者下载Python Wheel [**imageai-2.0.1-py3-none-any.whl**](https://github.com/OlafenwaMoses/ImageAI/releases/download/2.0.1/imageai-2.0.1-py3-none-any.whl) 安装文件并在命令行中指定安装文件的路径来安装**ImageAI**：

`pip3 install C:\User\MyUser\Downloads\imageai-2.0.1-py3-none-any.whl`

## 图像预测

**ImageAI** 提供4种不同的算法及模型来执行图像预测，并在ImageNet-1000数据集上进行训练。提供用于图像预测的4种算法包括 **SqueezeNet**，**ResNet**，**InceptionV3** 和 **DenseNet**。您将在下面看到的示例是使用ResNet50模型进行图像预测的结果。单击图像下方的“教程和文档”链接以查看完整的示例代码，相关说明，最佳实践指南和文档。

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/1.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/1.jpg)

```
convertible : 52.459555864334106
sports_car : 37.61284649372101
pickup : 3.1751200556755066
car_wheel : 1.817505806684494
minivan : 1.7487050965428352
```

[>>>教程和文档](https://imageai-cn.readthedocs.io/en/latest/ImageAI_Image_Prediction.html)

## 对象检测

**ImageAI** 提供了非常方便和强大的方法来对图像执行对象检测并从图像中提取每个对象。用于对象检测的类仅支持当前最先进的RetinaNet目标检测算法，但提供了性能调整和实时处理参数。您将在下面看到的示例是使用RetinaNet模型进行对象检测的结果。单击图像下方的“教程和文档”链接以查看完整的示例代码，相关说明，最佳实践指南和文档。

**_输入图像_** 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2.jpg)

 **_输出图像_** 

 [![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2new.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2new.jpg)

```
person : 91.946941614151
person : 73.61021637916565
laptop : 90.24320840835571
laptop : 73.6881673336029
laptop : 95.16398310661316
person : 87.10319399833679
```

[>>>教程和文档](https://imageai-cn.readthedocs.io/en/latest/ImageAI_Object_Detection.html)

## 视频对象检测和跟踪

**ImageAI** 提供了非常方便和强大的方法来在视频中执行对象检测并跟踪特定对象。用于视频对象检测的类仅支持当前最先进的RetinaNet目标检测算法，但提供了性能调整和实时处理参数。您将在下面看到的示例是使用RetinaNet模型进行对象检测的结果。单击图像下方的“教程和文档”链接以查看完整的示例代码，相关说明，最佳实践指南和文档。

_**视频对象检测**_

_下面是检测到对象的视频快照。_

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/video1.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/video1.jpg)

_**视频自定义对象检测（对象跟踪）**_

_以下是仅检测到人，自行车和摩托车的视频快照。_

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/video2.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/video2.jpg)

[>>>教程和文档](https://imageai-cn.readthedocs.io/en/latest/ImageAI_Video_Object_Detection_and_Tracking.html)

## 定制模型训练

**ImageAI** 为您提供了一些类和方法，用于训练可以对您自定义对象执行预测的新模型。您可以使用 SqueezeNet，ResNet50，InceptionV3 或 DenseNet 算法在不到12行代码中训练您自定义的模型。单击图像下方的“教程和文档”链接可查看完整视频，示例代码，相关说明，最佳实践指南和文档。

_在 IdenProf 数据集中训练模型用于预测专业人员。_

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/idenprof.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/idenprof.jpg)

[>>>教程和文档](https://imageai-cn.readthedocs.io/en/latest/ImageAI_Custom_Prediction_Model_Training.html)

## 自定义图像预测

**ImageAI** 为您提供了类和方法，您可以使用 **ImageAI** 模型训练类训练您自定义的模型并在图像中预测您自定义的对象。您可以使用使用 SqueezeNet，ResNet50，InceptionV3 或 DenseNet 训练自定义模型和包含自定义对象名称映射的JSON文件。单击图像下方的“教程和文档”链接以查看完整的示例代码，相关说明，最佳实践指南和文档。

_在 IdenProf 数据集中训练模型用于预测专业人员。_

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/4.jpg)](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/4.jpg)

```
mechanic : 76.82620286941528
chef : 10.106072574853897
waiter : 4.036874696612358
police : 2.6663416996598244
pilot : 2.239348366856575
```

[>>>教程和文档](https://imageai-cn.readthedocs.io/en/latest/ImageAI_Custom_Image_Prediction.html)

## 实时和高性能实施

**ImageAI** 提供了先进计算机视觉技术的抽象和方便的实现。所有 **ImageAI** 代码都可以在中等CPU核心数的计算机系统上运行。但是，CPU上的图像预测、对象检测等操作的处理速度很慢，不适合实时应用。要以高性能执行实时计算机视觉操作，您需要使用支持GPU的技术。

**ImageAI** 基于 Tensorflow 进行计算机视觉操作。Tensorflow 支持 CPU 和 GPU（特别是NVIDIA GPU），要使用支持 GPU 的 Tensorflow，请点击以下链接进行安装：

- FOR WINDOWS

    [https://www.tensorflow.org/install/install_windows](https://www.tensorflow.org/install/install_windows)

- FOR macOS

    [](https://www.tensorflow.org/install/install_mac)[https://www.tensorflow.org/install/install_mac](https://www.tensorflow.org/install/install_mac)

- FOR UBUNTU

    [](https://www.tensorflow.org/install/install_linux)[https://www.tensorflow.org/install/install_linux](https://www.tensorflow.org/install/install_linux)

## 样本应用

如果您需要用 **ImageAI** 进行演示，可以使用我们在Windows平台上基于 **ImageAI** 和 **Kivy** 构建的人工智能照片库 **IntelliP** 。点击此 [链接](https://github.com/OlafenwaMoses/IntelliP)下载应用程序页面及其源代码。

我们也欢迎您提交任何基于ImageAI构建的应用程序和系统。如果你需要获得开发者的支持，你可以通过下面的联系方式联系开发者。

## AI实践建议 

对于任何有兴趣建立人工智能系统并将其用于商业，经济，社会和研究目的的人来说，至关重要的是该人员知道这些技术的使用所产生的积极和消极影响。他们还需要听从那些具有经验丰富的行业专家们的实践建议和方法，以确保人工智能的每次使用都为人类带来整体利益。因此，我们建议所有希望使用ImageAI和其他人工智能工具的人阅读微软2018年1月出版的题为“未来计算：人工智能及其在社会中的作用”的出版物。请点击以下链接下载该出版物。

[https://blogs.microsoft.com/blog/2018/01/17/future-computed-artificial-intelligence-role-society](https://blogs.microsoft.com/blog/2018/01/17/future-computed-artificial-intelligence-role-society/)

## 联系开发者

**Moses Olafenwa**

_Email:_ [guymodscientist@gmail.com](mailto:guymodscientist@gmail.com)

_Website:_ [https://moses.specpal.science](https://moses.specpal.science)

_Twitter:_ [@OlafenwaMoses](https://twitter.com/OlafenwaMoses)

_Medium :_ [@guymodscientist](https://medium.com/@guymodscientist)

_Facebook :_ [moses.olafenwa](https://facebook.com/moses.olafenwa)

**John Olafenwa**

_Email:_ [johnolafenwa@gmail.com](mailto:johnolafenwa@gmail.com)

_Website:_ [https://john.specpal.science](https://john.specpal.science)

_Twitter:_ [@johnolafenwa](https://twitter.com/johnolafenwa)

_Medium :_ [@johnolafenwa](https://medium.com/@johnolafenwa)

_Facebook :_ [olafenwajohn](https://facebook.com/olafenwajohn)

## 参考文献 

1.  Somshubra Majumdar, DenseNet Implementation of the paper, Densely Connected Convolutional Networks in Keras

    [https://github.com/titu1994/DenseNet/](https://github.com/titu1994/DenseNet/)

2.  Broad Institute of MIT and Harvard, Keras package for deep residual networks

    [https://github.com/broadinstitute/keras-resnet](https://github.com/broadinstitute/keras-resnet)

3.  Fizyr, Keras implementation of RetinaNet object detection

    [https://github.com/fizyr/keras-retinanet](https://github.com/fizyr/keras-retinanet)

4.  Francois Chollet, Keras code and weights files for popular deeplearning models

    [https://github.com/fchollet/deep-learning-models](https://github.com/fchollet/deep-learning-models)

5.  Forrest N. et al, SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size

    [https://arxiv.org/abs/1602.07360](https://arxiv.org/abs/1602.07360)

6.  Kaiming H. et al, Deep Residual Learning for Image Recognition

    [https://arxiv.org/abs/1512.03385](https://arxiv.org/abs/1512.03385)

7.  Szegedy. et al, Rethinking the Inception Architecture for Computer Vision

    [https://arxiv.org/abs/1512.00567](https://arxiv.org/abs/1512.00567)

8.  Gao. et al, Densely Connected Convolutional Networks

    [https://arxiv.org/abs/1608.06993](https://arxiv.org/abs/1608.06993)

9.  Tsung-Yi. et al, Focal Loss for Dense Object Detection

    [https://arxiv.org/abs/1708.02002](https://arxiv.org/abs/1708.02002)

10.  O Russakovsky et al, ImageNet Large Scale Visual Recognition Challenge
 
    [https://arxiv.org/abs/1409.0575](https://arxiv.org/abs/1409.0575)

11.  TY Lin et al, Microsoft COCO: Common Objects in Context
    [https://arxiv.org/abs/1405.0312](https://arxiv.org/abs/1405.0312)

12.  Moses & John Olafenwa, A collection of images of identifiable professionals.
    [https://github.com/OlafenwaMoses/IdenProf](https://github.com/OlafenwaMoses/IdenProf)

# ImageAI：自定义图像预测

**AI共享**项目[https://commons.specpal.science](https://commons.specpal.science)

* * *

ImageAI提供4种不同的算法和模型，使你可以使用您自定义的模型执行自定义图像预测。您将能够使用**ImageAI**已训练的模型和相应的model_class JSON文件来预测您已训练模型的自定义对象。在这个例子中，我们将使用在**IdenProf**上进行20次实验训练的模型，**IdenProf**是统一专业人员的数据集，在测试数据集上达到65.17％的准确度（您可以使用自己训练的模型并生成JSON文件。此"CLASS"主要的目的是使你可以使用自己定制的模型。）下载以下链接中的ResNet模型和JSON文件：
**- [ResNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0.1/resnet_model_ex-020_acc-0.651714.h5)**（文件大小= 90.4MB）
**- [IdenProf model_class.json file](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0.1/model_class.json)**
很好！下载此模型文件和JSON文件后，启动一个新的python项目，然后将模型文件和JSON文件复制到python文件（.py文件）所在的项目文件夹中。下载下面的图像，或者在您的计算机上拍摄任何包含以下任何专业人员（厨师，医生，工程师 ，农民，消防员，法官，机械师，飞行员，警察和服务员）的图像，并将其复制到您的python项目文件夹中。然后创建一个python文件并为其命名; 以下是一个例子**FirstCustomPrediction.py**。然后将下面的代码写入python文件：

### **FirstCustomPrediction.py**

```
from imageai.Prediction.Custom import CustomImagePrediction
import os
execution_path = os.getcwd()


prediction = CustomImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath(os.path.join(execution_path, "resnet_model_ex-020_acc-0.651714.h5"))
prediction.setJsonPath(os.path.join(execution_path, "model_class.json"))
prediction.loadModel(num_objects=10)


predictions, probabilities = prediction.predictImage(os.path.join(execution_path, "4.jpg"), result_count=5)


for eachPrediction, eachProbability in zip(predictions, probabilities):
print(eachPrediction + " : " + eachProbability)
```

示例结果：

[![](https://github.com/kangvcar/ImageAI/raw/master/images/4.jpg)](/kangvcar/ImageAI/blob/master/images/4.jpg)

```
mechanic : 76.82620286941528
chef : 10.106072574853897
waiter : 4.036874696612358
police : 2.6663416996598244
pilot : 2.239348366856575
```

上面的代码如下：

```
from imageai.Prediction.Custom import CustomImagePrediction
import os
```

上面的代码导入**ImageAI**库以进行自定义图像预测和python **os**类。

```
execution_path = os.getcwd()
```

上面的行获取包含python文件（在本例中为FirstCustomPrediction.py）的文件夹的路径。

```
prediction = CustomImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath(os.path.join(execution_path, "resnet_model_ex-020_acc-0.651714.h5"))
prediction.setJsonPath(os.path.join(execution_path, "model_class.json"))
prediction.loadModel(num_objects=10)
```

在上面的行中，我们在第一行创建了`CustomImagePrediction()`类的实例，我们然后通过 在第二行中调用`.setModelTypeAsResNet()`来将预测对象的模型类型设置为ResNet  ，在第三行总我们设置模型路径为（**resnet_model_ex-020_acc-0.651714.h5**）文件所在的路径，第四行我们设置JSON文件（** model_class.json**）的路径，第五行我们加载模型和解析模型中可以预测的对象数。

```
predictions, probabilities = prediction.predictImage(os.path.join(execution_path, "4.jpg"), result_count=5)
```

在上面的行中，我们定义了两个变量，它们由`.predictImage()`函数返回，我们在其中解析了图像的路径，并说明了我们想要的预测结果的数量。`result_count=5`(可选值为1到10)。`.predictImage()`函数将返回预测的对象名和相应的百分比概率（**percentage_probabilities**）。

```
for eachPrediction, eachProbability in zip(predictions, probabilities):
    print(eachPrediction + " : " + eachProbability)
```

上面的行获取了**predictions**数组中的每个对象，并从**percentage_probabilities中**获得了相应的百分比概率，最后将两者的结果打印到控制台。

**CustomImagePrediction**类还支持**ImagePrediction**类中包含的多个预测，输入类型和预测速度。点击此[链接](/kangvcar/ImageAI/blob/master/imageai/Prediction/README.md)查看所有详细信息。

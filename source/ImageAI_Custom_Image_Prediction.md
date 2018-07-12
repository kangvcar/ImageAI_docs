# ImageAI：自定义图像预测

**AI共享**项目[https://commons.specpal.science](https://commons.specpal.science)

* * *

**ImageAI** 提供4种不同的算法和模型，使你可以用您自定义的模型执行图像预测。您将使用 **ImageAI** 已训练的模型和相应的 JSON 文件来预测自定义对象。在这个例子中，我们将使用在**IdenProf**上进行20次实验训练出的模型，**IdenProf**是一个专业人员的数据集，在测试数据集上达到65.17％的准确度（您可以使用自己训练的模型并生成JSON文件。此"CLASS"主要的目的是使你可以使用自己训练的模型。）通过以下链接下载ResNet模型和JSON文件：

**- [ResNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0.1/resnet_model_ex-020_acc-0.651714.h5)（文件大小= 90.4MB）**<br/>
**- [IdenProf model_class.json file](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0.1/model_class.json)**<br/>

很好！下载模型文件和JSON文件后，启动一个新的python项目，然后将模型文件和JSON文件复制到python文件（.py文件）所在的项目文件夹中。下载下面的图像，或者在您的计算机上拍摄任何包含以下专业人员（厨师，医生，工程师 ，农民，消防员，法官，机械师，飞行员，警察和服务员）的图像，并将其复制到您的python项目文件夹中。然后创建一个python文件并为其命名; 例如**FirstCustomPrediction.py**。然后将下面的代码写入python文件中：

### FirstCustomPrediction.py

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

[![](https://github.com/kangvcar/ImageAI/raw/master/images/4.jpg)](https://github.com/kangvcar/ImageAI/blob/master/images/4.jpg)

```
mechanic : 76.82620286941528
chef : 10.106072574853897
waiter : 4.036874696612358
police : 2.6663416996598244
pilot : 2.239348366856575
```

让我们对示例代码进行解读：
```
from imageai.Prediction.Custom import CustomImagePrediction
import os
```

上面的代码导入了**ImageAI**库的`CustomImagePrediction`类和Python `os`类。

```
execution_path = os.getcwd()
```

上面的代码获取包含python文件的文件夹路径（在本例中python文件为FirstCustomPrediction.py）

```
prediction = CustomImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath(os.path.join(execution_path, "resnet_model_ex-020_acc-0.651714.h5"))
prediction.setJsonPath(os.path.join(execution_path, "model_class.json"))
prediction.loadModel(num_objects=10)
```

在上面的代码中，我们在第一行我们对`CustomImagePrediction`类进行了实例化，第二行调用了`.setModelTypeAsResNet()`函数将预测对象的模型类型设置为ResNet，，第三行设置了模型文件（**resnet_model_ex-020_acc-0.651714.h5**）的路径，第四行设置JSON文件（** model_class.json**）的路径，第五行载入模型并设置需要预测的对象数。

```
predictions, probabilities = prediction.predictImage(os.path.join(execution_path, "4.jpg"), result_count=5)
```

在上面的代码中，我们定义了两个变量，他们的值将由所调用的函数`predictImage()`返回，其中`predictImage()`函数接受了两个参数，一个是指定要进行图像预测的图像文件路径，另一个参数`result_count`用于设置我们想要预测结果的数量（该参数的值可选1 to 100）。最后，`predictImage()`函数将返回预测的对象名和相应的百分比概率（`percentage_probabilities`）。

```
for eachPrediction, eachProbability in zip(predictions, probabilities):
    print(eachPrediction + " : " + eachProbability)
```

在上面的代码获取了`predictions`变量中的每个对象名，并从`probabilities`变量中获取相应的百分比概率，最后将两者的结果打印到终端。

**CustomImagePrediction**类还支持**ImagePrediction**类中包含的多图像预测，输入类型和预测速度功能。点击此[链接](https://imageai-cn.readthedocs.io/zh_CN/latest/ImageAI_Image_Prediction.html)查看详细介绍。
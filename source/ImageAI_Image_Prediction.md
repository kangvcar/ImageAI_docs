# ImageAI：图像预测

**AI共享**项目[https://commons.specpal.science](https://commons.specpal.science)

* * *

ImageAI 提供4种不同的算法及模型来执行图像预测，通过以下简单几个步骤即可对任何图片执行图像预测。提供用于图像预测的4种算法包括 **SqueezeNet**，**ResNet**，**InceptionV3** 和 **DenseNet**。这些算法中的每一个都有单独的模型文件，您必须根据所选算法使用相对应的模型文件，请单击以下链接下载所选算法的模型文件：

**- [SqueezeNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/squeezenet_weights_tf_dim_ordering_tf_kernels.h5)（文件大小：4.82 MB，预测时间最短，精准度适中）**

**-[](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_weights_tf_dim_ordering_tf_kernels.h5)** **[ResNet50](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_weights_tf_dim_ordering_tf_kernels.h5)** by Microsoft Research **（文件大小：98 MB，预测时间较快，精准度高）**

**-[](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/inception_v3_weights_tf_dim_ordering_tf_kernels.h5)**  **[InceptionV3](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/inception_v3_weights_tf_dim_ordering_tf_kernels.h5)** by Google Brain team **（文件大小：91.6 MB，预测时间慢，精度更高）**

**-[](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/DenseNet-BC-121-32.h5)** **[DenseNet121](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/DenseNet-BC-121-32.h5)** by Facebook AI Research **（文件大小：31.6 MB，预测时间较慢，精度最高）**

很好！下载对应模型文件后，启动一个新的python项目，然后将模型文件复制到python文件（.py文件）所在的项目文件夹中。下载下面的[图像](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/1.jpg)，或者您所拍摄的任何图像并将其复制到python项目的文件夹中。然后创建一个python文件并为其命名; 例如**FirstPrediction.py**。然后将下面的代码写入python文件中：

### FirstPrediction.py

```
from imageai.Prediction import ImagePrediction
import os
execution_path = os.getcwd()

prediction = ImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath(os.path.join(execution_path, "resnet50_weights_tf_dim_ordering_tf_kernels.h5"))
prediction.loadModel()

predictions, probabilities = prediction.predictImage(os.path.join(execution_path, "1.jpg"), result_count=5 )
for eachPrediction, eachProbability in zip(predictions, probabilities):
    print(eachPrediction + " : " + eachProbability)
```

示例结果：
[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/1.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/1.jpg)

```
convertible : 52.459555864334106
sports_car : 37.61284649372101
pickup : 3.1751200556755066
car_wheel : 1.817505806684494
minivan : 1.7487050965428352
```

我们对示例代码进行分析：
```
from imageai.Prediction import ImagePrediction
import os 
```
上面的代码导入了**ImageAI**库和 python **os** 类。
```
execution_path = os.getcwd()
```
上面的代码获取包含python文件的文件夹路径（在本例中python文件为`FirstPrediction.py`）。
```
prediction = ImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath(os.path.join(execution_path, "resnet50_weights_tf_dim_ordering_tf_kernels.h5"))
```

在上面的代码中，我们对`ImagePrediction()`类进行了实例化，然后调用了`.setModelTypeAsResNet()`函数将预测对象的模型类型设置为ResNet，并在第三行设置了模型文件（**resnet50_weights_tf_dim_ordering_tf_kernels.h5**）的路径。

```
predictions, probabilities = prediction.predictImage(os.path.join(execution_path, "1.jpg"), result_count=5 )
```

在上面的代码中，我们定义了两个变量，他们的值将由所调用的函数`predictImage()`返回，其中`predictImage()`函数接受了两个参数，一个是指定要进行图像预测的图像文件路径，另一个参数`result_count`用于设置我们想要预测结果的数量（该参数的值可选1 to 100）。最后，`predictImage()`函数将返回预测的对象名和相应的百分比概率（`percentage_probabilities`）。

```
for eachPrediction, eachProbability in zip(predictions, probabilities):
    print(eachPrediction + " : " + eachProbability)
```

在上面的代码获取了`predictions`变量中的每个对象名，并从`probabilities`变量中获取相应的百分比概率，最后将两者的结果打印到终端。

### 多图像预测

您可以多次调用`.predictImage()`函数来对多张图像进行预测，但是我们提供了一个简单的方法，您只需调用一次`.predictMultipleImages()`函数即可对多张图像进行预测。它的工作原理如下：

- 定义普通的`ImagePrediction`实例
- 通过`.setModelTypeAsResNet()`设置模型类型和`.setModelPath()`模型路径
- 调用`.loadModel()`函数载入模型
- 创建一个数组并将所有要预测的图像的路径添加到数组中。
- 然后通过调用`.predictMultipleImages()`函数解析包含图像路径的数组并执行图像预测，通过解析`result_count_per_image`（默认值为2）的值来设定每个图像需要预测多少种可能的结果

以下是示例代码：

```
from imageai.Prediction import ImagePrediction
import os

execution_path = os.getcwd()

multiple_prediction = ImagePrediction()
multiple_prediction.setModelTypeAsResNet()
multiple_prediction.setModelPath(os.path.join(execution_path, "resnet50_weights_tf_dim_ordering_tf_kernels.h5"))
multiple_prediction.loadModel()

all_images_array = []

all_files = os.listdir(execution_path)
for each_file in all_files:
    if(each_file.endswith(".jpg") or each_file.endswith(".png")):
        all_images_array.append(each_file)

results_array = multiple_prediction.predictMultipleImages(all_images_array, result_count_per_image=5)

for each_result in results_array:
    predictions, percentage_probabilities = each_result["predictions"], each_result["percentage_probabilities"]
    for index in range(len(predictions)):
        print(predictions[index] + " : " + percentage_probabilities[index])
    print("-----------------------")
```

在上面的代码中，`.predictMultipleImages()`函数将返回一个由dict组成的array，每个dict包含了每张图像所预测出的可能图像名list和相应的百分比概率list，并把所有可能的对象名和概率打印到终端。

示例结果：

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/1.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/1.jpg) 
[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/2.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/2.jpg) 
[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/3.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/3.jpg)

```
convertible : 52.459555864334106
sports_car : 37.61284649372101
pickup : 3.1751200556755066
car_wheel : 1.817505806684494
minivan : 1.7487050965428352
-----------------------
toilet_tissue : 13.99008333683014
jeep : 6.842949986457825
car_wheel : 6.71963095664978
seat_belt : 6.704962253570557
minivan : 5.861184373497963
-----------------------
bustard : 52.03368067741394
vulture : 20.936034619808197
crane : 10.620515048503876
kite : 10.20539253950119
white_stork : 1.6472270712256432
-----------------------
```

### 预测速度

**ImageAI** 为图像预测任务添加了预测速度调节功能，最多可使预测时间减少60％。可选的速度模式有`normal`(default), `fast`, `faster` , `fastest` 。您只需要在调用`loadModel()`函数是指定参数`prediction_speed`的值为你想要的速度模式即可，如下所示。

```
prediction.loadModel(prediction_speed="fast")
```

为了观察不同速度模式间的差异，请查看下面不同速度模式下相同图像预测所花费的时间。(实验环境 OS:Windows 8, CPU:Intel Celeron N2820 2.13GHz)

**_速度模式="normal"，花费时间= 5.9秒_**

```
convertible : 52.459555864334106
sports_car : 37.61284649372101
pickup : 3.1751200556755066
car_wheel : 1.817505806684494
minivan : 1.7487050965428352
-----------------------
toilet_tissue : 13.99008333683014
jeep : 6.842949986457825
car_wheel : 6.71963095664978
seat_belt : 6.704962253570557
minivan : 5.861184373497963
-----------------------
bustard : 52.03368067741394
vulture : 20.936034619808197
crane : 10.620515048503876
kite : 10.20539253950119
white_stork : 1.6472270712256432
-----------------------
```

**_速度模式="fast"，花费时间= 3.4秒_**

```
sports_car : 55.5136501789093
pickup : 19.860029220581055
convertible : 17.88402795791626
tow_truck : 2.357563190162182
car_wheel : 1.8646160140633583
-----------------------
drum : 12.241223454475403
toilet_tissue : 10.96322312951088
car_wheel : 10.776633024215698
dial_telephone : 9.840480983257294
toilet_seat : 8.989936858415604
-----------------------
vulture : 52.81011462211609
bustard : 45.628002285957336
kite : 0.8065823465585709
goose : 0.3629807382822037
crane : 0.21266008261591196
-----------------------
```

**_速度模式="faster"，花费时间= 2.7秒_**

```
sports_car : 79.90474104881287
tow_truck : 9.751049429178238
convertible : 7.056044787168503
racer : 1.8735893070697784
car_wheel : 0.7379394955933094
-----------------------
oil_filter : 73.52778315544128
jeep : 11.926891654729843
reflex_camera : 7.9965077340602875
Polaroid_camera : 0.9798810817301273
barbell : 0.8661789819598198
-----------------------
vulture : 93.00530552864075
bustard : 6.636220961809158
kite : 0.15161558985710144
bald_eagle : 0.10513027664273977
crane : 0.05982434959150851
-----------------------
```

**_速度模式="fastest"，花费时间= 2.2秒_**

```
tow_truck : 62.5033438205719
sports_car : 31.26143217086792
racer : 2.2139860317111015
fire_engine : 1.7813067883253098
ambulance : 0.8790366351604462
-----------------------
reflex_camera : 94.00787949562073
racer : 2.345871739089489
jeep : 1.6016140580177307
oil_filter : 1.4121259562671185
lens_cap : 0.1283118617720902
-----------------------
kite : 98.5377550125122
vulture : 0.7469987496733665
bustard : 0.36855682265013456
bald_eagle : 0.2437378279864788
great_grey_owl : 0.0699841941241175
-----------------------
```

### 请注意：

在需要调整速度模式时，最好使用具有更高精度的模型（DenseNet or InceptionV3），或者预测图像是标志性的（明显的）；这样更能确保预测的精准度。

### 图像输入类型

旧版本 **ImageAI** 图像预测功能仅支持指定图像文件路径的文件输入方式。新版本 **ImageAI** 图像预测功能支持3种输入类型，即 **file path to image file**（默认），**numpy array of image** 和 **image file stream**。这意味着您在进行图像预测是可以使用上述3种类型之一进行文件输入，并以上述3种类型之一进行文件输出。

要使用 **numpy array** 或 **file stream** 类型进行文件输入时，您只需要在`.predictImage()`或`.predictMultipleImages()`函数中声明`input_type`为`array`或`stream`即可，示例如下。

```
predictions, probabilities = prediction.predictImage(image_array, result_count=5 , input_type="array" ) # For numpy array input type
predictions, probabilities = prediction.predictImage(image_stream, result_count=5 , input_type="stream" ) # For file stream input type
```

### 多线程预测

当您在默认线程上开发像用户界面(UI)这样任务繁重的程序时，您应该考虑在新线程中运行图像预测。在新线程中使用ImageAI运行图像预测时注意以下事项：

- 您可以创建`ImagePrediction`类的实例，设置其模型类型`setModelTypeAsResNet()`和模型文件路径`setModelPath()`。
- `loadModel()` 和 `predictImage()` 函数必须在新线程中调用。

以下是使用多线程进行图像预测的示例代码：

```
from imageai.Prediction import ImagePrediction
import os
import threading
execution_path = os.getcwd()

prediction = ImagePrediction()
prediction.setModelTypeAsResNet()
prediction.setModelPath( os.path.join(execution_path, "resnet50_weights_tf_dim_ordering_tf_kernels.h5"))

picturesfolder = os.environ["USERPROFILE"] + "\Pictures\"
allfiles = os.listdir(picturesfolder)

class PredictionThread(threading.Thread):
    def init(self):
        threading.Thread.init(self)
    def run(self):
        prediction.loadModel()
    for eachPicture in allfiles:
        if eachPicture.endswith(".png") or eachPicture.endswith(".jpg"):
            predictions, probabilities = prediction.predictImage(picturesfolder + eachPicture, result_count=1)
            for prediction, percentage_probability in zip(predictions, probabilities):
                print(prediction + " : " + percentage_probability)

predictionThread = PredictionThread()
predictionThread.start()
```

### 文档

`imageai.Prediction.ImagePrediction` class

* * *

在任何的Python程序中通过实例化`ImagePrediction`类并调用下面的函数即进行图像预测：

- `setModelTypeAsSqueezeNet()`如果您选择使用 SqueezeNet 模型文件来预测图像，你只需调用一次该函数。
- `setModelTypeAsResNet()`如果您选择使用 ResNet 模型文件来预测图像，你只需调用一次该函数。
- `setModelTypeAsInceptionV3()`如果您选择使用 InceptionV3Net 模型文件来预测图像，你只需调用一次该函数。
- `setModelTypeAsDenseNet()`如果您选择使用 DenseNet 模型文件来预测图像，你只需调用一次该函数。
- `setModelPath()` 该函数用于设定模型文件的路径。模型文件必须与您设置的模型类型相对应。
- `loadModel()` 该函数用于载入模型，在调用`predictImage()`函数之前需要调用此函数一次。
该函数接收一个`prediction_speed`参数。该参数用于指定图像预测的速度模式，当速度模式设置为'fastest'时预测时间可缩短60%左右，具体取决于图像的质量。
    - ` prediction_speed`（可选）; 可接受的值是"normal", "fast", "faster" and "fastest" 

- `predictImage()` 此函数用于通过接收以下参数来预测指定图像：
    - `input_type`（可选），类型要解析的输入。可接受的值是“file”，“array”和“stream”
    - `image_input`，文件路径/ numpy数组/图像文件流的图像。
    - `result_count`（可选），要发送的预测数，必须是
1到1000之间的整数。默认值为5.

    此函数返回2个数组，即'prediction_results'和'prediction_probabilities'。'prediction_results'包含按百分比概率降序排列的可能对象类。'prediction_probabilities'包含每个对象类的概率百分比。
    数组中每个对象类的位置对应于'prediction_probabilities' 数组中百分比可能性。

    _**：param input_type:**_

    _**：param image_input:**_

    _**：param result_count:**_

    _**：return prediction_results，prediction_probabilities:**_

- `predictMultipleImages()`此函数用于通过接收以下参数来预测多个图像：
    - input_type，解析数组中包含的输入类型。可接受的值是“file”，“array”和“stream”
    - sent_images_array，图像文件路径数组，图像numpy数组或图像文件流
    - result_count_per_image（可选），数量每个图像要发送的预测，必须是1到1000之间的整数。默认值为2。

    此函数返回一个字典数组，每个字典包含2个数组，即'prediction_results'和'prediction_probabilities'。'prediction_results'包含按其百分比概率降序排列的可能对象类。'prediction_probabilities'包含每个对象类的概率百分比。'prediction_results'数组中每个对象类的位置对应于'prediction_probabilities'数组中百分比可能性。

    **_:param input_type:_**

    **_:param sent_images_array:_**

    **_:param result_count_per_image:_**

    **_:return output_array:_**
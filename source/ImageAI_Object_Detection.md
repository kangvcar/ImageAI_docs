# [](#imageai--object-detection-)ImageAI：对象检测

**AI共享**项目[https://commons.specpal.science](https://commons.specpal.science)

* * *

ImageAI 提供了非常方便和强大的方法来对图像执行对象检测并从图像中提取每个对象。目前仅支持当前最先进的 RetinaNet 算法进行对象检测，后续版本会加入对其他算法的支持。在开始对象检测任务前，您必须通过以下链接下载 RetinaNet 模型文件：

**- [RetinaNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5)** **（文件大小= 145 MB）**

下载 RetinaNet 模型文件后，应将模型文件复制到`.py`文件所在的项目文件夹中。然后创建一个python文件并为其命名; 例如 **FirstObjectDetection.py** 。然后将下面的代码写入python文件中：

### FirstObjectDetection.py
```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()

detector = ObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()
detections = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image2.jpg"), output_image_path=os.path.join(execution_path , "image2new.jpg"))

for eachObject in detections:
    print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
    print("--------------------------------")
```

示例结果：

 **_输入图像_** 

 [![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image2.jpg)

 **_输出图像_** 

 [![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image2new.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image2new.jpg)

```
person : 91.946941614151
--------------------------------
person : 73.61021637916565
--------------------------------
laptop : 90.24320840835571
--------------------------------
laptop : 73.6881673336029
--------------------------------
laptop : 95.16398310661316
--------------------------------
person : 87.10319399833679
--------------------------------
```

让我们对示例中的对象检测代码进行解读：

```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()
```

在上面的代码中，我们在第一行导入`ImageAI Object Detection`类，在第二行导入**os**库，在第三行获得当前python文件的文件夹的路径。 
```
detector = ObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()
```
在上面的代码中，我们在第一行创建了`ObjectDetection`类的新实例，在第二行中将模型类型设置为RetinaNet，第三行将模型路径设置为我们下载的RetinaNet模型文件所在文件夹的路径，第四行载入模型。
```
detections = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image2.jpg"), output_image_path=os.path.join(execution_path , "image2new.jpg"))

for eachObject in detections:
    print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
print("--------------------------------")
```

在上面的代码中，我们调用了`detectObjectsFromImage()`函数并传入`input_image`参数和`output_image_path`参数来指定输入文件和输出文件的路径。然后该函数返回一个字典数组，每个字典包含图像中检测到的对象信息，字典中的对象信息有`name`（对象类名）和 `percentage_probability`（概率）

### 物体检测，提取和微调

在我们上面使用的示例中，它的工作原理是将检测到的对象返回到数组中，然后利用数组中的数据在每个对象上绘制矩形标记来生成新图像。在下面的示例中，我们将从输入图像中提取每个检测到的对象并保存到文件夹中。

在下面的示例代码中，它与先前的对象检测代码非常相似，我们将检测到的每个对象保存为单独的图像。

```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()

detector = ObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()

detections, objects_path = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3new.jpg"), extract_detected_objects=True)

for eachObject, eachObjectPath in zip(detections, objects_path):
    print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
    print("Object's image saved in " + eachObjectPath)
    print("--------------------------------")
```

示例结果：

 **_输入图像_** 

 [![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3.jpg)

 **_输出图像_** 

 [![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg)

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-1.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-1.jpg)<br/>
_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-2.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-2.jpg)<br/>
_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-3.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-3.jpg)<br/>
_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-4.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-4.jpg)<br/>
_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/motorcycle-5.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/motorcycle-5.jpg)<br/>
_摩托车_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/dog-6.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/dog-6.jpg)<br/>
_狗_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/car-7.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/car-7.jpg)<br/>
_汽车_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-8.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-8.jpg)<br/>
_人_

让我们回顾一下执行对象检测并提取图像的代码部分：

```
detections, objects_path = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3new.jpg"), extract_detected_objects=True)

for eachObject, eachObjectPath in zip(detections, objects_path):
print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
print("Object's image saved in " + eachObjectPath)
print("--------------------------------")
```
在上面的代码中，我们在调用`detectObjectsFromImage()`函数时新增了`extract_detected_objects=True`参数；此参数的作用是将每个检测到的对象提取并保存为当单独的图像。默认情况下该参数的值为`false`；设置为`true`后，该函数将创建一个名为`output_image_path+"-objects"`的目录，然后它将所有提取的图像以`detected object name+"-number"`命名保存到这个新目录中。

我们添加的这个新参数（`extract_detected_objects=True`）将会把检测到的对象提取并保存为单独的图像；这将使函数返回2个值，第一个是字典数组，每个字典对应一个检测到的对象信息（包含对象类名和概率），第二个是所有提取出对象的图像保存路径，并且它们按照对象在第一个数组中的顺序排列。

### 您需要知道的一个重要功能！

您是否还记得`detectObjectsFromImage()`函数返回的信息中包每个检测到的对象的百分比概率。该函数有一个重要的参数 `minimum_percentage_probability`，该参数用于设定预测概率的阈值，其默认值为**50**（范围在0-100之间）。如果保持默认值，这意味着只有当百分比概率**大于等于50**时，该函数才会返回检测到的对象。使用默认值可以确保检测结果的完整性，但是在检测过程中可能会跳过许多对象。因此，您可以通过将`minimum_percentage_probability`设置更小的值以检测更多的对象或更高的值来检测更少的对象。

### 自定义对象检测

对象检测模型（由**RetinaNet**支持）**ImageAI** 可以检测80种不同类型的对象。他们包括：

```
      person,   bicycle,   car,   motorcycle,   airplane,
          bus,   train,   truck,   boat,   traffic light,   fire hydrant,   stop_sign,
          parking meter,   bench,   bird,   cat,   dog,   horse,   sheep,   cow,   elephant,   bear,   zebra,
          giraffe,   backpack,   umbrella,   handbag,   tie,   suitcase,   frisbee,   skis,   snowboard,
          sports ball,   kite,   baseball bat,   baseball glove,   skateboard,   surfboard,   tennis racket,
          bottle,   wine glass,   cup,   fork,   knife,   spoon,   bowl,   banana,   apple,   sandwich,   orange,
          broccoli,   carrot,   hot dog,   pizza,   donot,   cake,   chair,   couch,   potted plant,   bed,
          dining table,   toilet,   tv,   laptop,   mouse,   remote,   keyboard,   cell phone,   microwave,
          oven,   toaster,   sink,   refrigerator,   book,   clock,   vase,   scissors,   teddy bear,   hair dryer,
          toothbrush.
```

有趣的是，**ImageAI** 允许您同时对上面的多个对象执行检测。这意味着您可以自定义要在图像中检测的对象。我们来看看下面的代码：

```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()

detector = ObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()

custom_objects = detector.CustomObjects(car=True, motorcycle=True)
detections = detector.detectCustomObjectsFromImage(custom_objects=custom_objects, input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3custom.jpg"))

for eachObject in detections:
print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
print("--------------------------------")
```

结果：

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3custom.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3custom.jpg)

让我们看一下使这成为可能的代码：

```
custom_objects = detector.CustomObjects(car=True, motorcycle=True)
detections = detector.detectCustomObjectsFromImage(custom_objects=custom_objects, input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3custom.jpg"))
```

在上面的代码中，我们定义了一个新变量`custom_objects = detector.CustomObjects()`，其中我们将其car和motorcycle属性设置为`True`，这是为了告诉模型只检测我们设置为True的对象。然后我们调用`detector.detectCustomObjectsFromImage()`函数并传入了我们定义的变量`custom_objects`来指定我们需要从图像中识别的对象。

### 检测速度

**ImageAI** 为对象检测任务添加了速度调节参数`detection_speed`，结合`minimum_percentage_probability`参数使用效果更佳，最多可使检测时间缩短60％且几乎不影响检测结果的精准度。可用的检测速度是 "normal"(default), "fast", "faster" , "fastest" and "flash"。您只要在加载模型时说明您想要的速度模式，如下所示。

```
detector.loadModel(detection_speed="fast")
```

为了观察不同速度模式间的差异，请查看下面不同速度模式下（结合调整`minimum_percentage_probability`参数）检测相同图像所花费的时间。(实验环境 OS:Windows 8, CPU:Intel Celeron N2820 2.13GHz)：

**_检测速度="normal"，最小百分比概率= 50（默认值），检测时间= 63.5秒_**

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/5normal.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/5normal.jpg)

**_检测速度="fast"，最小百分比概率= 40（默认值），检测时间= 20.8秒_**

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/5fast.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/5fast.jpg)

**_检测速度="faster"，最小百分比概率= 30（默认值），检测时间= 11.2秒_**

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/5faster.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/5faster.jpg)

**_检测速度="fastest"，最小百分比概率= 30（默认值），检测时间= 7.6秒_**

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/5fastest.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/5fastest.jpg)

**_检测速度="flash"，最小百分比概率= 10（默认值），检测时间= 3.67秒_**

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/5flash.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/5flash.jpg)

您会注意到，在"flash"检测速度下，检测速度最快但准确度最低，那是因为图片中没有任何标志性的对象。在输入图像是代表性的（包含一个标志性图像）的时候，"flash"检测速度是最适合的。请查看下面的示例，以便了解何为标志性（代表性）图像下的检测：

**_检测速度="flash"，最小百分比概率= 30（默认值），检测时间= 3.85秒_** 
[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/6flash.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/6flash.jpg)

### 图像输入和输出类型

旧版本 **ImageAI** 对象检测功能仅支持指定图像文件路径的图像输入方式。新版本 **ImageAI** 对象检测功能支持3种输入类型，即 **file path to image file**（默认），**numpy array of image** 和 **image file stream**；以及2种输出类型，即 **image file**默认） 和 **numpy array of image**  这意味着您在进行对象检测时可以使用上述格式返回文件到系统中。

要使用 **numpy array** 或 **file stream** 类型进行图像输入时，您只需要在`.detectObjectsFromImage()`或`.detectCustomObjectsFromImage()`函数中声明`input_type`参数为`array`或`stream`即可，示例如下：

```
detections = detector.detectObjectsFromImage(input_type="array", input_image=image_array , output_image_path=os.path.join(execution_path , "image.jpg")) # For numpy array input type
detections = detector.detectObjectsFromImage(input_type="stream", input_image=image_stream , output_image_path=os.path.join(execution_path , "test2new.jpg")) # For file stream input type
```

要使用 **numpy array** 输出格式时，您只需要在`.detectObjectsFromImage()`或`.detectCustomObjectsFromImage()`函数中声明`input_type`参数为`array`即可，示例如下：
```
detected_image_array, detections = detector.detectObjectsFromImage(output_type="array", input_image="image.jpg" ) # For numpy array output type
```

### 文档

`imageai.Detection.ObjectDetection` class

* * *

在任何的Python程序中通过实例化`ObjectDetection`类并调用下面的函数即可进行对象检测：

- `setModelTypeAsRetinaNet()` 如果您选择使用RetinaNet 模型文件来进行对象检测，你只需调用一次该函数。
- `setModelPath()` 该函数用于设定模型文件的路径。模型文件必须与您设置的模型类型相对应。
- `loadModel()` 该函数用于载入模型。该函数接收一个`prediction_speed`参数。该参数用于指定对象检测的速度模式，当速度模式设置为'fastest'时预测时间可缩短60%左右，具体取决于图像的质量。
    - `detection_speed`（可选）; 可接受的值是"normal", "fast", "faster" and "fastest" 
- `detectObjectsFromImage()` 此函数用于通过接收以下参数来进行对象检测：
    - `input_image` 该参数用于指定输入图像的 file path/numpy array/image file stream 。
    - `output_image_path` 如果`output_type=file`，该参数用于指定输出（包含检测框和标签）图像的文件路径，。
    - `input_type`（可选），指定需要解析的输入类型。可接受的值是"file", "array" and "stream" 。
    - `output_type`（可选），指定需要输出的类型。可接受的值是"file", "array"
    - `extract_detected_objects`（可选，默认为 false），用于将每个检测到的对象单独保存为图像并返回每个图像路径的数组。
    - `minimum_percentage_probability`（可选，默认为50），用于设定预测概率的阈值，只有当百分比概率大于等于该值时才会返回检测到的对象。 

此函数返回的值取决于解析的参数。可返回的值如下所示
- 如果`extract_detected_objects = False`或其默认值和`output_type ='file'`或其
默认值，则必须将`output_image_path`解析为输出检测结果的路径，该函数将返回：
    1. 一个字典数组，每个字典对应于在图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability

- 如果`extract_detected_objects = False`或其默认值并且`output_type ='array'`，则该函数将返回：
    1. 检测到的图像的numpy数组
    2. 字典数组，每个字典对应于图像中检测到的对象。每个字典都包含以下属性：
        + name
        + percentage_probability

- 如果`extract_detected_objects = True`且`output_type ='file'`或
在默认值，您必须将`output_image_path`解析为输出检测结果的路径，该函数将返回：
    1. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability
    2. 一个字符串数组，包含了从图像中提取的每个对象的图像所保存的路径

- 如果`extract_detected_objects = True`且`output_type ='array'`，则该函数将返回：
    1. 检测到的图像的numpy数组
    2. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability
    3. 图像中检测到的每个对象的numpy数组

_:param input_image:_<br/>
_:param output_image_path:_<br/>
_:param input_type:_<br/>
_:param output_type:_<br/>
_:param extract_detected_objects:_<br/>
_:param minimum_percentage_probability:_<br/>
_:return output_objects_array:_<br/>
_:return detected_copy:_<br/>
_:return detected_detected_objects_image_array:_<br/>

- `CustomObjecs()` 调用此函数来选择需要在图像中进行检测的对象。这些对象在函数变量中预先创建并定义为 `False`。您可以将任何预创建的对象设置为`true`，最终此函数将返回一个字典被`detectCustomObjectsFromImage()`函数的一个参数`custom_objects`所使用。

- `detectCustomObjectsFromImage` 此函数通过接收以下参数在图像中对指定对象进行检测：
    - `custom_objects`，一个`CustomObject`类的实例，用于指定在图像中需要检测的对象
    - `input_image`，可以将文件转换为path，image numpy array或image file stream
    - `output_image_path`，包含检测框和标签的输出图像的文件路径，如果`output_type ="file"`
    - `input_type`（可选），file path/numpy array/image file stream of the image. 可选值为 "file" and "array" 
    - `output_type`（可选）， file path/numpy array/image file stream of the image. 可选值为 "file" and "array" 
    - `extract_detected_objects`（可选，默认为False），选项可以将每个检测到的对象单独保存为图像，并返回对象图像路径的数组。
    - `minimum_percentage_probability`（可选，默认为50），用于设置指定检测到的输出对象的最小百分比概率的选项。

此函数返回的值取决于解析的参数。可返回的可能值如下所示

- 如果`extract_detected_objects = False`或其默认值并且`output_type ='file'`或
在默认值下，您必须将`output_image_path`解析为您想要
保存检测到的图像的路径的字符串。然后该函数将返回：

    1. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        - name
        - percentage_probability

- 如果`extract_detected_objects = False`或其默认值并且`output_type ='array'`，则该函数将返回：
    1. 检测到的图像的numpy数组
    2. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability

- 如果`extract_detected_objects = True`且`output_type ='file'`或其
默认值，则必须将`output_image_path`解析为要保存检测到的图像的路径的字符串。然后该函数将返回：
    1. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典都包含以下属性：
        + name
        + percentage_probability
    2. 从图像中提取的每个对象的图像的字符串路径数组

- 如果`extract_detected_objects = True`且`output_type ='array'`，则该函数将返回：
    1. 检测到的图像的numpy数组
    2. 字典数组，每个字典对应于图像中检测到的对象。每个字典都包含以下属性：
        + name
        + percentage_probability
    3. 图像中检测到的每个对象的numpy数组

_:param input_image:_<br/>
_:param output_image_path:_<br/>
_:param input_type:_<br/>
_:param output_type:_<br/>
_:param extract_detected_objects:_<br/>
_:param minimum_percentage_probability:_<br/>
_:return output_objects_array:_<br/>
_:return detected_copy:_<br/>
_:return detected_detected_objects_image_array:_<br/>

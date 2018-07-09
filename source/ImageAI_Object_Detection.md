# [](#imageai--object-detection-)ImageAI：对象检测

**AI共享**项目[https://commons.specpal.science](https://commons.specpal.science)

* * *

### [](#table-of-contents)**目录**

- [第一个物体检测](#firstdetection)
- [物体检测，提取和微调](#objectextraction)
- [自定义物体检测](#customdetection)
- [检测速度](#detectionspeed)
- [图像输入和输出类型](#inputoutputtype)
- [文档](#documentation)

ImageAI提供了非常方便和强大的方法来对图像执行物体检测并从图像中提取每个物体。提供的对象检测类仅支持当前最先进的RetinaNet，而在最近的将来将支持其他对象检测网络。要开始执行对象检测，您必须通过以下链接下载RetinaNet模型文件：

**- [RetinaNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5)** **（文件大小= 145 MB）**

下载RetinaNet模型文件后，应将模型文件复制到.py文件所在的项目文件夹中。然后创建一个python文件并为其命名; 以下是一个例子FirstObjectDetection.py。然后将下面的代码写入python文件：

### [](#firstobjectdetectionpy)**FirstObjectDetection.py**
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

让我们对上面使用的对象检测代码进行解读。

```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()
```

在上面的3行中，我们在第一行导入**ImageAI Object Detection**类，在第二行导入**os**并获得运行python文件的文件夹的路径。 
```
detector = ObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()
```
在上面的4行中，我们在第一行创建了**ObjectDetection**类的新实例，在第二行中将模型类型设置为RetinaNet，第三行将模型路径设置为我们下载的RetinaNet模型文件并复制到python文件夹中的路径，第四行加载模型。
```
detections = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image2.jpg"), output_image_path=os.path.join(execution_path , "image2new.jpg"))

for eachObject in detections:
    print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
print("--------------------------------")
```

在上面的2行中，我们运行了`detectObjectsFromImage()`函数并解析了我们图像的路径，以及该函数将保存的新图像的路径。然后该函数返回一个字典数组，每个字典对应于图像中检测到的对象。每个字典都有属性**名称**（对象名称）和 **percentage_probability**（检测概率百分比）

### [](#object-detection-extraction-and-fine-tune)**物体检测，提取和微调**

在我们上面使用的示例中，我们在图像上运行对象检测，它将检测到的对象返回到数组中，并保存在每个对象上绘制矩形标记的新图像。在下面的示例中，我们将能够从输入图像中提取每个对象并单独保存。

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

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-1.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-1.jpg)

_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-2.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-2.jpg)

_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-3.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-3.jpg)

_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-4.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-4.jpg)

_人_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/motorcycle-5.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/motorcycle-5.jpg)

_摩托车_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/dog-6.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/dog-6.jpg)

_狗_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/car-7.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/car-7.jpg)

_汽车_ 

[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/image3new.jpg-objects/person-8.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/image3new.jpg-objects/person-8.jpg)

_人_

让我们回顾一下执行对象检测并提取图像的代码部分：

```
detections, objects_path = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3new.jpg"), extract_detected_objects=True)

for eachObject, eachObjectPath in zip(detections, objects_path):
print(eachObject["name"] + " : " + eachObject["percentage_probability"] )
print("Object's image saved in " + eachObjectPath)
print("--------------------------------")
```
在上面的行中，我们调用了`detectObjectsFromImage()`，在输入图像路径，输出图像部分新增了参数`extract_detected_objects=True`。此参数指出该函数应提取从图像中检测到的每个对象，并保存它具有单独的图像。默认情况下该参数为false。设置为**true后**，该函数将创建一个目录，即`输出图像路径+"-objects"`。然后它将所有提取的图像保存到这个新目录中，每个图像的名称是`检测到的对象名称+"-number"`，它对应于**检测到对象**的顺序。

我们设置的这个新参数将检测到的对象提取并保存为图像，这将使函数返回2个值。第一个是字典数组，每个字典对应一个检测到的对象。第二个是检测和提取的每个对象的保存图像的路径，并且它们按照对象在第一个阵列中的顺序排列。

### [](#and-one-important-feature-you-need-to-know)**您需要知道的一个重要功能！**

您会记得`detectObjectsFromImage()`函数返回每个检测到的对象的百分比概率。该函数有一个参数 `minimum_percentage_probability`，其默认值为**50**（值范围介于0 - 100之间）。这意味着只有当百分比概率为**50或更高时，**该函数才会返回检测到的对象。该值保持在该数字以确保检测结果的完整性。但是，在此过程中可能会跳过许多对象。因此，您可以通过将`minimum_percentage_probability`设置为较小的值来检测对象检测，以检测更多的对象或更高的值来检测更少的对象。

### [](#custom-object-detection)**自定义对象检测**

对象检测模型（由**RetinaNet**支持）**ImageAI**可以检测80种不同类型的对象。他们包括：

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

有趣的是，**ImageAI**允许您对上面的一个或多个项目执行检测。这意味着您可以自定义要在图像中检测到的对象类型。我们来看看下面的代码：

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

让我们看一下使这成为可能的代码

```
custom_objects = detector.CustomObjects(car=True, motorcycle=True)
detections = detector.detectCustomObjectsFromImage(custom_objects=custom_objects, input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3custom.jpg"))
```

在上面的代码中，在加载模型之后（也可以在加载模型之前完成），我们定义了一个新变量`custom_objects = detector.CustomObjects()`，其中我们将其car和motorcycle属性设置为等于**True**。这是为了告诉模型只检测我们设置为True的对象。然后我们调用`detector.detectCustomObjectsFromImage()`，这是允许我们执行自定义对象检测的函数。然后我们将`custom_objects`值设置为我们定义的自定义对象变量。

### [](#detection-speed)**检测速度**

**ImageAI**现在为所有对象检测任务提供检测速度选项。检测速度允许您以20％-80％的速率减少检测时间，但对检测结果准确性影响不大。结合降低`minimum_percentage_probability`参数，检测可以匹配normal速度，但却大大缩短了检测时间。可用的检测速度是 "normal"(default), "fast", "faster" , "fastest" and "flash"。您需要做的就是在加载模型时说明您想要的速度模式，如下所示。

```
detector.loadModel(detection_speed="fast")
```

要观察检测速度的差异，请查看下面的不同速度应用于物体检测的结果，同时调整检测时间`minimum_percentage_probability`。下面的结果来自在Intel Celeron N2820 CPU的Windows 8笔记本电脑上执行的检测，处理器速度为2.13GHz

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

您会注意到，在"flash"检测速度下，检测速度最快但准确度最低。那是因为图片中没有任何标志性的对象。"flash"检测速度下，其中输入图像是代表性的（包含一个标志性图像）模式是最适合的。请查看下面的示例，以便在标志性图像中进行检测。

**_检测速度="flash"，最小百分比概率= 30（默认值），检测时间= 3.85秒_** 
[![](https://github.com/OlafenwaMoses/ImageAI/raw/master/images/6flash.jpg)](/OlafenwaMoses/ImageAI/blob/master/images/6flash.jpg)

### [](#image-input--output-types)**图像输入和输出类型**

以前版本的**ImageAI**仅支持文件输入，并接受图像的文件路径以进行图像检测。现在，**ImageAI**支持3种输入类型的输入，它们是**图像文件的文件路径**（默认），**图像numpy数组**和**图像文件流** ，以及2种类型的输出，它们是图像**文件**（默认）和numpy **数组**。这意味着您现在可以在生产应用程序中执行对象检测，例如在Web服务器以上述格式返回文件到系统上。

要使用numpy数组或文件流输入执行对象检测，您只需要在`.detectObjectsFromImage()`或` .detectCustomObjectsFromImage() `函数中声明输入类型函数。见下面的例子。

```
detections = detector.detectObjectsFromImage(input_type="array", input_image=image_array , output_image_path=os.path.join(execution_path , "image.jpg")) ＃对于numpy数组输入类型
detections = detector.detectObjectsFromImage(input_type="stream", input_image=image_stream , output_image_path=os.path.join(execution_path , "test2new.jpg")) ＃对于文件流输入类型
```

要使用numpy数组输出执行对象检测，您只需要在`.detectObjectsFromImage() `或`.detectCustomObjectsFromImage()`函数中声明输出类型。见下面的例子。

```
detected_image_array, detections = detector.detectObjectsFromImage(output_type="array", input_image="image.jpg" )   ＃对于numpy数组输出类型
```

### [](#documentation)**文档**

`imageai.Detection.ObjectDetection` class

* * *

所述**ObjectDetection**类可用于通过实例化它并调用下面的可用功能来执行图像上，对象提取和多个对象检测：
- `setModelTypeAsRetinaNet()` 如果您选择使用RetinaNet 模型文件来预测图像，你需要调用这个函数。你只需要调用一次。
- `setModelPath()` 您只需要调用此函数一次，并将模型文件路径的路径解析为字符串。模型文件类型必须与您设置的模型类型相对应。
- `loadModel()` 此函数是必需的，用于从`setModelPath()`定义的文件路径将模型结构加载到程序中
。该函数接收一个`detection_speed`的可选值。该值用于减少预测图像所需的时间，降至正常时间的约60％，只需稍微改变或预测精度下降，具体取决于图像的性质。
    - `detection_speed`（可选）; 可接受的值是"normal", "fast", "faster" and "fastest" 

- `detectObjectsFromImage()`此函数用于检测给定图像路径中可观察的对象：
    - `input_image`，可以是文件路径，图像numpy数组或图像文件流
    - `output_image_path`（仅当output_type = file），如果output_type = file ，输出图像的文件路径，包含检测框和标签。
    - `input_type`（可选），文件路径/ 图像numpy数组/图像文件流。可接受的值是“文件”，“数组”和“流”
    - `output_type`（可选），文件路径/ numpy数组/图像文件流的图像。可接受的值是"file" and "array" 
    - `extract_detected_objects`（可选），用于将每个检测到的对象单独保存为图像并返回对象图像路径的数组。
    - `minimum_percentage_probability`（可选，默认为50），用于设置指定检测到的输出对象的最小百分比概率的选项。

此函数返回的值取决于解析的参数。可返回的值如下所示

- 如果`extract_detected_objects = False`或其默认值和`output_type ='file'`或其
默认值，则必须将`output_image_path`作为字符串解析为您希望
检测到的图像的路径然,该函数将返回：

    1. 一个字典数组，每个字典对应于在图像中检测到的对象。每个字典包含以下属性：
        - name
        - percentage_probability

- 如果`extract_detected_objects = False`或其默认值并且`output_type ='array'`，则该函数将返回：
    1. 检测到的图像的numpy数组
    2. 字典数组，每个字典对应于图像中检测到的对象。每个字典都包含以下属性：
        + name
        + percentage_probability

- 如果`extract_detected_objects = True`且`output_type ='file'`或
在默认值，您必须将`output_image_path`解析为您想要保存检测到的图像的路径的字符串。然后该函数将返回：
    1. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability
    2. 从图像中提取的每个对象的图像的字符串路径数组    

- 如果`extract_detected_objects = True`且`output_type ='array'`，则该函数将返回：
    1. numpy检测到的图像的数组
    2. 一个字典数组，每个字典对应于图像中检测到的对象。每个字典包含以下属性：
        + name
        + percentage_probability
    3. 图像中检测到的每个对象的numpy数组数组

_::param input_image::_

_::param output_image_path::_

_::param input_type::_

_::param output_type::_

_::param extract_detected_objects::_

_::param minimum_percentage_probability::_

_::return output_objects_array::_

_::return detected_copy::_

_::return detected_detected_objects_image_array::_

- `CustomObjecs()`可以选择调用此函数来**手动**选择
要从图像中检测的对象类型。这些对象在函数变量中预先创建，并预定义为“False”，
对于任意数量的可用对象，您可以轻松地将其设置为true。此函数
返回一个字典，必须将其解析用于为`detectCustomObjectsFromImage()`。
检测自定义对象发生在调用函数`detectCustomObjectsFromImage()`时。

- `detectCustomObjectsFromImage`此函数用于检测给定图像路径中可观察的预定义对象：
    - `custom_objects`，一个CustomObject类的实例，用于过滤要检测的对象
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

_::param input_image::_

_::param output_image_path::_

_::param input_type::_

_::param output_type::_

_::param extract_detected_objects::_

_::param minimum_percentage_probability::_

_::return output_objects_array::_

_::return detected_copy::_

_::return detected_detected_objects_image_array::_

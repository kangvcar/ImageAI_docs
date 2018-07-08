# ImageAI：视频对象检测和跟踪（预览版）

**AI 共享**项目 [https://commons.specpal.science](https://commons.specpal.science)

* * *

### **目录**

- [第一视频对象检测](#videodetection)
- [自定义视频对象检测（对象跟踪）](#customvideodetection)
- [检测速度](#videodetectionspeed)
- [帧检测间隔](#videodetectionintervals)
- [文档](#documentation)

ImageAI 提供方便，灵活和强大的方法来对视频进行对象检测。提供的视频对象检测类仅支持当前最先进的 RetinaNet，而在最近将支持其他对象检测网络。这是一个预览版本，但有很多令人难以置信的选择。要开始执行对象检测，您必须通过以下链接下载 RetinaNet 对象检测：

- [RetinaNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5) **（文件大小=145MB）**

由于视频对象检测是计算密集型任务，因此我们建议您使用安装了 NVIDIA GPU 和 GPU 版 Tensorflow 的计算机执行此实验。使用CPU进行视频对象检测将比使用 NVIDIA GPU 驱动的计算机慢。您也可以使用 Google Colab 进行此实验，因为它具有可用的 NVIDIA K80 GPU。

下载 RetinaNet 模型文件后，应将模型文件复制到`.py` 文件所在的项目文件夹中。然后创建一个 python 文件并为其命名; 以下是一个例子`FirstVideoObjectDetection.py`。然后将下面的代码写入 python 文件：

### **FirstVideoObjectDetection.py**

```
from imageai.Detection import VideoObjectDetection
import os

execution_path = os.getcwd()

detector = VideoObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path, "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()

video_path = detector.detectObjectsFromVideo(input_file_path=os.path.join(execution_path, "traffic.mp4"), output_file_path=os.path.join(execution_path, "traffic_detected"), frames_per_second=20, log_progress=True)
print(video_path)
```

 **_输入视频（1 分钟 24 秒视频）_**

 [![](https://github.com/kangvcar/ImageAI/raw/master/images/video--1.jpg)](https://github.com/OlafenwaMoses/ImageAI/blob/master/videos/traffic.mp4)  

 **_输出视频_** 

 [![](https://github.com/kangvcar/ImageAI/raw/master/images/video-2.jpg)](https://www.youtube.com/embed/qplVDqOmElI?rel=0)

C:\Users\User\PycharmProjects\ImageAITest\traffic_detected.avi

让我们对上面使用的对象检测代码进行分析。

```
from imageai.Detection import VideoObjectDetection
import os

execution_path = os.getcwd()
```

在上面的 3 行中，我们在第一行导入 **ImageAI video object detection**类，在第二行导入 **os** 并获取运行 python 文件的文件夹的路径。 

```
detector = VideoObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()
```

在上面的 4 行中，我们在第一行创建了一个 **VideoObjectDetection** 类的新实例，在第二行中将模型类型设置为 RetinaNet，第三行将模型路径设置为我们下载的 RetinaNet 模型文件并复制到 python 文件夹后的路径，第四行加载模型。

```
video_path = detector.detectObjectsFromVideo(input_file_path=os.path.join(execution_path, "traffic.mp4"),
output_file_path=os.path.join(execution_path, "traffic_detected")
, frames_per_second=20, log_progress=True)
print(video_path)
```

在上面的 2 行中，我们运行了 **detectObjectsFromVideo()**函数并解析了我们视频的路径`input_file_path`，新视频将保存为`output_file_path`所指定的路径（没有扩展名，默认保存. avi 视频），`frames_per_second`的值表示我们希望输出视频具有的每秒帧数（fps），`log_progress`表示在控制台中记录检测进度的选项。然后，该函数返回已保存视频的路径，该视频包含在视频中检测到的对象上呈现的框和百分比概率。

### **自定义视频对象检测**

该视频对象检测模型（由**RetinaNet**支持）**ImageAI** 可以检测 80 种不同类型的对象。他们包括：

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

有趣的是，**ImageAI** 允许您对上面的一个或多个项目执行检测。这意味着您可以自定义要在视频中检测到的对象类型。我们来看看下面的代码：

```
from imageai.Detection import VideoObjectDetection
import os

execution_path = os.getcwd()

detector = VideoObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()

custom_objects = detector.CustomObjects(person=True, bicycle=True, motorcycle=True)

video_path = detector.detectCustomObjectsFromVideo(custom_objects=custom_objects, input_file_path=os.path.join(execution_path, "traffic.mp4"), output_file_path=os.path.join(execution_path, "traffic_custom_detected"), frames_per_second=20, log_progress=True)
print(video_path)
```

让我们看一下使这成为可能的代码部分。

```
custom_objects = detector.CustomObjects(person=True, bicycle=True, motorcycle=True)

video_path = detector.detectCustomObjectsFromVideo(custom_objects=custom_objects, input_file_path=os.path.join(execution_path, "traffic.mp4"), output_file_path=os.path.join(execution_path, "traffic_custom_detected"), frames_per_second=20, log_progress=True)
```

在上面的代码中，在加载模型之后（也可以在加载模型之前完成），我们定义了一个新变量`custom_objects = detector.CustomObjects()`，其中我们将person,car,motorcycle属性设置为**True**。这是为了告诉模型只检测我们设置为True的对象。然后我们调用允许我们执行自定义对象检测的函数`detector.detectCustomObjectsFromVideo()`。然后我们将`custom_objects`值设置为我们定义的自定义对象变量。

**_输出视频_**

[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-3.jpg)](https://www.youtube.com/embed/YfAycAzkwPM?rel=0)

C:\Users\User\PycharmProjects\ImageAITest\traffic_custom_detected.avi

### **视频检测速度**

**ImageAI**现在为所有视频对象检测任务提供检测速度选项。检测速度允许您以20％-80％的速率减少检测时间，但只需稍微改变但检测结果依然准确。与降低`minimum_percentage_probability`参数相结合，检测可以与正常速度紧密匹配，但却大大缩短了检测时间。可用的检测速度是"normal"(default), "fast", "faster" , "fastest" and "flash". 。您需要做的就是在加载模型时说明您想要的速度模式，如下所示。

```
detector.loadModel(detection_speed="fast")
```

要观察检测速度的差异，请查看下面的不同速度应用于物体检测的示例，同时调整`minimum_percentage_probability`所需要的检测时间。以下结果来自在NVIDIA K80 GPU上执行的检测。下面提供链接以下载应用的每个检测速度的视频。

**_视频长度= 1分钟24秒，检测速度="normal"，最小百分比概率= 50（默认值），检测时间= 29分钟3秒_**

[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-4.jpg)](https://www.youtube.com/embed/qplVDqOmElI?rel=0) 

**_视频长度= 1分钟24秒，检测速度="fast"，最小百分比概率= 40，检测时间= 11分钟6秒_**

[>>>下载以速度"fast"检测的视频](https://drive.google.com/open?id=118m6UnEG7aFdzxO7uhO_6C-981LJ3Gpf)

**_视频长度= 1分钟24秒，检测速度="faster"，最小百分比概率= 30，检测时间= 7分47秒_**

[>>>下载以速度"faster"检测的视频](https://drive.google.com/open?id=1s1FQWFsEX1Yf4FvUPVleK7vRxaQ6pgUy)

**_视频长度= 1分钟24秒，检测速度="fastest"，最小百分比概率= 20，检测时间= 6分钟20秒_**

[>>>下载以速度"fastest"检测的视频](https://drive.google.com/open?id=1Wlt0DTGxl-JX7otd30MH4qhURv0rG9rw)

**_视频长度= 1分钟24秒，检测速度="flash"，最小百分比概率= 10，检测时间= 3分55_**

[>>>下载以速度"flash"检测的视频](https://drive.google.com/open?id=1V3irCpP49bEUtpjG7Vuk6vEQQAZI-4PI)

如果您使用功能更强大的NVIDIA GPU ，您肯定会有更快的检测时间比。

### **帧检测间隔**

上述视频对象检测任务针对帧实时对象检测进行了优化，以确保检测到视频的每个帧中的对象**.ImageAI**为您提供了调整视频帧检测的选项，可以加快视频检测过程。调用`.detectObjectsFromVideo()` 或 `.detectCustomObjectsFromVideo()`时，可以指定检测的帧间隔。通过将` frame_detection_interval`参数设置为等于5或20，这意味着视频中的对象检测将在5帧或20帧之后更新。如果输出视频`frames_per_second`设置为20，意味着视频中的对象检测将在每四分之一秒或每秒更新一次。如果可检测的对象较模糊且移动对象的速度较低，那此功能非常有用。这可确保您可以将对象检测为每两秒，每半秒或以适合您需求的方式。我们通过应用等于5的**frame_detection_interval**值对我们之前使用的输入视频进行了视频对象检测。下面的结果是从NVIDIA K80 GPU上执行的检测中获得的。点击链接下载对应视频：

**_视频长度= 1分钟24秒，检测速度="normal" ，最小百分比概率= 50（默认值），帧检测间隔= 5，检测时间= 15分49秒_** 

[>>>以速度"normal" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=10m6kXlXWGOGc-IPw6TsKxBi-SXXOH9xK)

**_视频长度= 1分钟24秒，检测速度="fast"，最小百分比概率= 40，帧检测间隔= 5，检测时间= 5分钟6秒_**

[>>>以速度"fast" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=17934YONVSXvd4uuJE0KwenEFks7fFYe4)

**_视频长度= 1分钟24秒，检测速度="faster"，最小百分比概率= 30，帧检测间隔= 5，检测时间= 3分钟18秒_**

[>>>以速度"faster" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=1cs_06CuhXDvZp3fHJWFpam-31eclOhc-)

**_视频长度= 1分钟24秒，检测速度="fastest"，最小百分比概率= 20，帧检测间隔= 5，检测时间= 2分钟18秒_**

[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-3.jpg)](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0) 

[**_视频长度= 1分钟24秒，检测速度="flash"，最小百分比概率= 10，帧检测间隔= 5，检测时间= 1分27秒_**](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0) [](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0)

[>>>以速度"flash" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=1aN2nnVoFjhUWpcz2Und3dsCT9OKrakM0)

### **文档**

`imageai.Detection.VideoObjectDetection` class

* * *

该`VideoObjectDetection`类可以用来对视频目标检测，通过实例化，并调用下面的功能函数：

- `setModelTypeAsRetinaNet()` 如果您选择使用RetinaNet模型文件来预测图像，你需要调用这个函数。你只需要调用一次。

- `setModelPath()` 您只需要调用此函数一次，并将模型文件路径的路径解析为字符串。模型文件类型必须与您设置的模型类型相对应。

- `loadModel()` 此函数是必需的，用于从`setModelPath()`定义的文件路径将模型结构加载到程序中
。该函数接收一个`prediction_speed`的可选值。该值用于减少预测图像所需的时间，降至正常时间的约60％，只需稍微改变或预测精度下降，具体取决于图像的性质。
    - ` prediction_speed`（可选）; 可接受的值是"normal", "fast", "faster" and "fastest" 


- `detectObjectsFromVideo()` 此函数用于检测给定视频路径中可观察的对象：
    + `input_file_path`，它是输入视频的文件路径
    + `output_file_path`，它是输出视频的路径
    + `frames_per_second` 它是输出视频中使用的帧数fps
    + `frame_detection_interval`（可选，默认为1）），这是检测视频的帧间隔，即间隔多少帧检测一次。
    + `minimum_percentage_probability`（可选，默认为50），用于设置指定检测到的输出对象的最小百分比概率的选项。
    + `log_progress`（任选），指出是否要将处理帧的进度输出到控制台

_::param input_file_path::_

_::param output_file_path::_

_::param frames_per_second::_

_::param frame_detection_interval::_

_::param minimum_percentage_probability::_

_::param log_progress::_

_::return output_video_filepath::_

- `CustomObjecs()` 可以选择调用此函数来**手动**指定
要从视频中检测的对象类型。这些对象在函数变量中预先创建，并预定义为“False”，
对于任意数量的可用对象，您可以轻松地将其设置为True。此函数
返回一个字典，必须将其解析用于为`detectCustomObjectsFromVideo()`。检测
自定义对象仅在调用函数`detectCustomObjectsFromVideo()`时。

- `detectCustomObjectsFromVideo()` 此函数用于检测给定视频路径中可观察的特定对象：
    + `custom_objects`，指定要检测的对象
    + `input_file_path` 指定输入视频的文件路径
    + `output_file_path` 指定输出视频的路径
    + `frames_per_second` 它是输出视频中使用的帧数fps
    + `frame_detection_interval`（可选，默认为1）），这是检测视频的帧间隔，即间隔多少帧检测一次。
    + `minimum_percentage_probability`（可选，默认为50），用于设置指定检测到的输出对象的最小百分比概率的选项。
    + `log_progress`（任选），指出是否要将处理帧的进度输出到控制台

_::param custom_objects::_

_::param input_file_path::_

_::param output_file_path::_

_::param frames_per_second::_

_::param frame_detection_interval::_

_::param minimum_percentage_probability::_

_::param log_progress::_

_::return output_video_filepath::_
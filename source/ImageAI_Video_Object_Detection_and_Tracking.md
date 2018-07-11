# ImageAI：视频对象检测和跟踪（预览版）

**AI共享**项目 [https://commons.specpal.science](https://commons.specpal.science)

* * *

ImageAI 提供方便，灵活和强大的方法来对视频进行对象检测和跟踪。目前仅支持当前最先进的 RetinaNet 算法进行对象检测和跟踪，后续版本会加入对其他算法的支持。虽然这只是预览版本，但提供了很多令人难以置信的选项。在开始视频对象检测和跟踪任务前，您必须通过以下链接下载 RetinaNet 模型文件：

- [RetinaNet](https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5) **（文件大小=145MB）**

由于视频对象检测是非常消耗硬件资源的任务，所以我们建议您使用安装了 NVIDIA GPU 和 GPU 版 Tensorflow 的计算机来完成此实验。使用CPU进行视频对象检测将比使用 NVIDIA GPU 驱动的计算机慢。您也可以使用 Google Colab 进行此实验，因为它具有可用的 NVIDIA K80 GPU。

下载 RetinaNet 模型文件后，应将模型文件复制到`.py`文件所在的项目文件夹中。然后创建一个python文件并为其命名; 例如 **FirstVideoObjectDetection.py** 。然后将下面的代码写入python文件中：

### FirstVideoObjectDetection.py

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

 **_输入视频（时长1分钟24秒）_**

 [![](https://github.com/kangvcar/ImageAI/raw/master/images/video--1.jpg)](https://github.com/OlafenwaMoses/ImageAI/blob/master/videos/traffic.mp4)  

 **_输出视频_** 

 [![](https://github.com/kangvcar/ImageAI/raw/master/images/video-2.jpg)](https://www.youtube.com/embed/qplVDqOmElI?rel=0)<br/>
C:\Users\User\PycharmProjects\ImageAITest\traffic_detected.avi

让我们对示例中的视频对象检测代码进行解读：

```
from imageai.Detection import VideoObjectDetection
import os

execution_path = os.getcwd()
```

在上面的代码中，我们在第一行导入`ImageAI video object detection`类，在第二行导入`os`库，在第三行获得当前python文件的文件夹的路径。 

```
detector = VideoObjectDetection()
detector.setModelTypeAsRetinaNet()
detector.setModelPath( os.path.join(execution_path , "resnet50_coco_best_v2.0.1.h5"))
detector.loadModel()
```

在上面的代码中，我们在第一行创建了`VideoObjectDetection`类的新实例，在第二行中将模型类型设置为RetinaNet，第三行将模型路径设置为我们下载的RetinaNet模型文件所在文件夹的路径，第四行载入模型。

```
video_path = detector.detectObjectsFromVideo(input_file_path=os.path.join(execution_path, "traffic.mp4"),
output_file_path=os.path.join(execution_path, "traffic_detected")
, frames_per_second=20, log_progress=True)
print(video_path)
```

在上面的代码中，我们调用了`detectObjectsFromVideo()`函数并传入了4个参数，其中`input_file_path`参数用于指定输入视频文件的路径；`output_file_path`参数用于指定输出视频文件的路径（没有扩展名，默认保存.avi格式的视频）；`frames_per_second`参数用于指定输出视频每秒帧数（fps），`log_progress`参数用于指定是否在控制台中输出检测进度。最后，该函数返回检测后所保存的视频路径，该视频包含在每个对象上绘制矩形标记和百分比概率。

### 自定义视频对象检测

**ImageAI** 视频对象检测模型（由**RetinaNet**支持），并且可以检测 80 种不同类型的对象。他们包括：

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

让我们看看使这成为可能的代码：

```
custom_objects = detector.CustomObjects(person=True, bicycle=True, motorcycle=True)

video_path = detector.detectCustomObjectsFromVideo(custom_objects=custom_objects, input_file_path=os.path.join(execution_path, "traffic.mp4"), output_file_path=os.path.join(execution_path, "traffic_custom_detected"), frames_per_second=20, log_progress=True)
```

在上面的代码中，我们定义了一个新变量`custom_objects = detector.CustomObjects()`，其中我们将person,car和motorcycle属性设置为`True`，这是为了告诉模型只检测我们设置为True的对象。然后我们调用`detector.detectCustomObjectsFromVideo()`函数并传入了我们定义的变量`custom_objects`来指定我们需要从图像中识别的对象。

**_输出视频_**

[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-3.jpg)](https://www.youtube.com/embed/YfAycAzkwPM?rel=0)<br/>
C:\Users\User\PycharmProjects\ImageAITest\traffic_custom_detected.avi

### 视频检测速度

**ImageAI** 为对象检测任务添加了速度调节参数`detection_speed`，结合`minimum_percentage_probability`参数使用效果更佳，最多可使检测时间缩短80％且几乎不影响检测结果的精准度。可用的检测速度是 "normal"(default), "fast", "faster" , "fastest" and "flash"。您只要在加载模型时说明您想要的速度模式，如下所示。

```
detector.loadModel(detection_speed="fast")
```

为了观察不同速度模式间的差异，请查看下面不同速度模式下（结合调整`minimum_percentage_probability`参数）检测相同图像所花费的时间(在NVIDIA K80 GPU)。下面提供每个视频的下载链接：

**_视频长度= 1分钟24秒，检测速度="normal"，最小百分比概率= 50（默认值），检测时间= 29分钟3秒_**

[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-4.jpg)](https://www.youtube.com/embed/qplVDqOmElI?rel=0) 

**_视频长度= 1分钟24秒，检测速度="fast"，最小百分比概率= 40，检测时间= 11分钟6秒_**<br/>
[>>>下载以速度"fast"检测的视频](https://drive.google.com/open?id=118m6UnEG7aFdzxO7uhO_6C-981LJ3Gpf)

**_视频长度= 1分钟24秒，检测速度="faster"，最小百分比概率= 30，检测时间= 7分47秒_**<br/>
[>>>下载以速度"faster"检测的视频](https://drive.google.com/open?id=1s1FQWFsEX1Yf4FvUPVleK7vRxaQ6pgUy)

**_视频长度= 1分钟24秒，检测速度="fastest"，最小百分比概率= 20，检测时间= 6分钟20秒_**<br/>
[>>>下载以速度"fastest"检测的视频](https://drive.google.com/open?id=1Wlt0DTGxl-JX7otd30MH4qhURv0rG9rw)

**_视频长度= 1分钟24秒，检测速度="flash"，最小百分比概率= 10，检测时间= 3分55_**<br/>
[>>>下载以速度"flash"检测的视频](https://drive.google.com/open?id=1V3irCpP49bEUtpjG7Vuk6vEQQAZI-4PI)

如果您使用功能更强大的NVIDIA GPU，检测时间将更加短。

### 帧检测间隔

上述视频对象检测任务检测了视频的每一帧。 **ImageAI** 为视频对象检测任务添加了帧检测间隔参数` frame_detection_interval`以加快视频检测过程。在调用`.detectObjectsFromVideo()` 或 `.detectCustomObjectsFromVideo()`函数时，通过指定参数`frame_detection_interval`的值来设置每隔多少帧检测一次对象，该参数的值可设置为5-20之间。
如果可检测的对象较模糊且对象移动速度较慢时此功能非常有用。我们通过设置参数`frame_detection_interval=5`对上面示例中的视频进行视频对象检测（实验环境：NVIDIA K80 GPU）。点击链接下载对应视频：

**_视频长度= 1分钟24秒，检测速度="normal" ，最小百分比概率= 50（默认值），帧检测间隔= 5，检测时间= 15分49秒_**<br/>
[>>>以速度"normal" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=10m6kXlXWGOGc-IPw6TsKxBi-SXXOH9xK)

**_视频长度= 1分钟24秒，检测速度="fast"，最小百分比概率= 40，帧检测间隔= 5，检测时间= 5分钟6秒_**<br/>
[>>>以速度"fast" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=17934YONVSXvd4uuJE0KwenEFks7fFYe4)

**_视频长度= 1分钟24秒，检测速度="faster"，最小百分比概率= 30，帧检测间隔= 5，检测时间= 3分钟18秒_**<br/>
[>>>以速度"faster" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=1cs_06CuhXDvZp3fHJWFpam-31eclOhc-)

**_视频长度= 1分钟24秒，检测速度="fastest"，最小百分比概率= 20，帧检测间隔= 5，检测时间= 2分钟18秒_**<br/>
[![](https://github.com/kangvcar/ImageAI/raw/master/images/video-3.jpg)](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0) 

[**_视频长度= 1分钟24秒，检测速度="flash"，最小百分比概率= 10，帧检测间隔= 5，检测时间= 1分27秒_**](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0) [](https://www.youtube.com/embed/S-jgBTQgbd4?rel=0)<br/>
[>>>以速度"flash" 和间隔= 5检测对象的视频](https://drive.google.com/open?id=1aN2nnVoFjhUWpcz2Und3dsCT9OKrakM0)

### **文档**

`imageai.Detection.VideoObjectDetection` class

* * *

在任何的Python程序中通过实例化`VideoObjectDetection`类并调用下面的函数即可进行视频对象检测：
- `setModelTypeAsRetinaNet()` 如果您选择使用RetinaNet 模型文件来进行对象检测，你只需调用一次该函数。
- `setModelPath()` 该函数用于设定模型文件的路径。模型文件必须与您设置的模型类型相对应。
- `loadModel()` 该函数用于载入模型。该函数接收一个`prediction_speed`参数。该参数用于指定对象检测的速度模式，当速度模式设置为'fastest'时预测时间可缩短60%左右，具体取决于图像的质量。
    - `detection_speed`（可选）; 可接受的值是"normal", "fast", "faster" and "fastest" 
- `detectObjectsFromVideo()` 此函数用于通过接收以下参数来进行视频对象检测：
    - `input_file_path`，该参数用于指定输入视频的文件路径
    - `output_file_path`，该参数用于指定输出视频的文件路径
    - `frames_per_second` 该参数用于指定输出视频中的每秒帧数fps
    - `frame_detection_interval`（可选，默认为1）），该参数用于指定视频检测的帧间隔，即间隔多少帧检测一次。
    - `minimum_percentage_probability`（可选，默认为50），用于设定预测概率的阈值，只有当百分比概率大于等于该值时才会返回检测到的对象。 
    - `log_progress`（可选），该参数用于指定是否将检测进度输出到控制台

**_:param input_file_path:_**<br/>
**_:param output_file_path:_**<br/>
**_:param frames_per_second:_**<br/>
**_:param frame_detection_interval:_**<br/>
**_:param minimum_percentage_probability:_**<br/>
**_:param log_progress:_**<br/>
**_:return output_video_filepath:_**<br/>

- `CustomObjecs()` 调用此函数来选择需要在视频中进行检测的对象。这些对象在函数变量中预先创建并定义为 `False`。您可以将任何预创建的对象设置为`true`，最终此函数将返回一个字典被`detectCustomObjectsFromVideo()`函数的一个参数`custom_objects`所使用。

- `detectCustomObjectsFromVideo()` 此函数通过接收以下参数在视频中对指定对象进行检测：
    - `custom_objects`，一个`CustomObject`类的实例，用于指定在视频中需要检测的对象
    - `input_file_path` 该参数用于指定输入视频的文件路径
    - `output_file_path` 该参数用于指定输出视频的文件路径
    - `frames_per_second` 该参数用于指定输出视频中的每秒帧数fps
    - `frame_detection_interval`（可选，默认为1）），该参数用于指定视频检测的帧间隔，即间隔多少帧检测一次。
    - `minimum_percentage_probability`（可选，默认为50），用于设定预测概率的阈值，只有当百分比概率大于等于该值时才会返回检测到的对象。 
    - `log_progress`（可选），该参数用于指定是否将检测进度输出到控制台

**_:param custom_objects:_**<br/>
**_:param input_file_path:_**<br/>
**_:param output_file_path:_**<br/>
**_:param frames_per_second:_**<br/>
**_:param frame_detection_interval:_**<br/>
**_:param minimum_percentage_probability:_**<br/>
**_:param log_progress:_**<br/>
**_:return output_video_filepath:_**<br/>
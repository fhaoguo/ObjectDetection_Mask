# 基于yolo的口罩目标检测

## 数据集
+ 400张人脸图像，其中戴口罩136张，没戴口罩264张
+ 放在data文件夹中，其中Annotations是标注文件夹，JPEGImages是原始图像
## 环境常量设置
+ `constants.py`文件中指定COMMAN_PATH
## 数据预处理
#### 1.在data文件夹中新建ImageSets和labels
#### 2.复制JPEGImages重命名为images
#### 3.执行`python makeTxt.py`
>主要是将数据集分类成训练数据集和测试数据集，默认按照9:1的比例进行随机分类。
运行makeTxt.py后，ImagesSets文件夹中会出现四个文件:train.txt, test.txt, val.txt, trainval.txt。
主要是生成的训练集和测试集的图片名称，同时data目录下也会出现这四个文件，内容是训练集和测试集的图片路径。

#### 4.执行`python voc_label.py`, 在voc_label.py文件中设置`classes = ['mask', 'unmask']（与rbc.names文件一致）`
>主要是将图片数据集标注后的xml文件中的标注信息读取出来并写入txt文件。
运行voc_label.py后在labels文件夹中出现所有图片数据集的标注信息。

## 训练
#### 1.在data文件下新建rbc.data，内容：
>classes=2</br>
train=data/train.txt</br>
valid=data/test.txt</br>
names=data/rbc.names</br>
backup=backup/</br>
eval=coco

#### 2.配置rbc.names文件，内容：
>mask</br>
unmask

#### 3.训练
>python train.py --data-cfg data/rbc.data --cfg cfg/yolov3-tiny.cfg --epochs 100

## 测试
>python detect.py --data-cfg data/rbc.data --cfg cfg/yolov3-tiny.cfg --weights weights/best.pt

对data/sample的图像进行口罩检测，检测结果输出到output文件夹中。


[labelImg标注工具](https://github.com/tzutalin/labelImg/), 出现: "no module named libs.resources"问题解决方案。
+ Step1，pip install pyqt5</br>
+ Step2，pyrcc5 -o resources.py resources.qrc
把Qt文件格式转为Python格式
+ Step3，将生成的resources.py copy到libs目录下，避免出现：no module named libs.resources
+ Step4，运行 python labelImg.py


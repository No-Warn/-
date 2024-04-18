# YOLOv7代码整体结构

1. 主干特征体系网络
2. 增强特征体系网络
3. YOLOhead

对于输入进的数据，会进行一系列卷积对图像数据进行不断缩小以及通道的扩张
安装gpu环境：
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

pip install torch==12.3+cu12.3 -f https://download.pytorch.org/whl/torch_stable.html

# 训练自己的数据集
1：下载yolov7.pt权重文件，在跑detect.py文件时会用到权重文件
2：修改yolov7.yaml文件中的nc(类的数量，本次训练数据集的数量为3)
3：修改classes字典中的类别名称
4：注意数据集的格式，包含train、val等，其中包含image以及label
5：拷贝一份coco.yaml文件命名为voc.yaml对其中的train文件路径以及val文件路径进行修改，以及其对应的类别个数和类别名称
6：将train.py文件中的531行的default中的yaml文件修改成拷贝的voc.yaml文件
7：train.py文件中534行的bach-size根据自己的显存大小设置，我设置的是2
8：train.py文件中的img-size可以根据自己的显存大小来修改
9：在train.py文件中的权重文件使用的路径最好为绝对路径

需要解决的问题：
在训练时训练速度过慢（在尝试训练数据集的过程中大概一个小时才跑了9张图片）

在训练完成之后，可以运行detect.py文件来进行预测，预测结果的输出形式可以进行修改


# 对train.py文件中各参数的解释
train,py文件用来训练数据集
其中


# wandb中实验结果的各项参数表示的含义



# 修改输出形式,将输出结果修改为txt文件格式，方便后续
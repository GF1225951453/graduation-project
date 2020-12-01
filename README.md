# graduation-project

## 智能植物养殖顾问
本毕设是来源于互联网+的创新创业项目
本项目针对植物养殖业中自动灌溉系统，采用嵌入式、物联网、人工智能技术，从植物
的养殖和图像信息学习建模植物的最佳生长状态，为自动灌溉系统提供更精准的控制依据，
以至为植物养殖带来更大的利益，并为用户提供手机端便携操作。本团队技术优势为嵌入式
数据采样、物联网数据传输、人工智能数据分析建模相结合的一体化技术系统，并且图像识
别的使用是其亮点所在。
本人在该项目中负责数据分析，数据建模。

### 所需环境
pytorch=1.7.0，win10 操作系统，pycharm
本毕设在实验中采用全程养殖绿萝，并对不同阶段，不同生长状态的绿萝进行拍照，并收集绿萝不同的生长状态的图片。
在拍照的同时也要使用光照传感器，温湿度传感器收集绿萝生长的环境数据，该环境数据与绿萝的生长状态图片一一对应。
经整理后保存到.csv文件中。
将收集来的图像数据进行一定的处理，如裁剪。通过裁剪，尽可能的保留下绿萝生长状态的特征。

### 文件结构
【1】clustering.py文件中存放聚类环境数据的代码，聚类算法采用kmeans。因为是标签未知的聚类，所以评价指标采用轮廓
系数，轮廓系数越大，聚类效果越好。

【2】data_process.py文件中存放用于生成后缀为.txt文件的代码，data.txt文件中存放着训练数据的路径以及对应标签。

【3】data_reader.py存放加载训练数据的代码，该文件中的代码会将用于训练的图像数据以及标签存放如tensor中用于模型训练。

【4】resnext50.py文件用于存放resnext50神经网络，该网络主要用于对比自己所搭建网络的性能。

【5】similar_to_unet.py文件用于存放本设计使用的神经网络，该网络是本人自己搭建的，受unet网络结构的启发，该网络feature map由少到多再到少，
在网络中间部分feature map最多的地方，使用bottle neck结构降低运算量，使用1x3,3x1旁路卷积降低运算量，提高网络分类精度。该网络在(256x256)的
分辨率下总参数量共130M关于它的性能，请查看network compare.jpg图片。

【6】similar_to_darknet.py文件存放自己搭建的神经网络，该网络受 yolov3 darknet53 启发，feature map从输入端到输出端多到少，在256x256的分
辨率下总的参数两为4.4G，由于其参数两太大，训练过慢，所以被弃用。

【7】train.py文件用于训练神经网络。

【8】inference.py文件用于预测，test_data.txt文件用于保存测试集数据路径。

【9】tmp.py用于测试调优，tmp.txt用于存放少量的训练集数据，便于快速迭代，发现代码漏洞。

### 训练
【1】在训练时加入数据增加，例如水平翻转，垂直翻转，随机反转，色域变换。在训练中加入色域变换后，最终模型精度下降，推测原因是可能是因为数据增强
的加入使得数据集难以训练。随即在测试集代码中也加入数据增强，但是测试精度一般。

【2】使用kaggle上找到的植物叶片分类数据集，用该植物叶片数据集训练10个epoch并保存模型权重，之后将该权重用作预训练权重，初始化自己的模型权重后，在本毕设的数据集上进行，
微调：即冻结骨干网络，训练全连接层10个epoch；解冻骨干网络全部训练10个epoch。模型精度有一定的提升。
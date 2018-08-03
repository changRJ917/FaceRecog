# face_recog
1.get_my_faces_dlib.py
使用opencv获取个人的人脸图像，并通过dlib库中的人脸特征提取，截取获取的正样本人脸数据集。
该程序共包括如下2个重要部分：
（1）定义relight函数，对摄像头拍摄的图像进行光线的转换，多样化样本。
（2）dlib函数针对的是灰度图像，对图像进行灰度转换和resize，存储图像均为64*64.
2.set_other_faces.py
对已有的数据集，如LFW数据集，通过重新提取图像到指定的输出文件夹中，形成train过程中的负样本集。
该程序共包括如下2个重要部分：
（1）读取已有数据集input_img中的图像。
（2）利用dlib库中的人脸特征函数将(1)中读取的图像进行人脸的截取，并重新写入新的文件夹other_faces中。
3.train_faces.py
训练my_faces和other_faces中的图像，获取准确率高于0.99的训练模型并保存。
该程序共包括如下2个重要部分：
（1）读入图像，特加入了填补函数getPaddingSize，防止图像进行resize的时候失真。
（2）图像转化可读入数据集，image转化为矩阵，label转化为one-hot编码，正样本为[0,1]，负样本为[1,0].
（3）将整个正样本和负样本进行随机划分为测试集和训练集，test_size=0.05。
（4）利用CNN进行训练，三层卷积层，维度分别为（32，64，64），卷积核大小均为3；每一层均采用maxpool，池化大小均为2；且都采用dropout操作来减小过拟合。全连接层维度为512，并使用dropout，softmax输出为2。
（5）当所有图像被训练两轮后且测试准确率大于0.99时，将训练结果模型保存在项目目录中，以供实时测试使用。
5.is_my_face.py
测试过程直接采用保存的训练模型中的参数，使用opencv获取实时图像，检测摄像头获取的人脸是否是本人，输出为True or False。
当摄像头读入本人时，显示True，是其他人的时候，显示False。
note：正样本数目不可过少，即，get_my_faces_dlib.py中数据应多一点
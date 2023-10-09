# RingNet

![alt text](https://github.com/soubhiksanyal/RingNet/blob/master/gif/celeba_reconstruction.gif?raw=true)

这是论文《从图像学习回归3D人脸形状和表情而无需3D监督》的官方存储库。该项目以前被称为RingNet。代码库包含推断代码，即通过使用此代码提供的人脸图像，可以生成具有脸部区域的完整头部的3D网格。有关该方法的详细信息，请参阅以下出版物，

```
从图像学习回归3D人脸形状和表情而无需3D监督
Soubhik Sanyal，Timo Bolkart，Haiwen Feng，Michael J. Black
CVPR 2019
```

有关我们的NoW基准数据集、3D人脸重建挑战的更多详细信息，请访问我们的[项目页面](https://ringnet.is.tue.mpg.de)。在[项目页面](https://ringnet.is.tue.mpg.de)上还提供了pdf预印本。

* **更新**：我们已更改RingNet代码和预训练权重的许可协议。两者现在都在**MIT许可证**下提供，不包括NoW挑战数据集。

* **更新**：我们已发布NoW Benchmark挑战的**评估代码** [此处](https://github.com/soubhiksanyal/now_evaluation)。

* **更新**：添加演示以从输入图像构建重建网格的纹理。

* **更新**：NoW数据集分为测试集和验证集。验证集提供**地面真实扫描**。有关更多详细信息，请查看我们的[项目页面](https://ringnet.is.tue.mpg.de)。

* **更新**：我们发布了**FLAME解码器的PyTorch实现，具有动态轮廓加载**，可直接用于训练网络。请查看 [FLAME_PyTorch](https://github.com/soubhiksanyal/FLAME_PyTorch) 获取代码。

## 安装

该代码使用**Python 2.7**，在Tensorflow gpu版本1.12.0上测试，CUDA-9.0和cuDNN-7.3。

### 设置RingNet虚拟环境

```
virtualenv --no-site-packages <your_home_dir>/.virtualenvs/RingNet
source <your_home_dir>/.virtualenvs/RingNet/bin/activate
pip install --upgrade pip==19.1.1
```
### 克隆项目并安装依赖项

```
git clone https://github.com/soubhiksanyal/RingNet.git
cd RingNet
pip install -r requirements.txt
pip install opendr==0.77
mkdir model
```
从[MPI-IS/mesh](https://github.com/MPI-IS/mesh)安装网格处理库。 （现在只能在python 3上工作，因此请不要安装它）

* 更新：请安装以下[fork](https://github.com/TimoBolkart/mesh)以在python 2.7中使用网格处理库。

## 下载模型

* 从[项目网站](https://ringnet.is.tue.mpg.de)的下载页面下载预训练的RingNet权重。将其复制到**model**文件夹中。
* 从[此处](http://flame.is.tue.mpg.de/)下载FLAME 2019模型。将其复制到**flame_model**文件夹中。此步骤是可选的，仅在要使用输出的Flame参数玩弄3D网格时才需要，即中性化姿势和表情，仅使用形状作为其他方法的模板，如[VOCA（Voice Operated Character Animation）](https://github.com/TimoBolkart/voca)。

* 从[这里](http://files.is.tue.mpg.de/tbolkart/FLAME/FLAME_texture_data.zip)下载[FLAME_texture_data](http://files.is.tue.mpg.de/tbolkart/FLAME/FLAME_texture_data.zip)并将其解压缩到**flame_model**文件夹中。

## 演示

RingNet需要图像中脸部的宽松裁剪。我们在**input_images**文件夹中提供了两个样本图像，这些图像来自[CelebA数据集](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)。

#### 输出预测网格渲染

从终端运行以下命令以检查RingNet的预测
```
python -m demo --img_path ./input_images/000001.jpg --out_folder ./RingNet_output
```
提供图像路径，它将在**./RingNet_output/images/**中输出预测结果。

#### 输出预测网格

如果要输出网格，则运行以下命令
```
python -m demo --img_path ./input_images/000001.jpg --out_folder ./RingNet_output --save_obj_file=True
```
它将在**./RingNet_output/mesh/**中保存预测网格的*.obj文件。

#### 输出带纹理的网格

如果要输出具有图像投影到网格上作为纹理的预测网格，则运行以下命令
```
python -m demo --img_path ./input_images/000001.jpg --out_folder ./RingNet_output --save_texture=True
```
它将在**./RingNet_output/texture/**中保存预测网格的*.obj，*.mtl和*.png文件。

#### 输出FLAME和相机参数

如果要获取预测的FLAME和相机参数，则运行以下命令
```
python -m demo --img_path ./input_images/000001.jpg --out_folder ./RingNet_output --save_obj_file=True --save_flame_parameters=True
```
它将在**./RingNet_output/params/**中保存预测的flame和相机参数的*.npy文件。

#### 生成VOCA模板

如果要玩弄3D网格，即中性化3D网格的姿势和表

情以将其用作[VOCA（Voice Operated Character Animation）](https://github.com/TimoBolkart/voca)中的模板，请运行以下命令
```
python -m demo --img_path ./input_images/000013.jpg --out_folder ./RingNet_output --save_obj_file=True --save_flame_parameters=True --neutralize_expression=True
```

## 许可证

仅限非商业和科学研究用途。使用此代码即表示您已阅读许可证条款（https://ringnet.is.tue.mpg.de/license.html），理解并同意受其约束。如果您不同意这些条款和条件，您不得使用该代码。有关商业用途，请查看网站（https://ringnet.is.tue.mpg.de/license.html）。

## 引用RingNet

如果您在研究/项目中直接或间接使用此代码，请引用以下论文：

```
@inproceedings{RingNet:CVPR:2019,
title = {Learning to Regress 3D Face Shape and Expression from an Image without 3D Supervision},
author = {Sanyal, Soubhik and Bolkart, Timo and Feng, Haiwen and Black, Michael},
booktitle = {Proceedings IEEE Conf. on Computer Vision and Pattern Recognition (CVPR)},
month = jun,
year = {2019},
month_numeric = {6}
}
```

## 联系

如果您有任何问题，请通过soubhik.sanyal@tuebingen.mpg.de和timo.bolkart@tuebingen.mpg.de与我们联系。

## 致谢

* 感谢[Ahmed Osman](https://github.com/ahmedosman)在FLAME的Tensorflow实现中的支持。
* 感谢Raffi Enficiaud和Ahmed Osman推动psbody.mesh的发布。
* 感谢Benjamin Pellkofer和Jonathan Williams协助我们的[RingNet项目网站](https://ringnet.is.tue.mpg.de)。

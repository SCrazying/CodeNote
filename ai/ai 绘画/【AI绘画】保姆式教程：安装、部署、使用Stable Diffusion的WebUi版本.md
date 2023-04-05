# 【AI绘画】安装、部署、使用Stable Diffusion的WebUi版本



### 1.安装git

[Git - Downloading Package (git-scm.com)](https://git-scm.com/download/win)'

### 2.Python环境

https://www.python.org/ftp/python/3.10.11/python-3.10.11-amd64.exe

### 3.下载模型

#### 3.1注册账号

[Hugging Face – The AI community building the future.](https://huggingface.co/)

#### 3.2下载model

[CompVis/stable-diffusion-v-1-4-original at main (huggingface.co)](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original/tree/main)

##### **3.2.1 权重文件**

```bash
# Make sure you have git-lfs installed (https://git-lfs.github.com)
git lfs install
git clone https://huggingface.co/CompVis/stable-diffusion-v-1-4-original

# if you want to clone without large files – just their pointers
# prepend your git clone with the following env var:
GIT_LFS_SKIP_SMUDGE=1
```

##### **3.2.2 真人风格图像绘制**

[naonovn/chilloutmix_NiPrunedFp32Fix at main (huggingface.co)](https://huggingface.co/naonovn/chilloutmix_NiPrunedFp32Fix/tree/main)

```bash
git lfs install
git clone https://huggingface.co/naonovn/chilloutmix_NiPrunedFp32Fix
```

##### **3.2.3 二次元模型**

[andite/anything-v4.0 at main (huggingface.co)](https://huggingface.co/andite/anything-v4.0/tree/main)

```bash
git lfs install
git clone https://huggingface.co/andite/anything-v4.0
```

#### 3.3 下载stable-diffusion-webui

[GitHub - AUTOMATIC1111/stable-diffusion-webui: Stable Diffusion web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

##### 3.3.1 “Code” -> "download ZIP"，下载；下载的zip文件解压。

##### 3.3.2 windows运行webui-user.bat

##### 3.3.3 将权重文件放到一下目录

```bash
stable-diffusion-webui-master\models\Stable-diffusion
```



### 4 问题汇总

#### **4.1 无法安装gfpgan**

```bash
#.\stable-diffusion-webui-master\venv\Scripts
git clone https://github.com/TencentARC/GFPGAN.git
cd GFPGAN
#安装GFPGAN的依赖
../python.exe -m pip install basicsr facexlib
../python.exe -m pip install -r requirements.txt
../python.exe setup.py develop
cd ..
rm -rf GFPGAN
# 重新运行
webui-user.bat

#上述过程中如果遇到pip 卡住，可以使用豆瓣源
../python.exe -m pip install basicsr facexlib -i http://pypi.douban.com/simple/ --trusted-host=pypi.douban.com/simple ipython
```

#### **4.2 clip安装不上**

```bash
# stable diffusion webui环境中的clip其实是open_clip，不能用pip install clip安装
# 解决方法是直接到github下载 open_clip 代码到本地，并进行本地安装
git clone https://github.com/mlfoundations/open_clip
../python.exe setup.py build install

```

#### **4.3 AssertionError: Torch is not able to use GPU**

```python
import torch
print(torch.__version__)
#使用cpu
1.13.1+cpu
#使用gpu
1.13.1+cu117
#pip uninstall torch卸载cpu版本
#torch官网https://pytorch.org/get-started/locally/ 选择自己匹配的版本,手动安装

```
#### **4.4 DiffusionWrapper卡住**

```python
#Creating model from config: D:\doc\ai\stable-diffusion-webui-master\configs\v1-inference.yaml
#LatentDiffusion: Running in eps-prediction mode
#DiffusionWrapper has 859.52 M params.

刚刚看了下，是huggingface_hub库在下载文件时的问题，获取etag返回了带双引号的字符串，本地创建不了这样的文件
把\Lib\site-packages\huggingface_hub\file_download.py文件的hf_hub_download函数中assert etag is not None, "etag must have been retrieved from server"前一行加上etag=etag.replace('"','')即可

# From now on, etag and commit_hash are not None.
etag=etag.replace('"','')
assert etag is not None, "etag must have been retrieved from server"
assert commit_hash is not None, "commit_hash must have been retrieved from server"


# 参考大佬的评论区：https://www.bilibili.com/read/cv22086806
    
```

### 5. 启动应用

```bash
@echo off
 ​
 :: 手动设置Python环境，如果不设置，使用环境变量的Python环境
 set PYTHON=
 set GIT=
 set VENV_DIR=
 set CUDA_VISIBLE_DEVICES=1
 set COMMANDLINE_ARGS=  --xformers  --medvram 
 ​
 call webui.bat
```

注意`set COMMANDLINE_ARGS= --xformers --medvram`参数很关键，在笔者上古显卡1050ti中，必须开`--medvram`才能勉强跑一些大图，不设置经常爆显存！它的意思是说是使用中等的显存，显存相关说明可参考原文

https://link.zhihu.com/?target=https%3A//github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Troubleshooting

### 6. 中文

```C++
cd ./stable-diffusion-webui/extensions/
git clone https://github.com/dtlnor/stable-diffusion-webui-localization-zh_CN
```



### 7. 相关资源网站

```bash
https://huggingface.co/
https://civitai.com/
https://pypi.org/
```


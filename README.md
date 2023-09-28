# Bert-VITS2

VITS2 Backbone with bert
## 成熟的旅行者/开拓者/舰长/博士/sensei/猎魔人/喵喵露/V应当参阅代码自己学习如何训练。
### 严禁将此项目用于一切违反《中华人民共和国宪法》，《中华人民共和国刑法》，《中华人民共和国治安管理处罚法》和《中华人民共和国民法典》之用途。
### 严禁用于任何政治相关用途。

## 使用方法
### 1.环境配置
```
!git clone https://github.com/Mr47121836/Bert-VITS2.git
%cd Bert-VITS2
```

```
!pip install -r requirements.txt
```
###  2.设置训练名称
```
train_name = "xss"
```
### 3.数据集准备
#### 3.1 数据集下载
```
!wget https://huggingface.co/datasets/guetLzy/genshin/resolve/main/%E7%94%B3%E9%B9%A4.zip -O ./wav/申鹤.zip
!wget https://huggingface.co/datasets/guetLzy/genshin/resolve/main/%E5%85%AB%E9%87%8D%E7%A5%9E%E5%AD%90.zip -O ./wav/八重神子.zip
!wget https://huggingface.co/datasets/guetLzy/genshin/resolve/main/%E9%A6%99%E8%8F%B1.zip -O ./wav/香菱.zip
```
#### 3.2数据集解压
```
!unzip  -j ./wav/八重神子.zip "*.wav" -d ./wav/八重神子/
!unzip  -j ./wav/申鹤.zip "*.wav" -d ./wav/申鹤/
!unzip  -j ./wav/香菱.zip "*.wav" -d ./wav/香菱/
```
#### 3.3 数据集格式
```
wav
├───神里绫华
├   ├───xxx.wav
├   ├───...
├   ├───yyy.wav
├   └───zzz.wav
├───刻晴
├   ├───xxx.wav
├   ├───...
├   ├───yyy.wav
├   └───zzz.wav
├───...
├
└───钟离
    ├───xxx.wav
    ├───...
    ├───yyy.wav
    └───zzz.wav
```
### 4.重命名并删除多余数据，请设置keep_num的值
```
import os
from tqdm import tqdm
# 每个说话人保留的语音条数
keep_num = 500
for dir_name in os.listdir("./wav"):
  if dir_name.endswith("zip") or dir_name.endswith("md") :
    os.remove(os.path.join("./wav",dir_name))

for dir_name in tqdm(os.listdir("./wav/")):
    folder_path = './wav/' + dir_name
    num = 1
    for file in tqdm(os.listdir(folder_path)):
        if(num <= keep_num):
            s = '%06d' % num  # 前面补零占位
            os.rename(os.path.join(folder_path, file), os.path.join(folder_path, str(s) + '.wav'))
        else:
            os.remove(os.path.join(folder_path, file))
        num += 1
```
### 5.标注音频并且进行重采样
```
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/medium.pt -O ./whisper_model/medium.pt
!python generate_filelist.py
```
### 6.生成音标文件并且划分训练集和验证集
```
!python  preprocess_text.py
```
### 7.运行monotonic_align
```
%cd monotonic_align/
!mkdir monotonic_align
!python setup.py build_ext --inplace
%cd ..
```
### 8.下载底模
```
!mkdir -p ./logs/{train_name}
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/DUR_0.pth -O ./logs/{train_name}/DUR_0.pth
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/D_0.pth -O ./logs/{train_name}/D_0.pth
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/G_0.pth -O ./logs/{train_name}/G_0.pth
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/pytorch_model.bin -O ./bert/chinese-roberta-wwm-ext-large/pytorch_model.bin
!wget https://huggingface.co/guetLzy/Bert-Vits2-PreWeight/resolve/main/medium.pt -O ./whisper_model/medium.pt
```
### 9. 生成bert.pt文件
```
!python bert_gen.py
```
### 10.开始训练
```
!python train_ms.py -c configs/config.json -m {train_name}
```
## 线上使用
推荐使用[Colab](https://colab.research.google.com/drive/1qPN_taeNU2GNPJl45mm5kQdEWqC-S3sc?usp=sharing)
## References
+ [anyvoiceai/MassTTS](https://github.com/anyvoiceai/MassTTS)
+ [jaywalnut310/vits](https://github.com/jaywalnut310/vits)
+ [p0p4k/vits2_pytorch](https://github.com/p0p4k/vits2_pytorch)
+ [svc-develop-team/so-vits-svc](https://github.com/svc-develop-team/so-vits-svc)
+ [PaddlePaddle/PaddleSpeech](https://github.com/PaddlePaddle/PaddleSpeech)
## 感谢所有贡献者作出的努力
<a href="https://github.com/fishaudio/Bert-VITS2/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=fishaudio/Bert-VITS2" />
</a>


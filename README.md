# chinese-dialect-recognition

## 20230823更新
大模型！大模型！大模型！
打不过了我学还不行么。 各类语音数据集以及预训练模型。

### Dataset 资源连接  
3D-Speaker https://3dspeaker.github.io  
AST: Audio Spectrogram Transformer https://www.dropbox.com/s/cv4knew8mvbrnvq/audioset_0.4593.pth?dl=1  
CI-AVSR https://github.com/HLTCHKUST/CI-AVSR  
MUSAN —— (OpenSLR17)  
rirs_noises —— (OpenSLR28)  
CNCeleb —— (OpenSLR82)  
CommonVoice https://huggingface.co/datasets/common_voice  
CommonVoice-zh https://commonvoice.mozilla.org/zh-CN/datasets  
CSTR_VCTK https://datashare.ed.ac.uk/handle/10283/2950  
HAL https://ai.100tal.com/openData/voice  
KeSpeech https://github.com/KeSpeech/KeSpeech  
multilingual_librispeech https://huggingface.co/datasets/multilingual_librispeech  
OpenSLR https://www.openslr.org/resources.php  
VoxCeleb1/2 https://www.robots.ox.ac.uk/~vgg/data/voxceleb  
中国语言保护平台 https://zhongguoyuyan.cn/index  

### Pretrained model 资源连接  
AST: Audio Spectrogram Transformer https://www.dropbox.com/s/cv4knew8mvbrnvq/audioset_0.4593.pth?dl=1  
chinese_speech_pretrain https://github.com/TencentGameMate/chinese_speech_pretrain  
chinese-hubert-large https://huggingface.co/TencentGameMate/chinese-hubert-large  
ECAPA-TDNN https://github.com/TaoRuijie/ECAPA-TDNN  
hubert-large-ll60k https://huggingface.co/facebook/hubert-large-ll60k  
Meta MMS https://huggingface.co/facebook/mms-lid-4017  
ccc-wav2vec2-base-100h https://huggingface.co/vasista22/ccc-wav2vec2-base-100h  
wav2vec2-large-xlsr-53-chinese-zh-cn https://huggingface.co/jonatasgrosman/wav2vec2-large-xlsr-53-chinese-zh-cn  
wav2vec2-large-xlsr-53-chinese-zh-cn-gpt https://huggingface.co/ydshieh/wav2vec2-large-xlsr-53-chinese-zh-cn-gpt  
wav2vec2-large-xlsr-53-english https://huggingface.co/jonatasgrosman/wav2vec2-large-xlsr-53-english  
wav2vec2-xls-r-2b https://huggingface.co/facebook/wav2vec2-xls-r-2b  
wav2vec2-xls-r-1b https://huggingface.co/facebook/wav2vec2-xls-r-1b  
wavlm-base-plus https://huggingface.co/microsoft/wavlm-base-plus  
wavlm-large https://huggingface.co/microsoft/wavlm-large  
whisper-base https://huggingface.co/openai/whisper-base  
whisper-large-v2 https://huggingface.co/openai/whisper-large-v2  
whisper-large-zh-cv11 https://huggingface.co/jonatasgrosman/whisper-large-zh-cv11  


## 20220809更新
站在2022的时间点往回看，当初的语种分类模型早已时过境迁。这4年，从最初的RNN到CNN的代表作Resnet系列，再到Transformer各个领域通吃，可供选择的模型、数据增强的方法、优化策略已眼花缭乱。  
【目前的思路】   
有语音文本标签：先预训练ASR模型，再将decoder部分换成分类层训练分类模型  
无语音文本标签：wav2vec无监督预训练


## 2018 Chinese Dialect Classification  
### Background:
For this challenge, a database covering China's 10 major dialects were provided by iFLYTEK which include Changsha Dialect, Hebei Dialect, Nanchang Dialect, Shanghai Dialect, Fujian Dialect and Kejia Dialect,Ningxia Dialect,Hefei Dialect,Sichuan Dialect and Shan3xi Dialect. In this task, challengers were required to build a system that automatically identifies and assorts the audio files with different durations ( >3s for the task) provided in the challenge. 

### Network used in this work
![image](https://github.com/Colt1990/chinese-dialect-recognizaiton/blob/master/image/dialect_recognition.svg)  

![image](https://github.com/Colt1990/chinese-dialect-recognizaiton/blob/master/image/network.png)
LanNet(  
  (layer1): Sequential(  
    (GRU): GRU(40, 512, num_layers=2, batch_first=True)  
  )  
  (layer2): Sequential(  
    (linear): Linear(in_features=512, out_features=192, bias=True)  
  )  
  (layer3): Sequential(  
    (linear): Linear(in_features=192, out_features=10, bias=True)  
  )  
) 

During the training process, the initial learning rate was choosen as 0.05. The optimizer was SGD with momentum=0.9. 
After four epoch training, the learning rate would be halved for every epoch.


### Feature used in this work
The following are the parameters used for HTK tools to obtain the FilterBank feature from the raw PCM files(16000Hz，16bit).
OURCEFORMAT = NOHEAD  
SOURCERATE = 625  
HEADERSIZE = 44  
TARGETKIND = FBANK  
TARGETRATE = 80000.0  
ZMEANSOURCE = T  
WINDOWSIZE = 200000.0  
USEHAMMING = T  
PREEMCOEF = 0.97  
NUMCHANS = 40  
LOFREQ = 0  
HIFREQ = 5500  
USEPOWER = F  
NUMCEPS = 12  
CEPLIFTER = 22  
SAVEWITHCRC = F  

### What I learned from this challenge
・PLP feature is quite useful for dialect recognition. Although MFCC and FilterBank features were tried in this work and FilterBank showed better performance than MFCC, PLP may give the best performance from the results of other contestants.  
・VAD processing and FilterBank feature with <5.5KHz frequencies were found quite useful in this task.    
・It is worth a try to use the powerful CNN network Resnet. Although I tried VGG16, failing to get a good performance.  
・For these ten kinds of dialect, some dialects like Sichuan and Shanghai are easily to be recognized when compared to other dialects. So two step learning(coarse classification and fine classification) or multi-task learning may improve your performance.

### Requirments
pytorch 0.4.0  
webrtcvad (you can check https://github.com/wiseman/py-webrtcvad to do the vad process)


### Reference
1. http://yerevann.github.io/2016/06/26/combining-cnn-and-rnn-for-spoken-language-identification/
2. https://www.kaggle.com/c/tensorflow-speech-recognition-challenge/discussion/46945

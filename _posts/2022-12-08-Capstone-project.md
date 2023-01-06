---
layout: post
title: Capstone project
author: [Yu-Xin, Lai]
category: [Lecture]
tags: [jekyll, ai]
---

以手機所錄取之手勢動作資料上傳至 Kaggle 平台訓練 AI 模型後,交由電腦啟動辨識服務器, 再以 AirDigit App傳送手勢動作之資料至電腦進行辨識。

---

---
⼈⼯智慧與物聯網裝置設計 期末專題
小組組員:電機4A 01053307 賴昱新

### AirDigit Recognition (手機手勢辨識)


**專題實作步驟:**
1. 開發 AirDigit 手機應用程式

    -在 MIT APP INVENTOR 進行 APP 開發
    
    -安裝 AirDigit 於手機(本專題使用安卓)
    
2. 使用 AirDigit App 紀錄手勢動作以建立資料集

    -進行資料集取樣
    
    -將檔案 AirDigit.zip 上傳至 Kaggle Datatsets
    
    -須將檔案分類成 train,test
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/2.png?raw=true)
    
    **Train: 0~10 各20筆訓練資料**<br /> 
    **Test: 0~10 各2筆訓練資料**<br />
    
3. 於Kaggle 平台使用資料集訓練 AI 模型

    -Kaggle 上執行 Airdigit-Classification 進行模型訓練
    
    -將程式執行結果 (Output) 的 airdigit_cnn.h5 模型檔，下載至電腦
    
    -把 airdigit_cnn.h5 檔案移至 tf/models 資料夾
    
4. 於電腦上開啟 AI 辨識服務器

    -打開 Anaconda3 安裝環境，執行 python airdigit_cnn.py

5. 進行手勢動作辨識
   
   -打開 AirDigit App
   
   -設定好檔案名稱及時間，按取樣上傳錄取動作資料並上傳至辨識服務器，然後回傳辨
    識結果
    
**程式Kaggle:**[yuxin01053307/airdigit-classification](https://www.kaggle.com/code/yuxin01053307/airdigit-classification)

** 檢查tensorflow版本, 並安裝與kaggle平台相同之tensorflow版本**<br>
`python -c 'import tensorflow as tf; print(tf.__version__)'`<br />
`pip install tensorflow==2.8.0` (安裝與kaggle相同之tensorflow版本)<br />

![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/1.png?raw=true)

**姿態辨識服務器之程式樣本** (PC with Camera)<br>

```
model = models.load_model('models/pose_dnn.h5')
labels = ['stand', 'raise-right-arm', 'raise-left-arm', 'cross arms','both-arms-left']

cap = cv2.VideoCapture(0)

while(cap.isOpened()):
    ret, frame = cap.read()
    image = cv2.cvtColor(frame,cv2.COLOR_BGR2RGB)
    
    mmdet_results = inference_detector(det_model, image) # 人物偵測產生BBox
    person_results = process_mmdet_results(mmdet_results, args.det_cat_id) # 記住人物之BBox  
    pose_results, returned_outputs = inference_top_down_pose_model(...) # 感測姿態產生pose keypoints
    
    x_test = np.array(preson_results).reshape(1,16,2) # 將Keypoints List 轉成 numpy Array
    preds = model.fit(x_test) # 辨識姿態動作
    maxindex = int(np.argmax(preds))
    txt = labels[maxindex]
    print(txt)
```

---
### Head Pose Estimation
**Kaggle:** [rkuo2000/head-pose-estimation](https://kaggle.com/rkuo2000/head-pose-estimation)<br>
<iframe width="652" height="489" src="https://www.youtube.com/embed/BHwHmCUHRyQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
### VTuber-Unity 
**Head-Pose-Estimation + Face-Alignment + GazeTracking**<br>

<u>Build-up Steps</u>:
1. Create a character: **[VRoid Studio](https://vroid.com/studio)**
2. Synchronize the face: **[VTuber_Unity](https://github.com/kwea123/VTuber_Unity)**
3. Take video: **[OBS Studio](https://obsproject.com/download)**
4. Post-processing:
 - Auto-subtitle: **[Autosub](https://github.com/kwea123/autosub)**
 - Auto-subtitle in live stream: **[Unity_live_caption](https://github.com/kwea123/Unity_live_caption)**
 - Encode the subtitle into video: **[小丸工具箱](https://maruko.appinn.me/)**
5. Upload: YouTube
6. [Optional] Install CUDA & CuDNN to enable GPU acceleration
7. To Run <br>
`$git clone https://github.com/kwea123/VTuber_Unity` <br>
`$python demo.py --debug --cpu` <br>

<p align="center"><img src="https://github.com/kwea123/VTuber_Unity/blob/master/images/debug_gpu.gif?raw=true"></p>

---
### OpenVtuber
<u>Build-up Steps</u>:
* Repro [Github](https://github.com/1996scarlet/OpenVtuber)<br>
`$git clone https://github.com/1996scarlet/OpenVtuber`<br>
`$cd OpenVtuber`<br>
`$pip3 install –r requirements.txt`<br>
* Install node.js for Windows <br>
* run Socket-IO Server <br>
`$cd NodeServer` <br>
`$npm install express socket.io` <br>
`$node. index.js` <br>
* Open a browser at  http://127.0.0.1:6789/kizuna <br>
* PythonClient with Webcam <br>
`$cd ../PythonClient` <br>
`$python3 vtuber_link_start.py` <br>

<p align="center"><img src="https://camo.githubusercontent.com/83ad3e28fa8a9b51d5e30cdf745324b09ac97650aea38742c8e4806f9526bc91/68747470733a2f2f73332e617831782e636f6d2f323032302f31322f31322f72564f33464f2e676966"></p>

---
### Hand Pose
![](https://github.com/facebookresearch/InterHand2.6M/blob/main/assets/teaser.gif?raw=true)
**Dataset:** [InterHand2.6M](https://github.com/facebookresearch/InterHand2.6M)<br>

1. Download pre-trained InterNet from [here](https://drive.google.com/drive/folders/1BET1f5p2-1OBOz6aNLuPBAVs_9NLz5Jo?usp=sharing)
2. Put the model at `demo` folder
3. Go to `demo` folder and edit `bbox` in [here](https://github.com/facebookresearch/InterHand2.6M/blob/5de679e614151ccfd140f0f20cc08a5f94d4b147/demo/demo.py#L74)
4. run `python demo.py --gpu 0 --test_epoch 20`
5. You can see `result_2D.jpg` and 3D viewer.

**Camera positios visualization demo**
1. `cd tool/camera_visualize`
2. Run `python camera_visualize.py`


<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*


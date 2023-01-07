---
layout: post
title: Capstone project
author: [Yu-Xin, Lai]
category: [Lecture]
tags: [jekyll, ai]
---

⼈⼯智慧與物聯網裝置設計 期末專題

小組組員:電機4A 01053307 賴昱新

### AirDigit Recognition (手機手勢辨識)

---

以手機所錄取之手勢動作資料上傳至 Kaggle 平台訓練 AI 模型後，交由電腦啟動辨識服務器，再以 AirDigit App 傳送手勢動作之資料至電腦進行辨識。

---

## 原理、操作流程
使用 AirDigit App 將空中手寫數字之手勢動作錄取三軸加速器資料儲存，然後上傳至 Kaggle 平台建立資料集來訓練 AI 模型，
再交由電腦執行辨識服務器程式，且輸入電腦所判別的服務器工作阜至 App 的 URL 位置，之後打開空中手寫數字 App， 傳送動作資料至 PC 進行辨識，並回覆數字顯示於 App 上。

---

 ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/圖片1.png?raw=true)

## 專題實作步驟:
1. 開發 AirDigit 手機應用程式

    -在 MIT APP INVENTOR 進行 APP 開發
    
    -安裝 AirDigit 於手機(本專題使用安卓手機)
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/14.jpg?raw=true)
    
2. 使用 AirDigit App 紀錄手勢動作以建立資料集

    -進行資料集取樣
    
    -將檔案 AirDigit.zip 上傳至 Kaggle Datatsets
    
    -須將檔案分類成 train,test
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/2.png?raw=true)
    
    **Train: 0~10 各20筆訓練資料**<br /> 
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/13.png?raw=true)
    
    **Test: 0~10 各2筆訓練資料**<br />
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/12.png?raw=true)
    
3. 於Kaggle 平台使用資料集訓練 AI 模型

    -Kaggle 上執行 Airdigit-Classification 進行模型訓練
    
    **程式Kaggle:**[yuxin01053307/airdigit-classification](https://www.kaggle.com/code/yuxin01053307/airdigit-classification)

    ** 檢查tensorflow版本, 並安裝與kaggle平台相同之tensorflow版本**<br>
    `python -c 'import tensorflow as tf; print(tf.__version__)'`<br />
    `pip install tensorflow==2.8.0` (安裝與kaggle相同之tensorflow版本)<br />

    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/1.png?raw=true)

    
    -將程式執行結果 (Output) 的 airdigit_cnn.h5 模型檔，下載至電腦
    
    -把 airdigit_cnn.h5 檔案移至 tf/models 資料夾
    
4. 於電腦上開啟 AI 辨識服務器

    -打開 Anaconda3 安裝環境，執行 python airdigit_cnn.py

5. 進行手勢動作辨識
   
   -打開 AirDigit App
   
   -按取樣上傳錄取動作資料並上傳至辨識服務器，然後回傳辨識結果
 
## 程式說明:

(airdigit_cnn.py)

`# for AirDigit Recognizer App`

`# $./ngork http 5000` 

`import pandas as pd` //引用panda

`import numpy as np`  //引用numpy

`from flask import Flask, request, jsonify` //記得要另外 pip install flask
 
`from tensorflow.keras import models`

`app = Flask(__name__)`

`labels = ['zero', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'none']` //設0~9&10(none)的標籤

`model = models.load_model('models/airdigit_cnn.h5')` //引用tf/models資料夾裡kaggle訓練結果output(.h5檔)
	
`@app.route("/", methods=["GET"])`

`def hell():`

    return "Hello!"
	
`@app.route("/predict", methods=["POST"])`

`def predict():`

    keys = request.form.to_dict().keys()
    
    data = list(keys)[0][:-1].split(',')
    
    if len(data)<72: # patch data to 72
    
    	for i in range(int((72-len(data))/3)):
            
	    data = data + ['0.0','9.8','0.0'] # patch ax,ay,az = 0.0,9.8,0.0
	    
    if len(data)>72: # cut data to 72
    
       data = data[:72]
       
    x_test = np.asarray(data, dtype=float) # convert string list to float array
    
    x_test = x_test.reshape(-1,24,3) # reshape x_test
    
    preds = model.predict(x_test) # run model to predict
   
    pred = np.argmax(preds) # find maximum probility
    
    print(pred, labels[pred]) //印出辨識出來的結果
    
    return labels[pred]

`if __name__ == "__main__":`

    app.run(host="0.0.0.0", port=5000, debug=True)


## 成果:

![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/11.jpg?raw=true)
![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/10.jpg?raw=true)

## 心得&結論:

我在想或許可以將這個實作應用於打電話，但好像有點麻煩。我另外又想到也許手機可以設定當你畫出某個數字時，想要啟動什麼服務，比如說0:播音樂，1:打開相機之類的。目前有看過三星的 S Pen，有應用這種空中手勢操作的概念，但原理可能不盡相同。

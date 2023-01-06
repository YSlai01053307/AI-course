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
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/4.jpg?raw=true)
    
2. 使用 AirDigit App 紀錄手勢動作以建立資料集

    -進行資料集取樣
    
    -將檔案 AirDigit.zip 上傳至 Kaggle Datatsets
    
    -須將檔案分類成 train,test
    
    ![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/2.png?raw=true)
    
    **Train: 0~10 各20筆訓練資料**<br /> 
    **Test: 0~10 各2筆訓練資料**<br />
    
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
    
**程式說明:**

**成果:**
![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/5.jpg?raw=true)
![](https://github.com/YSlai01053307/AI-course/blob/gh-pages/6.jpg?raw=true)

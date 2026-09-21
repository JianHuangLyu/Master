# [Project2] A Real-Time Style Transfer System

## Description
- 擔任專案領導者、規劃Pipeline及PTQ實作

- 本專案主要做即時影像風格轉換並最後部署邊緣裝置Xaiver中達成推論

- 透過找尋預訓練風格轉換模型並使用Post Training Quantization進行模型量化使其最終能在Xaiver上使用TensorRT在XaiverGPU模式下達成推論加速

- 在Xaiver上使用TensorRT INT8推論(0.1295 sec)相較CPU FP32推論(7.8896 sec)加速約60倍

- INT8量化使TensorRT Engine由47.83 MB壓縮至9.72 MB,模型大小降低約80%

- 在Xaiver GPU模式下,INT8相較FP32達成2.55倍加速(speedup),最終達成7.72 FPS @ 512x512之即時Camera推論

<br>


## Workflow Pipeline Design
![pipeline](Fig/pipeline-design.png)

**Source:** 呂建篁

<br>

## Model Architecture

![Model](Fig/model.jpg)
**Reference Paper:**  
Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization. Xun Huang, Serge Belongie. ICCV 2017 


**Style Dataset:** Painter by Numbers dataset

**Scene Dataset:** COCO 2014 Training dataset

<br>

## Post Training Quantization
![PTQ](Fig/PTQ.png)

**動機:**
- 考量裝置在運算資源有限
- 不追求精度


**Calibration Data:**  
同上面Style Dataset及Scene Dataset,只選用20張作為校正使用，目的在於讓模型各層 activation 在「真實資料」下數值分布


**Xaiver端:**
- 在Xaiver上進行PTQ量化(從模型FP32至TensorRT INT8)

- 使用TensorRT INT8專屬Calibrator進行校正

- 將量化後模型搭配Xaiver在GPU Mode 達成推論加速

- 對Weight和Activate進行量化

<br>

## Quantization Results
![table1](Fig/Result1.png)

![table2](Fig/Result2.png)

**數據說明:**
- 模型壓縮率約80%係以 FP32 TensorRT Engine (47.83 MB) 對比 INT8 TensorRT Engine (9.72 MB) 計算
- Speedup 2.55x 係 Xaiver GPU 模式下 INT8 相較 FP32 之加速 (0.3307 sec -> 0.1295 sec)
- 約60倍加速係跨裝置比較: Xaiver CPU FP32 (7.8896 sec) -> Xaiver GPU INT8 (0.1295 sec)
- 以上推論時間之量測輸入解析度均為 512x512

<br>

## GUI And Demo
![Results1](Fig/1.jpg)
![Results2](Fig/2.jpg)
![Results3](Fig/3.jpg)
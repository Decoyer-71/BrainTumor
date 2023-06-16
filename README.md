# BrainTumor

## 주제 
    TL(Transfer Learning) 아키텍쳐를 활용한 브레인 튜머 이미지 예측모델 생성

## 사용언어
<a href="https://www.python.org/" target="_blank"><img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white"/></a>
<a href="https://jupyter.org/" target="_blank"><img src="https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white"/></a>

## 폴더 분류
[code](https://github.com/Decoyer-71/BrainTumor/tree/master/code) : 학습 및 모델생성 코드

[data](https://github.com/Decoyer-71/BrainTumor/tree/master/data) : Train, Test용 분류된 이미지 파일


## 1. Data Set
    1) 출처 : [Kaggle Brain Tumor Classification (MRI)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri)
    2) 구성
        - Traning 폴더 : 2,872
        - Testing 폴더(lable용) : 374

## 2. 프로젝트 개요
    1) 목표 : Testing 결과 acc 0.90 이상 정확도 
    2) 소스명 : 
    3) 개발환경 
        - data폴더 -> env_list.txt
        - Google colab(fitting 용)
        
## 아키텍처별 실험(Goole colab)
### 1) Mobilenet
        (1) Hiddenlayer, Dropout 미설정 / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : loss 1.7956, acc 0.7208
            나. 소요시간 : 0:06:04
            다. 평가 : 과적합 심함
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/f9d138c7-657f-400d-a340-5d00aaf5072c)
        (2) Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : loss 0.9579, acc 0.7563
            나. 소요시간 : 0:06:58
            다. 평가 : 과적합은 없으나, evaluate 저조
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/30aae419-c0b0-44c4-9e55-b6d7ac9644d7)
        (3) Hiddenlayer(3개, node : 256), Dropout(0.25) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : loss 1.1784, acc: 0.7335
            나. 소요시간 : 0:05:59
            다. 평가 
                - Hiddenlayer node가 증가해도 test 와 train, validation 결과의 차이가 심함
                - train과 test 이미지간 편향 의심, shuffle 후 재분배하여 추가 test필요
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/4b664bf6-af9d-4763-b474-365b12760194)
        (4) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : loss 0.2921, acc: 0.8954
            나. 소요시간 : 0:05:10
            다. 평가 
                - shuffle 후 evaluate 결과 개선
                - 다소 과적합 발생, Dropout 수치를 상향조정하여 재실험       
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/dc645b45-4e0e-4248-af71-3d0803acfb67)
        (5) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.3) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : 
            나. 소요시간 : loss 0.2881, acc: 0.9049
            다. 평가 : 목표수치 도달, 과적합 해
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/8cf95a18-87ee-46ba-98c9-f5dc397a9990)

### 2) Xception
        (1) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : 
            나. 소요시간 : 
            다. 평가 : 



## 결론

![aaa]()


  

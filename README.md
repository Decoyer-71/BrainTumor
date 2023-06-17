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
        - Testing 폴더 : 374
        - Label : giloma_tumor, meningioma_tumor, no_tumor, pituitary_tumor

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
            다. 평가 : 목표수치 도달, 과적합 해소
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/8cf95a18-87ee-46ba-98c9-f5dc397a9990)


### 2) Xception 
        (1) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5)
            가. Evaluate 결과 : loss 0.5104, acc: 0.8764
            나. 소요시간 : 0:10:54
            다. 평가 
                - 과적합이 심하며, validation이 고르지 못함
                - Dropout과 optimizer 수치조정 후 재실험
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/ce9e773b-692b-4850-ac29-106a49a3deb9)

        (2) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.3) / Optimizer : Adam(2e-5)
            가. Evaluate 결과 : loss 0.6588, acc 0.8986
            나. 소요시간 : 0:12:32
            다. 평가 
                - 여전한 과적합, validation 그래프는 상대적으로 안정됨
                - Dropout 비율을 상향조정 후 재실험
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/e88fe16e-fd93-4a8a-9854-fe04f9f5facb)

        (3) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.5) / Optimizer : Adam(2e-5)
            가. Evaluate 결과 : loss 0.4918, acc 0.9017
            나. 소요시간 : 0:14:33
            다. 평가 
                - Hidden layer를 늘려서 테스트도 시도해보았으나, 정확도가 50 ~ 60대로 떨어지는 것을 확인해, Dropout의 수치만 상향조정
                - 유효한 성능을 보였으나, train과 validaion loss 수치에서 과적합이 발생
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/7d301fe7-9556-4c86-a2a4-2cfc648e4e4b)


## 결론
    1) Tuning 한 후 Test와 Train, Validation의 결과가 지속적으로 과도하게 차이가 나면 데이터의 편향을 의심하여 전체 Data를 섞어서 재분배 처리를 하는 것이 유효함
    2) Mobilenet이 상대적으로 적은 소요시간이 걸리는 장점을 가지며, 90%이상의 정확도 모델을 만드려면 Hiddenlayer(3개, node : 128), Dropout(0.3), Optimizer : Adam(1e-5) 설정필요
    3) Xception은 Mobilenet에 비해 긴 소요시간(12 ~ 14분)을 보여주며, 90%이상의 정확도 모델을 구현하기 위해 Dropout 수치를 상향하고 Optimizer의 Learning_rate 수치를 낮춰서 학습시켜야한다. Mobilenet에 비해 과적합이 쉽게 발생하는 현상을 보임.
    4) 



  

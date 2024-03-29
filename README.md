# BrainTumor

## 주제 
    TL(Transfer Learning) 아키텍쳐를 활용한 브레인 튜머 이미지 분류모델 생성

## 사용언어 및 모듈
<a href="https://www.python.org/" target="_blank"><img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white"/></a>
<a href="https://jupyter.org/" target="_blank"><img src="https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white"/></a>
<a href="https://www.tensorflow.org/?hl=ko" target="_blank"><img src="https://img.shields.io/badge/Tensorflow-FF6F00?style=flat&logo=tensorflow&logoColor=white"/></a>
<a href="https://keras.io/" target="_blank"><img src="https://img.shields.io/badge/Keras-D00000?style=flat&logo=keras&logoColor=white"/></a>
<a href="https://scikit-learn.org/stable/index.html" target="_blank"><img src="https://img.shields.io/badge/Scikitlearn-F7931E?style=flat&logo=Scikitlearn&logoColor=white"/></a>
<a href="https://numpy.org/" target="_blank"><img src="https://img.shields.io/badge/Numpy-013243?style=flat&logo=numpy&logoColor=white"/></a>
<a href="https://pandas.pydata.org/" target="_blank"><img src="https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white"/></a>

## 폴더 분류
[code](https://github.com/Decoyer-71/BrainTumor/tree/master/code) : 학습 및 모델생성 코드

[data](https://github.com/Decoyer-71/BrainTumor/tree/master/data) : Train, Test용 분류된 이미지 파일


## 1. Data Set
    1) 출처 : [Kaggle Brain Tumor Classification (MRI)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri)
    2) 구성
        - Traning 폴더 : 2,872
        - Testing 폴더 : 374
        - Label : giloma_tumor, meningioma_tumor, no_tumor, pituitary_tumor

## [Image Sample]
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/b6264b69-bc08-4158-a6e8-7700b8490142)


## 2. 프로젝트 개요
    1) 목표 : Testing 결과 acc 0.90 이상 정확도 
    2) 소스명 : data 폴더 -> Kaggle_Brain Tumor Classification.ipynb
    3) 개발환경 
        - data폴더 -> env_list.txt
        - Google colab(fitting 용)
        
## 아키텍처별 실험(Goole colab)
    - 공통사항 : FC계층에 대해 세부 수치변경을 통해 목표달성 모델 생성(FineTuning)
### 1) Mobilenet 
        (1) Hiddenlayer, Dropout 미설정 / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 1.7956, acc 0.7208
            나. 소요시간 : 0:06:04
            다. 평가 : 과적합 심함
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/f9d138c7-657f-400d-a340-5d00aaf5072c)

        (2) Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 0.9579, acc 0.7563
            나. 소요시간 : 0:06:58
            다. 평가 : 과적합은 없으나, evaluate 저조
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/30aae419-c0b0-44c4-9e55-b6d7ac9644d7)

        (3) Hiddenlayer(3개, node : 256), Dropout(0.25) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 1.1784, acc: 0.7335
            나. 소요시간 : 0:05:59
            다. 평가 
                - Hiddenlayer node가 증가해도 test 와 train, validation 결과의 차이가 심함
                - train과 test 이미지간 편향 의심, shuffle 후 재분배하여 추가 test필요
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/4b664bf6-af9d-4763-b474-365b12760194)

        (4) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 0.2921, acc: 0.8954
            나. 소요시간 : 0:05:10
            다. 평가 
                - shuffle 후 evaluate 결과 개선
                - 다소 과적합 발생, Dropout 수치를 상향조정하여 재실험       
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/dc645b45-4e0e-4248-af71-3d0803acfb67)

        (5) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.3) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : 
            나. 소요시간 : loss 0.2881, acc: 0.9049
            다. 평가 : 목표수치 도달, 과적합 해소
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/8cf95a18-87ee-46ba-98c9-f5dc397a9990)


### 2) Xception 
        (1) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 0.5104, acc: 0.8764
            나. 소요시간 : 0:10:54
            다. 평가 
                - 과적합이 심하며, validation이 고르지 못함
                - Dropout과 optimizer 수치조정 후 재실험
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/ce9e773b-692b-4850-ac29-106a49a3deb9)

        (2) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.3) / Optimizer : Adam(2e-5) / epochs : 20
            가. Evaluate 결과 : loss 0.6588, acc 0.8986
            나. 소요시간 : 0:12:32
            다. 평가 
                - 여전한 과적합, validation 그래프는 상대적으로 안정됨
                - Dropout 비율을 상향조정 후 재실험
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/e88fe16e-fd93-4a8a-9854-fe04f9f5facb)

        (3) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.5) / Optimizer : Adam(2e-5) / epochs : 20
            가. Evaluate 결과 : loss 0.4918, acc 0.9017
            나. 소요시간 : 0:14:33
            다. 평가 
                - Hidden layer를 늘려서 테스트도 시도해보았으나, 정확도가 50 ~ 60대로 떨어지는 것을 확인해, Dropout의 수치만 상향조정
                - 유효한 성능을 보이지만, train과 validaion loss 수치에서 다소 과적합이 발생
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/7d301fe7-9556-4c86-a2a4-2cfc648e4e4b)


### 3) ResNet50
        (1) [Shuffle한 Data set] Hiddenlayer(3개, node : 128), Dropout(0.25) / Optimizer : Adam(1e-5) / epochs : 20
            가. Evaluate 결과 : loss 3.4639, acc 0.1268
            나. 소요시간 : 0:03:50
            다. 평가 
                - val_loss 수치의 변동이 거의 없어 epochs 6회 만에 EarlyStopping에 의해 정지
                - 위의 두 모델과 같은 조건으로 실험을 시작할 시 매우 낮은 성능을 보여주어, 성능개선을 위해 Hidden layer와 node수치를 파라미터가 증가하도록 조정하고 Dropout 및 epochs 수치 증가하여 재실험(추이를 관찰하기 위해 EarlyStopping 해제)
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/a3a96d6e-8f44-4540-ab33-ddf6ac5242c4)

        (2) [Shuffle한 Data set] Hiddenlayer(6개, node : 256), Dropout(0.6) / Optimizer : Adam(1e-5) / epochs : 80
            가. Evaluate 결과 : loss 0.9763, acc 0.5769
            나. 소요시간 : 0:42:14
            다. 평가 : epochs 35회를 넘어가면서 유의미한 학습이 이루어지지 않아 목표미달성
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/4309b060-398f-4b92-be88-9efed61f42ae)

        (2) [Shuffle한 Data set] Hiddenlayer(6개, node : 256), Dropout(0.6) / Optimizer : SGD(1e-5) / epochs : 35
            가. Evaluate 결과 : loss 1.3845, acc 0.2837
            나. 소요시간 : 0:16:47
            다. 평가 : Optimizer를 변경해서 시도도 해보았으나 오히려 성능이 하락
![image](https://github.com/Decoyer-71/BrainTumor/assets/127948197/a41171fc-d140-4af7-ab28-6391f91a00be)


## 결론
### 1) Tuning 한 후 Test와 Train, Validation의 결과가 지속적으로 과도하게 차이가 나면 데이터의 편향을 의심하여 전체 Data를 섞어서 재분배 처리를 하는 것이 유효함
### 2) Mobilenet이 상대적으로 적은 소요시간이 걸리는 장점을 가지며, 90%이상의 정확도 모델을 만드려면 Hiddenlayer(3개, node : 128), Dropout(0.3), Optimizer : Adam(1e-5) 설정필요
### 3) Xception은 Mobilenet에 비해 긴 소요시간(12 ~ 14분)을 보여주며, 90%이상의 정확도 모델을 구현하기 위해 Dropout 수치를 상향하고 Optimizer의 Learning_rate 수치를 낮춰서 학습시켜야한다. Mobilenet에 비해 과적합이 쉽게 발생하는 현상을 보임.
### 4) ResNet은 같은조건에서 Mobilenet과 Xception에 비해 저조한 성능을 보이며, 성능을 향상하기 위한 파라미터 수치를 높여도 목표 정확도에는 미치지 못함
### 5) Brain_Tumor 이미지 판별모델을 생성 시 소요시간이 적으면서 높은 성능을 보여주는 Mobilenet기반 모델을 사용하는 것이 유용함 
    



  

# PROJECT - 01 Timeseries_Anomaly_Detection
## 주제) 시계열 데이터를 이용한 제조공정에서의 이상탐지
### 1. 프로젝트 개요
- **데이터 정보**
    - 인공지능 중소벤처 제조 플랫폼
    - 제공기관 KAIST

    [품질 이상탐지,진단(전해탈지) AI 데이터셋](https://www.kamp-ai.kr/front/dataset/AiDataDetail.jsp?AI_SEARCH=&page=2&DATASET_SEQ=20&EQUIP_SEL=&FILE_TYPE_SEL=&GUBUN_SEL=&WDATE_SEL=)
    
- **프로젝트 목적**
    - 생산제품의 이상을 조기에 탐지할 수 있는 모델을 만들어 이상 제품을 탐지하는 것
- **프로젝트 기대효과**
    - 학습된 모델을 통해 진행되는 품질검사로 품질검사에 투입되는 인건비와 시간 절감 기대
    - 공정 데이터를 학습모델에 적용시 품질예측 가능
    - 전처리 공정인 전해탈지 공정의 품질예측 후 진행되는 공정 및 완제품 품질 향상에 기여
    - 이 외의 공정에도 수정 모델을 적용해 품질 예측 활용에 적용 가능
### 2. 전처리
    - 그룹화, 정규화, Train-valid-test 분할
    - 가공되어 올라온 데이터이므로 결측치 제거 불필요
    - 일반적으로는 이상치 처리가 필요하지만, 이상탐지 모델이기 때문에 이상치를 활용
    - 나누어져 있는 공정단위 별 그룹화
    
- **데이터 설명**
    - 주요변수인 Time, pH, Temp, Current가 공정단위에 따라 기록된 시계열 데이터
    - 이상 여부인 Error는 각 공정단위 별 표기
    - pH, Temp, Current는 기준치가 존재하며 해당 값에서 멀어질수록 이상 발생확률 증가
![image](https://user-images.githubusercontent.com/94789091/160402350-3302a40e-0f80-4bd1-821b-5f7bef6667df.png)

- **변수 간 상관관계 파악**
     - 0.2미만의 낮은 상관계수로 변수 간 상관관계가 거의 없음
     - 이는 상관계수만으로 상관관계 파악이 어려워 모델 개발 필요
     
    ![image](https://user-images.githubusercontent.com/94789091/160402703-921abd21-503c-4dba-acf6-15a2f0be49ab.png)

- **파생 변수 생성**

![image](https://user-images.githubusercontent.com/94789091/160403047-50dbd0ed-8bc0-4cec-92c1-215006814c04.png)

### 3. 모델 설명 - Logistic Regression
    - pH 0.6이상, Temp 0.8이상, Current 0.3이하인 경우 가중치 부여
    - 변수의 중요도 별로 가중치를 다르게 부여
    
### 3. 모델 결과

![image](https://user-images.githubusercontent.com/94789091/160403991-34945ba4-7a69-4710-a1b0-f3527ff85762.png)

- **보완점**
    - 해당 공정에 관한 전문적인 조언을 구할 전문가의 부재, 전문 지식의 부족으로 분석에 한계를 보임
    - 데이터의 양이 많지 않아 모델이 충분히 학습되는데 어려웠다고 파악, 충분한 양의 데이터 확보 필요
    - 정상-비정상 분포가 불균형해 모델 학습에 과적합이 발생하여 데이터 불균형 해결방안 필요
    - 앙상블 기법 사용하여 성능 개선 도모

==========================================================================

# PROJECT - 02 Timeseries_Prediction
## 주제) Run-way Functions: Predict Reconfigurations at US Airports (Open Arena)

## 1. 프로젝트 개요

- **대회 정보**
    - 미국의 데이터 Competition Plotform ‘DRIVENDATA’에서 진행 된 Competition
    - Funded by NASA
    
    [Run-way Functions: Predict Reconfigurations at US Airports (Open Arena)](https://www.drivendata.org/competitions/89/competition-nasa-airport-configuration/)
    
- **프로젝트 목적**
    - 미국 10개 공항의 미래에 어떤 활주로가 활성화 될지 예측
- **프로젝트 설명**
    - 공항의 활주로는 항상 모두 사용되는 것이 아님
    - 기상, 시간, 항공량에 따라 활성화 되는 활주로가 다르고 이를 예측하는 것은 효율적인 공항 교통 통제에 있어서 매우 중요 
    - 따라서 과거의 데이터들을 이용해 미래에 어떤 활주로가 활성화 될지 예측하는 모델 개발
- **데이터**
    - 미국 10개 공항 별 이착륙 정보, 활주로 활성화 정보, 기상 데이터

## 2. 데이터 설명
### 2.1 config의 세부 설명 (활주로 구성의 설명)

![image](https://user-images.githubusercontent.com/94789091/162134123-9989f492-e7fa-4e05-a97c-6f035edfa37f.png)

- Runway는 크게 Departure 와 Arrival 두종류로 나뉘어 사용 되며 시간, 기상, 항공량에 따라 조합이 구성 됨
- 위 Table의 airport_config 컬럼이 해당 시간대에 활성화 된 runway의 조합을 뜻하며 D_로 시작하는 것들은 이륙에 사용 된 활주로, A로 시작하는 것들은 착륙에 사용 된 활주로를 뜻 함


**Target 변수 주의점**

![image](https://user-images.githubusercontent.com/94789091/162134573-9816dc2c-9915-4052-8ebf-fde6ab1fc174.png)

- 예측해야하는 변수는 활주로가 아닌 시간대별 '활주로 구성'을 예측하는 다중분류문제
- 공항 별 활주로 구성과 구성의 수가 달라 각각의 모델을 만들어야 할 것으로 보임


### 2.2 기상 데이터

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/999c55b5-55a6-45d2-bdb9-f4ab8d3049dc/Untitled.png)

- 기상 데이터에는 기록 시간, 예측 시간 그리고 온도, 풍향,풍속,등의 데이터가 존재 함

**Feature** 

- 변수로 풍향과 풍속만 사용 됨
- 비행기가 이착륙시 충분한 양력과 안정성을 위해 맞바람이 불어야 함. 즉 풍향은 다른 어떠한 변수 보다도 활주로 사용에 큰 영향을 끼치기 때문에 변수로 사용 됨

**Time** 

- 이 데이터 역시 Configuration과 마찬가지로 Log 성격의 데이터 이기 때문에 시간 간격이 모두 다름
- 이를 일정한 간격을 맞춰 모델에 input 될 때 특정 범위만 사용될 수 있도록 함

## 3. 전처리

- 제공 된 데이터 중 사용 된 것은 활주로 활성화 정보, 기상 데이터가 주로 사용 됨

**시간 간격 통일** 

- 해당 데이터들은 컴퓨터에 자동으로 기록 된 Log 성격의 데이터이기 때문에 기록 시간의 단위가 모두 다름
- 일정한 간격의 시간 단위로 자르기 위해서 30분 간격으로 통일 함

**One- hot Encoding** 

- 이 모델은 일종의 여러개의 Class(Configuration 경우의 수)를 분류하는 Multi class Classification
- 따라서 활성화 횟수가 10회 이하인 것들은 other로 묶고 나머지를 각각 class로 one hot encodling 함


## 4. 모델 설명

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b7eddc14-5c3e-4e67-a44c-e5b98a5b555b/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d531d94f-c4ae-4b81-befe-fb31ab47f288/Untitled.png)

- 기본적인 모델 컨셉은 기준 시간으로 부터 과거 12시간의 데이터를 참조 하여 미래 6시간의 데이터를 예측함
- 과거 12시간 데이터 - 미래 6시간 데이터 그리고 lamp데이터를 모두 연결해주는 key 로 timestamp를 사용 함

### 4.1 **모델 구성**

- 모델은 크게 두개로 나뉨
    - Configuration 데이터를 처리하는 LSTM 네트워크
    - 기상 데이터를 처리하는 임베딩 레이어
- 분류 모델이므로 출력 함수를 softmax를 사용 했으며 loss 로는 categorical crossentropy를 사용 함

**Post process** 

- softmax는 출력 값을 각 class의 확률을 의미 함
- 하지만 submission 에서 각 class의 확률 합이 1이 되도록 요구하기 때문에 모수를 나눔으로써 sub이 1이 되도록 만듬

**10개 공항** 

- 10개 공항의 각 configuration 확률을 모두 각각 예측 해야 함
- 10개 공항을 한번에 학습 하는 것은 수 많은 경우의 수를 유발하기 때문에 정확도가 낮다고 판단 10개 공항 각각 모델을 학습 시키는 것으로 진행 함
- 모델 네트워크는 동일하게 가져가되 데이터만 바꾸어 10개 모델을 만듬

### 4.2. Optimizer

- Optimizer로 Tensorflow Addons API의 **Rectifier Adam**를 사용 함
    - Total steps : 10000
    - warmup_proportion=0.1
    - min_lr=0.00001
    - Adam, SGD, AdamW를 모두 실험을 해 보았지만 RAdam이 가장 좋은 성능을 보여 줌
- Learning rate 조절을 위한 Scheduler로 **Exponential Decay**를 사용
    - Initial Learning rate : 0.001
    - Decay steps : 100000
    - Decay rate : 0.96
    - Stair case : True

## 5. 결과
![image](https://user-images.githubusercontent.com/94789091/162676712-1d394528-1763-4d5a-a779-3d81d327fbb0.png)

![image](https://user-images.githubusercontent.com/94789091/160774083-ed1285a2-1e19-4576-bd14-375b595acf91.png)


💡 Current Rank : 1등

</aside>

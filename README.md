# Dacon - 🥬 상추의 생육 환경 생성
![image](https://user-images.githubusercontent.com/92291198/236023180-d177d518-7f9f-45f0-b0b3-6fa41b3149de.png)

## 👍 리더보드 결과 공유
Goals : 상추의 일별 잎중량을 예측하는 AI 예측 모델 & 예측 모델을 활용한 생육 환경 생성 AI 모델 <br>
예측 모델에 대한 private ranking - 19th (상위 14%) <br>
예측모델과 생성모델을 모두 고려한 ranking - 10th <br>
<br>

## 📊 데이터 분석 및 전처리
1. input data & ouput data 확인
* output 형태에 맞추어, 인덱스를 날짜별로 설정하고 시간별 환경변수들을 column으로 설정 <br>
 <br>

2. 상추의 생육 환경에 영향을 미치는 요소 탐색
* 날짜별 환경변수들의 sum과 mean을 새로운 column으로 생성
* 광색의 비율, 소등시간 일교차, 오늘-내일 상추 무게의 차이를 새로운 column으로 생성 <br>
<br>

3. 환경변수들의 범위 설정
* 환경변수들의 범위 조절 (지정한 범위 이외의 값은 이전값이나 이후값으로 변경)
* 변경된 환경변수 값에 따른 누적값 업데이트  <br>
 <br>
 
## ⚛️ 예측모델 
### **[LightGBM Model]** <br> 
1. LightGBM parameters 변경
* boosting, objective 등 파라미터 최적화<br>
<br>

2. Feature Imfortance 확인
* 시간이 생장에 가장 중요한 영향을 미치는 것으로 확인 <br>
<br>

3. 성능 확인 후 분석
* 스케일링 시도했으나 RMSE의 감소
* 데이터의 양이 적어 과소 적합 의심
* case 간의 편차가 심함<br>
<br>

4. TimeGAN 으로 생성된 데이터를 lightgbm에 적용
* 새로운 환경변수에 대한 predicted_weight 출력<br>
<br>

5. 최적의 생육 환경 조성
* 모든 predicted weight을 날짜별로 sort하여 상추의 일별 최대 잎 중량을 도출하는 환경변수 값을 파악

[reference] https://dacon.io/competitions/official/235961/overview/description <br>
<br>
 
## ⚛️ 생성모델
### **[TimeGAN]** <br>
1. input data 변경
* case명 + DAT + obs time으로 인덱스 변경
* 누적 columns 삭제 및 환경변수 범위 설정 
* min-max scaling 진행 <br>
<br>

2. parameters 수정
* seq_len, n_seq, hidden_dim 등 변경 <br>
<br>

3. TimeGAN 모델 수정
* synthetic data가 원하는 형태로 생성되도록 모델을 수정 <br>
<br>

4. synthetic data 후처리
* origin data와 유사한 시계열 데이터가 생성
* rescaling 하여 환경변수등 범위로 값 복원
* 인덱스, columnd명, 누적값 column 처리 <br>
<br>

5. 시각화 하기
* origin data와 synthetic data 시각화하여 비교하기 <br>

[reference] https://www.kaggle.com/code/alincijov/stocks-generate-synthetic-data-timegan <br>
<br>

## ⭐ 담당 업무
데이터 분석 및 전처리 part - 새로운 column 탐색 및 환경변수들의 범위 설정 <br>
예측모델 part - 성능향상을 위한 parameters 수정 및 실험 <br>
생성모델 part - 생성모델의 전반적인 부분 담당 <br>
<br>


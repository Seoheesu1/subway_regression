# 일별 지하철 수요 예측🚝

## team member 
### [Seo heesu]('https://github.com/Seoheesu1')🍒
### [Park junhyuk]('https://github.com/Junhyuk93')🥑
### [An junhan]('')🍻

---

## 연구 배경


- 잘못 추정된 지하철역별 수요는 노선 건설에 대한 경제적 타당성을 비롯하여 역사 규모 설계, 운영상 적자 발생 등의 영향을 미침.

- 지하철역의 경우 건설 후 변경이 어려우므로 더욱 정확한 예측이 필요하지만 내외부적 다양한 요인으로 인하여 정확한 예측이 어려운 상황.



---

## 관련 선행논문 



![image](https://user-images.githubusercontent.com/61610411/133923383-fc608c72-38d7-4af8-85ed-3e0c0cf1aa6b.png)

기존에 존재하는 연구들은 **공공자전거 및 버스 이용량의 수치적 변수가 부재** 했으며, **공휴일에 대한 변수가 부족**하다고 생각되어 그 부분을 보완하여 분석을 진행

---

## 가설


- 대체 교통수단 이용량과 역별 지하철 수요량의 선형관계를 찾을 수 있을 것이다.

- 기후, 대기질 등의 환경변수에 의해서도 지하철 수요량의 영향에 대해 알 수 있을 것이다.



---

## 데이터 수집

- 서울교통공사 지하철역 주소 및 전화번호 정보 (http://www.seoulmetro.co.kr/kr/board.do?menuIdx=551&bbsIdx=2208459)
- 서울시 구별 기상 데이터(https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36)
- 서울시 대기질 정보 (http://data.seoul.go.kr/dataList/OA-15515/S/1/datasetView.do) API 활용
- 공휴일 여부 (http://me2.do/5uOupdMO) API활용
- 서울시 버스정류장 수(https://data.seoul.go.kr/dataList/OA-686/C/1/datasetView.do)
- 서울특별시 공공자전거 이용정보(시간대별)(https://data.seoul.go.kr/dataList/OA-15245/F/1/datasetView.do)
- 서울특별시 공공자전거 대여소 정보(https://data.seoul.go.kr/dataList/OA-13252/F/1/datasetView.do)
     
- 대여소 정보 crawling(https://www.bikeseoul.com)

- 서울시 버스노선별 정류장별 승하차 인원 정보
(https://data.seoul.go.kr/dataList/OA-12912/S/1/datasetView.do#)


---

## Data set

![image](https://user-images.githubusercontent.com/61610411/133924175-bff77be7-ee37-46ff-b692-5ac978ed5fa5.png)



---

## EDA

![image](https://user-images.githubusercontent.com/61610411/133924932-d2978991-522a-454f-aca0-ce22f0082f69.png)


![image](https://user-images.githubusercontent.com/61610411/133924900-13daed59-0c4b-4cbd-a588-2337a2f684de.png)

- 상위 10개의 역별 지하철 탑승객 인원 수

![image](https://user-images.githubusercontent.com/61610411/133924385-70475c3f-a279-4075-a2f7-c489509d3f3e.png)

![image](https://user-images.githubusercontent.com/61610411/133924514-e7a469d8-81ba-4430-b100-67001aba7073.png)

![image](https://user-images.githubusercontent.com/61610411/133924600-72a5ee37-ac82-41f5-b63e-aa09803952bd.png)

- 2015년 시작된 공공자전거 사업의 이용량이 빠르게 증가.

- <span style="color: #FF8000">공공자전거 이용량</span>이 증가함에 따라 <span style="color: #0174DF">지하철 이용량</span>이 감소하는 추세를 보이는 역이 존재.

- 공공자전거는 지하철의 대체재 혹은 보완재로서의 역할을 할 것으로 생각 됨. 이에 **일별 지하철 이용량 예측에 필요한 변수로 판단.**

![image](https://user-images.githubusercontent.com/61610411/133924853-da99a9f5-6730-4783-8334-33f502300f20.png)

- 요일별 버스 이용량과 지하철 이용량이 흡사한 양상을 보이는 것을 알 수 있음. 따라서 **버스 이용량 또한 지하철 이용량 예측에 꼭 필요한 변수로 예상됨.**


![](https://i.imgur.com/lxEScEB.png)


* 구 별로 휴일에 따른 이용량 추이가 많은 차이를 나타냄.

---

## Model Overview


![image](https://i.imgur.com/DSY8HgS.png)

### OLS Regression summary

![image](https://user-images.githubusercontent.com/61610411/133925157-e2c05011-71ef-4c53-83b3-f0d65a32eec8.png)

### 예측값과 실제값의 관계

![image](https://user-images.githubusercontent.com/61610411/133925227-59edd124-0f25-475c-9ae2-83f80733afce.png)


---

## Cross Validation

![image](https://user-images.githubusercontent.com/61610411/133925121-8a9c4148-77a7-4860-ab8f-b947438afb98.png)

### 변수 영향력 확인

![image](https://user-images.githubusercontent.com/61610411/133925458-cacf0648-4e13-4066-ba45-9aca834f3eba.png)

### Matric

![image](https://user-images.githubusercontent.com/61610411/133925626-04edba31-460e-4b19-b904-d09d9257dcc8.png)

- model1 : 구:휴일, 날씨, 대기질, 자동차
- model2 : 구:휴일, 날씨, 대기질, 자동차, 공공자전거
- model3 : 구:휴일, 날씨, 대기질, 자동차, 공공자전거, 버스
- model4 : 구:휴일, 날씨, 대기질, 자동차, 공공자전거, 구:버스


---

## 결론 및 한계점

- 버스와 공공 자전거는 지하철 수요 예측에 중요한 변수임을 확인할 수 있었다.
- 추후 공간적 범위를 확대하여 2호선뿐만 아닌 전체 역에 대한 분석이 필요할 것으로 예상된다.

--- 

## 프로젝트 회고

1) 총  대중교통 이용객수가 변함이 없다는 가정하에(물론 지하철 역이 신설되면 증가하겠지만) 
지하철역 신설전의 버스 이용객 수를 총 대중교통 이용객수로 두고 
"신설 후 버스 이용량 + 신설된 지하철 이용량 = 신설전 총 버스 이용량"
이런 식을 가정해 지하철 없는 지역의 데이터를 이용해 가상의 역이 신설됨을 가정해보면 재미있을것

2) 자전거 이용량과 시간의 상호작용이 들어가면 더 좋았을것

---

## 참고 문헌
- 초미세먼지 농도와 지하철 이용량의 관계분석과 미세먼지 대응정책의 실효성 평가(2019)
- A study on the number of passengers using the subway stations in Seoul-Department of Statistics, Ewha Womans University(2018)
- 시계열 및 회귀분석을 활용한 휘발유가격의 광역권별․수단별 대중교통수요 영향력 비교분석(2014)
- 대도시의 대중교통수요 영향요인 분석(2019)
- 빅데이터 분석을 이용한 지하철 혼잡도 예측 및 추천시스템(2016)
- 비용효과분석을 통한 서울시 지하철 9호선 혼잡도 개선방안에 관한 연구(2017)
- 서울시 대중교통 수단별 월별 이용수요의 변동에 영향을 미치는 요인 분석(2017)
- 서울시 대중교통 이용 패턴 및 영향요인 분석 연구(2015)
- 대중교통 선택행태 및 이용 영향요인 분석 : 도시철도 및 버스 간 비교를 중심으로(2019)
- 기상조건이 대중교통수요에 미치는 영향에 관한 연구(2013)
- 교통카드 자료를 이용한 서울시 지역별 대중교통 수단 선택 공간상관성 분석(2013)
- 서울시 대중교통 이용 패턴 및 영향요인 분석 연구(2015)
- 스마트카드 자료를 활용한 대중교통 승객의 통행목적 추정(2019)
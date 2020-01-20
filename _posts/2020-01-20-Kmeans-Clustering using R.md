---
title : "K-Means Clustering using R"
author : "금주"
#categories : - study
date: "2020-01-20"
---

##caret 패키지
- classification and regression training 의 약어로 복잡한 회귀와 분류 문제에 대한 모형 훈련(training)과 조절(tuning) 과정을 간소화하는 함수를 포함한다.
- 훈련데이터의 전처리, 변수의 중요성 계산 및 모형 시각화를 위한 방법을 포함한다.

###caret 패키지 다운로드 & 사용

  >install.packages("caret")
 library(caret)

Console 창에 이 두줄 만 입력하면  다운로드하고 사용할 수 있다.

---

## 군집 vs 분류

군집(clustering) : 데이터 간의 유사도를 정의하고 그 유사도에 가까운 것부터 순서대로 합쳐가는 방법

* 군집 분석은 <b>label 에 대한 정보를 모르기 때문에</b> 각  개체가 어떤 군집에 들어갈까 예측하기 보단<b> 이렇게도 나눠질 수 있구나 하는 정도의 지식을 발견</b>
* 군집은 애초에 label가 없기 때문에 <b>순수 데이터상의 특징으로 유사도를 정의하여 그룹을 만든다.</b> 그렇기 때문에 군집 분석 방법에 따라 차이가  날 수 있다.
*	마케팅 조사에서 소비자들의 상품구매행동이나 life style에 따른 소비자군을 분류하여 시장 전략 수집 등에 활용한다.


분류(classification)
* 기존에 존재하는 데이터의 category 관계를 파악하고, <b>새롭게 관측된 데이터의 category를 스스로 판별하는 과정</b>
* <b> 미리 labeling 된 데이터들을 label 대로 분류하는 과정 </b>


--------


## 비계층적 군집화(non-hierarchical clustering)
- 비계층적 군집방법은 <b>정해진 개수로 군집을 나누는데, 정해진 기준에 따라 더 이상 개선이 되지 않을 때 까지 군집화를 진행한다.</b>

### 비계층적 군집방법의 종류

1) <b><span style="color:rgb(255, 65, 65)"> K-means clustering : Centroid-based clustering </span></b>
2) <b><span style="color:rgb(255, 65, 65)"> Expectation-Maximization(EM) clustering : Distribution-based clustering </span></b>
3) <b><span style="color:rgb(255, 65, 65)">Density-vased clustering </span></b>
4) <b><span style="color:rgb(255, 65, 65)"> Fuzzy clustering : numeric variable 만 가능 </span></b>

-----

## K-Means Clustering
K-means Clustering : 주어진 군집 수 k에 대해서 군집 내 거리제곱합의 합을 최소화 하는것이 목적

<b><span style="color:rgb(255, 65, 65)">즉 거리제곱합의 합이 군집화가 얼마나 잘됐는지 알려주는 척도가 됨.</span></b>

##K-Means Clustering의 특징
- 거리 계산을 통해 군집화가 이루어지므로 <b>연속형 변수에 활용이 가능</b>
- K개의 초기 중심값은 임의로 선택 가능하며,<b> 가급적이면 멀리 떨어지는 것이 바람직하며 초기 값을 일렬로 선택하지 않는 것이 좋다.</b>
- 초기 중심으로부터의 오차의 제곱합을 최소화하는 방향으로<b> 군집이 형성되는 탐욕적(greedy) 알고리즘이므로 안정된 군집은 보장하나 최적이라는 보장은 없다.</b>

##K-Means Clustering 장·단점
<b>장점</b>
> * 간단한 원리  
> * 매우 유연하며 간단한 수정 가능
> * 효율적인 군집화 가능

<b>단점 </b>
> * 초기 클러스터링 중심점에 매우 종속적
>*  군집 갯수 선택에 대한 추측이 필요
>>군집 갯수에 대한 지식이 없다면 "데이터 수 (n) / 2" 의 제곱근으로 하거나 그래프를 그려 elbow point 로 선정

##K-Means Clustering의 알고리즘
1. <b><span style="color:rgb(255, 65, 65)"> k값을 초기값으로 먼저 받고, 데이터를 k개의 초기 군집으로 나눔.</span></b>
2. <b><span style="color:rgb(255, 65, 65)"> k개의 초기군집의 centroid(중심점)을 설정함.</span></b>
3. <b><span style="color:rgb(255, 65, 65)">각 데이터 개체와 현재 군집 중심점(centroid) 사이의 거리를 구함.</span></b>
4.  <b><span style="color:rgb(255, 65, 65)"> 만약 개체가 현재 군집 평균에 가까우면 현재 소속군집에 포함하고 그렇지 않으면 다른 군집으로 재할당.</span></b>
5. <b><span style="color:rgb(255, 65, 65)">개별 군집의 평균이 다시 계산되어, 클러스터의 중심점(centroid)을 다시 계산.</span></b>
6. <b><span style="color:rgb(255, 65, 65)"> 반복하면서 클러스터가 더 이상 재지정 되는 점이 없으면 마침.</span></b>

{% raw %} <img src="https://bcloved.github.io/assets/images/20200120K-MeansR/kmeans.png" alt=""> {% endraw %}



###dist함수의 거리 유형
군집을 구별시켜줄 거리를 계산하는 'dist'함수

- 유클리드 거리 (default)
- 맨하탄 거리
- 표준화 거리
- 민콥스키거리

예제 : iris 데이터를 이용하여 K-means Clustering 실습

>data(iris)
head(iris)


추후 계속 수정

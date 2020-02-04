﻿---
title : "hadoop spark"
author : "금주"
#categories : - Project
date: "2020-02-03"
---

## 하둡의 맵리듀스와 하이브
---
슈퍼컴퓨터 없이 서버를 여러대 연결해  빅데이터 분석을 가능하게 했다.
하지만 기술이 나오고 시간이 한참 지난 뒤부터 여러 단점들이 보이기 시작하며, Apache Spark 가 등장한다.
맵리듀스와 비슷한 목적의 업무를 수행하는데 메모리를 활용한 굉장히 빠른 데이터 처리를 특징으로 가지고 있다.

## 하둡(Hadoop)
---

대용량 데이터를 분산 처리할 수 있는 자바 기반의 오픈소스 프레임 워크.
<b>분산 저장 기술인 HDFS 와 분산 처리 기술인 맵리듀스 (MapReduce) 가 장점이다.

### 하둡의 분산처리 단계

하둡의 맵리듀스는

<b> 클러스터에서 데이터 읽기 -> 동작 실행 -> 클러스터에 결과 기록 -> 데이터에 업데이트 된 내용 읽기 -> 다음 동작 실행 -> 클러스터에 결과 기록 </b>

## 스파크(spark)
---

스파크는 전체의 데이터 셋을 한꺼번에 처리한다.
<b>클러스터에서 데이터 읽기 -> 애널리틱스 운영 수행 및 결과값 클러스터 입력 동작과 같은 전 과정 동시 진행</b>


속도는 하둡에 비해 월등히 빠름.

스파크는 스트리밍 데이터 처리와 머신러닝 알고리즘 처럼 애플리케이션과의 복합적 운영이 필요할 때 적합하다.
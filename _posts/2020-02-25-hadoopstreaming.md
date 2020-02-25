---
title : "[Project]하둡 스트리밍(hadoop streaming)"
author : "금주"
#categories : - Project
date: "2020-02-25"
---

## 하둡 스트리밍
---
기존 자바로 맵리듀스를 실행하던것 외에 스크립트 언어를 하둡에서 실행하게 해주는 인터페이스

맵 리듀스 : 일정 시간동안 쌓인 데이터를 한번에 배치처리하는 개념
하둡스트리밍 : 그때그때 데이터를 처리해야할 필요가 있을 때 사용

## 하둡 스트리밍 실행
---
하둡스트리밍을 실행하기 위해서는 hadoop-streaming-1.2.1.jar 파일이 필요
이 jar파일을 사용해 스크립트를 실행시키는 구조

> ./bin/hadoop jar contrib/streaming/hadoop-streaming-1.2.1.jar [generic옵션] [스트리밍옵션]

* generic옵션

옵션 | 필수여부 | 내용
|:---:|:---:|:---:|
-conf|옵션|맵리듀스 잡에서 사용할 환경설정 파일<br/> ex) -conf myprperty.conf
-D|옵션|맵리듀스 잡에서 사용할 속성값. "속성=값" 형식으로 설정<br/> ex) -D mapred.local.dir=/home/hadoop/mapred/local
-fs | 옵션 |	스트리밍 명령어에서 접근할 네임노드<br/> ex) -fs mynamenode:9000
-jt|옵션|스트리밍 명령어에서 접근할 잡트래커<br/> ex) j-jt myjobtracker:9001
-files|옵션|분산 캐시에 등록해서 사용할 텍스트 파일<br/> ex) -files "mapper.sh, reducer.sh"
-libjars|옵션|분산 캐시에 등록해서 사용할 JAR 파일<br/> ex) -libjars "/home/hadoop/mylib/myjob.jar"
-archives|옵션|분산 캐시에 등록해서 사용할 아카이브 파일<br/> ex) -archives "/home/hadoop/mylib/myjob.jar , /home/hadoop/mylib/myjob2.tar"

* 하둡 스트리밍 옵션

>./bin/hadoop jar contrib/streaming/hadoop-streaming-1.2.1.jar \ <br/>
<t>-input 입력 데이터 경로 \ <br/>
<t>-output 출력 데이터 경로 \  <br/>
<t>-mapper 매퍼로 사용할 소스경로 \  <br/>
<t>-reducer 리듀서로 사용할 소스 경로 <br/>

<출처 :http://blog.naver.com/PostView.nhn?blogId=imf4&logNo=220736720404&parentCategoryNo=&categoryNo=24&viewDate=&isShowPopularPosts=false&from=postView >

뭐 하나 매끄럽게 흘러가는ㄱ ㅔ없는 우당탕탕 하둡 일기 ㅠㅠ

----
참고
https://jyoondev.tistory.com/58

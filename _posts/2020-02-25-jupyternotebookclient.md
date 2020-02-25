---
title : "[Project]jupyter notebook에서 hadoop 서버로 결과 전송하기"
author : "금주"
#categories : - Project
date: "2020-02-25"
---

먼저 하둡서버와 jupyter notebook 클라이언트는 tcp 방식의 소켓 통신을 할 계획이다.
하둡 스트리밍은 자바 소켓통신과 동일하다고 한다.

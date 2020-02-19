---
title : "[Project]ubuntu spark on yarn"
author : "금주"
#categories : - Project
date: "2020-02-19"
---

하둡의 구성요소
* HDFS : 저장장치
* YARN : 처리 프레임 워크

---

## HDFS 와 YARN 이 무엇인가 ?

HDFS : 하둡의 저장장치로 분산 환경에서 서로 다른 종류의 데이터를 블록으로 저장한다. 마스터 슬레이브 토폴로지를 따른다.
	- NameNode / DataNode
YARN(Yet Another Resource Negtiator) : 리소스를 관리하고 프로세스에 실행 환경을 제공하는 하둡의 처리 프레임워크
	- ResourceManager : 처리 요청을 수신한 후 실제 처리가 수행되는 요청 부분을 해당 노드 매니저에게 전달. 필요에 따라 응용프로그램에 리소스를 할당.
	- NodeManager : 노드매니저는 모든 데이터 노드에 설치되며 모든 단일 데이터노드에 태스크실행을 담당.

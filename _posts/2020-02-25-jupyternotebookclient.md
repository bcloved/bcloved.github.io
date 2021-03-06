---
title : "[Project]jupyter notebook에서 hadoop 서버로 결과 전송하기"
author : "금주"
#categories : - Project
date: "2020-02-25"
---

통신은 크게 총 두가지에서 사용된다.
1. 하둡 서버와 jupyter notebook
2. 어플리케이션과 하둡 서버 ( spark 스트리밍 )

먼저 하둡서버와 jupyter notebook 클라이언트는 tcp 방식의 소켓 통신을 할 계획이다.
또 어플리케이션에서 데이터를 보내면 실시간으로 처리를 해야한다.

하둡은 실시간성의 처리구조가 아니기 때문에 실시간성의 데이터를 처리하기 위해 Storm과 Spark의 실시간 처리를 위한 Spark Streaming, Samza, Flink등의 처리인프라와 실시간성의 데이터 수집을 위한 Flume 과 Kafka 등의  다양한 처리 에코시스템이 발전 한다. 하지만 나는 파이썬 웹소켓을 통해 스트리밍 데이터를 보내도록 할 것이다.

한 마디로,  하둡 서버를 중심으로 안드로이드와 jupyter notebook 이 통신을 할 예정이다.
그러기 위해서는 먼저 하둡 Network 를  설정을 해줘야 한다.

## Hadoop Netwrok 설정
---




keywords : 실시간 스마트 디바이스 / spark 스트리밍


## 빅데이터 실시간 아키텍처 구성시 고려사항
---


* 데이터의 정합성 및 유실:  배치레이어나 스피드 레이어에서의 처리시 데이터의 정합성을 보장하고 모니터링 할수 있는 수단이 필요하며 비즈니스 적으로 중요한 데이터이고 스트리밍 처리시 데이터가 유실되지 않도록 일정기간 데이터의 롤링, 백업될수 있도록 설정이 필요합니다.

* 처리현황 모니터링: 실시간 처리 시스템 구성후에는 처리현황 및 인프라에 대한 이상유무 및 특정시점 데이터처리의 누락여부를 모니터링 할 수 있는 모니터링 체계가 중요하며 또한 유용합니다.

* 대시보드 및 자동화: 현황을 모니터링 하고 제어할수 있는 기능을 따로 모아서 대시보드를 구성하고 사용자레벨 별로 권한 관리할 경우 효율적으로 관리가 가능합니다. 또한 관리가 필요한 부분은 자동화하는 방안도 고려가 필요합니다.

* 빅데이터의 사용자관점 시각화: 처리된 데이터가 사용자에게  trend 상에 직관적으로 인지되도록 텍스트나 수치 보다는 그래프나 표, 색깔 등으로 시각화 될 경우 데이터 사용성이 증가합니다.

* 용량 산정 및 처리인프라의 Provisioning: 빅데이터 처리시의 필요 인프라의 용량 산정 및 그에 따른 인프라가 자동으로 투입되도록 AutoScaling 고려도 필요합니다.(처리 서버의 cpu, memory, network usage, active thread 등에 따른 AutoScaling)

* 오픈소스 version upgrade시 변경사항의 이력관리: 시스템을 구성하는 오픈소스의 버젼에 따라 관련 인프라간의 영향도 파악 및 반영에 따른 이력 및 문서화가 필요합니다.

* 사용자 데이터 보안 및 백업: 인프라의 access 권한에서 부터 처리데이터의 개인정보 보호 및 기밀성에 대한 고려가 필요할 수 있습니다.

참고 https://saintbinary.tistory.com/15






해당 지식이 없는 전공자가 쓴 글이여서 틀린 정보가 많을 수도 있음을 알려드립니다.

---

이건 내가 참고하려고
하둡의 HDFS 는 Master-slave 구조로 동작하며 TCP/IP 프로토콜을 이용해 노드가 통신을 한다.

{% raw %} <img src="https://bcloved.github.io/assets/images/sc/2158BA4757E10ACB11.jfif" alt=""> {% endraw %}

-MapReduce Layer : 대용량 세트를 처리하거나 생성하기 위한 프로그래밍 모델<br>
-Map Phase : 각 데이터를 한 줄씩 읽어서 Key와 Value로 묶어줌<br>
-Reduce Phase : Map Phase에서 데이터를 받아 합치고 정리<br>
-HDFS Layer : 범용 하드웨어로 구성된 클러스터에서 실행되고, 데이터 액세스 패턴을 스트리밍 방식으로 지원하여 대용량의 데이터 파일들을 저장할 수 있도록 설계된 파일시스템

출처: https://roynus.tistory.com/763 [back end 개발자!!]

---
참고
스파크 스트리밍 <https://jyoondev.tistory.com/m/97>
스파크 스트리밍 강의 <https://www.youtube.com/watch?v=cXfjBb0Vj7c>

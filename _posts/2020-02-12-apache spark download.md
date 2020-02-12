---
title : "[Ubuntu]Apache Spark 다운로드"
author : "금주"
#categories : - Project
date: "2020-02-12"
---

hadoop 다운로드는 .. <https://bcloved.github.io/hadoop%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C/> 여기서보실수 있습니당



spark?
---


python3 download
----
> sudo apt-get update
>sudo apt-get upgrade

>sudp apt-get install python3
>sudo apt-get install python3-pip

pip 다운로드 받는거 오래걸림 . . . . . 진짜 오래걸림

---

matplotlib download
----

>sudo pip3 install --upgrade pip

pip를 업그레이드 해준다.

> sudo pip3 install numpy
>sudo apt-get install python3-scipy
>sudo apt-get install python-matplotlib

>sudo pip3 install scikit-learn


---

locale 관련 설정

> sudo gedit ~/.bashrc

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/1.PNG" alt=""> {% endraw %}

export LC_ALL=en_US.UTF-8
export LANG = en_US.UTF-8

을 맨 마지막에 설정한 뒤 저장 !!

----
##JAVA 설치
자바 설치는 HADOOP 에서 이미 했지만 한번 더 적겠습니다

>sudo apt-get install openjdk-8-jdk

깔고나면 이 java가 설치된 경로를 알아야 합니다.

제가 깔려있는 경로는<b> /usr/lib/jvm/java-8-openjdk-amd64 </b>였어요<br>
사실 알아내기 귀찮아서 저 경로로 이동했는데 있길래 저기구나 했음

> cd  /usr/lib/jvm/java-8-openjdk-amd64 (자바가 깔린 경로)
>sudo gedit /etc/profile.d/java.sh

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/2.PNG" alt=""> {% endraw %}


export JAVA_HOME =/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

> . /etc/profile

이 문장 실행하여 환경설정 적용

----
##SPARK 다운로드

<http://spark.apache.org/downloads.html> 에 접속

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/3.PNG" alt=""> {% endraw %}

tgz 파일 다운로드

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/6.PNG" alt=""> {% endraw %}

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/7.PNG" alt=""> {% endraw %}
나는 /home/master 경로에 압축 풀어줌

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/8.PNG" alt=""> {% endraw %}

> sudo mv spark-3.0.0-bin-hadoop2.7 /opt/

이름 바꿔줬어요 preivew 어쩌고 이거 길어서 빼버렸는데

> sudo ln -s /opt/spark-3.0.0-bin-hadoop2.7/ /opt/spark

또바꾸는 바보가 있답니다.. 애초부터 spark 라고 바꿔놓고 옮길걸
ㅋ ㅋ ㅋ
ㅋ
ㅋ
쨋든 path 가 길면 고생 개고생 + 오타남발로 인한 오류가 날 가능성이 높아져서 이름을 spark 로 바꿧성요


spark도 환경변수에 세팅하기 위해서 아래의 파일을 하나 만듭니다.

> sudo gedit /etc/profile.d/spark.sh

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/9.PNG" alt=""> {% endraw %}

그런 뒤 이렇게 적어주면 돼요 !!!

export SPARK_HOME=/opt/spark
export PATH=$SPARK_HOME/bin:$PATH
export PYSPARK=/usr/bin/python3

파이썬 경로는 <b>which python3</b> 라고 커맨드창에 입력해서 알아내시면 댑니당

이제 환경설정이 모두 끝났습니다 (짝짝짝)

우리는 스파크홈 설정 및 path에도 모두 적용했기 때문에

> pyspark

라고 아무 디렉토리에서나 pyspark 라고 파이썬용 스파크명령을 실행하면

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/10.PNG" alt=""> {% endraw %}

이 화면이 뜨면 ! 성 ! 공 ! !

참고로 말하지만 pyspark라는 프로그램은 파이썬 대화형 명령(REPL) 이기 때문에 python 코드를 넣어서 엔터를 입력하면 바로 실행되어 결과가 나온다고 합니다
종료는 exit() 을 입력하면 됨 ! !

---

----
##jupyter NoteBook download

주피터 노트북은 바로바로 결과를 확인할 수 있어서 너무  선호하고 있습니다.
그런 의미로 가상머신에서도 다운로드를 한번 받아보겠습니다.

개인적으로 가장 리눅스 OS  장점이라고 생각하는데 진짜 명령어 한줄로 다운받아지는거 너!무!편!함!

> sudo apt-get install jupyter

> jupyter notebook --ip=0.0.0.0


{% raw %} <img src="https://bcloved.github.io/assets/images/spark/11.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/spark/12.PNG" alt=""> {% endraw %}


원격에서는 접근이 불가능 합니다.

---
## 브라우저 기반 pyspark 실행하기

> sudo gedit /etc/profile.d/spark.sh

환경변수 설정한 파일 다시 켜서

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/13.PNG" alt=""> {% endraw %}

형광펜 친 부분 추가 해주세용

> ./etc/profile

적용 하기 ㅎㅎ

>cd
>mkdir data
>cd data
>pyspark

{% raw %} <img src="https://bcloved.github.io/assets/images/spark/14.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/spark/15.PNG" alt=""> {% endraw %}


참고
-----
<https://blog.naver.com/PostView.nhn?blogId=semtul79&logNo=221489908486&parentCategoryNo=&categoryNo=14&viewDate=&isShowPopularPosts=true&from=search>


<https://blog.naver.com/PostView.nhn?blogId=semtul79&logNo=221490035474&parentCategoryNo=&categoryNo=14&viewDate=&isShowPopularPosts=true&from=search>

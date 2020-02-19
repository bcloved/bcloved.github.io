---
title : "[Project]ubuntu Hadoop 가상분산모드(Pseudo distributed) 구현"
author : "금주"
#categories : - Project
date: "2020-02-19"
---

> sudo install apt-get  yum

https://parksuseong.blogspot.com/2019/04/312-2-standalone-pseudo-distributed.html
https://jinjeecode.tistory.com/18
https://net0310.tistory.com/entry/Hadoop-%EC%84%A4%EC%B9%98
https://jy86.tistory.com/entry/%E3%85%87



root@localhost 의 비밀번호 설정
---
관리자 모드로 접속 후 password설정

>sudo -s
>passwd
{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/4.PNG" alt=""> {% endraw %}


{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/1.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/2.PNG" alt=""> {% endraw %}

> ~/hadoop/hadoop-2.10.0/bin/hadoop namenode -format
{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/3.PNG" alt=""> {% endraw %}

> ~/hadoop/hadoop-2.10.0/sbin/start-dfs.sh
{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/5.PNG" alt=""> {% endraw %}

>jps
{% raw %} <img src="https://bcloved.github.io/assets/images/pesudo-distributed/6.PNG" alt=""> {% endraw %}


가상분산모드 설치 완료

<https://1004jonghee.tistory.com/entry/%ED%95%98%EB%91%A1-%EA%B0%80%EC%83%81-%EB%B6%84%EC%82%B0-%EB%AA%A8%EB%93%9C-%EC%84%A4%EC%B9%98Hadoop-121-version>

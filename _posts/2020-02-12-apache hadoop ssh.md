---
title : "[ubuntu]hadoop ssh 설정"
author : "금주"
#categories : - Project
date: "2020-02-13"
---
## ssh 설정

hadoop 은 분산처리시에 서버들 간에 ssh 통신을 자동적으로 수행하기 때문에 암호 없이 접속이 가능하도록 master 서버에서 공개키를 생성한 후 각 서버들에게 배포해 줘야 한다.

* ssh 설정 변경

> sudo gedit /etc/ssh/sshd_config


{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/1.PNG" alt=""> {% endraw %}

이 파일들에서


{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/2.PNG" alt=""> {% endraw %}


형광펜 칠한 부분의 주석을 제거한다.

* 클라이언트 쪽에서 공개키를 생성

> mkdir ~/.ssh
> chmod 700 ~/.ssh
> ssh-keygen -t rsa -P ""
>  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys


사실 여기 부분은 hadoop 다운로드 하는 부분과 동일함

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/3.PNG" alt=""> {% endraw %}

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/4.PNG" alt=""> {% endraw %}

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/5.PNG" alt=""> {% endraw %}


포트포워딩 테이블 설정 각 master 와 slave ip를 설정해주었음.

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/6.PNG" alt=""> {% endraw %}

> sudo iptables -I INPUT -p TCP -s YOUR_CLIENT_IP -j ACCEPT

 sudo iptables -I INPUT -p TCP -s 192.168.0.6 -j ACCEPT
그러니까 나는 이렇게 입력했음.

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/7.PNG" alt=""> {% endraw %}



---
## ssh 접속 장애시 원인별 로그

※ 정리

1. 접속 대상이 없을 경우
ssh: connect to host 192.168.0.10 port 22: No route to host
서버가 down 상태이거나 ip 정보가 틀린 경우

2. netfilter(iptables)로 막아 놓았을 경우
ssh: connect to host 192.168.0.200 port 22: No route to host
웹서비스는 정상 접속되는 상태에서 ssh 접속이 안되는 경우

3. ssh 서비스가 구동중이지 않은 경우
ssh: connect to host 192.168.0.200 port 22: Connection refused

4. tcp_wrapper(/etc/hosts.deny)로 막아 놓은 경우
ssh_exchange_identification: Connection closed by remote host

5. 서비스 포트가 틀린 경우
ssh: connect to host 192.168.0.200 port 22: Connection refused

1번, 2번의 경우 접속 에러 로그는 동일하나 ping test 또는 다른 서비스 접속을 통해 어느 원인인지 확인 가능
3번, 5번의 경우 에러로그 상으로는 파악 불가


필자는 3번,5번이 떳고 확인 결과 slave VM 에 ssh 가 다운이 안받아져 있는 걸 확인함.

---


참고
----

<http://jeonghwan-kim.github.io/%EC%9B%90%EA%B2%A9%EB%A1%9C%EA%B7%B8%EC%9D%B8ssh-%EC%A0%91%EC%86%8D/>
<https://sancs.tistory.com/110>
<https://wookmania.tistory.com/79>
<https://shaeod.tistory.com/582>
sudo iptables -L

sudo iptables -A INPUT -p tcp --dport ssh -j ACCEPT

---
title : "[ubuntu/ssh]ssh 비밀번호 입력 없이 가상머신끼리 통신하기"
author : "금주"
#categories : - Project
date: "2020-02-13"
---

하둡에 이어 약 3일 동안 ssh 때문에 삽질만 했다 ㅎㅎ
13일에 올렸지만 오늘이 벌써 18일 ^^ ..

일단 나의 삽질의 기록과 Permission deny 에 대한 해결방법을 말하려고 한다.


---

먼저 hadoop 은 분산처리시에 서버들 간에 ssh 통신을 자동적으로 수행하기 때문에 암호 없이 접속이 가능하도록 master 서버에서 공개키를 생성한 후 각 서버들에게 배포해 줘야 한다. 그렇기 때문에 비밀번호가 없이 즉각적으로 소통이 가능하도록 설정을 해줘야 한다.


---
## 포트포워딩(portforwarding)

master ip

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/5.PNG" alt=""> {% endraw %}

slave ip

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/4.PNG" alt=""> {% endraw %}


각 ip 는 192.168.0.5 와 192.168.0.6 로 설저 되어져 있음
그렇기 때문에 아래 사진에서 Master SSH 게스트 ip 와 Slave SSH 게스트 ip 를 설정해준다.

ssh 의 port 번호는 22번이기 때문에 22로 설정


{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/3.PNG" alt=""> {% endraw %}

가상 머신에서 포트포워딩 규칙 설정 해준다.

포트포워딩 규칙 설정하는 방법 <https://bcloved.github.io/portforwarding/>



## slave 머신 IP 이름 지정
---

master 머신에  slave 머신의 IP에 대한 이름을  지정해 줘야 한다.

>  sudo gedit /etc/hosts

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/20.PNG" alt=""> {% endraw %}

ipv6 은 사용안할거기 때문에 다 주석처리 했음


---

## ssh-server/client 설치
---
master 머신과 slave 머신 둘 다 ssh 클라이언트와 서버 설치

> sudo apt-get install openssh-client openssh-server




## ssh 설정
---

* ssh 설정 변경

<b><span style="color:rgb(159, 125, 255)">(master)</span></b>

> sudo gedit /etc/ssh/sshd_config


이 파일들에서

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/2.PNG" alt=""> {% endraw %}


형광펜 칠한 부분의 주석(#)을 제거한다.


<b><span style="color:rgb(159, 125, 255)">(slave)</span></b>

> sudo gedit /etc/ssh/sshd_config

똑같이 sshd_config 파일을 연 뒤
PermitRootLogin = 어쩌고어쩌고 되어져 있었는데 '=' 이후 부분을  지운 뒤

> PermitRootLogin = yes

로 변경해준다.

---

* <b><span style="color:rgb(159, 125, 255)"> master 서버에서 해주기</span></b>


iptables 에 slave ip 추가
{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/6.PNG" alt=""> {% endraw %}


> sudo iptables -I INPUT -p TCP -s YOUR_CLIENT_IP -j ACCEPT

 sudo iptables -I INPUT -p TCP -s 192.168.0.6 -j ACCEPT
그러니까 나는 이렇게 입력했음.

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/7.PNG" alt=""> {% endraw %}

이렇게 확인할 수 있음


> mkdir ~/.ssh

.ssh 폴더 만들어줌


---

# 공개키 생성

 slave 와 master VM 둘 다 각자 공개키를 생성해 주어 한다.

> ssh-keygen -t rsa -P ""

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/9.PNG" alt=""> {% endraw %}

> cd ~/.ssh
> ls

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/10.PNG" alt=""> {% endraw %}

id_rsa와 id_rsa.pub 파일이 생성되어져 있을 것이다. ( authorized_keys  파일은 없을 수도 있음)

id_rsa.pub 가 바로 공개키다.


{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/17.PNG" alt=""> {% endraw %}

이와 같이 출력되면 성공한 것.


-|-|-
id_rsa|private key, 절대로 타인에게 노출되면 안된다.
id_rsa.pub|public key, 접속하려는 리모트 머신의 authorized_keys에 입력하는 키
authorized_keys| 리모트 머신의 .ssh 디렉토리 아래에 위치하면서 id_rsa.pub 키의 값을 저장한다.


.ssh 디렉토리는 매우 중요한 정보가 담긴 디렉토리다. 따라서 퍼미션 설정을 꼭 해줘야 하는데 아래와 같은 설정을 권장한다.
아래의 명령어를 순차대로 실행한다.

각 파일들의 퍼미션이다.
{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/19.PNG" alt=""> {% endraw %}

참고로 .ssh 폴더의 퍼미션은 700으로 설정하는 것이 좋다 (자동으로 설정되긴 함)

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/18.PNG" alt=""> {% endraw %}


>chmod 700 ~/.ssh<br>
chmod 600 ~/.ssh/id_rsa<br>
chmod 644 ~/.ssh/id_rsa.pub <br>
chmod 644 ~/.ssh/authorized_keys <br>
chmod 644 ~/.ssh/known_hosts


---
# 공개키 배포

모든 머신의 RSA 키가 생성되면 Master Slave 노드의 모든 Key를 공유해야 한다.
그렇기 위해서는 공개키 집합을 만들어 공유해야 하는데 그렇게 하기 위해서 <b><span style="color:rgb(159, 125, 255)">authorized_keys</span></b> 라는 파일을 생성해야 한다.


먼저 <b><span style="color:rgb(159, 125, 255)">master</span></b> 에서 id_rsa.pub 의 내용을 authorized_keys 라는 파일에 작성을 한다.

> cat id_rsa.pub >> authorized_keys

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/11.PNG" alt=""> {% endraw %}

이렇게 하면

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/12.png" alt=""> {% endraw %}

이렇게 복사된 걸 확인할 수 있다.



이제 slave1의 id_rsa.pub 을 다음과 같은 명령어 실행으로 master 노드의 authorized_keys 에 작성한다.

> ssh <b><span style="color:rgb(159, 125, 255)">slave1@slave1-VirtualBox</span></b> cat<b><span style="color:rgb(159, 125, 255)"> /home/slave1/.ssh/id_rsa.pub</span></b> >> ~/.ssh/authorized_keys

여기서 주의할점은 가상머신의 host 이름과 user 이름을 완벽하게 적어줘야 하고, 또한 slave 가상머신에서 id_rsa.pub 가 저장된 폴더의 전체 경로를 정확하게 적어줘야 함. ~이상 개고생한  사람의 말 ~

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/13.PNG" alt=""> {% endraw %}

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/14.PNG" alt=""> {% endraw %}

그러면 authorized_keys 파일 밑에 slave1 의 pub 키가 복사된 걸 볼 수 있음

<<< !! 본격 배포하기 중요 !! >>>

그 후에, authorized_keys 파일에 master 과 slave 의  모든 공개키가 작성되고 나면 scp 명령어를 이용해 모든 slave 들에게 이 파일을 배포해줘야 한다.

> scp authorized_keys slave1@slave1-VritualBox:~/.ssh/authorized_keys

{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/15.PNG" alt=""> {% endraw %}


이처럼 뜨면 배포가 끝난 걸  확인할  수 있음


{% raw %} <img src="https://bcloved.github.io/assets/images/hadoop2/16.PNG" alt=""> {% endraw %}

slave1에 접속 성공 ~!!

다음 쓸 게시물은 하둡에서 본격적으로 데이터를 분산처리 하도록 할 것이다.



---

참고 ! !

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
하지만 그러고 나서 발생한 Permission Deny 증상

이유는 없는 가상머신 호스트로 되어져 있어서 였음
현재 있는 가상 머신은 <b>master@master-VirtualBox 와 slave1@slave1-VirtualBox</b> 두 가지가 있었는데
내가 ssh slave1-VirtualBox 하다 보니 master@slave1-VirtualBox 로 연결이 되어 설정되지 않았기 때문에 Permission deny 가 발생했었음

해결법은

>> ssh slave1@slave1-VritualBox

라고 해주니 해결 되었음


---



spark on yarn

spark in standalone

spark history server


참고
----

<https://bitstudio.tistory.com/entry/%EC%9E%90%EB%8F%99-SSH-%EC%A0%91%EC%86%8D%EC%9D%84-%EC%9C%84%ED%95%9C-SSH-setup>
<https://opentutorials.org/module/432/3742>
<https://daeson.tistory.com/277?category=679387>

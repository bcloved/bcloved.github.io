---
title : "ubuntu 포트포워딩(Port Forwarding)"
author : "금주"
#categories : - Network
date: "2020-02-04"
---
본 게시물은 내가 과제로 제출햇던 ppt 임

hadoop 다운로드 과정에서 가상머신이 사용되길래 혹시 모르는 사람들을 위한 게시물 겸 내가 헷갈려서 보려고 올리는 게시물 ㅠ

1. 게이트웨이 설정

{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드2.PNG" alt=""> {% endraw %}

2. Port forwarding
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드3.PNG" alt=""> {% endraw %}

3. 게이트웨이 NAT 설정
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드4.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드5.PNG" alt=""> {% endraw %}

4. NAT vs NAT Network
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드6.PNG" alt=""> {% endraw %}

5. 게이트웨이 네트워크 설정
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드7.PNG" alt=""> {% endraw %}

6. Master IP 주소 설정
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드8.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드9.PNG" alt=""> {% endraw %}

7. Slave IP 주소 설정
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드10.PNG" alt=""> {% endraw %}

8. 게이트웨이 NAT 설정
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드11.PNG" alt=""> {% endraw %}

9. Putty를 이용하여 각 VM에 접속
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드12.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드13.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드14.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드15.PNG" alt=""> {% endraw %}

10. VM에 PuTTY 설치
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드16.PNG" alt=""> {% endraw %}

11. Slave VM 에서 Master VM 에 접속하기

{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드17.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드18.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드19.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드20.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드21.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200204portforwarding/슬라이드22.PNG" alt=""> {% endraw %}

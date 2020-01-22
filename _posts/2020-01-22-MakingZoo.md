---
title : "Making ZOO with SWI-Prolog"
author : "금주"
#categories : - AI
date: "2020-01-22"
---
## 규칙 기반 전문가 시스템

규칙 기반 전문가 시스템에서는 지식 표현 모델 중 생성 규칙을 사용합니다.
생성 규칙은 규칙들이 상대적으로 이해하기 쉽고 작성하기 쉽기 때문입니다.
주로 IF-THEN 절로 이루어 지는데 Prolog 에서는 IF-THEN 절 대신 하나의 사실에 대한 규칙들 “사실:-규칙 “ 으로 나타냅니다.
하지만 전문가들이 여러명이서 지식을 넣으면 상충되는 문제가 생기게 됩니다. 

{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/1.PNG" alt=""> {% endraw %}

-------


## 소스 코드

<script src="https://gist.github.com/bcloved/f7c14d5ea3897d05b3e87bc43eb5c6f8.js"></script>

## 코드 설명

(1)
먼저 메소드 실행 순서에 대해 설명 하겠습니다.
Go -> animal_is -> Cat -> carnivore -> mammal -> feature -> ask -> read -> animal_is 순서로 실행 됩니다.

(2) 
go :- 
write('GeumJu : Mom ! i want to see Animal'),
nl,
write('Mom : what is Animal'),
nl,
write('GeumJu : Umm, I do not remember that'),
nl,
animal_is(Animal,Locate),
write('I think that is '),
write(Animal),
write(' ! that is '),
write(Locate).

undo :- retract(yes(_)),fail. 
undo :- retract(no(_)),fail. 

undo.


go 는 prolog 가 실행된다는 의미입니다. 쉘에 go.를 입력하게 되면 write () 가 실행되어 화면에 문구를 출력해 줍니다. 여기서 nl 은 줄 바꿈의 의미입니다.
실행이 되다가 animal_is() 구조를 만나면 animal_is 이 실행됩니다. Prolog 에서는 구조가 여러 개의 매개 변수 와 함수기호로 구성되어져 있습니다. 즉 animal_is 가 함수기호이며 이 안의 Animal, Locate 가 매개변수가 됩니다. 그리고 대상과 관계의 이름은 소문자로 시작이 되어야 합니다. 여기서 관계가 animal_is 이고 매개변수가 대상이 됩니다. 이 animail_is  함수가 다 종료되고 나면 뒤에 wirte() 가 실행된 뒤 화면에 문자를 출력해 줍니다. 

undo :- retract(yes(_)),fail. 
undo :- retract(no(_)),fail.

그런 다음 뒤에서 더 자세히 설명하겠지만 yes 와 no 라는 동적인 데이터베이스에 추가된 규칙들을 다 제거(retract)합니다. 프로그램이 재 시작을 할 때 데이터베이스에 미리 규칙들이 저장이 되어져 있으면 오류가 발생할 수 도 있기 때문입니다.  Yes 데이터 베이스에 있는 규칙들을 제거 할 때 까지, 재 반복(undo)하고 데이터베이스가 비었으면 종료 한 뒤 no 데이터 베이스에 있는 규칙들을 제거 할 때 까지 재반복 합니다. 이때 yes와 no 의 매개변수로 들어가 있는 (_) 는 익명 변수로 정확히 무엇인 지 알 필요가 없는 경에 사용되는 변수 입니다. 
그런 다음에 있는 undo. 의 의미는 프로그램을 다시 시작하게 하는 역할을 해 줍니다. 예를 들어 쉘에 go. 를 입력하면 프로그램이 재 시작 됩니다. 


(3) 
:- dynamic yes/1,no/1.

feature(S) :- ( yes(S) -> true ;(no(S) -> fail ; ask(S))).

ask(Q) :-
	write('Mom : It '),write(Q),write('? '),
	read(R),
	( (R == yes ; R == y) 
         -> assert(yes(Q)) ;
	(R == no ; R == n)
         -> assert(no(Q)), fail).

:- dynamic yes/1,no/1. 은 동적인 데이터베이스를 선언해 주는 것 입니다. 프롤로그에서 :- 는 only if 라는 의미의 논리식 입니다. 즉 사실에 대한 규칙을 표현 할 때 “ :- “ 기호를 사용 합니다. 이 데이터 베이스는 전역으로 쓰여야 하기 때문에 전역 변수로 선언을 해주고, 동적인 데이터베이스 yes 와 no를 선언하는 문장입니다. ‘/1’ 은 Yes, no 는 각 각 매개변수를 하나 가지고 있다는 의미 입니다.


feature(S) :- ( yes(S) -> true ;(no(S) -> fail ; ask(S))).
여기서도 마찬가지로 “ :- “ 를 이용하여 feature(S) 라는 사실에 대한 규칙들을 표현 한 것 입니다. S가 feature 이라면 ,  S 가 yes 라면 true 이고 s 가 no 라면 fail 이며 S 를 ask 한다. 라는 표현 입니다. 프롤로그에서 변수는 대문자나 밑줄(_) 로 쓰여야 합니다.

 {% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/2.PNG" alt=""> {% endraw %}
	
ask(Q) :-
	write('Mom : It '), write(Q),  write('? '),
	read(R),
	( (R == yes ; R == y) 
         -> assert(yes(Q)) ;
	(R == no ; R == n)
         -> assert(no(Q)), fail). 
ask 에 대한 규칙들을 정의한 부분입니다. 위에서 ask에 s 를 매개변수로 전달 했고 변수 Q로 전달 받았습니다. Read() 는 사용자로부터 값을 받는 함수로 만약 전달받은 값(R)이 yes 이거나 y 이면 yes 데이터베이스에 해당 질문 Q를 넣고 R 값이 no 거나 n 인 경우에는 no 데이터베이스에 Q 값을 추가한다(assert)는 규칙들을 정의 합니다. 

이 데이터베이스에 저장된 값들은 프로그램이 종료되기전에 데이터베이스에서 제거 (retract) 해줘야 합니다. 프로그램을 실행시킬 때 마다 계속해서 사용될 규칙들이 아니고(계속해서 같은 결과가 나오는게 아니라) 실행 할 때마다 다른 규칙들이 사용되기 때문입니다. 즉 ,이 데이터베이스는 단기 기억장치로 사용됩니다.
 
{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/2-2.PNG" alt=""> {% endraw %}

(4) 

animal_is(cat,left_side_of_first_block) :- cat, !.
animal_is(elephant,right_side_of_first_block) :- elephant, !.
animal_is(cheetah,left_Side_of_Second_block) :- cheetah, !.
animal_is(lion,right_Side_of_Second_block) :- lion, !.
animal_is(tiger,center_of_Third_block) :- tiger, !.
animal_is(dog,left_of_Third_block) :- dog, !.
animal_is(parrot,left_of_the_fourth_block) :- parrot, !.
animal_is(penguin,center_of_Fourth_block) :- penguin, !.
animal_is(dolphin,right_of_Forth_block) :- dolphin, !.

animal_is 에 대한 규칙들을 정의한 부분입니다. Prolog 에서 ! 의 의미는 Cut 이라는 의미 입니다. Cut. Discard all choice points created since entering the predicate in which the cut appears. In other words, commit to the clause in which the cut appears and discard choice points that have been created by goals to the left of the cut in the current clause 출처 : https://stackoverflow.com/questions/22421995/what-does-do-in-prolog 
cut이 나타나는 술어를 입력 한 이후의 작성된 모든 선택지는 버리는 것 입니다. 예를 들자면 animal_is(cat,left_side_of_first_block) :- cat, !. 문장에서는 cat 이라는 사실만 실행합니다. 만약 cat 이 답한 규칙들이 다 맞다면 animal_is 규칙에 대한 매개변수로 cat과, left_side_of_first_block 이 들어갑니다. 이들은 변수가 아니기 때문에 소문자로 넣어줘야 합니다.



(5) 
cat :- carnivore,mammal,
	feature(is_Companion_animals),
	feature(hates_water),
	feature(is_Feline).

elephant :- herbivores, mammal,
	feature(is_big),
	feature(has_grey_skin_color),
	feature(has_long_nose).

cheetah :- carnivore,mammal,
	feature(is_Feline),
	feature(is_so_fast),
	feature(has_dark_color_spot).

	
lion :- carnivore, mammal,
	feature(has_sharp_Teeth),
	feature(is_called_the_king_of_the_Jungle),
	feature(has_golden_fur).

	
tiger :-carnivore, mammal,
	feature(has_sharp_Teeth),
	feature(is_Feline),
	feature(has_golden_fur).



dog :- carnivore,mammal,
	feature(is_Used_for_hunting),
	feature(is_Companion_animals),
	feature(has_a_good_sense_of_smell).
	
	
penguin :-carnivore,birds,
	feature(is_not_fly),
	feature(is_live_in_cold_local).


parrot :- herbivores,birds,
	feature(can_fly),
	feature(can_speak),
	feature(is_Companion_animals).

dolphin :- carnivore,mammal,
	feature(is_swimming_well),
	feature(has_high_IQ),
	feature(makes_an_ultrasonic_sound).


carnivore :- feature(eats_meat).

herbivores :- feature(eats_leaf).


birds :-	feature(lays_eggs),
	  feature(has_wings),
	  feature(has_beak).


mammal :- feature(gives_milk), feature(is_giving_birth),
	feature(has_hair).

각 동물들에 대한 사실들을 정의해놓은 문장입니다.
만약 

cat :- carnivore,mammal,
	feature(is_Companion_animals),
	feature(hates_water),
	feature(is_Feline).
이 실행됐다면 cat에 대한 규칙 carnivore , mammal 이며 Companion animals (반려동물)이며 hates water(물을 싫어하고) Feline(고양이 과) 이라는 규칙을 가지고 있습니다. 

또한carnivore 하고 mammal 의 규칙이 있기 때문에 carnivore 이라는 사실에 대한 규칙들이 실행 됩니다. 이 규칙에 대한 사실에 대해 예를 들면 carnivore하면 “eats_meat” (고기를 먹는다.) 이다. 라는 의미 입니다.




## 결과


{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/3.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/4.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/5.PNG" alt=""> {% endraw %}
{% raw %} <img src="https://bcloved.github.io/assets/images/20200122ZOO/6.PNG" alt=""> {% endraw %}
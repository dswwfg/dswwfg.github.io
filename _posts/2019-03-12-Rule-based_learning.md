---
layout: post
title: 인공지능 및 기계학습개론 2강(1) - Rule-based Learning
category: Machine Learning
tag: [ML Introduction, Rule-based ]
---

이 글에서는 문일철 교수님의 <b>[인공지능 및 기계학습 개론](https://www.edwith.org/machinelearning1_17)</b>  강의의 Chapter 2의 내용 중 Rule-based learning을 요약하고 필요한 내용을 보충설명한다.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-05.jpg?raw=true" width="500">
</p>

위의 슬라이드에서의 표와 같은 binary classification을 위한 데이터가 있다고 생각해보자. Sky, Temperature, Humid, Wind, Water, Forest의 상태에 따라 Sport를 즐길 수 있는지에 대한 데이터(EnjoySpt) 4개의 data가 있다. 예를들어 첫번째 row경우, Sunny, Warm, Normal, Strong, Warm, Same인 상황에서 EnjoySpt가 Yes이다. 

이 데이터는 관측 error가 없고 모든 데이터를 얻을 수 있는 완벽한 세상에서 얻어진 데이터로 가정하고, 이 데이터들로부터 EnjoySpt를 예측할 수 있는deterministic한 target function을 예측하고자 한다. 

>deterministic algorithm은 수학 함수의 형태로 어떤 특정한 입력이 들어오면 언제나 똑같은 과정을 거쳐서 언제나 똑같은 결과를 내놓는 알고리즘

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-06.jpg?raw=true" width="500">
</p>

즉 데이터(or 경험 Experience)로부터 특정 task를 잘 수행하기 위한 function을 approximation하는것이다. 

하나의 Data는 Instance라고 표현할 수 있고 이는 Feature들과 Label로 구성된다. Feature는 판단의 근거가 되고, Label은 실제 판단된 결과를 의미한다. 이 Instance(X)를 여러개 모아놓은 것이 Training Data set(D)가 되는 것이다. 그리고 이 Data set으로부터 가설(Hypotheses, H)을 세워볼 수 있다. 

$$
h_i : <Sunny, Warm, ?, ?, ? Same> \rightarrow Yes
$$

즉, Sky=Sunny, Temp(ferature)=Warm, Forecast=Same이라는 조건만 만족되면 Sport를 즐긴다(EnjoySpt=Yes)라고 가정해보는 것이다. 이렇듯, 각각의 Feature(ex) Sky)가 2개의 option(ex) Sunny and Rainy)이 가능하다고 할 때 총 $2^6 =64$ 개의 option이 가능하다. (사실 그 이상이다. 어떤 feature에 대해 don't care라는 조건이 있을 수 있다.) 
 그리고 Target function(C)가 있는데 이는 실제로는 알 수 없는 function으로 feature와 label을 이용해 H의 형태로 C를 Approximation하고자 하는 것이다.
 
<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-07.jpg?raw=true" width="500">
</p>

그럼 어떤 형태의 hypothesis가 target function을 잘 예측한 것이 될까? 예를들어 위 슬라이드의 $h_1$형태의 hypothesis가 있다. Sunny와 Warm 조건만 만족이 되면 EnjoySpt가 yes인 hypothesis로 instance $x_1, x_2, x_3$가 모두 만족하므로 잘 예측한 hypothesis이다. 하지만 $h_2$는 $h_1$의 조건에 Same조건이 더해진 hypothesis로 instance $x_3$는 만족하지 않는다. 상대적으로 $h_1$은 General한 hypothesis인 반면, $h_2, h_3$는 Specific한 hypothesis이다. 

## S Algorithm
<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-08.jpg?raw=true" width="500">
</p>

그럼 이런 hypothesis를 어떻게 찾을까? Find-S 알고리즘을 통해 찾아보자.
이를 간단히 정리하면 아래와 같다.

```  python
Initialize h to the most specific H 
for instance x in Dataset
	 x is positive
		 for features fs in O
			 if feature f in h == feature f in x
				Do nothing
			else
				f in h = f in h U f in x
return h 
```
위의 과정들을 쉬운 예로 진행해보자.
Instance 4개가 아래와 같이 주어졌다.
- $x_1$ : <Sunny, Warm, Normal, Strong, Warm, Same> $\rightarrow$ yes
- $x_2$ : <Sunny, Warm, Normal, Light, Warm, Same> $\rightarrow$ yes
- $x_3$ : <Rainy, Cold, High, Strong, Warm, Change> $\rightarrow$ no
- $x_4$ : <Sunny, Warm, Normal, Strong, Warm, Change> $\rightarrow$ yes

먼저 가장 specific한 Null hypothesis를 만들자. 이는 어떤 경우에도 만족하지 않는 hypothesis이다. 
- $h_0$=<null, null, null, null, null, null, null>

$x_1$은 positive case이므로 각각의 feature를 hypothesis에 update할 수 있다. 
그러므로 $x_1$을 반영한 $h_1$은 아래와같다.

- $h_1$=<Sunny, Warm, Normal, Strong, Warm, Same>

$x_2$를 반영해보자. $x_2$도 positive case이므로 $h_1$과 비교하여 $x_2$의 결과를 반영한 $h_2$를 만들 수 있다. $x_2$와 $h_1$을 비교했을 때, 4번째 feature만($x_2$:Light, $h_1$:Strong)다르다. feature가 다른 경우, 두 경우를 모두 포함하는 feature를 hypothesis에 반영하므로 4번째 feature는 label 결과에 영향을 미치지 않는다고 가정할 수 있다. 이를 '?' 로 표시하겠다.

- $h_2$=<Sunny, Warm, Normal, ?, Warm, Same>

$x_3$는 negative case 이므로 넘어가고, $x_4$를 보자. $h_2$와 비교했을 때, 6번째 feature만 다르다. 두 경우를 모두 포함할 수 있는 '?'로 표시하겠다.

- $h_3$=<Sunny, Warm, Normal, ?, Warm, ?> 

이 hypothesis가 target function일까? 아닐 수 있다. 실제로는 위의 $h_3$를 포함하여 다양한 hypothesis를 만들 수 있다. (ex) $h_4$=<Sunny, Warm, ?, ?, Warm, ?> 그러므로 이 알고리즘을 이용해서 하나의 hypothesis를 정하는게 맞는지에 대한 의문이 생길 수 있다.

## Version Space

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-09.jpg?raw=true" width="500">
</p>

위의 의문을 해결하기 위해 boundary를 설정하여 hypothesis의 set인 version space를 설정할 수 있다. 그러므로 version space는 specific boundary인 hypothesis보다는 general하고 general boundary인 hypothesis보다는 specific한 hypothesis의 집합이다. 

$$
VS_{H,D}=\{ h | g\ge h\ge s\}
$$

위의 슬라이드처럼 주어진 Data set에 대해 General Boundary와 Specific Boundary를 정할 수 있고, 이 범위의 사이에 있는 hypothesis들을 Version space라고 할 수 있다. 그럼 version space를 위한 General Boundary와 Specific Boundary는 어떻게 정할까? 아래의 Candidate Elimination Algorithm을 통해 구구할 수 있다. 

## Candidate Elimination Algorithm

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-10.jpg?raw=true" width="500">
</p>

Candidate Elimination Algorithm은 과정은 아래와 같다.

```
- Maximally specific한 hypothesis S를 만듦 S0:\{null, null, null, null, null, null  \} (무조건 EnjoySpt가 No)
- Maximally general한 hypothesis G를 만듦 G0:\{?, ?, ?, ?, ?, ?\} (무조건 EnjoySpt가 Yes)
- For instance x in Dataset
	- if x is positive
		- most specific hypothesis를 generalize
	- if x is negative
		- most general hypothesis를 specialize
```
아래의 실제 데이터들을 가지고 위의 과정을 진행해보자.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-11.jpg?raw=true" width="500">
</p>

첫, 두번째 row의 데이터들이 positive case이므로, 이 들을 이용해 S0를 generalize해줘야한다. 먼저 첫번째 row 데이터로 S0->S1으로 generalize해주면, S1은 첫번째 row의 feature들값과 같아진다.

 두번째 row 데이터도 positive case로 첫 데이터와 Humid 만 다르므로 이를 두 경우를 모두 포함할 수 있도록 generalize 해줄 수 있는 '?'로 feature를 변경하다. 


<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-12.jpg?raw=true" width="500">
</p>

3번째 row는 negative case로 general boundary를 update할 수 있다. 하지만, 이를 그냥 general boundary에 넣게되면, EnjoySpt가 Yes가 되는 상황이 되어 버린다. 그래서 specific boundary를 고려해서 모순이 되지 않게 general boundary를 update해야한다. 

첫번째 feature를 고려해보면, specific boundary의 hypothesis는 Sunny이고 negative case의 data는 Cold로 둘이 다르므로, 이를 이용해 version space를 좁힐 수 있다. (<Sunny, ?, ?, ?, ?, ?>) 두번째, 여섯번째 feature도 마찬가지다.

세번째 feature는 specific boundary에서와 negative case의 data 모두 High 이므로 version space를 더 좁힐 수 없다. 네번째, 여섯번째 feature도 마찬가지다.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-13.jpg?raw=true" width="500">
</p>

4번째 row의 data는 positive case 이므로, specific boundary를 업데이트할 수 있다. 이 때 기존의 specific boundary에서, 5, 6번째 feature가 'don't care'인 '?'로 바뀐다. 여기서 추가적으로 한가지를 더 해줘야하는데 아래의 가장 general한 boundary hypothesis중 6번째 feature가 'Same'이어야 하는 case가 data와 모순되므로, 이를 제거해줘야한다. 이러한 과정을 통해 version space를 좁힐 수 있고, target function이 이 범위 내에 있는 hypothesis라고 생각할 수 있다.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-14.jpg?raw=true" width="500">
</p>

그렇다면 새로운 instance에 대해 어떻게 classify하는가? 아래의 3가지 instance 예를 보자. 

- <Sunny, Warm, Normal, Strong,, Cool, Change>
- <Rainy, Cold, Normal, Light, Warm, Same>
- <Sunny, Warm, Normal, Light, Warm, Same>

첫번째 instance는 most specific boundary를 만족하는 instance로 yes이고, 두번째 instance는 most general boundary를 만족하지 못하는 instance로 no이다.

세번째 instance를 보자. 이는 most specific boundary는 만족시키지 못하지만, most general boundary는 만족을 시키는 데이터로 현재는 결정할 수 없는 상황이다. 이런 문제 때문에 rule-based learning이 실제 상황에 사용되기가 어렵다.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 1-icmoon-ver-1-14.jpg?raw=true" width="500">
</p>

이는 data의 noise, 다른 decision factor 등에 따라 정확한 hypothesis를 만들 수 없고, yes or no의 결정을 할 수 없다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgzODQ0NTY2MF19
-->
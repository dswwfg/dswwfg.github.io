---
layout: post
title: 인공지능 및 기계학습개론 2강(2) - Decision Tree
category: Machine Learning
tag: [ ML Introduction, Decision Tree]
---
이 글에서는 문일철 교수님의 <b>[인공지능 및 기계학습 개론](https://www.edwith.org/machinelearning1_17)</b>  강의의 Chapter 2의 내용 중 <b>Decision tree</b>를 요약하고 필요한 내용을 보충설명한다.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/IE661-Week 2-Part 2-icmoon-ver-1-04.jpg?raw=true" width="500">
</p>

우리는 noise가 있는 세상에서 살고 있고 noise를 고려해서 robust한 learning algorithm이 필요하다. 이에 대해 rule-based algorithm의 대안 중 가장 접근하기 쉬운 algorithm이 decision tree이다. 

Decision tree는 rule-based learning에서의 hypothesis를 tree형태로 나타내는 algorithm이다. 위의 슬라이드에서 hypothesis와 그에 대응하는 decision tree를 예로 보자.

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/week2/decision_tree_corresponding_hypothesis.PNG?raw=true" width="500">
</p>

<b><Sunny, ?,?,?,?,?></b> hypothesis는 Sunny 하나의 feature 값에 대해서만 영향을 받아 결과가 달라진다. 이를 tree구조로 나타내면 Sky라는 feature를 네모 박스 안(node)에 표시하고 이를 root로 하여 가질 수 있는 feature값들 (Rainy, Sunny)에 따라 결과값(label)을 leaf node로 표현할 수 있다. 

<b><Sunny, Warm, ?, Strong, ?, ?></b> hypothesis는 좀 더 복잡하게 표현이 된다. 3개의 feature(Sunny, Warm, Strong)에 따라 결과가 달라지기 때문에 3개의 네모 박스 안에 표시되고 각 feature가 가질 수 있는 값들에 따라 결과값이 달라지고 이를 타원형인 leaf node로 표현할 수 있다.

---- 진행중-----


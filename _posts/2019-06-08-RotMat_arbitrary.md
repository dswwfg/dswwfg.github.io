---
layout: post
title: Rotation matrix about artibrary axis
category: Robotics
tag: [ Robotics, Rotation Matrix]
---

책 Introduction to Robotics by Craig 참고하여 임의의 축에 대한 rotation matrix를 설명한다. 

먼저 X, Y, Z축에 대한 rotation matrix는 아래와 같다. 

$$
R_x(\theta ) = \begin{bmatrix} 1 & 0 & 0 \\
0 & cos\theta & -sin\theta \\
 0 & sin\theta & cos\theta\end{bmatrix} \tag{1}
$$

$$
R_y(\theta ) = \begin{bmatrix} cos\theta & 0 & sin\theta \\
0 & 1 & 0 \\
 -sin\theta & 0 & cos\theta\end{bmatrix} \tag{2}
$$

$$
R_z(\theta ) = \begin{bmatrix} cos\theta & -sin\theta & 0 \\
 sin\theta & cos\theta & 0 \\
 0 & 0 & 1\end{bmatrix} \tag{3}
$$

그렇다면 X, Y, Z축이 아닌 임의의 vector에 대한 회전을 나타내는 rotation matrix는 어떻게 나타낼까?

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/rotation_about_arbitrary_axis/rotation_about_arbitrary_axis.JPG?raw=true" width="500">
</p>

알려진 {A} frame과 일치했던 {B} frame을 임의의 vector $\hat{K}$ 에 대해 $\theta$만큼 회전했을 때, {A}에 대한 {B}의 orientation을 구해보자. 

<p align="center">
	<img src="https://github.com/dswwfg/dswwfg.github.io/blob/master/public/post_img/rotation_about_arbitrary_axis/rotation_about_arbitrary_axis_2.JPG?raw=true" width="500">
</p>

vector $^AK$ 에 대한 회전을 다음과 같이 생각할 수 있다. frame {C}를 도입해서 {C}의 Z축 $Z_C$가 vector $^AK$와 시작점과 방향이 같다고 가정하면, 아래와 같이 rotation 을 표현할 수 있다.

$$
R = {^A_CR}\ rot(^c\hat{Z}, \theta)\ {^C_AR} \tag{4}
$$
<p></p>
우변을 말로 풀어서 설명하면, {A}를 {C}로 회전 후, {C}의 Z축( $^AK$)에 대한 회전, 그리고 다시 {C}를 {A}로 회전해서, 임의의 vector에 대한 회전을 표현할 수 있다. 

위식의 우변 성분을 각각 행렬로 표현해보자. $^A_C{R}$을 다음과 같이 표현할 수 있다. 

$$
{^A_CR}=\begin{bmatrix} A & D & K_x \\
B & E & K_y \\
C & F & K_z\\
\end{bmatrix} \tag{5}
$$

$K_x, K_y, K_z$는 $^AK$를 {A}의 각 좌표축 성분으로 나타낸 것이다. 



${^C_AR} = {^A_CR}^{-1} = {^A_CR}^{T}$ 이므로 아래와 같다. 

$$
{^C_AR}={^A_CR}^{T}=\begin{bmatrix} A & B & C \\
D & E & F \\
K_x & K_y & K_z\\
\end{bmatrix} \tag{6}
$$

${^A_CR}$ 와 ${^C_AR}$는 roation matrix 이자 orthogonal matrix이므로 아래와 같은 성질을 가진다.



- 각각의 column vector $C_i$의 크기 $\mid C_i \mid = 1$ 

$A^2+B^2+C^2=1, D^2+E^2+F^2=1, {K_x}^2+{K_y}^2+{K_z}^2=1$ 

$A^2+D^2+{K_x}^2=1, B^2+E^2+{K_y}^2=1, C^2+F^2+{K_z}^2=1$			$\tag{7}$

- $C_i\cdot C_j = 0$

$AD+BE+CF=0, AK_x+BK_y+CK_z=0, DK_x+EK_y+FK_z=0$ 

$AB+DE+K_xK_y=0, AC+DF+K_xK_z=0, BC+EF+K_yK_z=0$ 	  					$\tag{8}$

- $C_1\otimes C_2 = C_3 $

 $$ det \begin{bmatrix}i & j &k \\ A & B & C \\ D &E & F\end{bmatrix} = K_x i + K_y j + K_z k$$

$BF-CE = K_x, CD-AF=K_y, AE-BD=K_z$ 																	$\tag{9}$

$$det \begin{bmatrix}i & j &k \\ A & D & K_x \\ B &E & K_y\end{bmatrix} = C i + F j + K_z k $$

  $DK_y-EK_x = C, BK_x-AK_y=F, AE-BD=K_z$ 																$\tag{10}$

$rot(^c\hat{Z}, \theta)$는 Z축에 대해 $\theta$만큼 회전에 대한 rotation matrix로 아래와 같다.

$$
R_z(\theta) = \begin{bmatrix} cos\theta & -sin\theta & 0 \\
 sin\theta & cos\theta & 0 \\
 0 & 0 & 1\end{bmatrix}
$$

다시 식을 정리해보면, 

$$
\begin{align*} 
R &= {^A_CR}\ rot(^c\hat{Z}, \theta)\ {^C_AR} \\
\\
&=\begin{bmatrix}
A & D & K_x \\
B & E & K_y \\
C & F & K_z \\
\end{bmatrix} 
\begin{bmatrix}
cos\theta & -sin\theta & 0 \\
sin\theta & cos\theta & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
A & B & C \\
D & E & F \\
K_x & K_y & K_z \\
\end{bmatrix} \\
\\
&= \begin{bmatrix}
Acos\theta+Dsin\theta & -Asin\theta+Dcos\theta & K_x \\
Bcos\theta+Esin\theta & -Bsin\theta+Ecos\theta & K_y \\
Ccos\theta+Fsin\theta& -Csin\theta+Fcos\theta & K_z \\
\end{bmatrix} 
\begin{bmatrix}
A & B & C \\
D & E & F \\
K_x & K_y & K_z
\end{bmatrix}
\\
\\&=\begin{bmatrix}
r_{11} & r_{12} & r_{13} \\
r_{21} & r_{22} & r_{23} \\
r_{31} & r_{32} & r_{33}
\end{bmatrix}
\end{align*}
$$
<p></p>
<p></p>
$$
\begin{align*} r_{11}&=A^2cos\theta + ADsin\theta - ADsin\theta + D^2cos\theta + K_xK_x \\ \\ &=(A^2+D^2)cos\theta + K_xK_x=(1-K_xK_x)cos\theta + K_xK_x (\because eq(7)) \\ \\ &=K_xK_x(1-cos\theta) + cos\theta \end{align*}
$$
<p></p>
$$
\begin{align*} r_{12}&=ABcos\theta + BDsin\theta - AEsin\theta + DEcos\theta + K_xK_y \\ \\ &=(AB+DE)cos\theta + (BD-AE)sin\theta + K_xK_y\\ \\&=(-K_xK_y)cos\theta + -K_zsin\theta+K_xK_y (\because eq(8),  eq(9)) \\ \\ &=K_xK_y(1-cos\theta) - K_zsin\theta\end{align*}
$$
<p></p>
$$
\begin{align*}r_{13}&=ACcos\theta+CDsin\theta-AFsin\theta+DFcos\theta+K_xK_z \\ \\ &= (AC+DF)cos\theta+(CD-AF)sin\theta+K_xK_z\\ \\&= -K_xK_zcos\theta +K_ysin\theta+K_xK_z (\because eq(8), eq(9)) \\ \\ &=K_xK_z(1-cos\theta)+K_ysin\theta \end{align*}
$$
<p></p>
$$
\begin{align*} r_{21}&=ABcos\theta+AEsin\theta-BDsin\theta+DEcos\theta+K_xK_y \\ \\ &=(AB+DE)cos\theta+(AE-BD)sin\theta+K_xK_y\\ \\&=(-K_xK_y)cos\theta+K_zsin\theta+K_xK_y(\because eq(8), eq(9)) \\ \\&=(1-cos\theta)K_xK_y+K_zsin\theta\end{align*}
$$
<p></p>
$$
\begin{align*} r_{22}&=BBcos\theta+BEsin\theta-BEsin\theta+EEcos\theta+K_yK_y \\ \\ &=(B^2+E^2)cos\theta+K_yK_y\\ \\&=(1-K_yK_y)cos\theta+K_yK_y(\because eq(7)) \\ \\&=(1-cos\theta)K_xK_y+cos\theta\end{align*}
$$
<p></p>
$$
\begin{align*} r_{23}&=BCcos\theta+CEsin\theta-BFsin\theta+EFcos\theta+K_yK_z \\ \\ &=(BC+EF)cos\theta+(CE-BF)sin\theta+K_yK_z\\ \\&=(-K_yK_z)cos\theta-K_xsin\theta+K_yK_z(\because eq(8), eq(9)) \\ \\&=(1-cos\theta)K_yK_z-K_xsin\theta\end{align*}
$$
<p></p>
$$
\begin{align*} r_{31}&=ACcos\theta+AFsin\theta-CDsin\theta+DFcos\theta+K_xK_z \\ \\ &=(AC+DF)cos\theta+(AF-CD)sin\theta+K_xK_z\\ \\&=(-K_xK_z)cos\theta-K_ysin\theta+K_xK_z(\because eq(8), eq(9)) \\ \\&=(1-cos\theta)K_xK_z-K_ysin\theta\end{align*}
$$
<p></p>
$$
\begin{align*} r_{32}&=BCcos\theta+BFsin\theta-CEsin\theta+EFcos\theta+K_yK_z \\ \\ &=(BC+EF)cos\theta+(BF-CE)sin\theta+K_yK_z\\ \\&=(-K_yK_z)cos\theta+K_xsin\theta+K_yK_z(\because eq(8), eq(9)) \\ \\&=(1-cos\theta)K_yK_z+K_xsin\theta\end{align*}
$$
<p></p>
$$
\begin{align*} r_{33}&=CCcos\theta+CFsin\theta-CFsin\theta+FFcos\theta+K_zK_z \\ \\ &=(C^2+F^2)cos\theta+K_zK_z\\ \\&=(1-{K_z}^2)cos\theta+K_zK_z(\because eq(7)) \\ \\&=(1-cos\theta)K_zK_z+cos\theta\end{align*}
$$
<p></p>

위의 과정을 통해 임의의 벡터와 회전 각도 $\theta$ 가 주어지면 rotation matrix를 구할 수 있다. 
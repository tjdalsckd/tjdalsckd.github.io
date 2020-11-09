---
title: "Grasp quality measures: review and performance"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
---
## Grasp quality measures: review and performance
### 1. Grasp과 Grasp Planning이란?
본 논문은 Autonomous Robots에 2014년 실린 저널로 Grasp Quality Measure에 대한 리뷰 페이퍼이다.

Grasp의 목표는 외부에서 작용하는 Disturbance(물체 자체 무게를 포함) 하에서 어떤 물체에 제약조건을 거는 것이다. 

Grasp Planning은 적절한 그리퍼 선택과, 그 그리퍼를 이용하여 물체와의 Contact Point를 결정하는 것을 말한다.

### 2. 일반적인 Grasp Planning 방식
Grasp Planning은 크게 두 가지 방식으로 나뉜다. 먼저 Empirical grasp 방식은 여러가지 방법들을 통해 Grasp에 성공하는 적절한 Contact point(또는 Hand Configuration)을 학습하고 결정한다. Neural Network를 이용한 학습, Fuzzy logic, Knowledge-based systems 등이 있다.

다른 방식은 Analytic한 방식으로 물체와 Hand 사이의 Interaction을 수학적인 모델을 세워 분석하는 방식이다. 2D polygonal을 이용한 해석, 3D polyhedral Objects를 이용한 해석 등이 있다.

### 3. Grasp Synthesis Algorithm의 특성
#### * Disturbance resistance
Grasp은 어떠한 방향에서 오는 Disturbance더라도 견뎌내야 한다. 즉 물체의 Immobility가 보장되어야만 한다.(Form closure, Force Closure)
#### * Dexterity
Hand를 이용하여 물체를 작업이 가능하도록 움직인다면 이를 Dextrous하다 라고 한다.
#### * Equilibrium
Grasp이 Equilibrium하다는 것은 물체에 가해지는 토크나 힘의 결과가 없는 경우를 의미한다.
#### * Stability
Disturbance에 의한 Object Position Error가 시간에 지남에 따라 사라지는 것을 Stable하다 한다.

### 4. Basic background and nomenclature
접촉점에 작용하는 force는 물체에 대해서만 작용할 수 있다.(positivity constraint), 그리고 fingertip과 물체 사이의 접촉은 다음과 같다.
#### Punctual contact without friction
마찰이 없는 표면에 가해지는 force는 정확히 접촉 점 경계의 normal 벡터이다.
#### Punctual contact with friction(hand contact)
마찰이 있는 경우는 여러가지 모델로 표현이 가능하지만 주로 쓰이는 것은 Coulomb's friciotn cone 방식이다.
#### Soft Contact
3D 물체에서만 가능한 분석으로 접촉 점에서 force 뿐만아니라 접촉 점 경계에서의 torque까지를 포함한 분석이다.
![그림1](https://user-images.githubusercontent.com/53217819/98537729-0c9a2200-22cd-11eb-9adf-350aac2c558a.png)

각 접촉점에 적용되는 wrenches의 indepedent components의 숫자를 $r$ 이라 했을 때에 $r = 1$일 때는 마찰이 없는 접촉, $r = 2$일 때는 마찰이 존재하는 2D 접촉, $r= 3$일 때는 마찰이 존재하는 3D 접촉, $r = 4$일 때는 soft contact이다.

물체에 작용하는 torque 는 $\tau_i = p_i \times F_i$ 이며 물체의 center of mass을 기준으로 작용한다. 이 때의 force 와 torqure를 묶어서 wrench vector $\omega_i = (F_i,\tau_i/\rho)^T$로 나타낸다. $\rho$는 wrench space의 metric이며 constant로 정의된다. $\rho$는 물체의 회전반경과 CM으로부터의 가장 먼 물체 표면의 점으로 기술된다. wrench vector의 dimension은 2D의 경우 $d = 3$, 3D의 경우 $d = 6 $이다.

CM에서의 선속도와 회전 속도를 twist라고 부르며 다음과 같이 나타낸다. $\dot{x} = (v,w)^T $ twist는 2D의 경우 $R^3$ 3D의 경우 $R^6$에 속한다.

각 finger의 joint에 작용하는 토크는 다음과 같이 표기한다.
$T = [ T _ { 1 j } ^ { T } \ldots T _ { n j } ^ { T } ] ^ { T } \in R ^ { n m }$

![그림2](https://user-images.githubusercontent.com/53217819/98538175-b679ae80-22cd-11eb-8993-c9aec3b116cd.png)

i개의 fingertip에서의 contact force 는 $f_i$ 로 표기하며 마찰력이 있는 경우 contact force component는 다음과 같이 표기한다.

$f = [ f _ { 1 k } ^ { T } \ldots f _ { n k } ^ { T } ] ^ { T } \in R ^ { n r } ( k = 1 , \ldots , r )$

i개의 fingertip에서의 contact point들의 velocity component는 다음과 같이 표기한다.

$\nu = [ \nu _ { 1 k } ^ { T } \ldots \nu _ { n k } ^ { T } ] ^ { T } \in R ^ { n r }$

![그림3](https://user-images.githubusercontent.com/53217819/98540689-ab288200-22d1-11eb-84cc-ccb5d96eb8f9.png)

### 5. Grasp Matrix $G$

![그림4](https://user-images.githubusercontent.com/53217819/98540105-c1820e00-22d0-11eb-9781-5609de7effd3.png)

#### Contact Jacobian

Joint velocity로부터 Contact Points의 velocity로 변환해주는 Jacobian


$v = J _ { h } \dot { \theta } $


$J _ { h } = \operatorname { diag } [ J _ { 1 } , \ldots , J _ { i } ] \in R ^ { n r \times n m }$

#### Grasp Matrix $G$

Object의 Twist로부터 Contact Points의 Velocity로 변환해주는 Jacobian $G^T$


$nu = G ^ { T } \dot { x }$


$G \in R ^ { d \times n r }$

dd



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

#### * 


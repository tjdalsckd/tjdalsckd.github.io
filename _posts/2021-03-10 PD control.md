---
title: "Pybullet을 이용한 Two link robot PD control"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
Shell:      console, shell
sitemap :
  changefreq : daily
  priority : 1.0
---

무게나 inertia를 고려하지 않은 단순 위치기반 토크 제어 코드입니다.


```bash
import pybullet as p
import time
import pybullet_data
import numpy as np
import math


## setup
useMaximalCoordinates = False
p.connect(p.GUI)
p.setGravity(0, 0, -10)
p.setAdditionalSearchPath(pybullet_data.getDataPath())

pole = p.loadURDF("twolink_robot/twolink.urdf", [0, 0, 0], useMaximalCoordinates=useMaximalCoordinates)
p.resetBasePositionAndOrientation(pole, [0, 0, 0], [0, 0, 0, 1])

p.setJointMotorControl2(pole, 0, p.POSITION_CONTROL, targetPosition=0, force=0)
p.setJointMotorControl2(pole, 1, p.POSITION_CONTROL, targetPosition=0, force=0)
timeStepId = p.addUserDebugParameter("timeStep", 0.001, 0.1, 0.01)
useRealTimeSim = False
p.setRealTimeSimulation(useRealTimeSim)


numJoints = p.getNumJoints(pole)

desired_q = np.array([0,0]);
desired_qdot = np.array([0,0]);
kpCart = 500
kdCart = 150
kpPole = 500
kdPole = 150
prev_q = 0

kps = [kpCart, kpPole]
kds = [kdCart, kdPole]
maxF = np.array([100,100]);


while p.isConnected():
	timeStep = p.readUserDebugParameter(timeStepId)
	p.setTimeStep(timeStep)
	jointStates = p.getJointStates(pole,[0,1])
	q = np.array([jointStates[0][0],jointStates[1][0]])
	qdot = np.array((q-prev_q )/timeStep)


###       CONTROL LOOP
	qError = desired_q - q;
	qdotError = desired_qdot - qdot;
	Kp = np.diagflat(kps)
	Kd = np.diagflat(kds)
	torques = Kp.dot(qError)+Kd.dot(qdotError)
	
	torques = np.clip(torques,-maxF,maxF)
	prev_q = q

###	


	p.setJointMotorControlArray(pole, [0,1], controlMode=p.TORQUE_CONTROL, forces=torques)
	
	if (not useRealTimeSim):
		p.stepSimulation()
		time.sleep(timeStep)
```
![Screenshot from 2021-03-10 20-54-59](https://user-images.githubusercontent.com/53217819/110625562-ed5fc280-81e2-11eb-870c-7bf2f35de5d7.png)

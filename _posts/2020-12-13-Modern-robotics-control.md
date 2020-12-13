---
title: "Indy7 Control matlab 코드"
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

https://github.com/tjdalsckd/indy7_matlab



```bash
clc; clear;
thetalist = [0;pi/2;0;0;0;0];
dthetalist = [0.1; 0.2; 0.3;0.1;0.1;0.1];
%Initialize robot description (Example with 3 links)
H1 = 0.3;
H2 = 0.45;
H3 = 0.350;
H4 = 0.228;
W1 = 0.0035;
W2 = 0.183;
S1 = [0; 0;  1;  0; 0;  0];
S2 =        [0; -1;  0; H1;0 ;  0];
S3 =        [0; -1;  0; H1+H2; 0; 0];
S4 =        [0; 0;   1; -W1; 0; 0];
S5 =        [0; -1;  0; H1+H2+H3; 0; 0];
S6 =        [0; 0;  1; -W1-W2;0; 0];
M01 = [1 0 0 0;...
       0 1 0 0;
       0 0 1 H1;
       0 0 0 1]
M12 = [0  -1   0   0;...
      0   0   -1   0;...
      1   0    0   0;
      0   0    0   1 ];
M23 = [0 -1 0 H2;...
       1 0 0 0;...
       0 0 1 0;
       0 0 0 1 ];
M34 = [-1 0  0 0;...
       0 0  -1 -H3;...
       0 -1 0 W1;...
       0 0 0 1 ];
M45 = [1 0 0 0;...
      0  0 -1 -W2 ;...
      0  1 0 0;...
      0  0 0 1 ];
M56 = [1 0 0 0;...
      0 0 1 H4;...
      0 -1 0 0;...
      0 0 0 1 ];

  
I0 = Inertia_matrix(0.00572623,0.00558989,0.00966674,0.00000251,-0.0000014,-0.00011380);
I1 = Inertia_matrix(0.15418559,0.12937017,0.05964415,-0.00000235,-0.04854267,0.00001739);
I2 = Inertia_matrix(0.2935698,0.28094142,0.03620609,-0.0000004,0.03727972,0.00001441);
I3 = Inertia_matrix(0.03424593,0.03406024,0.00450477,0.00000149,0.00186009,0.00000724);
I4 = Inertia_matrix(0.00670405,0.00279246,0.00619341,0.00000375,-0.00127967,0.0000015);
I5 = Inertia_matrix(0.00994891,0.00978189,0.00271492,0.00000014,-0.00093546,0.00000321);
I6 = Inertia_matrix(0.00043534,0.00044549,0.00059634,0.00000013,0.00000051,-0.00000002);
mass0 = 1.59306955;
mass1 = 11.8030102;
mass2 = 7.99292141;
mass3 = 2.99134127;
mass4 = 2.12317035;
mass5 = 2.28865091;
mass6 = 0.40083918;


G1 = eye(6)*mass1;
G1(1:3,1:3) = I1;
G2 = eye(6)*mass2;
G2(1:3,1:3) = I2;
G3 = eye(6)*mass3;
G3(1:3,1:3) = I3;
G4 = eye(6)*mass4;
G4(1:3,1:3) = I4;
G5 = eye(6)*mass5;
G5(1:3,1:3) = I5;
G6 = eye(6)*mass6;
G6(1:3,1:3) = I6;
Glist = cat(3, G1, G2, G3,G4,G5,G6);
Mlist = cat(3, eye(4),M01, M12, M23, M34,M45,M56);
Slist = [S1,S2,S3,S4,S5,S6];
g = [0; 0; -9.8];
dt = 0.01;
%Create a trajectory to follow
thetaend =[ 0; 0;0;0;0;0];
Tf = 1;
N = Tf / dt;
method = 5;
thetamatd = JointTrajectory(thetalist, thetaend, Tf, N, method);
dthetamatd = zeros(N, 6);
ddthetamatd = zeros(N, 6);
dt = Tf / (N - 1);
for i = 1: N - 1
  dthetamatd(i + 1, :) = (thetamatd(i + 1, :) - thetamatd(i, :)) / dt;
  ddthetamatd(i + 1, :) = (dthetamatd(i + 1, :) ...
                          - dthetamatd(i, :)) / dt;
end

Ftipmat = zeros(N, 6);
Kp = 100;
Ki = 100;
Kd = 10;
intRes = 8;
[taumat, thetamat] ...
= SimulateControl(thetalist, dthetalist, g, Ftipmat, Mlist, Glist, ...
                Slist, thetamatd, dthetamatd, ddthetamatd, g, ...
                Mlist, Glist, Kp, Ki, Kd, dt, intRes);

dthetalist = [0 0 0 0 0 0]';      
Ftip = [ 0 0 0 0 0 0]';
show_mesh = 1;
fv0 = stlread('mesh/Indy7_0.stl');
fv1 = stlread('mesh/Indy7_1.stl');
fv2 = stlread('mesh/Indy7_2.stl');
fv3 = stlread('mesh/Indy7_3.stl');    
fv4 = stlread('mesh/Indy7_4.stl');   
fv5 = stlread('mesh/Indy7_5.stl');
fv6 = stlread('mesh/Indy7_6.stl');   
v = VideoWriter('newfile.avi','Motion JPEG AVI');
v.Quality = 100;
open(v)
figure('Renderer', 'painters', 'Position', [0 0 1920 1080])
for i = 1:1:100
taulist = taumat(i,:);
thetalist = thetamat(i,:);
ddthetalist = ForwardDynamics(thetalist, dthetalist, taulist, g, ...
                             Ftip, Mlist, Glist, Slist);
dthetalist  = dthetalist+ddthetalist*dt;                         

subplot(1,2,1)

drawrobot(thetalist',show_mesh,fv0,fv1,fv2,fv3,fv4,fv5,fv6);
title('D-H parameter')
subplot(1,2,2)

drawrobot_screw(thetalist',show_mesh,fv0,fv1,fv2,fv3,fv4,fv5,fv6);

title('PoE')
frame = getframe(gcf);
   writeVideo(v,frame);
end
close(v);
```


![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/53217819/102008522-9c236c80-3d74-11eb-8cdf-ecf0cf1d97ce.gif)

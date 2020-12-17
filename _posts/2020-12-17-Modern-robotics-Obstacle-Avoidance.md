---
title: "Inverse Jacobian을 이용한 Obstacle Avoidance Trajectory 생성 matlab 코드"
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

https://github.com/tjdalsckd/ModernRobotics_Obstacle_Avoidance_example/



```bash
addpath('mr')
clear
close all;
L1 = 1;
L2 = 1;
L3 = 1;
L4 = 1;
L5 = 1;
L6 = 1;
L7 = 1;
L8 = 1;

S1 = [0 0 1 0 0 0]';
S2 = [0 0 1 0 -L1 0]';
S3 = [0 0 1 0 -L1-L2 0]';
S4 = [0 0 1 0 -L1-L2-L3 0]';
S5 = [0 0 1 0 -L1-L2-L3-L4 0]';
S6 = [0 0 1 0 -L1-L2-L3-L4-L5 0]';
S7 = [0 0 1 0 -L1-L2-L3-L4-L5-L6 0]';
S8 = [0 0 1 0 -L1-L2-L3-L4-L5-L6-L7 0]';


M = [1 0 0 L1+L2+L3+L4+L5+L6+L7+L8;...
    0 1 0 0;...
    0 0 1 0;...
    0 0 0 1];

M09 = [1 0 0 L1+L2+L3+L4+L5+L6+L7+L8;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M08 = [1 0 0 L1+L2+L3+L4+L5+L6+L7;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M07 = [1 0 0 L1+L2+L3+L4+L5+L6;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M06 = [1 0 0 L1+L2+L3+L4+L5;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M05 = [1 0 0 L1+L2+L3+L4;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M04 = [1 0 0 L1+L2+L3;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M03 = [1 0 0 L1+L2;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];


M02 = [1 0 0 L1;...
    0 1 0 0 ;...
    0 0 1 0;...
    0 0 0 1 ];

M01 = eye(4);



Slist = [S1,S2,S3,S4,S5,S6,S7,S8];
thetastart = [pi/2,pi/2,0,pi/2,0,-pi/2,-pi/2,pi/2]';

matS1 = MatrixExp6(VecTose3(S1)*thetastart(1));
matS2 = MatrixExp6(VecTose3(S2)*thetastart(2));
matS3 = MatrixExp6(VecTose3(S3)*thetastart(3));
matS4 = MatrixExp6(VecTose3(S4)*thetastart(4));
matS5 = MatrixExp6(VecTose3(S5)*thetastart(5));
matS6 = MatrixExp6(VecTose3(S6)*thetastart(6));
matS7 = MatrixExp6(VecTose3(S7)*thetastart(7));
matS8 = MatrixExp6(VecTose3(S8)*thetastart(8));

T_start = matS1*matS2*matS3*matS4*matS5*matS6*matS7*matS8*M;
T_end = [-1 0 0 -5; 0 -1 0 0; 0 0 1 0; 0 0 0 1];

Tf = 1
N = 100
dt = Tf/N;
traj = CartesianTrajectory(T_start, T_end, Tf, N, 5)

start_p = [-4,0,0];
end_p = [-6,0,0];
traj_w=cubic_polynomials(0,-pi/2,Tf,N);
traj_x=cubic_polynomials(start_p(1),end_p(1),Tf,N);
traj_y=cubic_polynomials(start_p(2),end_p(2),Tf,N);
traj_z=cubic_polynomials(start_p(3),end_p(3),Tf,N);

trajV = zeros(N,6);
trajV(:,3) =0;
trajV(:,4) = traj_x;
trajV(:,5) = traj_y;
trajV(:,6) = traj_z;

Twist = trajV
thetalist = thetastart;
x_obstacle_ = linspace(-4,-3,100);
y_obstacle_ = linspace(2,1,100);
z_obstacle_ = linspace(1,0,100) ;
radius =1.8;
Mlist = {M02,M03,M04,M05,M06,M07,M08,M09}
% 
% v = VideoWriter('newfile.avi','Motion JPEG AVI');
% v.Quality = 100;
% open(v)
% figure('Renderer', 'painters', 'Position', [0 0 1920 1080])

for i = 1:1:100
    x_obstacle = x_obstacle_(i)
    y_obstacle = y_obstacle_(i)
    z_obstacle = z_obstacle_(i)
    
    Js = JacobianSpace(Slist, thetalist');
    [dist,px,py,pz] = distanceJoint(Mlist,Slist,thetalist,x_obstacle,y_obstacle,z_obstacle);
    [min_dist,min_dist_ind] = min(dist);
    psuedo_Js = pinv(Js);
    J0 = Js(4:end,:);
    J0(:,10-min_dist_ind+1:end) = 0;

    ObTwist = 1/min_dist.^2*[(px(min_dist_ind)-x_obstacle)/(abs(px(min_dist_ind)-x_obstacle)+0.0001) (py(min_dist_ind)-y_obstacle)/(abs(py(min_dist_ind)-y_obstacle)+0.0001) (pz(min_dist_ind)-z_obstacle)/(abs(pz(min_dist_ind)-z_obstacle)+0.0001) ]';
    alpha_0 = 1;
    alpha_eta = 1;
    if min_dist >radius*2
         dthetalist  = psuedo_Js*Twist(i,:)';
         
    else
        dthetalist  = psuedo_Js*Twist(i,:)'+5*(eye(8)-psuedo_Js*Js)*pinv(J0)*(10*ObTwist-J0*psuedo_Js*Twist(i,:)');
%           dthetalist  = psuedo_Js*Twist(i,:)'+30*(eye(8)-psuedo_Js*Js)*pinv(J0)*ObTwist;
    end
    thetalist = thetalist + dthetalist*dt;
    drawrobot(M,Slist,thetalist);

    
    draswobstacle(x_obstacle,y_obstacle,z_obstacle,radius);
%     frame = getframe(gcf);
%    writeVideo(v,frame);

    hold off;
end
% close(v);

```
![ezgif com-gif-maker (5)](https://user-images.githubusercontent.com/53217819/102484872-36075400-40aa-11eb-9178-222ed005c758.gif)


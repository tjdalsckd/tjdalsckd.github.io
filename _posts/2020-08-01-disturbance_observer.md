---
title: " Matlab Disturbance Observer Code"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
---

    close all
    clear
    clc
    home

    % Control frequency is defined as follows %
    s_freq = 1000; %Hz
    s_time = 1/s_freq;

    tf = 2; %sec
    %% model plant
    x_state =[0];
    z_state =[0];

    n=1;
    %% disturbance
    d=-square(linspace(0,4*pi,length(0: s_time : tf)));

    r0 = 50;
    for t_proc=0: s_time : tf
        [t,x_dot] = ode45(@(t,x_dot) dynamic_model(t,x_state,d(n)),[0, s_time] , [x_state(1)] ); 
        x_state(1) = x_dot(end,1);

        [t,z_dot] = ode45(@(t,z_dot)disturbance_observer(t,x_state,z_state,r0),[0, s_time],[z_state(1)] ); 
        index = size(z_dot);
        d_hat = r0*(x_state(1)-z_state(1));
        z_state(1) = z_dot(end,1);    

        %data save
        data(n,1)=t_proc;
        data(n,2)=x_state;
        data(n,3)=d_hat;
        n=n+1;
    end
    %plot
    plot(data(:,1),d);
    hold on;
    d_hat = data(:,3);
    plot(data(:,1),d_hat);
    hold off;
    figure;
    d_error = d'-data(:,3);
    plot(data(:,1),d_error);

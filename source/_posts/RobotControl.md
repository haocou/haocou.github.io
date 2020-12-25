---
title: Robot control
date: 2020-11-27 14:59:25
categories: 
    - Study
    - Robot
    - c++
    - project
tags:
    - robot
comments: false
mathjax: true
---

This tutorials is for robot control, including PID control, force control, hybrid force and position control, impedance control.

<!-- more -->

# Robot dynamics

Usually, the robot dynamics can be formulated by Lagrange (analysis) or Newton-Euler methods (convenient to calculate). The manipulator dynamics is as follow shown,
$$
M(\theta)\ddot{\theta} + C(\theta)\dot\theta + g(\theta)=\tau
$$
where $M$ is the inertial mass term related to joint angle $\theta$, $C$ is the carios term, and gravity term, $g$, is the decomposition to joint space. $\tau$ is the torque attached to each joints.

**Problems:** the dynamics is generally not precise due to friction, assemble error and manufacture error, which requires dynamics calibration. Before that, the kinematics calibration should also be taken. 

## Easy controller case



# PID controller



# Impedance controller

**Motivation.** The manipulator is always rigid, which can be used in convey application. However, it will fail in touching fields. For example, the robot want to reach the desire position, the workpiece will provide the force feedback to preventing it, which will lead to the task failure. Therefore, it is critical that the robot can tune its' own pose according to the force dynamically. This may be like compliance control. 

The compliance is divided into 2 parts: Active control (RCC) and Passive control (Impedance control or admittance control). Usually, the admittance control is used in industrial robot due the closed port of industrial robot.

**Theory.** The basic idea is the simulation the second order system of mass, spring and damper. The goal is to let the robot perform the process of 2nd system. The "soft" or "rigid" situation is determined by the coefficient of 2nd system. This problem can be formulated in task space or configuration space. They have the similar theory as the below,
$$
M_d\ddot e + D_d\dot e + K_de = F_{ext}
$$
where $M_d$, $D_d$, $K_d$ is desired impedance parameter. $e$ is the position error. $F_{ext}$ is the force produced by environment, which can be express in task space or in joint space. If the controller is in joint space, the extern force $F_{ext}$ should be transformed to the joint space using the transpose of Jacobian Matrix.

## admittance control

The basic idea of admittance control is to acquire the  position error $e$. Then, the position error is attached to the desired position. The new desired position is sent to the robot position controller (PID) to control the robot. Finally, the manipulator will exhibit the same dynamic properties as impedance.

The process is that:
$$
\ddot e = M_d^{-1}(F_{ext}-D_d\dot e - K_de)
$$
Then the Euler or Runge Kuta  can be used to solve this equation.

The experiment conducted by ROS is as follow shown.
---
title: Rigid Body Motion
date: 2020-11-27 10:09:22
categories: 
    - Study
    - robot
    - rigid body motion
tags:
    - robot
comments: false
mathjax: true
typora-root-url: ..
---

This tutorials is for rigid body motion, including rotation, rigid-body motion and corresponding velocity. Finally the wrench is given in this tutorials.

<!-- more -->

# Rigid Body Motion

Rigid body motion means that: given any two points $p$ and $q$, which must satisfy the following formula
$$
\|p(t)-q(t)\|=\|p(0)-q(0)\|=const
$$
Besides, rigid body transformation has the following properties

-  Preserved length. Given mapping $g:\mathbb{R}^3\to \mathbb{R}^3$, $\|g(p)-g(q)\|=\|p-q\|$ holds.
- Preserved cross product. Given mapping $g:\mathbb{R}^3\to \mathbb{R}^3$, $g_*(v\times \omega)=g_*(v)\times g_*(\omega)$

# Rotation Motion

In order to describe the orientation of rigid body, the frame is attached to rigid body. The relative pose can be defined as $3\times3$ matrix:
$$
R_{ab}=[{\vec{x}}_{ab} \quad {\vec{y}}_{ab} \quad{\vec{z}}_{ab}]
$$

## Properties of $R$

- The columns of $R\in\mathbb{R}^{3\times3}$, which is $\vec{r}_1,\vec{r}_2,\vec{r}_3 \in \mathbb{R}^3$, are mutually orthonormal. It can be written as
  $$
  RR^T=R^TR=I, detR=+1
  $$
   **cautions**: the rotation matrix has 9 parameters, including 6 constraints (3 for orthogonality and 3 for dot product equal to 1.)

- $R\in SO(3)$. In general, the group satisfies the following axioms: *Closure*, *Identity*, *Inverse*, *Associativity*. The axioms is validated for $SO(3)$.
  $$
  SO(3)=\{R\in\mathbb{R}^3:RR^T=I,detR=+1\}
  $$

## Basic Calculation

- *Composition rule*. $R_{ac}=R_{ab}R_{bc}$

- *Cross product*. Given two vectors $a,b\in \mathbb{R^3}$, the cross product is defined as
  $$
  a\times b=\left[               
    \begin{array}{ccc}   
      a_2b_3-a_3b_2 \\  
      a_3b_1-a_1b_3 \\
      a_1b_2-a_2b_1 \\
    \end{array}
  \right]
  $$
  Since the product is a linear operator, $b\mapsto a\times b$ , can be represented using a matrix. Defining skew matrix
  $$
  a^{\wedge}=\left[               
    \begin{array}{ccc}   
      0 && -a_3 && a_2\\  
      a_3 && 0 && -a_1\\
      -a_2 && a_1 && 0\\
    \end{array}
  \right]
  $$
  we can write
  $$
  a\times b=a^{\wedge}b
  $$

  - ​	**Cross product properties**. 
    $$
    R(v\times w)=(Rv)\times(Rw) 
    $$
  
$$
  R(w)^{\wedge}R^T=(Rw)^{\wedge}
$$

  Caution: Cross product operator lies in Lie algebra space $so(3)$. It is analogy to tangent space in Lie algebra space.

## Exponential coordinates for rotation

<img src="/images/rigidbody/rotationExp.png" style="zoom:80% alt=robot align=center; "/>

<center style="color:#C0C0C0">Fig.1 Joint Exponential Cooridinate</center>



As for a condition for a joint rotated along axis $\vec\omega$, the velocity of a point $q$, in rigid body, can be written as
$$
\dot{q}(t)=\omega\times q(t)={\omega}^\wedge q(t)
$$
This is a time-invariant linear differential equation and the solution can be expressed as
$$
q(t)={e^{\omega t}}q(0)
$$
where $q(0)$ is the initial position and $e^{ \omega t}$ is the matrix exponential (Taylor expansion to calculate it).

### Skew matrix and $so(3)$

Generally, the skew matrix is equal to the $so(3)$. And $so(3)$ is the tangent space of $SO(3)$. Given a skew matrix ${\omega}^\wedge\in so(3)$, $\|\omega\|=1$, and a joint angle $\theta\in\mathbb R$, the rotation matrix is expressed as 
$$
R(\omega,\theta)=exp(\widehat{\omega}\theta)=e^{\widehat{\omega}\theta}=taylor\ expansion
$$
Here given some useful formula formulated by matrix Taylor expansion. Given $\widehat\omega\in so(3)$, the following relations hold:
$$
{\widehat\omega}^2=\omega\omega^T-\|\omega\|^2I
$$
$$
{\widehat\omega}^3=-\|\omega\|^2\widehat\omega
$$

*and higher powers of ${\widehat\omega}$ can be calculated recursively.*

### Rodrigues formula

Using Taylor expansion for $e^{\widehat{\omega}\theta}$, the *Rodrigues formula* is obtained (the term can be replace with sin and cos function.)
$$
e^{\widehat{\omega}\theta}=I+\widehat\omega sin\theta+{\widehat\omega}^2(1-cos\theta), \ \|\omega\|=1
$$

$$
e^{\widehat{\omega}\theta}=I+ {\frac{\widehat\omega} {\|\omega\|}} sin(\| \omega \| \theta) + {\frac{\widehat\omega^2} {\|\omega\|^2}} (1-cos(\|\omega\|\theta)),\|\omega\|\ne 1
$$

*Exponential of skew matrix are orthogonal*. Defining $R:=exp(\widehat\omega\theta) \in SO(3)$. The $R^TR=I$ should be verified. The following equations hold:
$$
[e^{\widehat\omega\theta}]^{-1}=e^{-\widehat\omega\theta}=e^{\widehat\omega^T \theta}=[e^{\widehat\omega\theta}]^T
$$
**Given a rotation matrix $R$, how to get $\widehat\omega$ and $\theta$ ??**  The following equation answer this question.
$$
trace(R)=r_{11}+r_{22}+r_{33}=1+2cos\theta
$$

$$
\theta=cos^{-1}\left( \frac{trace(R)-1} {2} \right)
$$

$$
\omega=\frac{1} {2sin\theta}\left[               
  \begin{array}{ccc}   
    r_{32}-r_{23} \\  
    r_{13}-r_{31} \\
    r_{21}-r_{12} \\
  \end{array}
\right]
$$

### Other representations for rotation

Euler angles(singularity). 

Quaternion is usually used in robotics, for example ROS, due to non-singularity. 

# Rigid Motion 

 Generally, rigid-body motion can be divided into two parts: translation $p\in\mathbb{R^3}$ and rotation $R\in SO(3)$. A configuration of the rigid body is the product space of $\mathbb R^3$ with $SO(3)$, which is denoted as $SE(3)$ (for special Euclidean group):
$$
SE(3)=\{(p,R):p\in\mathbb{R^3},R\in SO(3) \}=\mathbb{R^3}\times SO(3)
$$

## Homogeneous representation

Given $4\times 4$ matrix $g_{ab}\in SE(3)$, the homogeneous representation is written as 
$$
g_{ab}=\left[               
  \begin{array}{ccc}   
    R_{ab} && p_{ab} \\  
    0 && 1 \\
  \end{array}
\right]
,the\ frame\ b\ is\ relative\ to\ frame\ b.
$$
Using homogeneous transformation, some calculation rules are given below:

- If $g_1, g_2 \in SE(3)$, then $g_1g_2\in SE(3)$.

- If $g\in SE(3)$, then the inverse of $g$ is determined by straightforward matrix inversion to be:
  $$
  g^{-1}=\left[               
    \begin{array}{ccc}   
      R^{T} && -R^{T}p \\  
      0 && 1 \\
    \end{array}
  \right],is\ denoted\ as\ g^{-1}=(-R^{T}p, R^{T})
  $$

## Twists

### Exponential coordinates for $SE(3)$

Analogy to the $SO(3)$ coordinates expression, $SE(3)$ is also can be obtained by differential equation. The tangent space of $SE(3)$ is $se(3)$, which is called twists.

Consider the 1-joint robot as shown in Figure, where the rotation axis is $\omega\in \mathbb{R^3},\|\omega\|=1$, and $q \in \mathbb{R^3}$ is the point on the axis. Then, the velocity of the tip point, $p(t)$, is
$$
\dot{p}(t)=\omega\times(p(t)-q)
$$
This formula can be conveniently converted into homogeneous coordinates by defining $4\times4$ matrix $\widehat\xi \in se(3)$ to be
$$
\widehat\xi=\left[               
  \begin{array}{ccc}   
    \widehat\omega && v \\  
    0 && 0 \\
  \end{array}
\right]=
\left[               
  \begin{array}{ccc}   
    v \\  
    \omega \\
  \end{array}
\right]^\wedge
$$
with $v=-\omega\times q$. The differential equation can be written with an extra row appended to it as
$$
\left[\begin{array}{c}
\dot{p} \\
0
\end{array}\right]=\left[\begin{array}{cc}
\widehat{\omega} & -\omega \times q \\
0 & 0
\end{array}\right]\left[\begin{array}{l}
p \\
1
\end{array}\right]=\widehat{\xi}\left[\begin{array}{l}
p \\
1
\end{array}\right] \quad \Longrightarrow \quad \dot{\bar{p}}=\widehat{\xi} \bar{p}
$$
The solution of differential equation is given by
$$
p(t)=e^{\widehat\xi t}p(0)
$$
where $e^{\widehat\xi t}$ is the matrix exponential of the $4\times 4$ matrix ${\widehat\xi t}$, expanded by Taylor series
$$
e^{\widehat{\xi} t}=I+\widehat{\xi} t+\frac{(\widehat{\xi} t)^{2}}{2 !}+\frac{(\widehat{\xi} t)^{3}}{3 !}+\cdots
$$

### $se(3)$ to $SE(3)$ by exponential map

- *Case 1* ($\omega=0$). It means that the translation is the only motion without rotation. A straightforward calculation shows that 

$$
\widehat\xi^2=\widehat\xi^3=\widehat\xi^4=\dots=0
$$

​		so that $exp(\widehat\xi\theta)=I+\widehat\xi\theta$ and hence
$$
e^{\widehat\xi\theta}=\left[               
  \begin{array}{ccc}   
    I && v\theta \\  
    0 && 1 \\
  \end{array}
\right]
$$
​		which is in $SE(3)$ as desired.

- *Case 2* ($\omega\ne0$). Assume $\|\omega\|=1$, and the $SE(3)$ is obtained by some skills, including some defined variables.
  $$
  e^{\widehat\xi\theta}=\left[               
    \begin{array}{ccc}   
      e^{\widehat\omega\theta} && (I-e^{\widehat\omega\theta})(\omega\times v)+\omega\omega^Tv\theta \\  
      0 && 1 \\
    \end{array}
  \right]\quad\omega\ne0
  $$
  which is an element of $SE(3)$.

### $SE(3)$ to $se(3)$ by logarithm

One element of $se(3)$ can be mapped to $SE(3)$ by exponential mapping, called surjectivity. But, the unique $se(3)$ can not be found by Logarithm mapping from $SE(3)$. 

- *Case 1* ($R= I$). If there is no rotational motion, set
  $$
  \widehat\xi=\left[               
    \begin{array}{ccc}   
      0 && \frac{p}{\|p\|} \\  
      0 && 0 \\
    \end{array}
  \right]\quad \theta=\|p\|
  $$
  
- *Case 2* ($R\ne I$). To find $\xi=(v,\omega)$, we equate $e^{\widehat\xi\theta}$ to solve for $v,\omega$. Using equation
  $$
  e^{\widehat\xi\theta}=\left[               
    \begin{array}{ccc}   
      e^{\widehat\omega\theta} && (I-e^{\widehat\omega\theta})(\omega\times v)+\omega\omega^Tv\theta \\  
      0 && 1 \\
    \end{array}
  \right]
  $$
  $\omega$ and $\theta$ are obtained by solving the rotation equation $exp({\widehat\omega\theta})=R$. According to the form of rigid-body motion, $v$ is acquired through the equation below
  $$
  (I-e^{\widehat\omega\theta})(\omega\times v)+\omega\omega^Tv\theta=p
  $$
  the equation is analogy to $Ax=0$, which is expressed as
  $$
  A=(I-e^{\widehat\omega\theta})\widehat\omega+\omega\omega^T\theta
  $$

## Screws: a geometric description of twists

![screw_motions](/images/rigidbody/screw_motions.png)

![generalized_screw_motoin](/images/rigidbody/generalized_screw_motoin.png)

*Lemma: Chasles theory. Every rigid body motion can be realized by a rotation about an axis combined with a translation parallel to that axis.*

To compute the rigid body transformation associated with a screw., we analyze the motion of a point $p\in\mathbb R^3$, as shown in Figure. The final location of the point is given by
$$
gp=q+e^{\widehat\omega\theta}(p-q)+h\theta\omega
$$
where $h$ is pitch, and $h=d/\theta$. This equation can be in homogeneous coordinates,
$$
g\left[               
  \begin{array}{ccc}   
    p\\  
    1 \\
  \end{array}
\right]
=\left[               
  \begin{array}{ccc}   
    e^{\widehat\omega\theta} && (I-e^{\widehat\omega\theta})q+h\theta\omega \\  
    0 && 1 \\
  \end{array}
\right]
\left[               
  \begin{array}{ccc}   
    p  \\  
    1 \\
  \end{array}
\right]
$$
where $q$ is any point in axis.

According to the equation of Twist to $SE(3)$, the twist of screw motion is defined as

- *Case 1* $(h=\infty)$.

$$
\widehat\xi=\left[               
  \begin{array}{ccc}   
    0 && v \\  
    0 && 0 \\
  \end{array}
\right]
$$

- *Case 2* $(h finite)$.
  $$
  \widehat\xi=\left[               
    \begin{array}{ccc}   
      \widehat\omega && -\omega\times q+h\omega \\  
      0 && 0 \\
    \end{array}
  \right]
  $$
  

# Velocity of a Rigid Body

In this section, the velocity of a rigid body whose motion is given by $g(t)$, and  is a curve parameterized by time $t$ in $SE(3)$, is formulated. As for the Euclidean space, the velocity is obvious to be solved. But, the rigid body motion is in special group space: $\dot g(t)\notin SE(3)$ and  $\dot g(t)\notin se(3)$.

## Rotation velocity

### Derivate of Rotation

From the properties of rotation matrix,$R(t)R(t)^T=I$, taking the derivative of parameter t, we obtained the following equation
$$
\dot R R^T+R\dot R^T=0 \Rightarrow \dot R R^T=-(\dot R R^T)^T
$$
The term $\dot R R^T$ is a skew matrix, defined as $\widehat\omega$.

### Velocity of rotation

We know that a vector $q_a,q_b\in\mathbb{R^3}$ is expressed in frame ${a},{b}$ respectively. By applying the rotation matrix, the equation is established
$$
q_a(t)=R_{ab}(t)q_b
$$
Now, taking derivative of time $t$, the rotation velocity is acquired
$$
v^a=\frac{d}{dt}q_a(t)=\dot R_{ab}(t)q_b=\dot R_{ab}R_{ab}^TR_{ab}q_b=\dot R_{ab}R_{ab}^Tq_a
$$
combining with rotation derivation $\widehat\omega_{ab}^s=\dot R_{ab}R_{ab}^T$
$$
v^a=\widehat\omega_{ab}^s q_a=\omega_{ab}^s \times q_a
$$
where $\widehat\omega_{ab}^s$ is expressed in static frame (usually in base frame).

---

**Velocity in body frame**

Define $\widehat\omega_{ab}^b=R_{ab}^T\dot R_{ab}$,  then $v^b=R_{ab}^Tv^a=\omega_{ab}^b \times q_b$

**transformation between frames**

From the definition of $\omega_{ab}$ in static and body frame, the following formula holds
$$
\widehat\omega_{ab}^b=R_{ab}^T\widehat\omega_{ab}^sR_{ab}
$$ {formulation connection}
or in vector space
$$
\omega_{ab}^b=R_{ab}^T\omega_{ab}^s
$$
where $\widehat\omega_{ab}\in\mathbb{R^{4\times 4}}, \omega_{ab}\in\mathbb R^3$.

## Rigid body velocity

Similar to the process of rotation velocity, the rigid body motion in $SE(3)$, $g_{ab}$, $q_a(t)=g_{ab}(t)q_b$. The velocity is obtained through derivation with respect to time $t$.
$$
v^a=\frac{d}{dt}q_a(t)=\dot g_{ab}(t)q_b=\dot g_{ab}g_{ab}^Tg_{ab}q_b=\dot g_{ab}g_{ab}^Tq_a
$$
Define $\widehat v_{ab}^s=\dot g_{ab} g_{ab}^{-1}$, which has the matrix form
$$
\dot g_{ab} g_{ab}^{-1}=\left[               
  \begin{array}{ccc}   
    \widehat\omega_{ab}^s && -\omega_{ab}^s\times p_{ab}+\dot p_{ab} \\  
    0 && 0 \\
  \end{array}
\right]=\left[               
  \begin{array}{ccc}   
    \widehat\omega_{ab}^s && v_{ab}^s \\  
    0 && 0 \\
  \end{array}
\right]
$$

$$
v_{ab}^s=\left[               
  \begin{array}{ccc}   
    v_{ab}^s \\  
    \omega_{ab}^s \\
  \end{array}
\right]
=\left[               
  \begin{array}{ccc}   
   -\omega_{ab}^s\times p_{ab}+\dot p_{ab} \\  
    (\dot R_{ab}R_{ab}^T)^{\vee} \\
  \end{array}
\right]
$$

**velocity expressed in body frame** 
$$
v_{qb}=g_{ab}^{-1}v_{qa}=g_{ab}^{-1}\dot g_{ab}q_b=\widehat v_{ab}^bq_b
$$
**transformation between body and static frame**
$$
\widehat v_{ab}^s=\dot g_{ab} g_{ab}^{-1}=g_{ab}g_{ab}^{-1}\dot g_{ab} g_{ab}^{-1}=g_{ab} \widehat v_{ab}^b g_{ab}^{-1}
$$
using the Adjoint to express in vector form
$$
v_{ab}^s=\left[               
  \begin{array}{ccc}   
    v_{ab}^s \\  
    \omega_{ab}^s \\
  \end{array}
\right]=
\left[               
  \begin{array}{ccc}   
    R_{ab}^s && \widehat p_{ab}R_{ab}\\  
    0 && R_{ab}\\
  \end{array}
\right]v_{ab}^b
$$
*Definition of Adjoint mapping:* 
$$
Ad_g\triangleq \left[               
  \begin{array}{ccc}   
    R && \widehat p R \\  
    0 && R \\
  \end{array}
\right] \in \mathbb{R^{6\times 6}}, for\ SE(3)\ motion\ g=(p,R)
$$
which has the following properities
$$
\operatorname{Ad}_{g}^{-1}=\left[\begin{array}{cc}
R^{T} & -\left(R^{T} p\right)^{\wedge} R^{T} \\
0 & R^{T}
\end{array}\right]=\left[\begin{array}{cc}
R^{T} & -R^{T} \widehat{p} \\
0 & R^{T}
\end{array}\right]=\operatorname{Ad}_{g^{-1}}
$$

## Coordinate transformations

Consider the motion of three coordinate frames, A, B, and C. The following relation exists between their velocities:
$$
v_{ac}^s=v_{ab}^s+Ad_{g_{ab}}v_{bc}^s
$$
or the body velocities:
$$
v_{ac}^b=Ad_{g_{bc}^{-1}}v_{bc}^b+v_{ab}^b
$$

# Wrenches

In this section, the forces and moments acting on rigid body is introduced.

## Wrenches Definition 

A generalized force acting on a rigid body consists of a linear component (pure force) and an angular component (pure moment) acting at a point. This force is represented as a vector in $\mathbb{R^6}$:
$$
F=\left[               
  \begin{array}{ccc}   
    f  \\  
    \tau \\
  \end{array}
\right]
$$
where $f\in\mathbb{R^3}$, $\tau\in\mathbb{R^3}$. This definition is named as **wrenches**.

A simpler and more insightful way to derive the relationship between different wrenches is th use the rigid body velocity.

![wrenches_transformation](/images/rigidbody/wrenches_transformation.png)

Recall that the dot product of a force and a velocity is a power, the power is a coordinate-independent quality. Because of this, the following holds
$$
v_B^TF_B=v_C^TF_C
$$
we know that $v_C=Ad_{g_{CB}}v_B$, and therefore the power can be rewritten as
$$
v_B^TF_B=v_C^TF_C=(Ad_{g_{CB}}v_B)^T F_C=v_B^T (Ad_{g_{CB}})^T F_C
$$
Since this must hold for all $v_b$, this simplifies to
$$
F_B=(Ad_{g_{CB}})^T F_C
$$



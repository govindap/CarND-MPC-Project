# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---
## 1. Student describes their model in detail. This includes the state, actuators and update equations.

- State is described by x and y positions, velocity and orientation
- We have two actuators that controls the system, one controls steering angle and the other acceleration

State variables: 

	x: x- position
	y: y- position
	ψ: car orientation
	v: velocity
	cte: cross track error
	epsi: error in car angle

Update Equations for state:
	
	1) x = x + v*cos(ψ)*dt	
	2) y = y + v*sin(psi)*dt	
	3) v=v+a∗dt	
	4) ψ=ψ+(v/L_f)*δ∗dt
	5) cte = cte+ v* sin(epsi)*dt
	6) epsi= epsi+(v/Lf)*delta*dt


## 2. Student discusses the reasoning behind the chosen N (timestep length) and dt (elapsed duration between timesteps) values. Additionally the student details the previous values tried.

N=10 and dt=0.1 is a good approximation since smaller values of dt provides finer resolution and approximate a continuous reference trajectory and reduce discretization error. 

Large N will increase computation time and cause big error in trajectory estimation and different constraints are needed to prevent oscillation. 

Distance covered based on car velocity in N``*``dt time is also important, if N``*``dt time is low then the adjustment needs to be done more for each timestep, if N``*``dt is large, then less corrections is needed for the path. In this track N``*``dt of 1 sec is good approximation since we are driving around 70 and few sharp zig-zag turns in the track. 

## 3. A polynomial is fitted to waypoints. If the student preprocesses waypoints, the vehicle state, and/or actuators prior to the MPC procedure it is described.

Waypoints are converted to vehicle coordinates after substracting x and y positions of the vehicle coordinates and rotating the x and y coordinates by angle psi to the new coordinate system. The processed coordinates are used for polynomial fit. The new state is calculated for these processed waypoints and sent to MPC with coeffs.

## 4. The student implements Model Predictive Control that handles a 100 millisecond latency. Student provides details on how they deal with latency.

Latency is handled by adjusting the state of these variables as done in main.cpp
- adjusting x by considering velocity
- adjusting psi by -(velocity/Lf)``*``steering_angle_change``*``time_step
- velocity adjusted based on throttle
- cte error adjusted by velocity ``*`` sin(error psi) ``*`` time_step
- error psi increased by psi angle


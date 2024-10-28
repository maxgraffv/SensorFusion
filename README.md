# SensorFusion
Sensor Fusion Algorithms description, explanation and examples

## Table Of Contents
- [Kalman Filter](#kalman-filter)
    - [Overview]
    - [Simple Example]
    - [Less Simple Example]

## Kalman Filter
### Overview
Kalman Filter is an algorithm used for tracking and predicting the dynamic state of the system, based on uncertain and noisy measurements.  
Below is a step by step guide  
<br>

#### The Problem: Estimating the State
A state - in kalman filter, includes variables we want to estimate (position, velocity, acceleration).  
Because the measurements are often noisy and incomplete, the filter balances two sources of information:
- **A predictive Model** - that describes how the system's state changes over time
- **Sensor Measurement** - that provides information about the current state but is typically noisy
<br>
<br>

#### State Representation and Initialization

**X - State Vector**  
holds values that will be estimated.  

$$
X = \begin{bmatrix}
x\\
v\\
a
\end{bmatrix}
$$

x - position  
v - velocity  
a - acceleration  

<br><br>

**P - Error Covariance Matrix**  
Reflects the uncertainty in our current estimate of the state.  
Stores how much we trust in the current estimate for each part of the state.  

$$
P = \begin{bmatrix}
1000 & 0 & 0\\
0 & 1000 & 0\\
0 & 0 & 10
\end{bmatrix}
$$

Higher uncertainty for position and velocity, but lower for acceleration  

<br><br>

#### Prediction  
Prediction uses the system model to estimate the next state based on the current state.  
This involves two main components.  

**F - State Transition Matrix**  
Describes how each part of the state influences the next state.  

$$
F = \begin{bmatrix}
1 & \Delta t & \frac{1}{2}(\Delta t)^2 \\
0 & 1 & \Delta t \\
0 & 0 & 1
\end{bmatrix}
$$
<br>
Multiplying F by X projects the current state forward, estimating the next state.

<br><br>

**Q - Process Noise Covariance Matrix**  
Accounts for uncertainty in the model.  
It assumes that even though we have a mathematical model for the system, there might still be random variations (small changes not captured ye model, etc.)  


$$
Q = \begin{bmatrix}
\Ro_x & 0 & 0\\
0 & \Ro_v & 0\\
0 & 0 & \Ro_a
\end{bmatrix}
$$

### Simple Example

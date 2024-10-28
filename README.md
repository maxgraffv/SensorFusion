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






### Simple Example

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
<br>
<br>

#### PREDICTION

$$
currentEstimate = lastEstimate + expectedChange
$$
$$
X_{predicted} = F * X
$$
$$
currentUncertainty = lastUncertainty + processNoise
$$
$$
P_{redicted} = P + Q
$$

<br>
<br>

#### UPDATE

$$
KalmanGain = \frac{currentUncertainty}{currentUncertainty + measurementNoise}
$$

Simplified version

$$
K = \frac{P_{predicted}}{P_{predicted} + R}
$$

General version

$$
K = P_{prediction} * H^T * (H * P_{prediction} * H^T + R)^{-1}
$$

$$
currentEstimate = currentEstimate + (KalmanGain * (measurement - currentEstimate))
$$
$$
X = X_{prediction} + K * y where y = Z - H * X_{prediction}
$$

$$
currentUncertainty = (1-KalmanGain) * currentUncertainty
$$
$$
P = (I - K * H) * P_{prediction}
$$

<br>

Here is where you read the filtered data from currentEstimate.  
Then set the following
<br>

$$
lastEstimate = currentEstimate
$$

$$
lastUncertainty = currentUncertainty
$$





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

$$
x_{pred} = F * x
$$

Multiplying F by X projects the current state forward, estimating the next state.

<br><br>

**Q - Process Noise Covariance Matrix**  
Accounts for uncertainty in the model.  
It assumes that even though we have a mathematical model for the system, there might still be random variations (small changes not captured ye model, etc.)  


$$
Q = \begin{bmatrix}
\sigma_x^2 & 0 & 0\\
0 & \sigma_v^2 & 0\\
0 & 0 & \sigma_a^2
\end{bmatrix}
$$


Where  $\sigma$  represents expected deviations in postion, velocity and acceleration.

The Predicted error covariance becomes:

$$
P = F * P * F^T + Q
$$

This equation means: we predict the uncertainty in our state estimate, by projecting the previous uncertainty forward through F and adding the process noise Q

<br><br>

#### Measurement Update (Correction)  
After predicting the next state, we update our estimate based on new the new measurement from the sensor.

**z - Measurement Vector**  
z holds the sensor's reading.  
If measuring only acceleration:

$$
z = \begin{bmatrix}
a_{measured}
\end{bmatrix}
$$

<br><br>

**H - Measurement Matrix**
maps the state vector X to what we expect to observe from the sensor.  
If me only measured acceleration:  

$$
H = \begin{bmatrix}
0 & 0 & 1
\end{bmatrix}
$$

So that when multiplied, it extracts only the acceleration component  

<br><br>

**R - Measurement Noise Covariance Matrix**  
describes how uncertain we are about the measurement.  
If the sensor is noisy, R should be large, leading the filter to weigh the predicted state more heavily than the measurement, for instance:  

$$
R = \begin{bmatrix}
\sigma_{measurement}^2
\end{bmatrix}
$$

where $\sigma$ is the standard deviation of the sensor noise.  

<br><br>

**Innovation**  
the difference between the actual measurement **z** and the expected measurement  

$$
y = z - H*x_{pred}
$$

<br><br>

**S - Innovation Covariance**  
Mesures the uncertainty of this innovation:

$$
S = H * P_{pred} * H^T + R
$$

A high S value indicates high uncertainty, causing the Kalman gain to reduce, meaining less correction applied to the state.  

<br><br>

**K - Kalman Gain**  
determines how much weight to give the measurement relative to the prediction:  

$$
K = P_{pred} * H^T * S^{-1}
$$

- If K is large - the filter trusts the measurement more than the prediction
- If K is small - the filter trusts the prediction more than the measurement

<br><br>

**Update the State Estimate**  
update the state estimate

$$
x = x_{pred} + K*y
$$

This correction combines the prediction and measurement to get an optimal estimate

<br><br>

**Update the Error Covariance**  
Updated to reflect the reduced uncertainty after the emasurement update

$$
P = (I - K * H) * P_{pred}
$$

This step reduces the uncertainty in our estimate based on the new information from the measurement











### Simple Example

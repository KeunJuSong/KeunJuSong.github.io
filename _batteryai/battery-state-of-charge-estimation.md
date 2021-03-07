---
title: "Battery State-of-Charge Estimation"
excerpt: "Why estimating SOC is main part in BMS, Methods & classical approach about SOC estimation."
toc: true
use_math: true
---
# Battery State-of-Charge Estimation

# Reference
:star: **NICELABE Seminar Lecture Note**

:star: [Algorithms for Battery Management Systems course, Prof. Gregory Plett](https://www.coursera.org/specializations/algorithms-for-battery-management-systems)

---

# What is the Importance of a good SOC estimator?
- BMS must be able to estimate two fundamentally different types of **nonmeasurable battery-pack quantity**
  - **States** (values that change quickly) : e.g., SOC, Diffusion voltage, hysteresis voltage
  - **Parameters** (values that change slowly) : e.g., cell capacities, resistances(내부저항), aging effects
- SOC estimate for all cells is important input to **balancing, energy, power calculations**

🗯️ Balancing
- 각각의 cell이 동일 level SOC를 가지도록 하는 기술

## Additional benefits
- Longevity
- Performance
- Reliability
- Density
- Economy

---

# Fully charged
- DEFINITION : A cell is fully charged when its OCV equals $v_h(T)$, a manufacturer-specified voltage (may be function of T (Temperatrue))
- e.g., $v_h(300K) = 4.2V$ for LMO; $v_h(300K) = 3.6V$ for LFP
  - LMO & LFP are kind of Cathode Active Materials (양극성 활물질, 리튬 베터리 종류)
- Define the SOC of a fully charged cell to be 100%

---

# Fully discharged
- DEFINITION : A cell is fully discharged when OCV equals $v_l(T)$, a manufacturer-specified voltage (may be function of T (Temperature))
- e.g., $v_l(300K) = 3.0V$ for LMO; $v_l(300K) = 2.0V$ for LFP
- Define the SOC of a fully discharged cell to be 0%

🗯️ **OCV? Terminal Voltage? Assume with Diffusion voltage!!**
- OCV (Open Circuit Voltage) → General Idea
  - Open Circuit에서 측정한 voltage
  - Battery 관점에서는 내부저항을 고려해야 함!
- Terminal voltage → General Idea
  - 외부 회로가 연결된 상태에서의 전압
  - 내부저항 뿐만 아니라 회로에 따른 여러 영향들까지 고려
- **Diffusion voltage**
  - Diffusion
    - 시간에 따라 평행상태에 이르기 위해 리튬 농도가 확산되어 균일해지는 현상
    - 리튬이 가지는 고유 특성
- OCV & Terminal voltage → **Lithium Ion Battery Idea**
  - So, OCV is additionally depend on powder 표면 리튬 농도 (Diffusion)
  - Also Terminal voltage must be considered with diffusion voltage

---

# Total capacity
- DEFINITION : Cell total capacity $Q$ is quantity of charge removed as cell is brought from fully charged state to fully discharged state
- SI unit for charge is coulombs (C), but more common to use units of ampere hours(Ah) to measure the total capacity of a battery cell
- Total capacity of a cell is not a fixed quantity
  - It generally decays slowly over time as the cell degrades c.g. aging effects
  - (느린) 시간에 대한 함수로 정의할 수 있음

---

# Discharge capacity
- DEFINITION : Discharge capacity $Q_{rate}$ is quantity of charge removed as cell discharged at constant rate from fully charged state until terminal voltage $v(t)$ **reaches** $v_l(t)$
- Strongly dependent on cell’s internal resistance, and so also rate and temperature
  - e.g., rate 높게 방전 → 내부저항으로 인한 $v(t)$ ↓ → Discharge capacity ↓
- The discharge capacity of a cell at a particular rate and temperature is not a fixed quantity: It also generally decays slowly over time as the cell degrades
  - Discharge capacity 만큼 방전 후, cell 휴식상태 → diffusion 현상에 의한 cell 전압 ↑ ⇒ 낮은 rate으로 방전 가능!
  - Discharge capacity 만큼 방전된 상태 $\ne$ fully discharged 상태!
  - **즉 Discharge capacity는 SOC 정의하는 데 적합하지 않음!**

---

# Nominal capacity
- DEFINITION : Cell nominal capacity $Q_{nom}$ is a manufacturer-specified quantity intended to be representative of 1C-rate discharge capacity $Q_{1C}$ of a particular manufactured lot of cells at room temperature
- 표준 정격 용량
- The nominal capacity is a constant value
- But, each of a cell's total capacity is different!
  - $Q_{nom} \ne Q_{1C}$, $Q_{nom} \ne Q$
  - **따라서 Nominal capacity는 SOC 정의하는 데 적합하지 않음!**

---

# Residual capacity & state of charge
- DEFINITION : Cell residual capacity $Q_{res}$ is quantity of charge that would be removed from cell if it were brought from its present state to a fully discharged state
- DEFINITION : **Cell state-of-charge** is ratio of residual capacity to total capacity, $Q \over Q_{res}$

---

# Voltage-based method to estimate SOC
- OCV is a deterministic function of SOC, $OCV(z(t))$
- Measure cell terminal voltage, v(t), and look up on OCV versus SOC curve
  **Photo position 1**
- Ignores effects of  $i(t) \times R_0$ losses (내부저항), diffusion voltages, and hysteresis on $v(t)$
  **Photo position 2**
- Allow effect of  $i(t) \times R_0$ losses (내부저항)
  - $v(t) = OCV(z(t)) - i(t)R_0$
  - Better, but still ignores effects of diffusion voltages, hysteresis and so is still noisy
  - Filtering helps but adds delay, which must be accounted for
    **Photo position 3**

---

# Current-based method to estimate SOC
**Photo position 4**
- Okay for short periods of operation when initial conditions are known or can be frequently **reset** (for long periods)
- Subject to drift due to current sensor’s fluctuations, current-sensor bias, incorrect capacity estimate, other losses
- Uncertainty/error bounds grow (without limit) over time until estimate is **reset**

---

# Model-based state estimation
**Photo position 5**
- Model-based estimators implement algorithms that use sensed measurements to infer internal hidden state of dynamic system
- Same input propagated through true system, model, measured and predicted outputs compared; error used to update model’s state estimate
- Combination of [Voltage-based method]() and [Current-based method]()
- Apply this estimation to Battery, parameters would be like this:
  - Input : Cell current
  - Output : Cell terminal voltage
  - Model : e.g., [ESC](https://www.notion.so/Battery-AI-Equivalent-Cricuit-Model-Simulation-85388bde23734bdf9f84636682d8bf9c)
  - Output error 요소 : State 추정 오차, 측정 오차 (Process, Sensor), Model 오차

## Linear Kalman filter
- Kalman filter gives optimal state estimate
  **Photo position 6**
  $u_k$ : measured input signal - Cell current
  $w_k$ : process-noise random input
  $v_k$ : sensor-noise random input
  $x_k$ : SOC
  $y_k$ : Cell terminal voltage
  Functions $f()$ and $h()$ may be time-varying, also can be non-linear (→ In this case, use nonlinear Kalman filter)

## 👀 Vector notation
- Superscript “-” indicates a predicted quantity based only on past measurements
- Superscript “+” indicates an estimated quantity based on both past and present measurements
- Symbol “^” indicates a predicted or estimated
- quantity : $\widehat x^+$  or $\widehat x^-$

## Cost function to minimize (optimize)
- $\widehat x_k^{MMSE}(\mathbb Y_k) = \underset{\widehat x_k}{\operatorname{argmin}}(\mathbb E[\;||x_k - \widehat x_k^+||_2^2\mid \mathbb Y_k\;])$
  $$$\qquad\qquad\quad\;\, = \underset{\widehat x_k}{\operatorname{argmin}}(\mathbb E[\;(x_k - \widehat x_k^+)^T(x_k - \widehat x_k^+)\mid \mathbb Y_k\;])$
  $\qquad\qquad\quad\;\, = \underset{\widehat x_k}{\operatorname{argmin}}(\mathbb E[\;x_k^Tx_k - 2x_k^T \widehat x_k^+ + (\widehat x_k^+)^T \widehat x_k^+ \mid \mathbb Y_k\;])$
- $0 = {d \over d\widehat x_k^+} \mathbb E[\; x_k^+ x_k - 2x_k^T \widehat x_k^+ + (\widehat x_k^+) \widehat x_k^+ \mid \mathbb Y_k \;]$
- $0 = \mathbb E[-2(x_k - x_k^+) \mid \mathbb Y_k] = 2 \widehat x_k^+ - 2\mathbb E[x_k \mid \mathbb Y_k]$
- $\therefore \widehat x_k^+ = \mathbb E[x_k \mid \mathbb Y_k]$

🗯️ Additional vector calculus
${d\over dX}Y^T X = Y$, ${d \over dX}X^T Y = Y$, and ${d \over dX}X^T A X = (A + A^T)X$

## Prediction error and Innovation
- Prediction error
  $\tilde x_k^- = x_k - \widehat x_k^- \; where \; \widehat x_k^- = E[x_k \mid Y_{k-1}]$ 
- Innovation
  $\tilde y_k = y_k - \widehat y _k \; where \; \tilde y_k = E[y_k \mid Y_{k-1}]$
- Properties can be like this:
  $\mathbb E[\tilde x_k^-] = \mathbb E[x_k] - \mathbb E[\mathbb E[x_k \mid \mathbb Y_{k-1}]] = \mathbb E[x_k] - \mathbb E[x_k] = 0 \\ \mathbb E[\tilde y_k] = \mathbb E[y_k] - \mathbb E[\mathbb E[y_k \mid \mathbb Y_{k-1}]] = \mathbb E[y_k] - \mathbb E[y_k] = 0$

## Predict / correct solution
- Properties can be like this:
  $\mathbb E[\tilde x_k^- \mid \mathbb Y_{k-1}] = \mathbb E[x_k - \mathbb E[x_k \mid \mathbb Y_{k-1}] \mid \mathbb Y_{k-1}] = 0 = \mathbb E[\tilde x_k^-] \\ \mathbb E[\tilde x_k^- \mid \mathbb Y_k] = \mathbb E[\tilde x_k^- \mid \mathbb Y_{k-1},y_k] = \mathbb E[\tilde x_k^- \mid y_k]$ 
  $\mathbb E[\tilde x_k^- \mid \mathbb Y_k] = \mathbb E[x_k \mid \mathbb Y_k] - \mathbb E[\widehat x_k^- \mid \mathbb Y_k] = \widehat x_k^+ - \widehat x_k^-$
- Predict/correct sequence of steps
  $\therefore \widehat x_k^+ = \widehat x_k^- + \mathbb E[\tilde x_k^- \mid y_k]$
  
## State update equation
- General equation
  $\mathbb E[x\mid y] = \mathbb E[x] + \Sigma_{\tilde x \tilde y}\Sigma_{\tilde y}^-(y - \mathbb E[y]), \quad \Sigma \; is \; covariance \; matrix$ 
- Applying this equation to our problem, we get
  **Photo position 7**
  $\therefore \widehat x_k^+ = \widehat x_k^- + L_k\tilde y_k, \quad L \; is \; Kalman \; gain$

## Uncertainty of state estimate
- Calculation of $\Sigma_{\tilde x, k}^+$,
  **Photo position 8**
- $\widehat x_k^+ \pm 3 \sqrt{diag(\Sigma_{\tilde x,k}^+)}$,  3 is something specific scaling factor of probability(%)
- Boundary를 형성하여 추정의 정확성을 판단

## Visualizing the linear Kalman filter
- $A, \; B$ matrix → State space model notation
- $C, \; D$ matrix → Model notation of representing output from state and input
  **Photo position 9**

---

# Kinds of nonlinear Kalman filters
- Three basic KF generalizations for nonlinear systems
  - **Extended Kalman filter (EKF)**
    - Analytic linearization of model at each point in time
  - **Sigma-point (Unscented) Kalman filter (SPKF/UKF)**
    - Statistical/empirical linearization of model at each point in time: Can be much better than EKF at same computational complexity
  - Particle filters
    - Most precise, but often thousands of times more computations required than either EKF/SPKF

🗯️ **Idea of nonlinear Kalman filters**
- Nonlinear 부분을 linear로 근사화해서 정리!

---

# Prediction steps of nonlinear Kalman filters
- EKF and SPKF both follows same set of general steps as KF
  **Photo position 10**
  **Photo position 11**
- EKF and SPKF simply have different expressions from KF for evaluating the expectation operations (in Step 1a, 2a) → Because of different system model

---

# Extended Kalman filter (EKF)
- EKF makes two simplifying assumptions when adapting general sequential inference equations to a nonlinear system:
  - When computing estimates of the output of a nonlinear function, EKF assumes $E[fn(x)] \approx fn(E[x])$, which is not true in general
  - When computing covariance estimates, EKF uses **Taylor-series expansion to linearize** the system equations around the present operating point

## EKF on ESC results
- EKF was executed for a test having dynamic profiles from 100% SOC down to around 10% SOC
  - RMS SOC estimation error = 0.46%
  - Percent of time error outside bounds = 0%
    **Photo position 12**

---

# Pack-average SOC
- While “pack SOC” does not make sense, concept of **pack-average SOC** is a useful one
- Since all cells in series experience same current, their SOC values will
  - Move in the same direction for any given applied current, by a similar amount (but different because of unequal cell capacities)
- We take advantage of this similarity by creating:
  - Algorithm to determine the composite average behavior of all cells in battery pack
  - Algorithm to determine the individual differences between specific cells and that composite average behavior

## Defining the pack-average state / cell-difference states
- We define pack-average state **x-bar** as $\bar x = {1\over N_s}\sum_{i=1}^{N_s}x_k^{(i)}$
- Can then write an individual cell’s state vector as $x_k^{(i)} = \bar x_k + \Delta x_k^{(i)}$
  **Photo position 13**
- Complexity
  - Bar filter is of same computational complexity as individual state estimators used as a basis
  - But, delta filters can be made very simple
  - Also, delta states change much more slowly than average state, so delta filters can be run less frequently, down to $1/N_s$ times rate of bar filter

---

# Summary
- SOC를 추정하면 가질 수 있는 Good advantages → Battery SOC 추정이 매우 중요
- SOC를 추정하기 위한 여러가지 방법론들 (Voltage-based, Current-based, Model-based)
- Classical한 SOC approach - using Kalman Filter (linear/non-linear)

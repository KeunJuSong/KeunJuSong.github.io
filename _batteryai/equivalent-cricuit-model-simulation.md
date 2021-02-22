---
title: "Equivalent Cricuit Model Simulation"
excerpt: "Apply measured voltage, current, temperature with equivalent model for estimating SOC"
toc: true
use_math: true
---

# **Reference**

:star:NICELAB Seminar

:star:[Algorithms for Battery Mangement Systems course, Prof. Gregory Plett](https://www.coursera.org/specializations/algorithms-for-battery-management-systems)

---

# **Which part that we handle in this time?**

- **Measure voltage, current, temperature** → From designing Equivalent Cricuit Model!

  **Photo position 1**

---

# **Defining an Equivalent Circuit Model of Li-Ion Cell**

## Modeling OCV & SOC

- OCV (Open Circuit Voltage)

  - Difference of electrical potential between two terminal of device
  - 외부회로에서 분리했을 때 장치 자체의 전위차를 의미
  - Battery에서는 내부저항을 고려한 내부 전압!
    - Lithium Ion Battery : Diffusion voltage 요소도 추가적으로 고려 → 고유한 특성

- SOC (State of Charge)

  - Level of charge of an electric battery relative to its capacity
  - 용량에 따른 상대적인 충전 수준, 0~100 %
  - Can be expressed $Z$ value, which is in the range of 0 ~ 1

- Coulombic efficiency

  - (Charge out) / (Charge in) [unit: 충전량]

  **Photo position 2**

  Caption : Modeling OCV & SOC - Equivalent Curcuit

- Continuous time

  **Photo position 3**

- Discrete time

  **Photo position 4**

🗯️ Coulombic efficiency VS Energy efficiency

- Coulombic efficiency,  $\eta$ : Cell 효율 문제에 대한 척도! (Cell 표면에서의 전자 배출 ↔ 확산 과정 수치화)
- Energy efficiency : 저항에 대한 열 손실 문제에 대한 척도!

## Modeling with Thevenin circuit

- **Based on experimental graph**

  - 그래프를 보고 수식에 사용할 소자를 설계하는 방식

- Caused by slow diffusion processes in the cell

- Voltage dosen't immediately return to OCV(relaxes gradually)

- Example

  **Photo position 5**

  - 20 min 동안 constant current(?)로 discharge

  - 20 min ~ 60 min : Resting → Slow diffusion 현상으로 인한 일정 전압 상승

  - Thevenin model

    **Photo position 6**

    **Photo position 7**

## Modeling with Randles Circuit

  - **Based on electrochemistry**

    - Cell의 전기화학적 특성을 기반으로 설계하는 방식

    **Photo position 8**

  - $R_0$ : Models the electrolyte resistance (전해질 저항)
  - $R_{ct}$ : is charge-transfer resistance, models voltage drop over the electrode - electrolyte interface due to a load (리튬의 산화-환원 반응에 의해서 생기는 저항)
  - $C_{d1}$ : is double-layer capacitance, models effect of charges building up in the electrolyte at electrode surface (전기 이중층, 전해질과 전극 표면에서 충전되는 전압 존재)
  - $Z_w$ : a Warburg impedance, models show diffusion processes (내부 확산 현상을 임피던스로 표현)

  리튬이온 셀에서 $C_{d1}$은 매우 작은 역할을 가지므로, 0 으로 간주할 수 있다!

  따라서, $C_{d1}$을 제거하게 되면 Thevenin Cricuit과 유사

  Thevenin cricuit is a reasonable description of cell dynamic

## Equation derivation(Circuit model)

  - General equation ($u(t) : input, \quad x(t): state\; model$)

    - Generically (When $a \ne 0$),

      **Photo position 9**

    - In the special case (When $a = 0$),

      **Photo position 10**

  - Adapt general equation(above) in our model:

    **Photo position 11**

## **Hysteresis voltages**

  - Hysteresis : the dependence of the state of a system on its history

    - 과거의 정보가 현재의 상태에 영향을 주는 현상

  - For every SOC, have a range of possible stable "OCV" values

    - 이 때문에 SOC는 안정적으로 가능한 OCV의 범위를 가짐
    - Example) 40% SOC → OCV는 특정한 점 형태가 아닌 이전 상태에 따라 여러 값을 가짐!

  - Ignoring can cause large prediction errors! → Must consider...

    - 전압, SOC 측정 시 매우 좋지 못한 현상 → 완전 충전, 완전 방전 부분에서 voltage 값이 튀는 부분
    - 이를 제거하여 SOC와 voltage(OCV)에 대한 single curve를 얻기 위함

    **Photo position 12**

    **Photo position 13**

  - Experiment result of setting hysteresis model

    **Photo position 14**

    - 전압, SOC 측정 시 매우 좋지 못한 현상 → 완전 충전, 완전 방전 부분에서 voltage 값이 튀는 부분
    - 이를 제거하여 SOC와 voltage에 대한 single curve를 얻기 위함

## **Equation derivation (Hysteresis voltage)**

  - Combining observations with model (SOC에 따른 Hysteresis 상태 변화)

    **Photo position 15**

    - $\gamma$  : 수치 조정해주는 상수 (Positive)
    - $sgn(\dot z)$ : SOC 변화 방향(부호 +/-)
    - $M(z,\dot z)$
      - Appears there is a maximum plus/minus hysteresis, may be SOC dependent : $M(z)$
      - Amount is positive if cell is presently charging; otherwise negative : $M(z,\dot z)$
        - $\dot z$ is the sign of charging or discharging
    - $h(z,t)$ : Actual amount of hysteresis
    - $M(z,\dot z) - h(z,t)$ term causes hysteresis rate-of-change to be proportional to the distance away from major hystersis loop
      - So this term is helping us to get this kind of **exponential shape of decay (error)** of hysteresis towards either the maximum charge or maximum discharge curve.

  - To fit differential equation for $h(z,t)$ into cell model, **must manipulate it to be a differential equation in time** (not in SOC)

    **Photo position 16**

    - Left side becomes $\dot h(t)$; on right side, note $$\dot zsgn(\dot z) = |\dot z|$$ and $$\dot z(t) = -\eta(t)i(t)/Q$$

      **Photo position 17**

    - So we can have continuous hysteresis model

  - Convert to discrete time

    **Photo position 18**
    Caption : Apply general equation and show in discrete time

    - **Untiless hysteresis state**
      - 실험 도중(Optimizing model parameter values) 유용한 방정식이 아닌 것이 밝혀져, re-write 한 equation.
      - Re-write in equivalent but slightly different representation, which has $-1 \leq h[k] \leq 1$

  - **순간적 hysteresis 변화에 따른 model equation** ⇒ Dynamic model part에서 다룰 부분!

    - Defining new value, $s[k]$ which must get 1 or -1

      **Photo position 19**

## ESC(Enhanced Self-Correction) cell model

  - Can summarize from below definition!

    - [SOC-OCV](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#modeling-ocv--soc)
    - [Ohmic R - diffusion V](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#equation-derivationcircuit-model)
    - [Hysteresis](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#equation-derivation-hysteresis-voltage)

  - ESC : State equation - matrix form

    **Photo position 20**

    - $u[k]$ : 충전/방전 상태, 얼마만큼 속도로 충/방전 되는지
    - $x[k]$ : SOC - Ohmic R - Hysteresis 상태의 Vector(List) 표현

  - ESC : Output equation

    **Photo position 21**

  - ESC cell model - Equivalent Circuit

    **Photo position 22**

    **Photo position 23**

  - IDEA of ESC cell model

    - Design function $f$ to predict terminal voltage of cell!
    - Input : $x[k], u[k]$
    - $f$ is universal function form for simulation
    - But, how we know about OCV? Is it defined from manufacturer?

  - My interpretation of ESC cell model

    - 정의되지 않은 $f$ 를 시뮬레이션을 통해 설계했을 때, 우리는 cell의 terminal voltage $v[k]$ 를 추정할 수 있다
    - 추정한 $v[k]$가 실제 $v[k]$를 잘 following 한다면 우리는 [SOC-OCV](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#modeling-ocv--soc) 정의로부터 $Z[k]$, 즉 $v[k]$ 에 대응하는 보다 정확한 SOC를 추정할 수 있다!

  ---

# **Identify Parameters of Static Model**

  - Long time에 대해 온도 변화에 따른 Coulombic eff & Capacity → OCV & SOC 관계

  ## Cell testing steps

  - OCV test script #1 (at test temperature)

    - 100% charged, test T

    - Discharge cell at constant-current c/30 rate until terminal volatge equals $V_{min}$

      **Photo position 24**

  - OCV test script #2 (at test temperature)

    - Test T

    - Charge the cell at constant-current rate of C/30 until cell terminal voltage equals $V_{max}$

      **Photo position 25**

## Cell testing to determine coulombic eff ($\eta$) & capacity($Q$)

- The test steps for OCV testing in 25 C

- Processing data for 25 C

  - Coulombic efficiency

    - Assume 1 when full-charged, full-discharged

    **Photo position 26**

  - Capacity

    **Photo position 27**

- Processing data for other T

  **Photo position 28**

  **Photo position 29**

  **Photo position 30**

🗯️ Therotical & Experimental properties about total-capacity

- Capacity properties

  - 전압 및 전류와 관계 없음!

- Therotical : 총 용량은 온도에 의해 변화하지 않음

- Experimental : 온도에 의해 변화되는 discharge capacity와 charge capacity를 통해 total capacity를 추정할 수 있음!

  - 리튬 화학물질 : 온도에 따른 Coulombic effiency 변화
  - 총 용량 : 온도에 따른 변화 X

  **Photo position 31**

  - 실험을 통한 (충전 용량 - 방전 용량) vs (총 용량) 관계

    **Photo position 32**

    - 저온에서 충전-방전 용량이 변화가 일어난다
    - 즉, 상온에서는 총 용량의 therotical properties에 따라 충전-방전 용량도 변화하지 않는다.

## Cell testing to determine OCV & SOC(1)

- DOD(Depth of Discharge)

  - $DOD(t)/Q = 1 - SOC(t)$
  - depth of discharge(t) = total Ah discharged until t - [ $\eta(25C) \; \times$ total Ah charged at 25C until t ] - [ $\eta(T) \; \times$ total Ah charged at temperature T until t ]

- Missing data?

  - Missing discharge V at low SOC because of $V_{min}$ (Before SOC 0% reached)

  - Missing charge V at high SOC because of $V_{max}$ (Before SOC 100% reached)

    **Photo position 33**

- IDEA of Approximate OCV

  - Because of missing data, Approximate OCV is followed charge voltage in low SOC, discharge voltage in high SOC!

## Cell testing to determine OCV & SOC(2)

- Modeling temperature dependence

  - Combine individual approximate single-temperature OCV
  - $OCV0(Z(t))$ : OCV relationship at 0 C
  - $OCVrel(Z(t))$ : linear temperature-correction factor at each SOC

  **Photo position 34**

  **Photo position 35**

---

# **Identify Parameters of Dynamic Model**

## Needed to determine dynamic model parameters

- Application : 전기 자동차, 휴대전화 ... etc.

- 짧은 시간에 대해 온도에 따른 Coulombic eff & Capacity → OCV & SOC 관계

- Dynamic "discharge" portion of test

  - Using UDDS profile for an example

    **Photo position 36**

- Dynamic test script #1 (at test T)

  - 100% charged, test T
  - Discharge cell at constant-current at a C/1 rate for 6 min
  - Execute dynamic profiles over SOC range of interest

  **Photo position 37**

  ## Tests Needed to determine dynamic model parameters

  ### Discharge/charge calibration portion of test

  - Dynamic test script #2 (at 25C)

    - Bring cell terminal voltage to $V_{min}$ by discharging at C/30 rate
    - Follow-on Dither profile(s) can be used to eliminate hysteresis to the greatest degree possible

    **Photo position 38**

  - Dynamic test script #3 (at 25C)

    - Charge cell using a constant-current C/1 rate until voltage equals $V_{max}$
    - Maintain voltage constant at $V_{max}$ until current drops below C/30
    - Follow-on Dither profiles(s) at same reason

    **Photo position 39**

## How data used to find dynamic model parameter values

- Follow below steps to find parameter values

  **Photo position 40**

  - 1. Compute coulombic efficiency & total capacity

      - Simillar with [Static Model](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#cell-testing-to-determine-coulombic-eff-eta--capacityq)

  - 2. Subtract OCV form voltage

      - Assume that we solve [OCV in Static Model](https://keunjusong.github.io/batteryai/equivalent-cricuit-model-simulation/#cell-testing-to-determine-ocv--soc2)

    **Photo position 41**

  - 3. Finding R-C time constants

      - Using Subspace system identification
        - Replace nonlinear optimization with linear-algebra technique

  - 4-5. Compute $i_R[k],s[k],h[k]$

    - 맨 아래 definition은  $i_R[k],s[k],h[k]$의 초기값

    **Photo position 42**

  - 6. Solve for linear output parameters

      - $X = A^{-1}Y$, $X$ parameters(vector) that we want to get

    **Photo position 43**

  - 7. Compute RMS voltage prediction error

    **Photo position 44**

    **Photo position 45**

  - 8. Iterate to find best $\gamma$ (adapt hysteresis)

      - Iterate step 5 ~ 8
      - Iterate until satisfied "close enough" to best
      - Bisection search is one simple possibility

    **Photo position 46**

### Final Result

- Final model is able to predict cell voltage very well...
- Figure show 3 R-C pairs ; RMS error 5.7mV (Less than 10mV is well!)

**Photo position 47**

### Improvement

- additional R-C pairs - 복잡도 증가, 비추천 방식
- improve hysteresis model
- Refinement OCV prediction error
- Finding unknown phenomenon

---

# **Simulating Battery Packs in different configurations**

## Cells for battery packing

- Paralled-connected modules (PCM)

  **Photo position 48**

  **Photo position 49**

- Series-connected modules (SCM)

  **Photo position 50**

  **Photo position 51**

- Packing을 위해 각 Cell voltage를 추정해야 함 → 등가모델을 구축하는 main reason

---

# **Summary**

## Why we design Equivalent Circuit?

- Parameters가 베터리 효율의 근거
- Tunning parameters로 효율 최적화
- BMS에서 존재하는 여러 error에 대한 보정

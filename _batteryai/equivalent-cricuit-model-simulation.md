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
  <figure>
    <img src="{{ '/assets/images/BatteryAI_2-(1).png' | relative_url }}" alt="Battery AI_2-(1)">
  </figure>

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
- Modeling OCV & SOC - Equivalent Curcuit

  <img src="/assets/images/BatteryAI_2-(2).png" alt="Battery_AI_2-(2)" height="260" width="350">
- Continuous time
  
  <img src="/assets/images/BatteryAI_2-(3).png" alt="Battery_AI_2-(3)" height="95" width="275">
- Discrete time
  <figure>
    <img src="{{ '/assets/images/BatteryAI_2-(4).png' | relative_url }}" alt="Battery AI_2-(4)">
  </figure>

🗯️ Coulombic efficiency VS Energy efficiency
- Coulombic efficiency,  $\eta$ : Cell 효율 문제에 대한 척도! (Cell 표면에서의 전자 배출 ↔ 확산 과정 수치화)
- Energy efficiency : 저항에 대한 열 손실 문제에 대한 척도!

## Modeling with Thevenin circuit
- **Based on experimental graph**
  - 그래프를 보고 수식에 사용할 소자를 설계하는 방식
- Caused by slow diffusion processes in the cell
- Voltage dosen't immediately return to OCV(relaxes gradually)
- Example

  <img src="/assets/images/BatteryAI_2-(5).png" alt="Battery_AI_2-(5)" height="275" width="385">
  - 20 min 동안 constant current(?)로 discharge
  - 20 min ~ 60 min : Resting → Slow diffusion 현상으로 인한 일정 전압 상승

  - Thevenin model
  <figure>
    <img src="{{ '/assets/images/BatteryAI_2-(6).png' | relative_url }}" alt="Battery AI_2-(6)">
  </figure>
  <figure>
    <img src="{{ '/assets/images/BatteryAI_2-(7).png' | relative_url }}" alt="Battery AI_2-(7)">
  </figure>

## Modeling with Randles Circuit
  - **Based on electrochemistry**
    - Cell의 전기화학적 특성을 기반으로 설계하는 방식
    
      <img src="/assets/images/BatteryAI_2-(8).png" alt="Battery_AI_2-(8)" height="285" width="915">
  - $R_0$ : Models the electrolyte resistance (전해질 저항)
  - $R_{ct}$ : is charge-transfer resistance, models voltage drop over the electrode - electrolyte interface due to a load (리튬의 산화-환원 반응에 의해서 생기는 저항)
  - $C_{d1}$ : is double-layer capacitance, models effect of charges building up in the electrolyte at electrode surface (전기 이중층, 전해질과 전극 표면에서 충전되는 전압 존재)
  - $Z_w$ : a Warburg impedance, models show diffusion processes (내부 확산 현상을 임피던스로 표현)
  
  - Properties
    - 리튬이온 셀에서 $C_{d1}$은 매우 작은 역할을 가지므로, 0 으로 간주할 수 있다!
    - 따라서, $C_{d1}$을 제거하게 되면 Thevenin Cricuit과 유사
    - Thevenin cricuit is a reasonable description of cell dynamic

## Equation derivation(Circuit model)
  - General equation ($u(t) : input, \quad x(t): state\; model$)
    - Generically (When $a \ne 0$),
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(9).png' | relative_url }}" alt="Battery AI_2-(9)">
      </figure>
    - In the special case (When $a = 0$),
      <figure>
       <img src="{{ '/assets/images/BatteryAI_2-(10).png' | relative_url }}" alt="Battery AI_2-(10)">
      </figure>
  - Adapt general equation(above) in our model:
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(11).png' | relative_url }}" alt="Battery AI_2-(11)">
    </figure>

## **Hysteresis voltages**
  - Hysteresis : the dependence of the state of a system on its history
    - 과거의 정보가 현재의 상태에 영향을 주는 현상

  - For every SOC, have a range of possible stable "OCV" values
    - 이 때문에 SOC는 안정적으로 가능한 OCV의 범위를 가짐
    - Example) 40% SOC → OCV는 특정한 점 형태가 아닌 이전 상태에 따라 여러 값을 가짐!

  - Ignoring can cause large prediction errors! → Must consider...
    - 전압, SOC 측정 시 매우 좋지 못한 현상 → 완전 충전, 완전 방전 부분에서 voltage 값이 튀는 부분
    - 이를 제거하여 SOC와 voltage(OCV)에 대한 single curve를 얻기 위함
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(12).png' | relative_url }}" alt="Battery AI_2-(12)">
    </figure>  
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(13).png' | relative_url }}" alt="Battery AI_2-(13)">
    </figure>

  - Experiment result of setting hysteresis model

    <img src="/assets/images/BatteryAI_2-(14).png" alt="Battery_AI_2-(14)" height="260" width="400">
   - 전압, SOC 측정 시 매우 좋지 못한 현상 → 완전 충전, 완전 방전 부분에서 voltage 값이 튀는 부분
   - 이를 제거하여 SOC와 voltage에 대한 single curve를 얻기 위함

## **Equation derivation (Hysteresis voltage)**
  - Combining observations with model (SOC에 따른 Hysteresis 상태 변화)
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(15).png' | relative_url }}" alt="Battery AI_2-(15)">
    </figure>
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
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(16).png' | relative_url }}" alt="Battery AI_2-(16)">
    </figure>
    - Left side becomes $\dot h(t)$; on right side, note $\dot z sgn(\dot z) = \mid\dot z\mid$ and $\dot z(t) = -\eta(t)i(t)/Q$
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(17).png' | relative_url }}" alt="Battery AI_2-(17)">
        <figcaption>Continuous Hysteresis model</figcaption>
      </figure>

  - Convert to discrete time
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(18).png' | relative_url }}" alt="Battery AI_2-(18)">
      <figcaption>Apply general equation and show in discrete time</figcaption>
    </figure>
    - **Untiless hysteresis state**
      - 실험 도중(Optimizing model parameter values) 유용한 방정식이 아닌 것이 밝혀져, re-write 한 equation.
      - Re-write in equivalent but slightly different representation, which has $-1 \leq h[k] \leq 1$
  - **순간적 hysteresis 변화에 따른 model equation** ⇒ Dynamic model part에서 다룰 부분!
    - Defining new value, $s[k]$ which must get 1 or -1
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(19).png' | relative_url }}" alt="Battery AI_2-(19)">
      </figure>
      
## ESC(Enhanced Self-Correction) cell model
  - Can summarize from below definition!
    - **SOC-OCV**
    - **Ohmic R - diffusion V**
    - **Hysteresis**
  - ESC : State equation - matrix form
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(20).png' | relative_url }}" alt="Battery AI_2-(20)">
    </figure>
    - $u[k]$ : 충전/방전 상태, 얼마만큼 속도로 충/방전 되는지
    - $x[k]$ : SOC - Ohmic R - Hysteresis 상태의 Vector(List) 표현

  - ESC : Output equation
  
    <img src="/assets/images/BatteryAI_2-(21).png" alt="Battery_AI_2-(21)" height="225" width="1020">
      
  - ESC cell model - Equivalent Circuit
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(22).png' | relative_url }}" alt="Battery AI_2-(22)">
    </figure>
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(23).png' | relative_url }}" alt="Battery AI_2-(23)">
    </figure>
  - IDEA of ESC cell model
    - Design function $f$ to predict terminal voltage of cell!
    - Input : $x[k], u[k]$
    - $f$ is universal function form for simulation
    - But, how we know about OCV? Is it defined from manufacturer?
  - My interpretation of ESC cell model
    - 정의되지 않은 $f$ 를 시뮬레이션을 통해 설계했을 때, 우리는 cell의 terminal voltage $v[k]$ 를 추정할 수 있다
    - 추정한 $v[k]$가 실제 $v[k]$를 잘 following 한다면 우리는 SOC-OCV 정의로부터 $Z[k]$, 즉 $v[k]$ 에 대응하는 보다 정확한 SOC를 추정할 수 있다!

---

# **Identify Parameters of Static Model**
  - Long time에 대해 온도 변화에 따른 Coulombic eff & Capacity → OCV & SOC 관계
## Cell testing steps
  - OCV test script #1 (at test temperature)
    - 100% charged, test T
    - Discharge cell at constant-current c/30 rate until terminal volatge equals $V_{min}$
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(24).png' | relative_url }}" alt="Battery AI_2-(24)">
      </figure>
  - OCV test script #2 (at test temperature)
    - Test T
    - Charge the cell at constant-current rate of C/30 until cell terminal voltage equals $V_{max}$
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(25).png' | relative_url }}" alt="Battery AI_2-(25)">
      </figure>

## Cell testing to determine coulombic eff ($\eta$) & capacity($Q$)
- The test steps for OCV testing in 25 C
- Processing data for 25 C
  - Coulombic efficiency
    - Assume 1 when full-charged, full-discharged
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(26).png' | relative_url }}" alt="Battery AI_2-(26)">
      </figure>
  - Capacity
  
    <img src="/assets/images/BatteryAI_2-(27).png" alt="Battery_AI_2-(27)" height="78" width="315">

- Processing data for other T
  <figure>
     <img src="{{ '/assets/images/BatteryAI_2-(28).png' | relative_url }}" alt="Battery AI_2-(28)">
  </figure>
  <figure>
     <img src="{{ '/assets/images/BatteryAI_2-(29).png' | relative_url }}" alt="Battery AI_2-(29)">
  </figure>
  <figure>
     <img src="{{ '/assets/images/BatteryAI_2-(30).png' | relative_url }}" alt="Battery AI_2-(30)">
  </figure>

🗯️ Therotical & Experimental properties about total-capacity
- Capacity properties
  - 전압 및 전류와 관계 없음!
- Therotical : 총 용량은 온도에 의해 변화하지 않음
- Experimental : 온도에 의해 변화되는 discharge capacity와 charge capacity를 통해 total capacity를 추정할 수 있음!
  - 리튬 화학물질 : 온도에 따른 Coulombic effiency 변화
  - 총 용량 : 온도에 따른 변화 X
 
    <img src="/assets/images/BatteryAI_2-(31).png" alt="Battery_AI_2-(31)" height="326" width="940">
  - 실험을 통한 (충전 용량 - 방전 용량) vs (총 용량) 관계
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(32).png' | relative_url }}" alt="Battery AI_2-(32)">
    </figure>
    - 저온에서 충전-방전 용량이 변화가 일어난다
    - 즉, 상온에서는 총 용량의 therotical properties에 따라 충전-방전 용량도 변화하지 않는다.

## Cell testing to determine OCV & SOC(1)
- DOD(Depth of Discharge)
  - $DOD(t)/Q = 1 - SOC(t)$
  - depth of discharge(t) = total Ah discharged until t - [ $\eta(25C) \; \times$ total Ah charged at 25C until t ] - [ $\eta(T) \; \times$ total Ah charged at temperature T until t ]
- Missing data?
  - Missing discharge V at low SOC because of $V_{min}$ (Before SOC 0% reached)
  - Missing charge V at high SOC because of $V_{max}$ (Before SOC 100% reached)
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(33).png' | relative_url }}" alt="Battery AI_2-(33)">
    </figure>
- IDEA of Approximate OCV
  - Because of missing data, Approximate OCV is followed charge voltage in low SOC, discharge voltage in high SOC!

## Cell testing to determine OCV & SOC(2)
- Modeling temperature dependence
  - Combine individual approximate single-temperature OCV
  - $OCV0(Z(t))$ : OCV relationship at 0 C
  - $OCVrel(Z(t))$ : linear temperature-correction factor at each SOC
 
    <img src="/assets/images/BatteryAI_2-(34).png" alt="Battery_AI_2-(34)" height="130" width="705">
    
    <img src="/assets/images/BatteryAI_2-(35).png" alt="Battery_AI_2-(35)" height="346" width="960">

---

# **Identify Parameters of Dynamic Model**
## Needed to determine dynamic model parameters
- Application : 전기 자동차, 휴대전화 ... etc.
- 짧은 시간에 대해 온도에 따른 Coulombic eff & Capacity → OCV & SOC 관계
- Dynamic "discharge" portion of test
  - Using UDDS profile for an example
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(36).png' | relative_url }}" alt="Battery AI_2-(36)">
    </figure>
- Dynamic test script #1 (at test T)
  - 100% charged, test T
  - Discharge cell at constant-current at a C/1 rate for 6 min
  - Execute dynamic profiles over SOC range of interest
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(37).png' | relative_url }}" alt="Battery AI_2-(37)">
    </figure>
    
## Tests Needed to determine dynamic model parameters
### Discharge/charge calibration portion of test
  - Dynamic test script #2 (at 25C)
    - Bring cell terminal voltage to $V_{min}$ by discharging at C/30 rate
    - Follow-on Dither profile(s) can be used to eliminate hysteresis to the greatest degree possible
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(38).png' | relative_url }}" alt="Battery AI_2-(38)">
      </figure>
  - Dynamic test script #3 (at 25C)
    - Charge cell using a constant-current C/1 rate until voltage equals $V_{max}$
    - Maintain voltage constant at $V_{max}$ until current drops below C/30
    - Follow-on Dither profiles(s) at same reason
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(39).png' | relative_url }}" alt="Battery AI_2-(39)">
      </figure>
      
## How data used to find dynamic model parameter values
- Follow below steps to find parameter values
  <figure>
    <img src="{{ '/assets/images/BatteryAI_2-(40).png' | relative_url }}" alt="Battery AI_2-(40)">
  </figure>
  - (1) Compute coulombic efficiency & total capacity
      - Simillar with **Static Model**
  - (2) Subtract OCV form voltage
      - Assume that we solve **OCV in Static Model**
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(41).png' | relative_url }}" alt="Battery AI_2-(41)">
      </figure>
  - (3) Finding R-C time constants
      - Using Subspace system identification
        - Replace nonlinear optimization with linear-algebra technique
  - (4-5) Compute $i_R[k],s[k],h[k]$
    - 맨 아래 definition은  $i_R[k],s[k],h[k]$의 초기값
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(42).png' | relative_url }}" alt="Battery AI_2-(42)">
      </figure>
  - (6) Solve for linear output parameters
      - $X = A^{-1}Y$, $X$ parameters(vector) that we want to get
      <figure>
        <img src="{{ '/assets/images/BatteryAI_2-(43).png' | relative_url }}" alt="Battery AI_2-(43)">
      </figure>
  - (7) Compute RMS voltage prediction error
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(44).png' | relative_url }}" alt="Battery AI_2-(44)">
    </figure>
    <figure>
      <img src="{{ '/assets/images/BatteryAI_2-(45).png' | relative_url }}" alt="Battery AI_2-(45)">
    </figure>
  - (8) Iterate to find best $\gamma$ (adapt hysteresis)
      - Iterate step 5 ~ 8
      - Iterate until satisfied "close enough" to best
      - Bisection search is one simple possibility

      <img src="/assets/images/BatteryAI_2-(46).png" alt="Battery_AI_2-(46)" height="268" width="306">
      
### Final Result
- Final model is able to predict cell voltage very well...
- Figure show 3 R-C pairs ; RMS error 5.7mV (Less than 10mV is well!)
<figure>   
  <img src="{{ '/assets/images/BatteryAI_2-(47).png' | relative_url }}" alt="Battery AI_2-(47)">
</figure>
### Improvement
- additional R-C pairs - 복잡도 증가, 비추천 방식
- improve hysteresis model
- Refinement OCV prediction error
- Finding unknown phenomenon

---

# **Simulating Battery Packs in different configurations**
## Cells for battery packing
- Paralled-connected modules (PCM)
 <figure>
   <img src="{{ '/assets/images/BatteryAI_2-(48).png' | relative_url }}" alt="Battery AI_2-(48)">
 </figure>
 
 <img src="/assets/images/BatteryAI_2-(49).png" alt="Battery_AI_2-(49)" height="338" width="233">

- Series-connected modules (SCM)
 
 <img src="/assets/images/BatteryAI_2-(50).png" alt="Battery_AI_2-(50)" height="120" width="270">
 
 <img src="/assets/images/BatteryAI_2-(51).png" alt="Battery_AI_2-(51)" height="410" width="360">
- Packing을 위해 각 Cell voltage를 추정해야 함 → 등가모델을 구축하는 main reason

---

# **Summary**
## Why we design Equivalent Circuit?
- Parameters가 베터리 효율의 근거
- Tunning parameters로 효율 최적화
- BMS에서 존재하는 여러 error에 대한 보정

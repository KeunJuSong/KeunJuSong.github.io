---
title: "Battery AI - Introduction to Battery Management System"
excerpt: "Looking for Basic of Lithium Ion Battery & overview of BMS"
toc: true
use_math: true
---

:star: **This archive is refer from NICELAB Seminar** :star:

# **Lithium Ion Battery - Basic**

## **Lithum Ion Battery properties**

- High Cost, 화재 위험 → But, 계속해서 사용하는 이유?

1. Lower Self Discharge Rate

2. Long Life ⇒ More than 500 cycles

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(1).png' | relative_url }}" alt="Battery AI_1-(1)" width="419" height="218" >
  </figure>

🗯️ Lithium air, Zinc air 
- 음극(-)은 공기 중에 사용, 즉 양극(+)만 구성.  부피 ↓ 무게 ↓ → 에너지 밀도 ↑
- Lithium 원자 번호 : 3 → very light!  '궁극의 베터리'
- But, Both are in the research stage.

## **Functional Components of an Electrochemical Cell**

- 2차 전지의 기본적 구조
  1. Negative electrode (-)
  2. Positive electrode (+)
  3. Electrolyte
  4. Separator
  5. Current Collectors
    
  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(2).png' | relative_url }}" alt="Battery AI_1-(2)" width="456" height="90">
  </figure>

- Roles of each part
  1. Negative electrode 
     - Discharge : 음극(-) 산화 → 전자 배출
     - Charge : 음극(-) 환원 → 잔자 수용
  2. Positive electrode
     - Discharge : 양극(+) 환원 → 전자 수용
     - Charge : 양극(+) 산화 → 전자 배출
  3. Electrolyte
     - Negative & Positive의 전자 수용 및 배출은 모두 외부 회로에서 이뤄짐
     - 이에 상응하여 내부에서는 Ion의 이동(Diffusion 현상)이 필요
     - 이동 통로 역할을 수행
     - 전자가 외부에서 이동하고 내부에서는 이동하지 않도록 부도체로 구현
  4. Separator
     - +/- 극이 서로 섞이지 않도록 물리적 격리
     - 내부 쇼트, 자가 방전 현상 방지
  5. Current Collectors
     - 내부 +/- 입자에서 나온 전자를 회로 밖으로 보내는 역할
     - 파우더를 코딩하여 구현
     - Negative electrode Current Collectors : 구리
     - Positive electrode Current Collectors : 알루미늄

## **Electorde Structure**

- Lithium → intercalation 저장 방식 (like Sponge in water = Layer structure)

- These electrodes have two key properties

  1. Open crystals structures (빈 공간에 리튬 삽입 / 추출)
  2. Ablilty to accept compensating electrons (전자 수용 능력)
  
  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(3).png' | relative_url }}" alt="Battery AI_1-(3)">
  </figure>

- Negative electrode : graphite (흑연)

  🗯️ 실리콘 (차세대 재료)
  - 흑연보다 10배 이상 리튬 저장
  - But 리튬 양에 비례하는 팽창 면에서 오히려 흑연의 성능이 더 좋음 (margin size 확보 우수)

- Positive electrode : 양극 활물질(리튬 금속 산화물 = 결합형태) (5개 종류 - LCO, NCM, NCA, LMO, LFP)

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(4).png' | relative_url }}" alt="Battery AI_1-(4)">
  </figure>

  - 활물질 사용 이유 : 순수 리튬은 화학반응에 예민 ⇒ 불안정 So, make it Stable!

- 아래 화학식 참조 (Left: Negative electrode  Right: Positive electrode)
  
  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(5).png' | relative_url }}" alt="Battery AI_1-(5)">
    <figcaption>Left: Negative electrode     Right: Positive electrode</figcaption>
  </figure>

- Powder 입자

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(6).png' | relative_url }}" alt="Battery AI_1-(6)">
    <figcaption>Left: Positive electrode Powder     Right: Negative electrode Powder</figcaption>
  </figure>
  
  💡 3가지의 복합 형태
  1. 활물질 - 화학반응에 직접 관여
  2. 도전제 - 활물질들의 전도성 ↑
  3. 바인더 - 잘 섞일 수 있도록 하는 접착제 역할 → 리튬 확산에 기여 (골고루 잘 퍼지도록)

## **Charge Process (Discharge is reversed form)**

- During charge, Li exits surface of positive-electrode particles, gives up an electron, becoming Li+ in the electrolyte
- Meanwhile, the electron is forced (by charger) through external circuit to negative electrode
- Li+ joins with the electron, and Li enters negative-electrode particles at their surface
- Diffusion of Li in both electrodes equalizes internal concentrations (over time..)

---

# **BMS(Battery Management System)**

## **What must a BMS do?**

- Protect human safety of device's operator
- Protect cells of battery from damage in abuse / failure cases
- Prolong life of battery
- Maintain battery in a state in which it can fulfill its functional design requirements
- Inform the application controller how to make the best use of the pack right now

💡 All Lithium-ion battery packs require at least a minimal BMS for safety!!

## **General BMS functionality**

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(7).png' | relative_url }}" alt="Battery AI_1-(7)">
  </figure>

- Sensing and high-voltage control
  - Measure voltage, current, temperature of the each cells
  - Pre-charge for control contactor
  - Ground-fault detection
  - Thermal management
- Protection against over-charge, over-discharge, over-current, short circuit, extreme temperatures
- Interface
  - host application interface with Battery
- Performance Management
  - **SOC estimation** → Power-limit computation → Balance / equalize cells
  - 특정 셀만 과충전/과방전 될 수 있기 때문!
- Diagnostics (Abuse Detection : SOH estimation, SOL estimation)

🗯️ Pre-charge?
- 충전 시 전기를 인가할 때 전류가 갑자기 커지는 현상이 발생 → 스위치 자체가 납땜이 되버림
- 이를 극복하기 위해 미리 전압을 커패시터를 통해 충전하여 충전 시 전류가 튀지 않게 하는 방식.

🗯️ Over-Charge in Lithium Battery?
- Dendrite 현상
  - 리튬이 많아져서 음극으로 가지 못하고 Electrolyte에 남게됨!
  - 이후 쌓여서 뾰족하게 결정체를 이룸
  - 결정체가 분리막을 뚫고 음극으로 가 touch 하게 되면 short 현상이 일어나게 됨
  - 또한 과전류가 일어나 폭발의 위험성도 있음

🗯️ Over-Discharge in Lithium Battery?
- -극 current collector 구리막이 산화되어 전해질에 녹아들게 됨 → 내부 short 문제 발생

🗯️ Over-Current in Lithium Battery?
- 배터리 내부저항에 의해 $I^2R$에 해당하는 발열 발생
- 배터리 발열 → 전해(질) 끓어버림 / 분리막 녹아버림 → 열폭주 현상

## **BMS requirement 3 : Interface**

- **Charger Control** ⇒ Battery packs are charged in two ways
  1. Random 
     - Control by providing inverter power limits (e.g., regenerative braking)
  2. Plug - in 
     - Control charger current, voltage, balancing with CP/CV (+ CCV).
     - Heating systems may also be required.
- **Log Book Function**
  - For warrantee and Diagnostic purpose, BMS must store a log of atypical/abuse events
  - Can also store Diagnostic information regarding number of charge/discharge cycles completed & SOH estimates at beginning of each driving cycle
- **Range Estimation**
  - Heavily influenced by environmental factors
  - 베터리 제조사의 requirement에 따라가기 때문에 (제약) 현재 자동차 제조사마다 각자의 알고리즘을 보유. (자체 BMS 알고리즘 개발 대두)

🗯️ Regenerative Braking?
- 제동 시 관성력을 통해 회전자를 돌려 전동기를 발전기 기능으로 작동하게 함으로써 운동 에너지를 전기 에너지로 변환하는 전기 제동 방법

🗯️ CP/CV 
- 기준치를 두어 (예: 충전량 80%) 해당 기준치 아래는 일정한 Power로 충전하고, 기준치를 넘어갈 때는 일정한 전압으로 충전하는 방식
- 위 방법 외에도 다양한 방법이 있을 수 있음. 현재는 CP/CV를 많이 쓰는 경향이 있음.

## **BMS requirement 4 : Performance Management**

- Battery applications need to know **two battery quantities**
  1. How much energy is available? → in battery pack
  2. How much power is available? → for immdiate future
- Must estimate these values
  - To estimate energy : cell-states-of-charge $Z_k$ (unit: %) and capacities $Q_k$
  - To estimate power : cell-states-of-charge $Z_k$ (unit: %) and resistances $R_k$

### **Physical basis for cell SOC**

- $C_{s, max}$ is maximum theoretical concentration(농도) of lithium in electrode particle.

- $C_{s, avg,k}$ is average concentration of Li in particle at time $k$

- Present lithium stoichiometry(전극의 SOC) is $\theta_k = C_{s,avg,k} / C_{s,max}$

  💡 $\theta_k$ is a kind of electrode SOC, different from cell SOC!

- cell SOC is like :

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(8).png' | relative_url }}" alt="Battery AI_1-(8)">
  </figure>

### **How does SOC relate to cell voltage?**

- Cell voltage depends on Li surface concentration in particles that contact positive and negative current collectros
- SOC depends on average concentration over entire electrode
- Average concentration is not affected by
  1. Changing temperature → changes cell voltage
  2. Resting a cell (Diffusion) → changes cell voltage
- In summary, SOC changes only due to **passage of current**, or due to **self-discharge within the cell**

### **How does SOC relate to cell current?**

- SOC is related to cell current via

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(9).png' | relative_url }}" alt="Battery AI_1-(9)">
  </figure>

- $\eta$ is coulombic efficiency → 1에 근접하지만 1 이하

- Idea : SOC를 변화시키지 않는 전류들을 제외하고, 변화시키는 전류만 적분

💡 Coulombic Counting method is very hard to use because of lots of noise. (오차 누적)

### **What about "pack SOC"?**

<figure>
  <img src="{{ '/assets/images/BatteryAi_1-(10).png' | relative_url }}" alt="Battery AI_1-(10)">
</figure>

- pack SOC is ill-defined, so should never be used → 기준이 애매모호
- Issue : 이러한 문제는 cell balancing의 필요성을 제기함
- But we want to know battery pack's available energy!!

### **Cell available power estimate**

- Estimate must provide moving-window power limits
- HPPC test (Hybrid Pulse Power Characterization)
  - 이용가능 전력 추정 방법, 특정 SOC에서의 가용 출력을 측정하는 테스트
- Idea of HPPC
  - 방전 전류 인가 시 전압 강하, 충전 전류 인가 시 전압 상승 특성을 이용하여 내부 저항을 구함
  - cell resistance를 알면 배터리가 감당할 수 있는 전류의 양을 계산할 수 있음

<figure>
  <img src="{{ '/assets/images/BatteryAi_1-(11).png' | relative_url }}" alt="Battery AI_1-(11)">
</figure>

🗯️ Moving-Window Power limits
- Calculate(사용 가능 전력) to enforce design limits (e.g., on cell voltage and current), predictive over $\Delta T$(일반적으로 10~20초) second future time horizon
- Update at a faster rate(사용 가능 전력) than once every $\Delta T$ second
- 즉, 매 초마다 앞으로 $\Delta T$ 초 간 사용가능 한 전력이 얼마인지를 추정!

### **HPPC discharge power**

- For HPPC discharge power, assume simplified cell model

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(12).png' | relative_url }}" alt="Battery AI_1-(12)">
  </figure>

- Assume concerning only with keeping cell voltage between $V_{min}$ and $V_{max}$

- For discharge power, set $R = R_{dis, \Delta T}$ and clamp $v(t) = v_{min}$

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(13).png' | relative_url }}" alt="Battery AI_1-(13)">
  </figure>

- Simillar with charge power, set $R = R_{chg, \Delta T}$ and clamp $v(t) = v_{max}$

  <figure>
    <img src="{{ '/assets/images/BatteryAi_1-(14).png' | relative_url }}" alt="Battery AI_1-(14)">
  </figure>

- HPPC test는 평형 상태를 가정 → 따라서 이 경우 power limit을 HPPC test 값보다 낮게 설정!

---

# **State-of-health(SOH) - scheme**

- BMS must report a battery SOH estimate
- Two measurable indicators change as cell ages naturally
  1. Capacity decreases 20% to 30%
  2. Resistance increases 50% to 100%
- Estimating $R_k$ and $Q_k$ as the pack operators will give indicators of life

---

# **Summary**

- For Battery AI, we gonna consider about **SOH-estimate** part (배터리의 안정성 부분에 초점)
- Sensing & Monitoring to find specific features that caused Battery Problems!

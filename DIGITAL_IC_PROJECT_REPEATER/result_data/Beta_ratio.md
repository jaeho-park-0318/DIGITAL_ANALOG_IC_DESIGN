# CMOS Inverter의 Beta Ratio 설정

## 1. Beta Ratio의 의미

CMOS inverter에서 NMOS와 PMOS의 구동 능력은 각각 다음과 같이 표현할 수 있다.

- beta_n = mu_n * Cox * (Wn / Ln)
- beta_p = mu_p * Cox * (Wp / Lp)

따라서 PMOS와 NMOS의 상대적인 구동 능력은 다음과 같다.

- beta_p / beta_n
  = [mu_p * (Wp / Lp)] / [mu_n * (Wn / Ln)]

같은 공정에서 Ln = Lp 라면 다음과 같이 단순화할 수 있다.

- beta_p / beta_n ≈ (mu_p * Wp) / (mu_n * Wn)

여기서 Wp / Wn은 단순한 width ratio이고,
beta_p / beta_n은 mobility까지 포함한 실제 구동 능력의 비율이다.

---

## 2. PMOS Width를 크게 설정하는 이유

일반적으로 전자 이동도는 정공 이동도보다 크다.

- mu_n > mu_p

따라서 NMOS와 PMOS의 width를 동일하게 설정하면
NMOS의 구동력이 PMOS보다 더 강해질 수 있다.

이를 보상하기 위해 일반적으로

- Wp > Wn

으로 PMOS width를 크게 설정한다.

즉,

- mu_p * Wp ≈ mu_n * Wn

이 되도록 조절하여 Pull-up과 Pull-down의 구동 능력을 비슷하게 맞추는 것이
Beta Ratio 설정의 기본 목적이다.

---

## 3. VTC를 이용한 Beta Ratio 설정

CMOS inverter의 Beta Ratio는 VTC(Voltage Transfer Characteristic)를 이용하여 설정할 수 있다.

VTC에서

- Vin = Vout = Vm

이 되는 지점을 switching point라고 한다.

이 지점에서는 NMOS와 PMOS의 전류가 같아진다.

- Idn = |Idp|

이상적인 대칭 inverter에서는 switching point가 다음 값에 가까운 것이 바람직하다.

- Vm ≈ VDD / 2

따라서 NMOS width를 고정하고 PMOS width를 변화시키면서 VTC를 측정한 뒤,

- |Vm - VDD / 2|

가 가장 작아지는 Wp / Wn 값을 선택할 수 있다.

이 값을 PMOS와 NMOS의 구동 능력을 균형화하기 위한
최적 width ratio로 볼 수 있다.

---

## 4. 본 프로젝트의 Beta Ratio 설정 방법

GPDK045, GPDK090, GPDK180 각각에 대해 다음과 같이 진행한다.

1. NMOS의 Wn과 Ln을 고정한다.
2. PMOS의 Wp를 변화시킨다.
3. DC Sweep을 통해 inverter VTC를 측정한다.
4. Vin = Vout이 되는 switching point Vm을 찾는다.
5. Vm이 VDD / 2에 가장 가까운 Wp / Wn을 선택한다.

즉, 각 공정에서

- |Vm - VDD / 2|

가 최소가 되는 width ratio를 찾는다.

프레젠테이션에서는 공정별로 다음과 같은 width ratio가 사용되었다.

| 공정 | Wn | Wp | Wp / Wn |
|---|---:|---:|---:|
| GPDK180 | 400 nm | 1.80 um | 4.50 |
| GPDK090 | 120 nm | 350 nm | 2.93 |
| GPDK045 | 120 nm | 140 nm | 1.17 |

즉, 공정 미세화에 따라 선택된 PMOS / NMOS width ratio가 감소하는 결과가 나타났다.

---

## 5. Beta Ratio와 Rise / Fall Time

VTC를 통해 결정한 width ratio는 transient 특성에서도 확인할 필요가 있다.

PMOS는 output을 충전하므로 rise time에 주로 영향을 준다.

- beta_p 증가
- PMOS의 등가 저항 감소
- rise time 감소

NMOS는 output을 방전하므로 fall time에 주로 영향을 준다.

- beta_n 증가
- NMOS의 등가 저항 감소
- fall time 감소

따라서 적절한 Beta Ratio가 설정되면 이상적으로 다음과 같은 조건에 가까워지는 것이 바람직하다.

- rise time ≈ fall time

Propagation delay도 다음과 같이 비교할 수 있다.

- tpLH ≈ tpHL

---

## 6. VTC Balance와 Transient Balance

중요한 점은

- Vm = VDD / 2

가 되는 조건과

- rise time = fall time

이 되는 조건이 반드시 동일하지는 않는다는 것이다.

Vm은 DC 특성에 의해 결정되지만,
rise time과 fall time은 다음 요소들의 영향도 받는다.

- Load capacitance
- Gate capacitance
- Diffusion capacitance
- Input slew
- Parasitic capacitance
- MOSFET의 비선형 ON resistance

따라서 본 프로젝트에서는 다음 세 가지를 함께 비교하는 것이 적절하다.

1. Vm ≈ VDD / 2
2. rise time ≈ fall time
3. tpLH ≈ tpHL

이를 통해 각 공정에서 PMOS와 NMOS의 구동 능력이 얼마나 균형을 이루는지 확인할 수 있다.

---

## 7. 1X, 2X, 4X Repeater와의 관계

Beta Ratio와 1X, 2X, 4X sizing은 서로 다른 개념이다.

Beta Ratio는 PMOS와 NMOS의 상대적인 구동 능력을 결정한다.

반면,

- 1X
- 2X
- 4X

는 inverter 전체의 구동 능력을 증가시키는 scaling이다.

예를 들어 Wp / Wn = 2로 결정되었다고 하면 다음과 같이 구성할 수 있다.

- 1X : Wn = W, Wp = 2W
- 2X : Wn = 2W, Wp = 4W
- 4X : Wn = 4W, Wp = 8W

이 경우 PMOS와 NMOS의 상대적인 width ratio는 그대로 유지되면서
전체 transistor 크기만 증가한다.

즉,

- Beta Ratio : PMOS / NMOS의 상대적인 drive strength 결정
- 1X, 2X, 4X : inverter 전체의 drive strength 결정

으로 구분할 수 있다.

---

## 8. 결론

본 프로젝트에서는 각 공정에서 NMOS width를 고정하고
PMOS width를 변화시켜 inverter VTC의 switching point가

- Vm ≈ VDD / 2

가 되도록 Wp / Wn을 선정한다.

이 값은 엄밀히 말하면 Beta Ratio 자체라기보다는
PMOS와 NMOS의 effective drive strength를 균형화하기 위한
Beta-balanced width ratio라고 보는 것이 정확하다.

이후 transient simulation을 통해

- rise time ≈ fall time
- tpLH ≈ tpHL

인지 확인함으로써 공정별 inverter sizing이 적절한지 검증할 수 있다.

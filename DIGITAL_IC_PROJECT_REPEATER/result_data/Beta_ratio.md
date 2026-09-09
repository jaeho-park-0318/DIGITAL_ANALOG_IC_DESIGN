# CMOS Inverter의 β-Ratio 설정

## 1. β-Ratio의 의미

CMOS inverter에서 NMOS와 PMOS의 구동 능력은 다음과 같이 표현할 수 있다.

\[
\beta_n = \mu_n C_{ox}\frac{W_n}{L_n}
\]

\[
\beta_p = \mu_p C_{ox}\frac{W_p}{L_p}
\]

따라서 PMOS와 NMOS의 상대적인 구동 능력은

\[
\frac{\beta_p}{\beta_n}
=
\frac{\mu_p(W_p/L_p)}
{\mu_n(W_n/L_n)}
\]

으로 나타낼 수 있다.

같은 공정에서

\[
L_n=L_p
\]

라면

\[
\frac{\beta_p}{\beta_n}
\approx
\frac{\mu_pW_p}{\mu_nW_n}
\]

으로 단순화할 수 있다.

여기서 \(W_p/W_n\)은 단순한 **width ratio**이고, \(\beta_p/\beta_n\)은 mobility까지 포함한 **실제 구동 능력의 비율**이라는 점을 구분해야 한다.

---

## 2. PMOS Width를 크게 설정하는 이유

일반적으로 전자 이동도는 정공 이동도보다 크다.

\[
\mu_n > \mu_p
\]

따라서 NMOS와 PMOS의 width를 같게 설정하면 NMOS의 구동력이 더 강해질 수 있다.

이를 보상하기 위해 보통

\[
W_p > W_n
\]

으로 PMOS width를 증가시킨다.

즉,

\[
\mu_pW_p \approx \mu_nW_n
\]

이 되도록 조절하여 Pull-up과 Pull-down의 구동력을 비슷하게 맞추는 것이 β-ratio 설정의 기본 목적이다.

---

## 3. VTC를 이용한 β-Ratio 설정

CMOS inverter의 β-ratio는 VTC(Voltage Transfer Characteristic)를 이용해 설정할 수 있다.

VTC에서

\[
V_{in}=V_{out}=V_M
\]

이 되는 지점을 switching point라고 한다.

이 지점에서는 NMOS와 PMOS의 전류가 같아지며,

\[
I_{Dn}=|I_{Dp}|
\]

가 성립한다.

이상적인 대칭 inverter에서는 switching point가

\[
V_M \approx \frac{V_{DD}}{2}
\]

에 위치하는 것이 바람직하다.

따라서 NMOS width를 고정하고 PMOS width를 변화시키면서 VTC를 측정한 후,

\[
\left|V_M-\frac{V_{DD}}{2}\right|
\]

가 가장 작아지는 \(W_p/W_n\)을 최적 width ratio로 선정할 수 있다.

---

## 4. 본 프로젝트의 β-Ratio 설정 방법

GPDK045, GPDK090, GPDK180 각각에 대해 다음 순서로 진행한다.

1. NMOS의 \(W_n\), \(L_n\)을 고정한다.
2. PMOS의 \(W_p\)를 변화시킨다.
3. DC Sweep을 통해 inverter VTC를 구한다.
4. \(V_{in}=V_{out}\)이 되는 \(V_M\)을 찾는다.
5. \(V_M\)이 \(V_{DD}/2\)에 가장 가까운 \(W_p/W_n\)을 선정한다.

즉,

\[
\left(\frac{W_p}{W_n}\right)_{opt}
=
\arg\min
\left|
V_M-\frac{V_{DD}}{2}
\right|
\]

로 표현할 수 있다.

프레젠테이션에서는 공정별로 다음과 같은 width ratio가 사용되었다. :contentReference[oaicite:0]{index=0}

| 공정 | \(W_n\) | \(W_p\) | \(W_p/W_n\) |
|---|---:|---:|---:|
| GPDK180 | 400 nm | 1.80 µm | 4.50 |
| GPDK090 | 120 nm | 350 nm | 2.93 |
| GPDK045 | 120 nm | 140 nm | 1.17 |

공정이 미세화될수록 필요한 \(W_p/W_n\)이 감소하는 경향을 보였다.

---

## 5. β-Ratio와 Rise/Fall Time

VTC에서 결정한 width ratio는 transient 특성에서도 확인할 필요가 있다.

PMOS는 output을 충전하므로 rise time과 관련이 있다.

\[
\beta_p \uparrow
\Rightarrow
R_p \downarrow
\Rightarrow
t_r \downarrow
\]

NMOS는 output을 방전하므로 fall time과 관련이 있다.

\[
\beta_n \uparrow
\Rightarrow
R_n \downarrow
\Rightarrow
t_f \downarrow
\]

따라서 적절한 β-ratio가 설정되면 이상적으로

\[
t_r \approx t_f
\]

가 되는 것이 바람직하다.

Propagation delay 역시

\[
t_{pLH} \approx t_{pHL}
\]

에 가까워지는지 확인하면 된다.

---

## 6. VTC Balance와 Transient Balance

중요한 점은

\[
V_M=\frac{V_{DD}}{2}
\]

가 되는 조건과

\[
t_r=t_f
\]

가 되는 조건이 반드시 동일하지는 않는다는 것이다.

VTC는 DC 특성이고, rise/fall time은 capacitance, input slew, parasitic 성분 등이 포함된 transient 특성이기 때문이다.

따라서 본 프로젝트에서는 다음 두 가지 기준을 함께 확인하는 것이 적절하다.

\[
V_M \approx \frac{V_{DD}}{2}
\]

\[
t_r \approx t_f
\]

그리고 추가로

\[
t_{pLH} \approx t_{pHL}
\]

까지 비교하면 공정별 inverter의 균형 특성을 보다 정확하게 분석할 수 있다.

---

## 7. 1X, 2X, 4X Repeater와의 관계

β-ratio와 1X, 2X, 4X sizing은 서로 다른 개념이다.

β-ratio는 PMOS와 NMOS의 상대적인 구동력을 결정하고,

\[
1X \rightarrow 2X \rightarrow 4X
\]

는 inverter 전체의 구동 능력을 증가시키는 것이다.

예를 들어

\[
W_p/W_n=2
\]

로 결정했다면,

- 1X : \(W_n=W\), \(W_p=2W\)
- 2X : \(W_n=2W\), \(W_p=4W\)
- 4X : \(W_n=4W\), \(W_p=8W\)

와 같이 β-ratio는 유지하면서 전체 transistor size만 증가시킬 수 있다.

---

## 8. 결론

본 프로젝트에서는 각 공정에서 NMOS width를 고정하고 PMOS width를 조절하여 VTC의 switching point가

\[
V_M \approx \frac{V_{DD}}{2}
\]

가 되도록 \(W_p/W_n\)을 선정한다.

이 값은 엄밀한 의미에서 β-ratio 자체라기보다는 PMOS와 NMOS의 effective drive strength를 균형화하기 위한 **β-balanced width ratio**라고 보는 것이 정확하다.

이후 transient simulation을 통해

\[
t_r \approx t_f
\]

및

\[
t_{pLH} \approx t_{pHL}
\]

가 되는지 확인하여 공정별 최적 inverter sizing을 검증한다.

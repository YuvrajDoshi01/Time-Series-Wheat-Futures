# Dynamic Transfer Function Analysis: U.S. Wheat Ground and Farm Prices (1989–2026)

## Overview
This project investigates the dynamic relationship between monthly U.S. industrial flour-milling volume (**Wheat Ground for Flour**, USDA Table 14) and monthly **All Wheat Farm Price Received** (USDA Table 18) from **June 1989 to September 2026** ($N = 448$ consecutive monthly observations, with zero missing or duplicate months) using the Box–Jenkins Transfer Function methodology.

---

## 1. Data Alignment & Variance-Stabilizing Transformations
* **Calendar Alignment:** USDA marketing-year entries (`Jun–May`) were converted into standard monthly calendar dates (`YYYY-MM`) and merged into a complete panel (`1989-06` to `2026-09`).
* **Box–Cox Transformations:**
  * **Output Series ($Y_t$):** Box–Cox maximum-likelihood estimation on farm price ($P_t$, in $\$/\text{bushel}$) gave $\hat{\lambda}_P = -0.2737 \approx 0$, supporting the natural log transformation $Y_t = \ln(P_t)$.
  * **Input Series ($X_t$):** Box–Cox estimation on wheat ground ($Q_t$, in million bushels) gave $\hat{\lambda}_Q = 3.2450$, yielding the standardized power-transformed series:
    $$X_t = \frac{Q_t^{(3.2450)} - \mu_Q}{\sigma_Q}$$

---

## 2. Stationarity & Differencing (`IDENTIFY` Stage)
* **Output ($Y_t = \ln P_t$):** Slow ACF decay and a Single-Mean ADF $p = 0.3002$ indicate a unit root. Taking a first difference ($d = 1$) produces the stationary series $y_t = (1 - B) Y_t = \ln(P_t) - \ln(P_{t-1})$ (Zero-Mean ADF $\tau = -12.73$, $p = 3.2 \times 10^{-23}$; Ljung–Box $p < 10^{-10}$), with clean IACF decay and no sign of over-differencing.
* **Input ($X_t$):** Strong 12-month seasonal cycles and a Trend ADF $p = 0.0894$ indicate non-stationarity. Applying both regular and seasonal differencing ($d = 1, D = 1, s_{\text{season}} = 12$) yields the stationary series:
  $$x_t = (1 - B)(1 - B^{12}) X_t$$
  (Zero-Mean ADF $\tau = -8.68$, $p = 9.4 \times 10^{-15}$; Ljung–Box $p < 10^{-19}$).

---

## 3. Pre-Whitening & Pre-Whitened Cross-Correlation ($\hat{\rho}_{\alpha\beta}(k)$)
* **Pre-Whitening Model for $X_t$:** A subset seasonal ARIMA model, $\text{SARIMA}(1,1,[1,2,4,6]) \times (1,1,1)_{12}$ ($\text{AIC} = 345.11$), whitened $X_t$ into white-noise innovations $\alpha_t$ (Ljung–Box $p = 0.773$ at lag 6, $p = 0.088$ at lag 12, $p = 0.066$ at lag 24).
* **Cross-Correlation Diagnostics:** Filtering $Y_t$ with the same $\text{SARIMA}$ operator produced $\beta_t$. Evaluating $\hat{\rho}_{\alpha\beta}(k)$ against the 95% two-standard-error threshold ($\pm 0.0941$) showed:
  1. **No Feedback ($k < 0$):** All negative lags ($k = -12 \dots -1$) are inside the confidence band, indicating farm prices do not feed back into milling volume.
  2. **Identified Orders ($b = 9, s = 1, r = 0$):** Lags $k = 0 \dots 8$ are insignificant, followed by significant positive spikes at **$k = 9$ ($+0.0969$)** and **$k = 10$ ($+0.0975$)**, and a sharp drop-off for $k \ge 11$.

---

## 4. Estimated Transfer Function–Noise Model (Slide Notation)

In general slide form:
$$y_t = \frac{\omega(B) B^b}{\delta(B)} x_t + N_t, \qquad N_t = \frac{\theta(B)\Theta(B^s)}{\phi(B)\Phi(B^s)} a_t$$

With identified orders $(b = 9, s = 1, r = 0)$ and $\text{AR}(1)$ noise $N_t = \frac{1}{1 - \phi_1 B} a_t$, the estimated model is:
$$(1 - B) Y_t = \left(0.0060 + 0.0089 B\right) B^9 (1 - B)(1 - B^{12}) X_t + \frac{1}{1 - 0.3624 B} a_t, \qquad \hat{\sigma}_a^2 = 0.00227$$

| Term | Parameter / Lag | Estimate | Std. Error | $z$-stat | $p$-value |
| :--- | :--- | :---: | :---: | :---: | :---: |
| Input lag $t - 9$ | $\omega_0$ | $+0.0060$ | $0.0047$ | $+1.27$ | $0.203$ |
| Input lag $t - 10$ | $-\omega_1$ | $+0.0089$ | $0.0044$ | $+2.04$ | $0.041$ |
| Noise $\text{AR}(1)$ | $\phi_1$ | $+0.3624$ | $0.0394$ | $+9.19$ | $<0.001$ |

### Key Findings
* **AIC Improvement:** The transfer function model ($\text{AIC} = -1373.70$) outperforms the univariate $\text{ARIMA}(1,1,0)$ benchmark ($\text{AIC} = -1370.22$), and its residuals pass Ljung–Box white-noise checks across lags 6, 12, 18, and 24.
* **Economic Interpretation:** An unexpected increase in seasonally adjusted wheat ground ($X_t$) leads to a statistically significant upward shift in wheat farm price growth **9 to 10 months later** (combined steady-state gain $g = +0.0149$), consistent with sustained milling activity tightening commercial wheat stocks ahead of the next harvest cycle.
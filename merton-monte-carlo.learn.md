---
title: "The Merton Model and Monte Carlo Simulation"
lang: en
tags: [merton, monte-carlo, credit-risk, stochastic-calculus, quantitative-finance]
estimated_time: 90min
math: true
---

# The Merton Model and Monte Carlo Simulation

This path teaches you the Merton structural credit risk model from first principles, derives its key formulas, and shows you how to estimate default probabilities using Monte Carlo simulation.

**Prerequisites:** basic probability, some familiarity with stochastic calculus is helpful but not required.

---

## Part 1 — Geometric Brownian Motion

### What is GBM?

A **Geometric Brownian Motion** (GBM) is the standard continuous-time model for the evolution of a positive quantity — asset prices, firm values, commodity prices.

The dynamics are governed by the stochastic differential equation (SDE):

$$dV_t = \mu V_t \, dt + \sigma V_t \, dW_t$$

where:
- $V_t$ — value of the process at time $t$
- $\mu$ — drift (expected instantaneous rate of return)
- $\sigma$ — volatility (instantaneous standard deviation of returns)
- $W_t$ — standard Brownian motion (Wiener process)

The key feature: **both drift and diffusion are proportional to $V_t$**. This ensures the process stays positive and that returns (not levels) are normally distributed.

### Itô's Lemma and the Exact Solution

To solve this SDE, apply **Itô's lemma** to $f(V_t) = \ln V_t$:

$$d(\ln V_t) = \frac{1}{V_t} dV_t - \frac{1}{2V_t^2}(dV_t)^2$$

Using $dV_t = \mu V_t\,dt + \sigma V_t\,dW_t$ and the Itô rule $(dW_t)^2 = dt$:

$$d(\ln V_t) = \mu\,dt + \sigma\,dW_t - \frac{1}{2}\sigma^2\,dt = \left(\mu - \frac{\sigma^2}{2}\right)dt + \sigma\,dW_t$$

Integrating from $0$ to $T$:

$$\ln\frac{V_T}{V_0} = \left(\mu - \frac{\sigma^2}{2}\right)T + \sigma W_T$$

Therefore the **exact GBM solution** is:

$$\boxed{V_T = V_0 \exp\!\left(\left(\mu - \frac{\sigma^2}{2}\right)T + \sigma W_T\right)}$$

```example title="Numerical illustration"
V0    = 100     # initial firm value
mu    = 0.06    # annual drift (6%)
sigma = 0.20    # annual volatility (20%)
T     = 1       # 1 year

# Log-return is normally distributed:
# ln(V_T/V_0) ~ N((0.06 - 0.02) × 1, 0.20² × 1)
#             ~ N(0.04, 0.04)

# Expected value:   E[V_T] = 100 × exp(0.06) ≈ 106.18
# Median value:     V_0 × exp((μ - σ²/2)T) = 100 × exp(0.04) ≈ 104.08

# Note: mean > median because the log-normal is right-skewed
```

> [!note]
> The $-\sigma^2/2$ term is the **Itô correction**. Without it, the expected value would be incorrect. This is a direct consequence of the non-linearity of the exponential and the quadratic variation of Brownian motion.

### Key Statistical Properties

Given the exact solution, log-returns are normally distributed:

$$\ln\frac{V_T}{V_0} \sim \mathcal{N}\!\left(\left(\mu - \frac{\sigma^2}{2}\right)T,\ \sigma^2 T\right)$$

And $V_T$ itself is **log-normally distributed** with:
- $\mathbb{E}[V_T] = V_0 e^{\mu T}$
- $\text{Var}(V_T) = V_0^2 e^{2\mu T}\left(e^{\sigma^2 T} - 1\right)$

!import ./quiz-gbm.quiz.md

---

## Part 2 — The Merton Structural Model

### The Setup

Robert Merton (1974) applied option pricing theory to corporate debt. The key insight: **equity is a call option on the firm's assets**.

The model assumes:
- The firm's total asset value $V_t$ follows a GBM under the risk-neutral measure $\mathbb{Q}$:
$$dV_t = r V_t\,dt + \sigma_V V_t\,dW_t^{\mathbb{Q}}$$
- The firm has issued a **single zero-coupon bond** with face value $D$ maturing at $T$
- Financial markets are frictionless (no taxes, no bankruptcy costs)
- At maturity $T$, if $V_T \geq D$: bondholders are repaid in full, shareholders keep the residual
- At maturity $T$, if $V_T < D$: default — bondholders receive $V_T$, shareholders receive $0$

### Equity as a Call Option

The payoff to shareholders at maturity is:

$$E_T = \max(V_T - D,\ 0)$$

This is exactly the payoff of a European call option on $V_T$ with strike $D$. By the Black-Scholes formula, the current equity value is:

$$\boxed{E_0 = V_0 N(d_1) - D e^{-rT} N(d_2)}$$

with:

$$d_1 = \frac{\ln(V_0/D) + \left(r + \frac{\sigma_V^2}{2}\right)T}{\sigma_V\sqrt{T}}, \qquad d_2 = d_1 - \sigma_V\sqrt{T}$$

where $N(\cdot)$ is the standard normal CDF.

### Debt Value and Credit Spread

By the balance sheet identity $V_0 = E_0 + B_0$:

$$B_0 = V_0 - E_0 = D e^{-rT} N(d_2) + V_0 N(-d_1)$$

Equivalently, risky debt = risk-free bond − put on firm assets:

$$B_0 = D e^{-rT} - \underbrace{\left[D e^{-rT} N(-d_2) - V_0 N(-d_1)\right]}_{\text{put option value (credit risk premium)}}$$

The **credit spread** $s$ is defined by $B_0 = D e^{-(r+s)T}$, giving:

$$s = -\frac{1}{T}\ln\!\left(\frac{B_0}{D e^{-rT}}\right) = -\frac{1}{T}\ln\!\left[N(d_2) + \frac{V_0}{D e^{-rT}} N(-d_1)\right]$$

### Default Probability

**Risk-neutral probability of default** (used for pricing):

$$\text{PD}^{\mathbb{Q}} = \mathbb{Q}(V_T < D) = N(-d_2)$$

**Physical (real-world) probability of default** (used for risk management), replacing $r$ with the actual drift $\mu$:

$$\text{PD}^{\mathbb{P}} = \mathbb{P}(V_T < D) = N\!\left(-\frac{\ln(V_0/D) + \left(\mu - \frac{\sigma_V^2}{2}\right)T}{\sigma_V\sqrt{T}}\right)$$

### Distance to Default

The **Distance to Default** (DD) is the number of standard deviations the log firm value is above the default threshold:

$$\text{DD} = \frac{\ln(V_0/D) + \left(\mu - \frac{\sigma_V^2}{2}\right)T}{\sigma_V\sqrt{T}}$$

So $\text{PD}^{\mathbb{P}} = N(-\text{DD})$. This is the KMV/Moody's Analytics metric widely used in practice.

```example title="Merton model — worked example"
# Firm parameters
V0    = 150     # firm asset value (£M)
D     = 100     # debt face value (£M)
sigma = 0.25    # asset volatility
mu    = 0.08    # real-world drift
r     = 0.04    # risk-free rate
T     = 1       # 1 year

# d1, d2 (risk-neutral)
d1 = (ln(150/100) + (0.04 + 0.03125) × 1) / (0.25 × 1)
   = (0.4055 + 0.0713) / 0.25
   = 1.906

d2 = 1.906 - 0.25 = 1.656

# Equity value
E0 = 150 × N(1.906) - 100 × exp(-0.04) × N(1.656)
   = 150 × 0.9717 - 96.08 × 0.9511
   ≈ 145.75 - 91.37 ≈ 54.38 (£M)

# Risk-neutral PD
PD_Q = N(-1.656) ≈ 4.9%

# Physical PD (using mu instead of r)
d2_phys = (ln(150/100) + (0.08 - 0.03125)) / 0.25
        = (0.4055 + 0.04875) / 0.25 = 1.817
PD_P = N(-1.817) ≈ 3.5%

# Distance to Default
DD = 1.817   (firm is ~1.8 std devs above default threshold)
```

> [!tip]
> In practice, $V_0$ and $\sigma_V$ are **not directly observable**. They must be inferred from observable equity data ($E_0$, $\sigma_E$) by solving the two-equation system:
> $$E_0 = V_0 N(d_1) - D e^{-rT} N(d_2)$$
> $$\sigma_E E_0 = N(d_1)\,\sigma_V V_0$$
> This requires a numerical solver (e.g. Newton-Raphson).

!import ./quiz-merton.quiz.md

---

## Part 3 — Monte Carlo Simulation

### Why Monte Carlo?

The Merton model has closed-form solutions. So why simulate? Several reasons:

- **Extensions**: jump-diffusion, stochastic volatility, multi-factor models — no closed form
- **Portfolio credit risk**: correlated defaults require simulation
- **Sensitivity analysis**: full distribution of outcomes, not just moments
- **Validation**: check analytical results, build intuition

### Simulating GBM Paths

Since GBM has an **exact discretisation**, we can simulate terminal firm values directly — no need to discretise the path:

$$V_T^{(i)} = V_0 \exp\!\left(\left(\mu - \frac{\sigma^2}{2}\right)T + \sigma\sqrt{T}\cdot Z^{(i)}\right), \quad Z^{(i)} \sim \mathcal{N}(0,1)$$

For path-dependent problems (barriers, early default), discretise over $n$ steps of size $\Delta t = T/n$:

$$V_{t+\Delta t}^{(i)} = V_t^{(i)} \exp\!\left(\left(\mu - \frac{\sigma^2}{2}\right)\Delta t + \sigma\sqrt{\Delta t}\cdot Z^{(i)}_t\right)$$

```example title="Monte Carlo PD estimation — Python pseudocode"
import numpy as np

# Parameters
V0, D, mu, sigma, T = 100, 80, 0.06, 0.20, 1
N = 100_000  # number of paths

# Simulate terminal firm values (exact, 1 step)
Z   = np.random.standard_normal(N)
V_T = V0 * np.exp((mu - 0.5 * sigma**2) * T + sigma * np.sqrt(T) * Z)

# Estimate PD
defaults = V_T < D
PD_mc = defaults.mean()
std_err = np.sqrt(PD_mc * (1 - PD_mc) / N)

print(f"Monte Carlo PD: {PD_mc:.4f}")
print(f"95% CI: [{PD_mc - 1.96*std_err:.4f}, {PD_mc + 1.96*std_err:.4f}]")

# Compare with analytical result
from scipy.stats import norm
d2 = (np.log(V0/D) + (mu - 0.5*sigma**2)*T) / (sigma*np.sqrt(T))
PD_analytical = norm.cdf(-d2)
print(f"Analytical PD:  {PD_analytical:.4f}")
```

### Statistical Precision

The Monte Carlo estimate $\hat{\text{PD}} = \frac{1}{N}\sum_{i=1}^N \mathbf{1}[V_T^{(i)} < D]$ has standard error:

$$\text{SE}(\hat{\text{PD}}) = \sqrt{\frac{\hat{\text{PD}}(1-\hat{\text{PD}})}{N}}$$

The $95\%$ confidence interval is $\hat{\text{PD}} \pm 1.96 \cdot \text{SE}$.

> [!tip]
> To halve the standard error, you need to quadruple $N$. For rare events (PD $< 1\%$), plain Monte Carlo is inefficient — consider **importance sampling** or **quasi-Monte Carlo** (Sobol sequences).

### Variance Reduction

| Technique | Mechanism | Typical gain |
|-----------|-----------|-------------|
| **Antithetic variates** | Pair each $Z^{(i)}$ with $-Z^{(i)}$ | $2\times$ or more |
| **Control variates** | Use analytical price as a control | $5\text{–}20\times$ |
| **Quasi-Monte Carlo** | Sobol/Halton low-discrepancy sequences | $10\text{–}100\times$ |
| **Importance sampling** | Shift the sampling distribution towards the rare event | Large for small PD |

```example title="Antithetic variates"
# Instead of N standard normals:
Z_pos = np.random.standard_normal(N // 2)
Z_neg = -Z_pos                              # antithetic pair

Z = np.concatenate([Z_pos, Z_neg])          # N samples, but correlated in pairs

V_T = V0 * np.exp((mu - 0.5*sigma**2)*T + sigma*np.sqrt(T)*Z)
PD_av = (V_T < D).mean()

# The negative correlation between pairs reduces variance of the mean
# at essentially zero extra cost.
```

### Extending to Correlated Defaults

For a portfolio of $m$ firms, simulate correlated GBMs using the **Cholesky decomposition** of the correlation matrix $\Sigma$:

$$\mathbf{Z} = L \mathbf{U}, \quad \mathbf{U} \sim \mathcal{N}(\mathbf{0}, I_m), \quad \Sigma = L L^\top$$

Each firm $j$ defaults if $V_T^{(j)} < D^{(j)}$. The joint simulation captures default correlation — critical for CDO pricing and portfolio VaR.

!import ./quiz-montecarlo.quiz.md

---

## Part 4 — Calibration and Practical Considerations

### Calibrating the Model from Equity Data

Since $V_0$ and $\sigma_V$ are unobservable, practitioners solve the system:

$$\begin{cases} E_0 = V_0 N(d_1) - D e^{-rT} N(d_2) \\ \sigma_E E_0 = N(d_1) \cdot \sigma_V V_0 \end{cases}$$

**Iterative procedure (KMV approach):**

1. Start with an initial guess: $V_0^{(0)} = E_0 + D$, $\sigma_V^{(0)} = \sigma_E \cdot E_0/(E_0 + D)$
2. Compute a time series of implied $V_t$ from daily equity prices using the current $\sigma_V$
3. Re-estimate $\sigma_V$ from the time series of $V_t$
4. Repeat until convergence

```example title="Calibration challenge"
# Observed market data:
E0      = 50       # equity market cap (£M)
sigma_E = 0.45     # equity volatility (annualised)
D       = 80       # book value of debt (£M)
r       = 0.04     # risk-free rate
T       = 1        # 1-year horizon

# Solve for V0, sigma_V numerically:
# Two equations, two unknowns
# Solution (approximately):
#   V0     ≈ 128 (£M)  — firm total assets
#   sigma_V ≈ 0.176     — asset volatility

# Note: asset volatility (17.6%) << equity volatility (45%)
# because equity is a leveraged claim on assets.
# Financial leverage amplifies volatility.
```

### Limitations of the Merton Model

> [!warning]
> The Merton model, while elegant, has well-documented empirical shortcomings:

1. **Default only at maturity** — in reality firms can default at any time. The Black-Cox (1976) first-passage model addresses this.

2. **Flat credit spreads at short maturities** — the model predicts near-zero spreads for short-term debt, contradicting market data. Jump-diffusion models (Merton 1976) help.

3. **Simple capital structure** — one zero-coupon bond. Real firms have complex liabilities (revolving credit, senior/subordinated tranches, covenants).

4. **Constant volatility** — real asset volatility changes over time. Stochastic volatility extensions exist.

5. **Observable inputs** — $V_0$ and $\sigma_V$ must be inferred, introducing calibration error.

### Extensions

| Model | Key extension |
|-------|--------------|
| Black-Cox (1976) | First-passage default: default when $V_t$ hits a lower barrier |
| Longstaff-Schwartz (1995) | Stochastic interest rates |
| Leland (1994) | Endogenous default boundary, optimal capital structure |
| CreditGrades (2002) | Uncertain default threshold, better short-spread calibration |
| Merton + jumps (1976) | Jump-diffusion asset process, fat tails |

---

## Summary

```summary
- **GBM**: $dV_t = \mu V_t\,dt + \sigma V_t\,dW_t$ — exact solution via Itô's lemma gives log-normal $V_T$
- **Merton model**: equity = call on firm assets; default if $V_T < D$ at maturity $T$
- **Equity pricing**: $E_0 = V_0 N(d_1) - D e^{-rT} N(d_2)$ (Black-Scholes formula)
- **Default probabilities**: $\text{PD}^{\mathbb{Q}} = N(-d_2)$ (risk-neutral), $\text{PD}^{\mathbb{P}} = N(-\text{DD})$ (physical)
- **Distance to Default**: $\text{DD} = [\ln(V_0/D) + (\mu - \sigma^2/2)T] / (\sigma\sqrt{T})$
- **Monte Carlo**: simulate $V_T^{(i)} = V_0\exp((\mu-\sigma^2/2)T + \sigma\sqrt{T}Z^{(i)})$, count defaults
- **Precision**: SE $= \sqrt{p(1-p)/N}$ — quadruple $N$ to halve the error
- **Variance reduction**: antithetic variates, control variates, quasi-Monte Carlo
- **Calibration**: solve two-equation system for $V_0$, $\sigma_V$ from observable equity data
- **Limitations**: maturity-only default, flat short spreads, simple capital structure
```

---

## Final Assessment

!import ./quiz-merton-final.quiz.md

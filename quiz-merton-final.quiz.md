---
title: "Merton Model & Monte Carlo — Final Assessment"
lang: en
tags: [merton, monte-carlo, credit-risk, quantitative-finance]
passing_score: 0.70
feedback_mode: deferred
scoring:
  correct: 1
  incorrect: 0
---

# Merton Model & Monte Carlo — Final Assessment

## Q1 · A GBM has $\mu = 0.08$, $\sigma = 0.25$, $V_0 = 120$, $D = 100$, $T = 2$. What is the Distance to Default?

```quiz
type: open
hint: "DD = (ln(V₀/D) + (μ − σ²/2)T) / (σ√T)"
```

**Answer:** 1.03

> $\text{DD} = \frac{\ln(120/100) + (0.08 - 0.03125)\times 2}{0.25\times\sqrt{2}} = \frac{0.1823 + 0.0975}{0.3536} \approx 1.03$

## Q2 · Which correction term appears in the GBM exact solution, and why?

- [x] $-\sigma^2/2$, arising from Itô's lemma applied to $\ln V_t$
- [ ] $+\sigma^2/2$, arising from Jensen's inequality
- [ ] $-\sigma^2$, arising from the quadratic variation of $W_t$
- [ ] No correction is needed

> When applying Itô's lemma to $f(V_t) = \ln V_t$, the second-order term $\frac{1}{2}\sigma^2 V_t^2 \cdot (-1/V_t^2) = -\sigma^2/2$ appears, shifting the drift downward.

## Q3 · In the Merton model, the value of risky debt $B_0$ satisfies:

- [x] $B_0 = D e^{-rT} - \text{value of a put on } V$ with strike $D$
- [ ] $B_0 = D e^{-rT} + \text{value of a put on } V$ with strike $D$
- [ ] $B_0 = V_0 - E_0$
- [ ] Both A and C are correct

```quiz
type: multi
```

- [x] $B_0 = D e^{-rT} - P_0$ where $P_0$ is a put on $V$ with strike $D$
- [x] $B_0 = V_0 - E_0$ (balance sheet identity)
- [ ] $B_0 = D e^{-rT} + P_0$
- [ ] $B_0 = E_0 - V_0$

> By put-call parity on the firm's balance sheet: $V_0 = E_0 + B_0$, so $B_0 = V_0 - E_0$. Equivalently, lenders hold a risk-free bond minus a put sold to shareholders.

## Q4 · True or false: the Merton model assumes that default can only occur at debt maturity $T$.

- [x] True
- [ ] False

> This is the defining and most-criticised assumption of the basic Merton model. Extensions (Black-Cox first-passage model) allow early default when $V_t$ hits a barrier before $T$.

## Q5 · You observe equity value $E_0 = 40$ and equity volatility $\sigma_E = 0.50$. Using the Merton relationships, what additional equation links $\sigma_E$ to $\sigma_V$?

- [x] $\sigma_E E_0 = N(d_1)\, \sigma_V V_0$
- [ ] $\sigma_E = \sigma_V \cdot N(d_2)$
- [ ] $\sigma_E E_0 = \sigma_V V_0$
- [ ] $\sigma_E = \sigma_V / N(d_1)$

> From Itô's lemma applied to $E = f(V)$: $\sigma_E E_0 = \frac{\partial E}{\partial V} \sigma_V V_0 = N(d_1)\,\sigma_V V_0$. This, combined with the BS equity pricing equation, forms the two-equation system used to back out $V_0$ and $\sigma_V$.

## Q6 · Match each quantity with its formula in the Merton model.

```quiz
type: match
points: 4
```

| Quantity | Formula |
|----------|---------|
| $d_1$ | $\frac{\ln(V_0/D) + (r + \sigma^2/2)T}{\sigma\sqrt{T}}$ |
| $d_2$ | $d_1 - \sigma\sqrt{T}$ |
| Risk-neutral PD | $N(-d_2)$ |
| Distance to Default | $\frac{\ln(V_0/D) + (\mu - \sigma^2/2)T}{\sigma\sqrt{T}}$ |

## Q7 · A Monte Carlo simulation with $N = 50{,}000$ paths gives $\hat{\text{PD}} = 2\%$. What is the half-width of the $95\%$ confidence interval?

```quiz
type: open
hint: "1.96 × sqrt(p(1−p)/N)"
```

**Answer:** 0.00123 (or 0.123%)

> $1.96 \times \sqrt{0.02 \times 0.98 / 50000} = 1.96 \times \sqrt{3.92 \times 10^{-7}} \approx 1.96 \times 0.000626 \approx 0.00123$

## Q8 · Which of the following are well-known limitations of the Merton model? (select all that apply)

```quiz
type: multi
points: 3
```

- [x] Default can only occur at maturity, not before
- [x] Firm asset value and volatility are not directly observable
- [x] The model generates credit spreads that are too low for short maturities
- [ ] It cannot produce a probability of default
- [x] Debt structure is oversimplified (single zero-coupon bond)
- [ ] It requires Monte Carlo simulation to compute PD

> The Merton model has closed-form solutions for PD. Its main weaknesses are the maturity-only default, unobservable inputs, flat short-end credit spreads, and simple debt structure.

## Q9 · Place these steps in the correct order for a Merton Monte Carlo simulation.

```quiz
type: order
points: 3
```

1. Estimate or calibrate $V_0$, $\sigma_V$, $\mu$, $D$, $T$
2. Draw $N$ standard normal samples $Z^{(i)} \sim \mathcal{N}(0,1)$
3. Compute terminal firm values $V_T^{(i)} = V_0 \exp\!\left((\mu - \sigma^2/2)T + \sigma\sqrt{T}\,Z^{(i)}\right)$
4. Count defaults: $\mathbf{1}[V_T^{(i)} < D]$
5. Estimate $\hat{\text{PD}} = \frac{1}{N}\sum_i \mathbf{1}[V_T^{(i)} < D]$

## Q10 · True or false: antithetic variates improve Monte Carlo accuracy by reducing the bias of the estimator.

- [ ] True
- [x] False

> Antithetic variates reduce **variance**, not bias. They pair each path $Z$ with its mirror $-Z$, introducing negative correlation between paired estimates and lowering the overall variance of the mean.

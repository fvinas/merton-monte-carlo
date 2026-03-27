# Geometric Brownian Motion — Checkpoint

## Q1 · Which SDE describes a Geometric Brownian Motion?

- [x] $dV_t = \mu V_t \, dt + \sigma V_t \, dW_t$
- [ ] $dV_t = \mu \, dt + \sigma \, dW_t$
- [ ] $dV_t = \mu V_t \, dt + \sigma \, dW_t$
- [ ] $dV_t = \mu \, dt + \sigma V_t \, dW_t$

> In GBM, both the drift and diffusion terms are proportional to the current value $V_t$, making returns (not levels) normally distributed.

## Q2 · What is the exact solution of the GBM SDE at time $T$?

- [x] $V_T = V_0 \exp\!\left(\left(\mu - \frac{\sigma^2}{2}\right)T + \sigma W_T\right)$
- [ ] $V_T = V_0 \exp\!\left(\mu T + \sigma W_T\right)$
- [ ] $V_T = V_0 \left(\mu T + \sigma W_T\right)$
- [ ] $V_T = V_0 + \mu T + \sigma W_T$

> The $-\sigma^2/2$ correction (Itô correction) arises from Itô's lemma. Without it, the expectation of $V_T$ would not equal $V_0 e^{\mu T}$.

## Q3 · If $V_0 = 100$, $\mu = 0.05$, $\sigma = 0.20$, $T = 1$, what is $\mathbb{E}[V_T]$?

- [x] $100 \, e^{0.05} \approx 105.13$
- [ ] $100 \times 1.05 = 105$
- [ ] $100 \, e^{0.05 - 0.02} \approx 103.05$
- [ ] $100 \, e^{0.03} \approx 103.05$

> $\mathbb{E}[V_T] = V_0 e^{\mu T}$. The drift $\mu$ governs the expected growth; $\sigma$ affects variance but not the mean.

## Q4 · Under GBM, $\ln(V_T / V_0)$ follows which distribution?

```quiz
type: mcq
```

- [x] $\mathcal{N}\!\left(\left(\mu - \frac{\sigma^2}{2}\right)T,\ \sigma^2 T\right)$
- [ ] $\mathcal{N}\!\left(\mu T,\ \sigma^2 T\right)$
- [ ] $\mathcal{N}\!\left(\mu T,\ \sigma T\right)$
- [ ] A log-normal distribution

> Log-returns are normally distributed with mean $(\mu - \sigma^2/2)T$ and variance $\sigma^2 T$. The value $V_T$ itself is log-normally distributed.

## Q5 · True or false: a GBM path can become negative.

- [ ] True
- [x] False

> Because $V_t = V_0 \exp(\cdots)$ and the exponential is always positive, GBM paths are strictly positive. This makes GBM suitable for modelling firm values and asset prices.

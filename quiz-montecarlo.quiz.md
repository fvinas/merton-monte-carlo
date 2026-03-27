# Monte Carlo Simulation — Checkpoint

## Q1 · What is the Euler-Maruyama discretisation of GBM over a step $\Delta t$?

- [x] $V_{t+\Delta t} = V_t \exp\!\left(\left(\mu - \frac{\sigma^2}{2}\right)\Delta t + \sigma\sqrt{\Delta t}\, Z\right),\quad Z \sim \mathcal{N}(0,1)$
- [ ] $V_{t+\Delta t} = V_t + \mu V_t \Delta t + \sigma V_t \Delta t \cdot Z$
- [ ] $V_{t+\Delta t} = V_t \exp\!\left(\mu \Delta t + \sigma \Delta t \cdot Z\right)$
- [ ] $V_{t+\Delta t} = V_t \left(1 + \mu \Delta t + \sigma \sqrt{\Delta t}\, Z\right)$

> The exact GBM discretisation uses the log-normal step. The additive Euler scheme (second option) introduces bias; the exact scheme is preferred for GBM.

## Q2 · You simulate $N = 10{,}000$ paths and observe 430 defaults. What is your Monte Carlo estimate of PD?

- [x] $4.3\%$
- [ ] $0.043\%$
- [ ] $43\%$
- [ ] Cannot be determined without knowing $D$

> $\hat{\text{PD}} = 430 / 10{,}000 = 0.043 = 4.3\%$. Monte Carlo PD is simply the fraction of simulated paths ending below $D$.

## Q3 · Why is the exact GBM step preferred over the simple Euler step for Monte Carlo?

```quiz
type: mcq
```

- [x] It is exact regardless of step size, eliminating discretisation error
- [ ] It is faster to compute
- [ ] It produces lower variance estimates
- [ ] It automatically handles the risk-neutral drift

> The exact step $V_{t+\Delta t} = V_t \exp((\mu - \sigma^2/2)\Delta t + \sigma\sqrt{\Delta t}\,Z)$ is the true GBM increment — no approximation is involved regardless of how large $\Delta t$ is.

## Q4 · Which of the following reduce Monte Carlo estimation variance? (select all that apply)

```quiz
type: multi
```

- [x] Increasing the number of simulated paths $N$
- [x] Antithetic variates (pairing $Z$ with $-Z$)
- [x] Control variates (using the analytical Black-Scholes price as a control)
- [ ] Using a smaller time step $\Delta t$
- [x] Quasi-Monte Carlo (Sobol sequences instead of pseudo-random numbers)

> Smaller $\Delta t$ reduces discretisation error but not statistical variance. The other four are standard variance reduction techniques.

## Q5 · The $95\%$ confidence interval for a Monte Carlo PD estimate with $N$ paths is approximately:

- [x] $\hat{\text{PD}} \pm 1.96 \sqrt{\hat{\text{PD}}(1 - \hat{\text{PD}}) / N}$
- [ ] $\hat{\text{PD}} \pm 1.96 / \sqrt{N}$
- [ ] $\hat{\text{PD}} \pm \sigma / \sqrt{N}$
- [ ] $\hat{\text{PD}} \pm 1.96 \hat{\text{PD}} / N$

> Default is a Bernoulli random variable. The standard error of the sample proportion is $\sqrt{p(1-p)/N}$, giving the Wilson/normal-approximation confidence interval.

# The Merton Model — Checkpoint

## Q1 · In the Merton model, what triggers default?

- [x] The firm value $V_T$ falls below the debt face value $D$ at maturity
- [ ] The firm value $V_t$ falls below $D$ at any point before maturity
- [ ] The firm's equity price falls below zero
- [ ] The firm's credit spread exceeds a threshold

> Merton (1974) models debt as a zero-coupon bond maturing at $T$. Default only occurs at maturity — this is the key simplification of the structural approach.

## Q2 · Equity in the Merton model is equivalent to which financial instrument?

- [x] A European call option on firm assets with strike $D$
- [ ] A European put option on firm assets with strike $D$
- [ ] A forward contract on firm assets
- [ ] A zero-coupon bond

> $E_T = \max(V_T - D,\ 0)$. Shareholders receive what is left after repaying debt, which is exactly the payoff of a call option with strike $D$.

## Q3 · What is the risk-neutral probability of default in the Merton model?

```quiz
type: mcq
```

- [x] $N(-d_2)$
- [ ] $N(-d_1)$
- [ ] $N(d_2)$
- [ ] $1 - N(d_1)$

> Under the risk-neutral measure, $\text{PD}^Q = N(-d_2)$ where $d_2 = \frac{\ln(V_0/D) + (r - \sigma^2/2)T}{\sigma\sqrt{T}}$. Note: $N(-d_2) = 1 - N(d_2)$.

## Q4 · The Distance to Default (DD) measures:

- [x] How many standard deviations the log firm value is above the default threshold
- [ ] The time remaining until likely default
- [ ] The expected loss given default
- [ ] The credit spread in basis points

> $\text{DD} = \frac{\ln(V_0/D) + (\mu - \sigma^2/2)T}{\sigma\sqrt{T}}$. A higher DD means the firm is further from default — lower default probability.

## Q5 · True or false: the physical and risk-neutral default probabilities in Merton are always equal.

- [ ] True
- [x] False

> They differ because the physical measure uses the actual drift $\mu$, while the risk-neutral measure uses the risk-free rate $r$. Only if $\mu = r$ (risk-neutral world) would they coincide.

## Q6 · Which of the following are inputs to the Merton model? (select all that apply)

```quiz
type: multi
```

- [x] Current firm asset value $V_0$
- [x] Asset volatility $\sigma_V$
- [x] Debt face value $D$
- [x] Time to maturity $T$
- [x] Risk-free rate $r$
- [ ] Equity beta
- [ ] Credit rating

> The five core inputs are $V_0$, $\sigma_V$, $D$, $T$, and $r$. Equity beta and credit ratings are outputs or external assessments, not model inputs.

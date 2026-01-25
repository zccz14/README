---
"title": "Full-Spectrum Analysis: The Optimal Method for Monetizing Information"
"summary": "This paper proposes Full-Spectrum Analysis (FSA), an investment and trading strategy framework optimized based on the Kelly criterion. The article first analyzes the limitations of the traditional Kelly formula in investment applications, such as the lack of consideration for leverage and short selling, liquidation timing issues, etc. FSA then constructs a systematic trading decision model by defining the outcome space, calculating optimal leverage, and compound yield. The paper elaborates on the mathematical principles of FSA, including the calculation of expected yield and compound yield, as well as the algorithm for solving optimal leverage using Newton's method. Furthermore, it introduces historical backtesting methods (e.g., Gross Profit Margin calculation), considerations for live trading modules, and measures to handle black swan events. The core advantage of FSA lies in its ability to utilize imperfect probability information to maximize long-term returns by optimizing leverage decisions, thereby reducing the high requirement for information quality."
"tags":
  - "Full-Spectrum Analysis"
  - "Kelly Criterion"
  - "Investment Strategy"
  - "Leverage Optimization"
  - "Compound Yield"
  - "Risk Management"
  - "Algorithmic Trading"
  - "Black Swan Events"
"date": "2025-08-10"
---

# Full-Spectrum Analysis: The Optimal Method for Monetizing Information

2025-08-10

## Background

**How to maximize the growth rate of assets?**

In 1956, J.L. Kelly published "A New Interpretation of Information Rate." The paper discussed how a gambler could use known information to maximize the compound growth rate of their assets. It proposed a probabilistic method to guide betting strategies. However, applying this paper to actual investment activities presents several issues:

1.  It does not utilize borrowing leverage or short selling, confining its conclusions to a practical leverage range of 0~1.
2.  It assumes the gambler bets on a specific event symbol, receiving a payout if it occurs next, otherwise losing the entire bet. But traders can only bet long/short, and the corresponding price movement events (up/down) present different outcomes and yields across different time windows.
3.  **Liquidation Timing:** Kelly assumes the gambler cannot decide when the bet ends and liquidation occurs. However, traders can decide to exit or continue holding at any time.
4.  **Redundant Bayesian Method:** It assumes event development depends on prior and conditional probabilities. Removing the Bayesian method does not affect the derived mathematical essence. It increases complexity, making direct application of the paper's formula difficult; practical use requires simplification.

We will follow the probabilistic method from Kelly's paper to explore a practical strategy applicable to the investment and trading domain.

## Full-Spectrum Analysis: Full-Spectrum Analysis, FSA

### Outcome Space, Optimal Leverage, and Optimal Yield

Assume implementing a certain trading strategy will result in several different outcome events. Let the outcome space be $X$. $X$ is non-empty (in practical scenarios, at least 2 outcomes exist).

Make some additional assumptions:

1.  **Deterministic Yield:** Each outcome event corresponds to a definite yield. Let the yield for outcome $i$ be $R_i$.
    1.  ✅ Events with different yields should be considered different outcome events, e.g., a 1% profit and a 2% profit are different.
    2.  ❌ Grouping profit events with significantly different yields into the same outcome violates deterministic yield.
2.  **Deterministic Probability:** Let the probability of outcome $i \in X$ occurring be $P_i$, ($0 \le P_i \le 1$).
    1.  ✅ The probability of each outcome needs to be assessed deterministically.
    2.  ❌ Stating that an outcome has an "over 80% chance": 80%～ 100%, violates deterministic probability.
3.  **Mutual Exclusivity:** It is impossible for two outcomes to occur simultaneously.
    1.  ✅ Design outcome events that are completely impossible to occur simultaneously.
    2.  ❌ If outcome events have intersections, e.g., "holding for 20 minutes" and "falling 20%" might occur together, violating mutual exclusivity.
4.  **Completeness:** The sum of probabilities for all outcomes is 100%. No outcome exists outside the predefined set, i.e., $\sum_{i \in X} P_i = 1$.
    1.  ✅ Consider all possible outcomes that might occur.
    2.  ❌ If the outcome events only define "rise over 20%" and "fall over 20%", without defining "price change between -20% and 20%", it violates completeness.
5.  **Leverage Effect:** Yield follows the leverage effect. If using $k$ times leverage, when outcome $i$ occurs, the yield is $k \cdot R_i$, meaning the asset becomes $1 + k \cdot R_i$ times its original value.
    1.  ✅ Investment products with sufficient liquidity conform to this characteristic.
    2.  ❌ When the position size becomes too high, due to liquidity reasons, market impact costs arise, and returns no longer follow a linear relationship.

For the outcome space $X$, we can calculate its **Expected Earning Yield**:
$$E(X) = \sum_{i \in X} P_i R_i$$

The concept of expected yield is intuitive and easy to calculate. The problems with expected yield are:

1.  It leads to extreme decisions—either zero position or full position—failing to control bankruptcy risk. If the expected yield is positive, it tends to infinitely amplify leverage to seek higher returns, even if high leverage can lead to ruin.
2.  It does not consider the compounding effect. It only considers the return of a single investment, not that the market allows for repeated investments. This does not align with actual investment management.

**Compound Earning Yield** refers to the average yield per trade after repeating many independent trades:

$$C(X, k) = (\prod_{i \in X} (1 + k \cdot R_i)^{P_i}) - 1$$

**Optimal Earning Yield** is the maximum value of the compound yield across different leverage levels:

$$O(X) = \max_k C(X, k)$$

To avoid ruin (zeroing out), it must satisfy $1 + k \cdot R_i >0$ for all outcomes, leading to a basic feasible region $K$ for k:

Upper bound:
$$\max k = \max(0, \min_{R_i < 0} {-\frac{1}{R_i}})$$

Lower bound:
$$\min k = \min(0, \max_{R_i > 0} {-\frac{1}{R_i}})$$

And an always feasible solution $k = 0$,
$$K = (\min k, \max k) \cup \{ 0 \}$$

The optimal leverage is the $k$ within the feasible region $K$ that maximizes $C(X, k)$.

$$k_o = \argmax_k C(X, k)$$

At any moment, we can calculate the actual leverage in the account: $k = \frac{\text{Position Size}}{\text{Account Equity}}$, and also evaluate the optimal leverage: $k_o$.
Any trading strategy ultimately corresponds to an actual leverage. Making a deterministic trading decision is equivalent to determining the value of $k$. The optimal leverage corresponds to the ideal position.

Thus, we obtain a trading framework:

1.  Design the outcome space $X$.
2.  Continuously evaluate the probabilities $P$ of different outcomes.
3.  Calculate the actual leverage $k$ and the optimal leverage $k_O$.
4.  Control the actual leverage to approach the optimal leverage.

> **Special Points**
>
> The trivial point $C(X, 0) = 0$, meaning "no participation, no profit or loss."
> When we have no position, it is equivalent to deciding $k = 0$. For any instrument, whether holding a position or not, we are making a decision at every moment.
> Short selling corresponds to the case $k < 0$; if $|k| > 1$, it means additional leverage is required.

### Solving the Optimization Problem

Recap the problem: Given outcome space $X$, probability distribution $P_i$, yield $E_i$, feasible region $[L, R]$, find the leverage ratio $k \in [L, R]$ that maximizes the yield:

Rearranging the compound yield and taking the logarithm on both sides (preserving monotonicity), we get:

$$\ln (1 + C(X, k)) = \sum_{i \in X} P_i \ln (1 + k \cdot R_i)$$

Let

$$G(k) = \ln(1 + C(X, k)) = \sum_{i \in X} P_i \ln (1 + k R_i)$$

According to the definition of optimal leverage:

$$k_o = \argmax_k C(X, k)$$

Since $f(x) = \ln(1+x)$ is monotonically increasing:

$$k_o = \argmax_k C(X, k) = \argmax_k G(k)$$

Taking the first and second derivatives of $G(k)$:

$$G'(k) = \sum_{i \in X} \frac{P_i R_i}{1 + k R_i} $$

$$G''(k) = \sum\_{i \in X} -\frac{P_i R_i^2}{(1 + k R_i)^2} $$

Each term in the second derivative is non-positive, so $G''(k) \le 0$. If and only if $P_i R_i \equiv 0$, then $G''(k) = 0$, but in this case, $G(k) \equiv 0$, yielding no profit and having no practical significance. In other cases, $G''(k) < 0$. This means $G(k)$ is a strictly concave function, and $G'(k)$ is strictly monotonically decreasing. $G(k)$ has one and only one maximum point, and this maximum point is the global maximum.

As shown in the figure below, regardless of the number of terms, the $G(k)$ curve is concave.

![Kelly Criterion - Curve Illustration](image-1.png)

The maximum point of $G(k)$ is the zero of its first derivative:

$$G'(k) = \sum_{i \in X} \frac{P_i R_i}{1 + k R_i} = 0$$

If we rearrange this equation, it can be transformed into a polynomial equation in k.
If there are $N$ distinct values of $R_i$, the highest degree of the polynomial equation is $N-1$.
According to the Abel–Ruffini theorem, polynomials of degree five and higher have no general algebraic solution formula.

The common Kelly formula is a special case where $N=2$: Let win probability be $p$, and odds be $b$.

| Outcome | Probability | Yield |
| ------- | ----------- | ----- |
| Win     | $p$         | $b$   |
| Loss    | $1-p$       | $-1$  |

Substituting yields the equation:

$$G'(k) = \frac{pb}{1+kb} + \frac{-(1-p)}{1 -k} = 0$$

Simplifying gives

$$k = \frac{p(b+1)-1}{b}$$

Which is the Kelly formula.

For trading scenarios, there might be countless outcomes with different yields; therefore, we can only seek numerical solutions.

As a strictly concave function of a single variable, using Newton's method is the gold standard.
Starting the iteration from the fixed point $$G(0) = 0$$ is a good choice; for a strictly concave function, iteration from any starting point will converge to the same result.

#### Newton's Method Exceeding the Feasible Region

A potential issue is that the iteration point obtained by Newton's method might fall outside the problem's feasible region.
Use a minimal example to illustrate this scenario:

$$p_1 = 0.9, r_1 = 0.5, p_2 = 0.1, r_2 = -1$$

We get

$$G(k) = 0.9 \times \ln(1+0.5k) + 0.1 \times \ln(1 - k)$$

![G(k) Function Plot](image-2.png)

Calculation shows the feasible region $K = (-2, 1)$, with analytical optimal leverage $k_o = 0.7$.
However, when using Newton's method, the first iteration point is

$$G(0) - \frac{G'(0)}{G''(0)} = \frac{14}{13} > 1$$

![G'(k) and Newton's Method Demonstration](image-3.png)

The first iteration point exceeds the feasible region, going beyond the domain where the derivative is defined, making further iteration meaningless. This shows the naive Newton's method itself cannot handle cases where the iteration point leaves the feasible region.

The solution is: after calculating the iteration point using Newton's method, we need to additionally check if it lies within the feasible region. If it does, iterate to that point. If not, based on its direction, take a point between the feasible region boundary and the current point as the next iteration point.

#### Algorithm Pseudocode

Algorithm $\text{resolve}(G, L, R, \epsilon = 10^{-9}, N = 100, \alpha = 0.9)$

1.  Initialize $k \gets 0$
2.  Calculate the basic feasible region $K$ based on return rates $r_i$
3.  Clip the feasible region: $L \gets \max(L, \min K), R \gets \min(R, \max K)$
4.  Loop for a maximum of $N$ iterations:
    1.  Calculate the next point: $k' \gets k - \frac{G'(k)}{G''(k)}$
    2.  If $k'$ does not belong to $K$:
        1.  If $k' \ge R$, set $k' \gets \alpha R + (1-\alpha)k$
        2.  If $k' \le L$, set $k' \gets \alpha L + (1 - \alpha) k$
    3.  If the difference is less than the precision threshold $| k' - k | < \epsilon$, break the loop.
    4.  Update $k \gets k'$
5.  Return $k$

#### Code Implementation

From a programming perspective, a more suitable abstraction is to let each outcome in the set have two attributes: return rate r and weight w. This outcome space can be traversed, and the algorithm can be written as:

```ts
/**
 * Calculates the optimal leverage k and expected yield e based on the Kelly criterion.
 *
 * @param R - Array of return rates
 * @param W - Array of weights
 * @param lower - Minimum leverage constraint
 * @param upper - Maximum leverage constraint
 * @param eps - Convergence tolerance
 * @param max_iter - Maximum number of iterations
 * @param alpha - Convergence acceleration factor
 * @returns An object containing the optimal leverage k and expected yield e
 */
export function resolve_k(
  R: number[],
  W: number[],
  lower = -Infinity,
  upper = Infinity,
  eps = 1e-9,
  max_iter = 100,
  alpha = 0.9
) {
  const n = R.length;
  if (n !== W.length)
    throw new Error(
      "Returning and Probability vectors must have the same length"
    );

  // Calculate the basic feasible region K, ensuring 1 + k * r > 0
  let minK = NaN;
  let maxK = NaN;
  for (let i = 0; i < n; i++) {
    const r = R[i];
    if (r === 0) continue;
    const k = -1 / r; // Critical value
    if (r > 0) minK = isNaN(minK) ? k : Math.max(minK, k);
    if (r < 0) maxK = isNaN(maxK) ? k : Math.min(maxK, k);
  }
  if (isNaN(minK)) minK = 0; // If no positive R, set minK to 0
  if (isNaN(maxK)) maxK = 0; // If no negative R, set maxK to 0
  lower = Math.max(lower, minK);
  upper = Math.min(upper, maxK);

  let sum_w = 0;
  for (let i = 0; i < n; i++) {
    const w = W[i];
    if (w < 0) throw new Error(`Weight[${i}] = ${w} must be non-negative`);
    sum_w += w;
  }
  if (sum_w === 0) throw new Error("Sum of weights must be greater than zero");

  let k = 0;
  let it;
  for (it = 0; it < max_iter; it++) {
    let acc_g1 = 0;
    let acc_g2 = 0;
    for (let i = 0; i < n; i++) {
      const r = R[i];
      const w = W[i];
      acc_g1 += (w * r) / (1 + k * r);
      acc_g2 += (w * r * r) / (1 + k * r) ** 2;
    }
    const delta_k = acc_g1 / acc_g2;
    if (!(Math.abs(delta_k) > eps)) break;
    let next_k = k + delta_k;
    if (next_k <= lower) {
      next_k = lower * alpha + k * (1 - alpha);
    } else if (next_k >= upper) {
      next_k = upper * alpha + k * (1 - alpha);
    }

    k = next_k;
  }

  const lne =
    R.reduce((acc, r, i) => acc + W[i] * Math.log(1 + k * r), 0) / sum_w;
  const e = Math.exp(lne) - 1;

  return { k, e, it, sum_w, lne, upper, lower };
}
```

### Other Mathematical Properties

#### When the feasible region is unrestricted, if the expected yield is positive, the optimal yield is positive.

Proof:
Since $f(x) = \ln(1+x)$ has the same sign as $x$, the signs of $C(X, k)$ and $G(k)$ are the same.
Consider the derivative of $G(k)$ at $k = 0$: $G'(0) = \sum_{i \in X} P_i R_i$, which is precisely the expected yield $E(X)$.
For an infinitesimal $\Delta k$, by the definition of the derivative, $G(\Delta k) = G(0) + G'(0) \cdot \Delta k = E(X) \cdot \Delta k$

If $E(X) > 0$, then there exists $\Delta k > 0$ such that $G(\Delta k) > 0$, i.e., $C(X, \Delta k) >0$. And the optimal yield
$$O(X) = \max C(X, k) \ge C(X, \Delta k) > 0$$

Q.E.D.

Additionally, the following table can be proven:

| Expected Yield | Optimal Leverage | Optimal Yield |
| -------------- | ---------------- | ------------- |
| Positive       | Positive         | Positive      |
| 0              | 0                | 0             |
| Negative       | Negative         | Positive      |

## Historical Backtesting Method for FSA

### Gross Profit Margin: Gross Profit Margin, GPM

According to the trading framework described, at each moment, we can calculate the actual leverage $k$ and the optimal leverage $k_O$, and control the actual leverage to converge to the optimal leverage.

Here, we **default to using simple interest** for backtesting because the compound interest model affects subsequent cost estimation. The compound interest model can cause very large changes in turnover, leading to additional market impact costs when reaching strategy capacity. This causes the actual executable volume or cost to deviate severely from the model values, distorting the backtest. In real scenarios, compounding is often controlled subjectively, i.e., manually adjusting initial equity or trading multiples to create a "partial compounding" scheme between "simple interest" and "compound interest."

Key constraint: $K_t$ depends only on known information at times $0, 1, ..., t$; no future data is used. $K_t$ affects the position size at time $t+1$.

To perform historical backtesting, we first need to know the price $P_t$ and the corresponding planned net position size $V_t$.

From a micro perspective, at time $t$, knowing the price $P_t$ also means knowing the equity $E_t$ and the net position size $H_t$.

First, consider the boundary condition: $V_0 = 0, E_0 = 0$.

After a negligible analysis time, we obtain the planned net position size $V_t$.

The trading volume required for rebalancing is $V_t - H_t$

Thereafter, until time $t + 1$, orders are placed immediately.

Under the assumption of sufficient liquidity, execution occurs completely at price $P_{t+1}$, resulting in $H_{t+1} = V_t$.
Let $c$ be the cost based on turnover; then the cost is $c \cdot |V_t - H_t| \cdot P_{t+1}$.
Furthermore, the net position $H_t$ is affected by price changes, generating profit/loss $H_t (P_{t+1} - P_t)$.

In summary, at time $t + 1$:

$$H_{t+1} = V_t$$
$$E_{t+1} = E_t + H_t (P_{t+1} - P_t) - c \cdot |H_{t+1} - H_t| \cdot P_{t+1}$$

In simple interest mode, both position size and transaction costs are proportional to the initial equity.

The total profit and loss (PnL) from price changes after holding a position:

$$\text{PnL} = \sum_t H_t \cdot (P_{t + 1} - P_t)$$

Total turnover:

$$\text{Turnover} = \sum_t |H_{t+1} - H_t| \cdot P_{t+1}$$

We can estimate the maximum transaction cost the model can bear to break even, i.e., the Gross Profit Margin (GPM):

$$\text{GPM}  = \frac{\text{PnL}}{\text{Turnover}}$$

Subsequently, in live trading, an actual cost rate lower than this GPM is profitable. This GPM also implies the model's capacity. A higher GPM means larger trading slippage can be tolerated in live trading, allowing for higher actual trading volume.

The model's task is to maximize GPM, while the trading module's task is to achieve profitability under this GPM constraint. Specifically, in subsequent live trading, the trading module's task is to complete as much turnover as possible without exceeding the GPM. The trading module cannot avoid the exchange's inherent fee rate, which may be influenced by various factors such as VIP status, rebates, Maker/Taker roles, etc., affecting the actual fee rate. If the model's GPM is greater than an exchange's fee rate, it can be considered that the model is unlikely to be profitable on that exchange and needs improvement. If the trading module deems the current task unachievable, it can choose to reduce turnover or not trade, maintaining a zero position.

The final profit can be considered as: Turnover \* (GPM - Actual Cost Rate). Increasing the initial equity will increase turnover, causing the actual cost rate to continuously approach the GPM until it becomes unprofitable. However, from the formula, there should be an optimization problem for profit. The initial equity corresponding to this maximum profit is the trading model's capacity. A detailed evaluation requires further research into the relationship between turnover and cost.

### Holding Resolution

In actual trading, products have a minimum tradeable volume step (volume_step). Positions can only be traded in integer multiples of this step.
Therefore, given a floating-point target position, we cannot 100% track this target position. We need to round this target position to a tradeable position size.

Holding Resolution is a positive integer.

If we apply the simple interest method to the optimal leverage $k_O$ and then map it via resolution, we get the target position $V$. Substituting this into the backtesting framework allows us to calculate the MER.

-   If Holding Resolution = 1, it means the strategy only trades the base position. The target position values are -1, 0, 1.
-   If Holding Resolution = 2, it means the strategy starts to require position splitting. The target position values are -2, -1, 0, 1, 2.
-   If Holding Resolution = ∞, it means the strategy can adjust the position with any precision. But this does not conform to reality.

Lower holding resolution means less base capital is needed for subsequent small-scale live trading, but the information utilization becomes coarser.

Theoretically, holding resolution affects turnover. Lower resolution leads to lower turnover (situations that originally required rebalancing become unnecessary). The impact of holding resolution on returns is not yet clear. Empirically, if the MER is sufficiently high and insensitive to resolution, it suggests readiness for live trading.

## Live Trading Module

The live trading module needs to achieve profitability under the MER constraint.

However, the constraints for opening and closing positions are not the same. When opening a position, it can tolerate incomplete execution of the target turnover, but when closing a position, it cannot tolerate this. Therefore, closing positions has stricter constraints; in the worst case, market orders must be used, resulting in higher fees and slippage.

Assuming the market order cost rate is $c$, then when opening a position, the cost rate should be at most $2 \times \text{MER} - c$ to be safe.

For example, if MER = 0.02% and market order cost is 0.03%, then the opening cost should be at most 0.01%.

## Regarding the Outcome Space

### Measures Against Black Swan Events

A black swan event is an event that is considered extremely improbable but actually occurs.

1.  It is impossible to estimate the probability of a black swan event through any model; any estimation is futile, as its probability is unknowable.
2.  Once a black swan event occurs, it causes enormous losses.
    Therefore, designing any outcome space must defend against black swan events.
3.  When a black swan occurs, it inevitably results in a -100% yield, i.e., a total loss.
4.  A black swan cannot be fitted by any probability distribution using known samples.
    Therefore, fabricating a pseudo-probability for black swan events is necessary.

Assume we have assigned probabilities $P_i$ to our defined outcome space $X$ from existing samples.

We need to manually add two symmetric black swan events: $P_b^- = 0.0013, r_b = -1, P_b^+ = 0.0013, r_b = 1$. Here, 0.0013 is the probability outside $3\sigma$ in a normal distribution, approximately 1 in 770 samples. Assigning a higher probability to black swan events makes the strategy more conservative.

Designing symmetric black swan events is to avoid affecting the expected yield, preventing a change in the sign of the optimal leverage, and avoiding situations where a zero leverage scenario incorrectly signals a need to short.

The original probabilities need to be scaled down by a factor of $1 - P_b^- - P_b^+ = 0.9974$ to ensure the completeness of the new outcome space.

Adding black swan events shrinks the feasible region, strictly limiting the optimal leverage ratio to $(-1, 1)$. Short selling is still possible, but additional leverage cannot be applied. Incorporating black swan events effectively prevents leverage abuse.

```ts
export function withBlackSwan(R: number[], W: number[], Pb = 0.0013) {
  const sum_w = W.reduce((acc, cur) => acc + cur, 0);
  const w_b = (Pb * sum_w) / (1 - 2 * Pb);
  return {
    R: R.concat([1, -1]),
    W: W.concat([w_b, w_b]),
  };
}
```

## Summary

For a given outcome space $X$, the $R_i$ are determined. As long as we estimate the probability distribution $P_i$ within the outcome space, we can obtain a deterministic optimal leverage $k_O$. If you believe the trading system should be consistent, their probabilities should be repeatable. In this context, Full-Spectrum Analysis losslessly utilizes imperfect information; therefore, there is no reason not to strictly follow this optimal leverage.

Some trading systems aim to find the outcome with the highest probability of occurrence and formulate trading plans based on that outcome. This is a maximum likelihood method. The risk of this approach is that if the likelihood function is relatively flat, choosing any single interpretation is insufficiently accurate. Such a strategy may appear effective at times and ineffective at others. Full-Spectrum Analysis does not need to follow the highest probability outcome; it can calculate returns under different outcomes and select the optimal position. It can capture subtle information and make optimal decisions. Thus, Full-Spectrum Analysis significantly lowers the quality threshold for monetizing information.

As for how to design the outcome space and estimate the probability distribution, that pertains to the content of the information itself that needs to be monetized. More on that next time.
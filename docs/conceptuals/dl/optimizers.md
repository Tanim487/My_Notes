# Optimizers in Deep Learning

An **optimizer** updates the weights of a neural network to minimize the loss function.

---

## Gradient Descent

The foundational optimizer. The weight update rule is:

$$W_n = W_0 - \eta \cdot \frac{dL}{dW}$$

| Variant | Description |
|---------|-------------|
| **Batch Gradient Descent** | Uses entire dataset per update |
| **Stochastic Gradient Descent (SGD)** | Uses one sample per update |
| **Mini-Batch Gradient Descent** | Uses a small batch per update |

### Challenges with Basic Gradient Descent

!!! warning "Challenge 1 — Learning Rate is Hard to Set"
    - Too high → overshoots the minimum
    - Too low → very slow convergence
    - **Workaround:** Learning Rate Scheduling — but you have to predefine the schedule before training, so it may not work well for every dataset.

!!! warning "Challenge 2 — Same Learning Rate for All Directions"
    The learning rate should be **flexible per direction** — some directions need to move fast, others slow.

!!! warning "Challenge 3 — Local Minima"
    The optimizer can get stuck in a local minimum instead of the global one.

!!! danger "Challenge 4 — Saddle Point ⭐ Biggest Problem"
    A saddle point has zero gradient but is not a minimum. Most basic optimizers get stuck here.

---

## Exponential Weighted Moving Average (EWMA)

A technique to find the **trend in time-series data**. Used in time series, finance, and signal processing — and now in deep learning to build better optimizers.

$$V_t = \beta \cdot V_{t-1} + (1 - \beta) \cdot \theta_t$$

Most optimum value: **β = 0.9**

```python
# In code (using pandas)
x1 = df['meantemp'].ewm(alpha=0.9).mean()   # 0 < alpha < 1
df['ewma'] = x1
df
```

---

## Overview of Optimizers

| Optimizer | Best For | Key Advantage |
|-----------|----------|---------------|
| **Momentum** | High curvature, noisy gradients | Faster than vanilla GD |
| **NAG** | Similar to Momentum | Looks ahead before updating |
| **Adagrad** | Sparse data / linear regression | Adaptive learning rate |
| **RMSProp** | Neural networks | Fixes Adagrad's stalling problem |
| **Adam** | ANN, CNN, RNN | Combines Momentum + RMSProp |

---

## Momentum

Momentum uses EWMA on gradients to build up speed in consistent directions and dampen oscillations.

Performs better in 3 scenarios: **high curvature**, **consistent gradients**, **noisy gradients (local minima)**

$$V_t = \beta \cdot V_{t-1} + \eta \cdot \nabla W_t$$

$$W_{t+1} = W_t - V_t$$

When β = 0, this reduces to standard SGD:

$$W_{t+1} = W_t - \eta \cdot \nabla W_t$$

!!! success "Pros"
    Faster convergence than vanilla batch gradient descent.

!!! failure "Cons"
    Can build too much momentum — causes bouncing around the minima, taking extra time to settle.

---

## NAG (Nesterov Accelerated Gradient)

NAG is a smarter version of Momentum — it **looks ahead** before computing the gradient, so it slows down earlier near the minima.

**Step 1** — Compute look-ahead position:

$$W_{la} = W_t - \beta \cdot V_{t-1}$$

**Step 2** — Compute velocity using look-ahead gradient:

$$V_t = \beta \cdot V_{t-1} + \eta \cdot \nabla W_{la}$$

**Step 3** — Update weights:

$$W_{t+1} = W_t - V_t$$

!!! failure "Disadvantage"
    Oscillation dampening — can still oscillate near minima.

### Momentum & NAG in Keras

```python
# Vanilla SGD
tf.keras.optimizers.SGD(
    learning_rate=0.01, momentum=0.0, nesterov=False
)

# With Momentum
tf.keras.optimizers.SGD(
    learning_rate=0.01, momentum=0.9, nesterov=False
)

# With NAG
tf.keras.optimizers.SGD(
    learning_rate=0.01, momentum=0.9, nesterov=True
)
```

---

## Adagrad (Adaptive Gradient)

Adagrad adapts the learning rate **per parameter** — parameters with large gradients get smaller learning rates and vice versa.

$$V_t = V_{t-1} + (\nabla W_t)^2$$

$$W_{t+1} = W_t - \frac{\eta}{\sqrt{V_t + \epsilon}} \cdot \nabla W_t$$

!!! failure "Critical Disadvantage"
    $V_t$ keeps growing → the learning rate shrinks to near zero → **training stalls before reaching the minima**.
    This is why Adagrad is **not used in neural networks**.

✅ Suitable for: **Linear Regression** (sparse data)

❌ Not suitable for: **Neural Networks**

---

## RMSProp (Root Mean Square Propagation)

RMSProp fixes Adagrad's stalling problem by using EWMA on the squared gradients instead of accumulating them infinitely.

$$V_t = \beta \cdot V_{t-1} + (1 - \beta)(\nabla W_t)^2 \qquad \beta = 0.95$$

$$W_{t+1} = W_t - \frac{\eta}{\sqrt{V_t + \epsilon}} \cdot \nabla W_t$$

!!! success "No significant disadvantages"
    RMSProp is one of the most powerful optimizers and works well for both linear and non-linear models.

✅ Suitable for: **Linear Regression AND Neural Networks**

---

## Adam (Adaptive Moment Estimation)

Adam combines **Momentum** (1st moment) and **RMSProp** (2nd moment) — the best of both worlds.
Most powerful optimizer for ANN, CNN, and RNN.

**Main update rule:**

$$W_{t+1} = W_t - \frac{\eta}{\sqrt{\hat{V}_t + \epsilon}} \cdot \hat{m}_t$$

**Momentum term** (1st moment):

$$m_t = \beta_1 \cdot m_{t-1} + (1 - \beta_1) \cdot \nabla W_t$$

**RMSProp term** (2nd moment):

$$V_t = \beta_2 \cdot V_{t-1} + (1 - \beta_2) \cdot (\nabla W_t)^2$$

Default values (Keras): **β₁ = 0.9**, **β₂ = 0.99**, **η = 0.01 or 0.1**

### Bias Correction

Early in training, $m_t$ and $V_t$ are biased towards zero. Bias correction fixes this:

$$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}$$

$$\hat{V}_t = \frac{V_t}{1 - \beta_2^t}$$

Here **t** is the current epoch number.

### Adam in Keras

```python
tf.keras.optimizers.Adam(
    learning_rate=0.001,
    beta_1=0.9,
    beta_2=0.999,
    epsilon=1e-07
)
```

---

## Summary: Which Optimizer to Use?

```
Simple problem / baseline     →  SGD
Need faster convergence       →  Momentum or NAG
Sparse features               →  Adagrad
RNNs / general deep learning  →  RMSProp
ANN / CNN / RNN (default)     →  Adam  ⭐
```
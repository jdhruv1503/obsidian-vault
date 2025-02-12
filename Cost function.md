
# Base Reward Function

The total reward $R$ is composed of several components:

$R = R_{metrics} + R_{balance} + R_{discretization}$

## Core Metrics Component

The basic metrics reward is:

$R_{metrics} = -\sum_{i} w_i \cdot \text{norm}(m_i)$

where:
- $w_i$ are the weights for each metric
- $\text{norm}(m_i)$ are the normalized metrics:
  - $\text{norm}(DP) = \frac{DP}{DP_{scale}}$
  - $\text{norm}(Delay) = \frac{Delay}{Delay_{scale}}$
  - $\text{norm}(PDP) = \frac{DP \cdot Delay}{PDP_{scale}}$
  - $\text{norm}(V_{offset}) = \frac{V_{offset}}{V_{offset\_scale}}$

## Balance Penalty Component

The balance penalty prevents one metric from dominating:

$R_{balance} = -w_{balance} \cdot \text{Var}(\text{norm}(m_i))$

where:

$\text{Var}(\text{norm}(m_i)) = \frac{1}{N}\sum_{i=1}^N (\text{norm}(m_i) - \mu)^2$

$\mu = \frac{1}{N}\sum_{i=1}^N \text{norm}(m_i)$

## Discretization Component

For transistor size discretization:

$R_{discretization} = -w_{disc} \sum_{j} \min(\text{dist}(s_j), 1.0)$

where:
- $s_j$ are the transistor sizes
- $\text{dist}(s_j) = |s_j - \text{round}(\frac{s_j}{f_{disc}}) \cdot f_{disc}|$
- $f_{disc}$ is the discretization factor (e.g., 5nm)

## Optional Components

You can add additional terms such as:

1. Soft constraints using exponential penalties:
   $R_{constraint} = -w_c \cdot e^{k(x-x_{limit})}$

2. Hard thresholds using step functions:
   $R_{threshold} = -w_t \cdot H(x-x_{limit})$

3. Smoothness penalty for adjacent transistor sizes:
   $R_{smooth} = -w_s \sum_{j=1}^{N-1} (s_{j+1} - s_j)^2$

4. Area penalty for total transistor size:
   $R_{area} = -w_a \sum_{j} s_j$

## Complete Formulation

The full reward function with all components is:

$R_{total} = -\sum_{i} w_i \cdot \text{norm}(m_i) - w_{balance} \cdot \text{Var}(\text{norm}(m_i)) - w_{disc} \sum_{j} \min(\text{dist}(s_j), 1.0) + R_{additional}$

where $R_{additional}$ can include any combination of the optional components above.
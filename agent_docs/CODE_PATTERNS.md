2. code_patterns.md
Markdown
# code_patterns.md: Engineering Standards for Topological Alpha

This document establishes the coding standards, design patterns, and performance guidelines for the **Topological Alpha** project. Given the project's reliance on high-performance math and machine learning, strict adherence to these patterns is required for maintainability and execution speed.

## 1. Core Principles

* **Vectorization First:** Avoid explicit Python `for` loops for numerical data. Use `NumPy`, `Polars`, or `PyTorch` tensors.
* **Type Safety:** All Python code must use type hints (`typing` module) to facilitate AI debugging and static analysis.
* **Immutability in Strategy:** Dataframes representing historical prices should be treated as immutable once loaded to prevent accidental data leakage.
* **Mathematical Transparency:** Code should contain comments referencing the specific LaTeX equations from the PRD/Tech Design.

## 2. Performance Patterns

### 2.1. Numba JIT for Information Theory
Transfer Entropy and PIN likelihood estimations are computationally expensive. Use `@njit` for these bottlenecks.

```python
from numba import njit
import numpy as np

@njit
def fast_entropy_sum(p_joint, p_marginal):
    """Optimized summation for Transfer Entropy calculation."""
    result = 0.0
    for i in range(p_joint.shape[0]):
        if p_joint[i] > 0 and p_marginal[i] > 0:
            result += p_joint[i] * np.log2(p_joint[i] / p_marginal[i])
    return result
2.2. Polars for Pipeline Efficiency
Use Polars for the initial data cleaning and feature engineering before passing data to GUDHI or PyTorch.

Python
import polars as pl

def compute_log_returns(df: pl.DataFrame) -> pl.DataFrame:
    return df.with_columns([
        (pl.col("close").log().diff()).alias("log_return")
    ]).drop_nulls()
3. Machine Learning Patterns (RL)
3.1. The Reward Wrapper
Always encapsulate the reward logic in a separate class to allow for independent unit testing of the Differential Sharpe Ratio.

Python
class DifferentialSharpeReward:
    def __init__(self, kappa: float = 0.1):
        self.kappa = kappa
        self.eta = 0.0
        self.sigma2 = 1.0

    def update(self, r_t: float) -> float:
        # Implementation of recursive moment updates
        # Returns the reward signal for the PPO agent
        ...
3.2. State Normalization
Market data varies in scale. All inputs to the Meta-Learner must be Z-score normalized using a rolling window to prevent gradient explosion.

4. Architecture Patterns
4.1. Observer Pattern for Regime Changes
The TDA_Governor acts as a Subject. The AlphaEngine and MetaLearner are Observers that scale their activity based on the topological contraction signal.

4.2. Strategy Factory
Use a Factory pattern to initialize different "Alpha Snipers" (OU, Momentum, Mean Reversion) based on the regime detected.

Python
class StrategyFactory:
    @staticmethod
    def get_strategy(regime: str):
        if regime == "CONTRACTION":
            return DefensiveStrategy()
        return AggressiveOUStrategy()
5. Error Handling & Safety
5.1. The "Anti-Explosion" Guard
Every execution service must include a decorator that checks for NaN or Inf values in tensors before sending orders to the broker.

Python
def validate_tensor(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        if np.isnan(result).any():
            raise ValueError("NaN detected in strategy output")
        return result
    return wrapper
5.2. Data Leakage Prevention
Ensure that all rolling window calculations use closed='left' or equivalent settings to ensure the current timestamp's data isn't used to predict its own outcome.

6. Documentation Standards
Docstrings: Use Google Style docstrings.

Math: Standalone math logic must include a link to the corresponding section in tech_design.md.

Logging: Use structured logging (JSON) via loguru or structlog for easy ingestion into Grafana.
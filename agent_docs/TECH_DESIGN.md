### 3. tech_design.md
```markdown
# Technical Design Document: Topological Alpha MVP

## Executive Summary
**System:** Topological Alpha (Hedge Fund Engine)
**Version:** MVP 1.0
**Architecture Pattern:** Event-Driven Microservices / Hierarchical Ensemble

## Architecture Overview

### High-Level Architecture
1. **Data Ingest Layer:** TimescaleDB (PostgreSQL) for time-series persistence.
2. **Compute Layer:**
    * **TDA Governor:** Vietoris-Rips filtration & persistence landscapes.
    * **Intelligence Layer:** Transfer Entropy causal graphs.
    * **Alpha Engine:** OU mean-reversion & HJB solvers.
3. **Meta-Learner:** PPO RL Agent for portfolio allocation.
4. **Execution:** Broker API (Alpaca/IBKR) integration.

## Tech Stack
* **Runtime:** Python 3.13
* **Math/TDA:** `GUDHI`, `NumPy`, `SciPy`
* **ML/RL:** `PyTorch`, `Stable Baselines3`
* **Data:** `Polars`, `TimescaleDB`, `Redis`
* **Infrastructure:** `Docker`, `Kubernetes`

## Component Design

### 1. TDA Governor (Layer 1)
* **File:** `src/engine/governor/tda_manager.py`
* **Responsibility:** Construct Vietoris-Rips complexes and calculate Betti-1 persistence. Triggers "Risk-Off" state if homology contracts significantly.

### 2. Intelligence Layer (Layer 2)
* **File:** `src/engine/intelligence/causal_graph.py`
* **Responsibility:** Compute Transfer Entropy (TE) between assets to identify market leaders vs. followers.

### 3. Alpha Engine (Layer 3)
* **File:** `src/engine/alpha/ou_sniper.py`
* **Responsibility:** Execute Ornstein-Uhlenbeck (OU) mean-reversion trades on cointegrated pairs.

### 4. Meta-Learner (Layer 4)
* **File:** `src/engine/meta/ppo_agent.py`
* **Responsibility:** Dynamic asset allocation using an RL agent optimized via the Differential Sharpe Ratio.

## Performance Optimization
* **Vectorization:** Use Polars for multi-threaded data processing.
* **Numba:** JIT compile Information Theory loops.
* **Redis:** Cache persistence landscapes for O(1) retrieval by the RL agent.

## Testing Strategy
* **Mathematical Unit Tests:** Validate TDA Betti-1 calculations against known point clouds (e.g., Torus, Circle).
* **Backtest Guardrails:** Use Walk-Forward Optimization and Monte Carlo noise injection.
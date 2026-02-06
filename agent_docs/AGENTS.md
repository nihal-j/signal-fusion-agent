1. AGENTS.md
Markdown
# AGENTS.md - Master Plan for Topological Alpha

## 🎯 Project Overview

**App:** Topological Alpha
**Goal:** A hierarchical quantitative trading engine integrating Topological Data Analysis (TDA), Information Theory, and Reinforcement Learning (RL) for robust alpha generation.
**Stack:** Python 3.13, PyTorch (PPO), GUDHI (TDA), TimescaleDB, Redis, Polars.
**Current Phase:** Phase 1 - Foundation & TDA Governor

## 🧠 How I Should Think

1.  **Understand Intent First**: Recognize that this is a high-stakes financial application; accuracy and risk management take precedence over speed.
2.  **Ask If Unsure**: If the mathematical parameters (e.g., Ornstein-Uhlenbeck mean-reversion thresholds or Vietoris-Rips epsilon values) are unclear, ask for clarification before implementing.
3.  **Plan Before Coding**: For complex modules like Persistent Homology or Transfer Entropy, outline the mathematical logic and vectorization strategy before writing a single line of Python.
4.  **Test After Changes**: Rigorously backtest every module. A "working" code snippet is useless if it introduces look-ahead bias or fails to handle non-stationary data.
5.  **Explain Trade-offs**: Always mention the computational cost vs. accuracy trade-offs, especially when choosing between `GUDHI` (simplicial complexes) and simpler statistical approximations.

## 📁 Context Files

Refer to these for details (load only when needed):

* `agent_docs/tech_design.md`: Full architecture, math specs, and DB schema.
* `agent_docs/product_requirements.md`: The high-level vision and "Why it Wins" rationale.
* `agent_docs/rl_reward_specs.md`: Detailed breakdown of the Differential Sharpe Ratio and PPO logic.

## 🔄 Current State (Update This!)

**Last Updated:** February 6, 2026
**Working On:** Initialization of the TDA Governor and Vietoris-Rips complex construction.
**Recently Completed:** System architecture design and tech stack finalization.
**Blocked By:** None.

## 🚀 Roadmap

### Phase 1: Foundation & TDA Governor

* [ ] Initialize Python project structure with Poetry/Conda.
* [ ] Implement `TDA_Governor` module (Betti-1 persistence calculation).
* [ ] Connect TimescaleDB and load historical OHLCV data for backtesting.
* [ ] Create "Regime Shift" triggers based on topological contraction.

### Phase 2: Intelligence & Alpha Engine

* [ ] Implement `CausalGraph` (Transfer Entropy) between asset pairs.
* [ ] Setup `PIN` (Probability of Informed Trading) likelihood estimation.
* [ ] Build `OU_Sniper` for cointegrated mean-reversion trading.

### Phase 3: Meta-Learner (RL)

* [ ] Build Gymnasium environment for the meta-allocator.
* [ ] Implement PPO agent with Differential Sharpe reward function.
* [ ] Execute walk-forward optimization and adversarial noise testing.

## ⚠️ What NOT To Do

* **Do NOT** introduce look-ahead bias (using future data to calculate current TDA persistence).
* **Do NOT** ignore transaction costs or slippage in reward function calculations.
* **Do NOT** modify the `TDA_Governor` logic without validating it against historical crash regimes (e.g., 2008, 2020).
* **Do NOT** hard-code stock symbols; use a dynamic universe filter.
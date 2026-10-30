# Monte-Carlo Pricing & Delta Hedging of Exotic Options in C++

Monte-Carlo engine that prices path-dependent options in a multidimensional Black–Scholes model,
computes their **deltas**, and **backtests a discrete delta-hedging strategy** on simulated market paths.

> Co-authored with **Tariq Sereir** ([@tsereir](https://github.com/tsereir)) – course project, Grenoble INP – Ensimag (2023–2024).

## What it does

- **Pricing at t = 0** (`price0`): Monte-Carlo price and deltas, each with the standard deviation of its estimator.
- **Pricing at t > 0**: conditional expectation given the observed past, by re-simulating only the future
  part of the path (Black–Scholes multiplicative property).
- **Deltas** by central finite differences on the simulated paths (common random numbers, so the
  estimator variance stays low).
- **Hedging** (`hedge`): rebalances a replicating portfolio (risky assets + risk-free account) along a
  market path and reports the **final hedging error (P&L)**.

## Supported payoffs

| Product | Description |
|---|---|
| Asian | Call on the discrete arithmetic average of a basket |
| Basket | Call on a weighted sum of assets |
| Performance | 1 + sum of the positive period-over-period returns of a weighted basket |

## Code structure

```
src/
  BlackScholesModel.*   multi-asset path simulation, conditional simulation, spot shifts for deltas
  Option.hpp            abstract payoff interface
  AsianOption.cpp, BasketOption.cpp, PerformanceOption.cpp
  MonteCarlo.*          price, deltas and their standard deviations
  price0.cpp            CLI: price and deltas at t = 0
  hedge.cpp             CLI: delta-hedging backtest on a market file
  PricingResults.*, HedgingResults.*   JSON output
```

## Build and run

Dependencies: CMake ≥ 3.11, a C++17 compiler, [PNL](https://github.com/pnlnum/pnl), nlohmann/json (fetched automatically if missing).

```bash
mkdir build && cd build
cmake .. -DCMAKE_PREFIX_PATH=/path/to/pnl/build -DCMAKE_BUILD_TYPE=Release
make
./price0 <params.json>
./hedge <market_file> <params.json>
```

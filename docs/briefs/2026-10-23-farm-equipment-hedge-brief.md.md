```
type: brief
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-23
status: committed
hypothesis: "I expect the forward contract to provide the optimal risk-adjusted
outcome for our 91-day receivable, because the upfront put option premium of
$0.0147 per EUR will unnecessarily erode our realized cash margin on a short 3-
month horizon."
```

```
# Executive Brief: Foreign Exchange Risk Mitigation Plan
```

```
## Executive Summary & Exposure Definition
```

```
Our U.S. combine and tractor manufacturing operations have invoiced an Irish
farm cooperative for **EUR 7,700,000**, payable in exactly **91 days (3
months)**. While this invoice amount is fixed in euros, our corporate budget,
overhead obligations, and financial reporting are denominated in U.S. Dollars
(USD). This creates unhedged foreign exchange (FX) exposure.
```

```
To contextualize the risk:
```

```
* **Indicative Forward Rate:** EURUSD 1.1653 $\rightarrow$ Yields **$8,972,810**
at settlement.
```

```
* If the euro depreciates by just 5% prior to settlement (e.g., dropping toward
EURUSD = 1.1070), our cash proceeds drop by roughly **$450,000**, directly
impacting operating cash flow without any offsetting decrease in our domestic
manufacturing cost structure.
```

# `## Evaluation of Risk Mitigation Instruments` 

```
To safeguard our cash inflows against adverse currency movements, we evaluate
three primary hedging families:
```

`1. **Forward / Futures Contracts**` 

```
   * **Mechanics:** Lock in the indicative forward rate of EURUSD 1.1653 today
to sell EUR 7.7M on the 91-day maturity date.
```

```
   * **Pro:** Complete cash flow certainty; perfectly locks in USD inflows for
our budgeting cycle.
```

```
   * **Con:** Forfeits all upside potential if EURUSD strengthens above 1.1653
prior to settlement; requires credit line or margin compliance.
```

`2. **Money-Market Hedge**` 

```
   * **Mechanics:** Borrow the present value of EUR today, convert immediately
to USD at spot, and invest the USD proceeds locally. The incoming EUR 7.7M
receivable pays off the EUR loan at maturity.
```

```
   * **Pro:** Delivers identical cash flow certainty to a forward contract;
highly useful if forward pricing is less efficient than borrowing spreads.
   * **Con:** Utilizes corporate borrowing capacity and involves multi-step
administrative execution.
```

`3. **Options (Put Floor)**` 

```
   * **Mechanics:** Buy a EUR put option at a spot strike, paying an upfront
premium of **$0.0147 per EUR** (totaling $113,190 on EUR 7.7M).
```

```
   * **Pro:** Establishes a firm price floor against downside depreciation while
preserving full participation in dollar gains if the euro strengthens.
   * **Con:** Requires an immediate, non-refundable cash outflow for the
premium, acting as a direct drag on net returns whether the hedge is ultimately
exercised or expires worthless.
```

```
## Key Assumptions, Hypothesis, and Falsification
### Working Assumptions
```

```
* The 91-day settlement date is fixed, and the Irish cooperative counterparty
credit risk is minimal.
```

```
* Market interest rate differentials and spot/forward quotes remain liquid
```

# `throughout the execution window.` 

# `### Operational Hypothesis` 

```
I expect the **Forward Contract** to outperform the Put Option on a net cash-
realized basis, because paying the $0.0147/EUR option premium over a short 3-
month horizon creates unnecessary cash drag compared to locking in the firm
forward rate of 1.1653.
```

# `### Falsification Criteria` 

```
This hypothesis will be proven wrong if market implied volatility spikes
significantly or if structural momentum indicates a high probability of severe
euro appreciation, making the asymmetric upside of the put option worth more
than its upfront premium.
```

# `## Analytical Roadmap (Stages 2–5)` 

```
Before executing any hedge transaction, we will follow a rigorous 4-stage
analytical framework:
```

```
* **Stage 2 — Model Specification:** Design a standardized financial model
workbook in `capabilities/fx-hedging/spec.md` with structured pricing tabs and
validation controls.
```

```
* **Stage 3 — AI-Assisted Build & Audit:** Generate and line-by-line audit the
functional workbook (`model.xlsx`).
```

```
* **Stage 4 — Live Market Data Population:** Pull real, timestamped spot rates,
interest rate inputs, and option volatility surfaces into `data/`.
* **Stage 5 — Validation & Decision Memo:** Conduct sensitivity testing and
deliver the final execution recommendation in our decision memo
(`docs/decisions/`).
```


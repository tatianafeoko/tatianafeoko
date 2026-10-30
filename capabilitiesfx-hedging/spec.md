# Model Specification: FX Risk Management Workbook

## Section 1: Problem Statement
A U.S. farm equipment manufacturer has invoiced an Irish agricultural cooperative for **EUR 7,700,000**, payable in **91 days**. While the revenue is fixed in euros, our corporate reporting, operational expenses, and financial obligations are denominated in U.S. Dollars (USD). 

An unhedged euro depreciation over the 91-day holding period poses direct downside risk to USD cash flow proceeds. This workbook models, compares, and evaluates three hedging mechanisms—Forward Contract, Money-Market Hedge, and Option Hedge—against an unhedged baseline to support an evidence-based hedging execution decision for the CFO.

---

## Section 2: Inputs & Named-Range Contract

Every input cell in the workbook uses a standard named range. All values below represent indicative placeholders and will be replaced with live market data during Stage 4.

| Named Range | Description | Value (Placeholder) | Unit | Stage 4 Data Source |
| :--- | :--- | :--- | :--- | :--- |
| `FC_AMT` | Foreign Currency Receivable Amount | 7,700,000 | EUR | Commercial Invoice |
| `S0_in` | Spot Exchange Rate at Inception | 1.1650 | USD per EUR | Live Market Spot Quote |
| `F0_in` | Indicative Forward Exchange Rate (91-day) | 1.1653 | USD per EUR | Bank Forward Quote / CME |
| `R_USD` | USD Annual Risk-Free Borrowing/Lending Rate | 5.25% | Annual % (ACT/360) | Federal Reserve H.15 / SOFR |
| `R_FC` | EUR Annual Risk-Free Borrowing Rate | 3.65% | Annual % (ACT/360) | ECB / Euribor (3-Month) |
| `K_PUT` | Strike Price for EUR Put Option | 1.1650 | USD per EUR | Option Chain (At-the-Spot) |
| `K_CALL` | Strike Price for EUR Call Option | 1.1650 | USD per EUR | Option Chain (At-the-Spot) |
| `PREM_PUT` | Put Option Premium per Unit of FC | 0.0147 | USD per EUR | Dealer Quote / Market Data |
| `PREM_CALL` | Call Option Premium per Unit of FC | 0.0199 | USD per EUR | Dealer Quote / Market Data |
| `T_DAYS` | Days to Settlement | 91 | Days | Invoice Payment Terms |

---

## Section 3: Tab Architecture

The workbook contains seven dedicated worksheets:

1. **Cover:** Project title, scenario slug (`farm-equipment-hedge`), author name, date, model status, and version history log.
2. **Legend_Key:** Standardized color codes (e.g., blue text/yellow fill for hardcoded inputs, gray fill for formulas, red text for validation alerts).
3. **Inputs:** Central repository for the 10 global named ranges. All downstream tabs reference cells on this tab exclusively.
4. **Hedge_Calculations:** Detailed pipeline calculations for the Unhedged position, Forward Hedge, Money-Market Hedge, and Option Hedges (Put & Call).
5. **Sensitivity_Analysis:** Data table testing settlement spot rates ($S_T$) across a $\pm 5\%$ range in $1\%$ increments, paired with a visual payoff comparison chart.
6. **Validation_Checks:** Automated logic checks and parity verification tests.
7. **Notes_Assumptions:** Explicit record of day-count conventions, transaction cost simplifications, and market pricing assumptions.

---

## Section 4: Assumptions & Constraints

1. **Day-Count Basis:** All interest rate accruals use the money market convention of **ACT/360**.
2. **Transaction Costs & Fees:** Zero transaction fees, exchange fees, or bank spreads are included in baseline calculations (can be added as friction parameters in sensitivity runs).
3. **Credit Risk:** Zero counterparty default risk is assumed for the Irish farm cooperative or financial intermediaries.
4. **Option Premium Accounting:** The option premium (`PREM_PUT`) is paid up front at $t=0$ but evaluated on a nominal net proceeds basis at $t=91$ days without future-value compounding.
5. **Covered Interest Parity (CIP):** Forward rates and money-market borrowing spreads are evaluated under theoretical CIP, with market deviations highlighted in validation checks.

---

## Section 5: Calculation Flow Logic

### 1. Unhedged Position
$$\text{USD\_Proceeds\_Unhedged} = FC\_AMT \times S_T$$

### 2. Forward Hedge
$$\text{USD\_Proceeds\_Forward} = FC\_AMT \times F0\_in$$

### 3. Money-Market Hedge (3-Step Pipeline)
* **Step 1 (Borrow EUR):** Sized so the EUR $7,700,000$ receivable repays the loan principal plus accrued interest at maturity ($T$).
  $$\text{EUR\_Borrow} = \frac{FC\_AMT}{1 + \left(R\_FC \times \frac{T\_DAYS}{360}\right)}$$
* **Step 2 (Convert to USD):** Convert borrowed EUR to USD at spot ($S0\_in$).
  $$\text{USD\_Now} = \text{EUR\_Borrow} \times S0\_in$$
* **Step 3 (Invest USD):** Invest USD proceeds at the USD risk-free rate ($R\_USD$) for $T\_DAYS$.
  $$\text{USD\_Proceeds\_MM} = \text{USD\_Now} \times \left(1 + \left(R\_USD \times \frac{T\_DAYS}{360}\right)\right)$$

### 4. Put Option Hedge (Floor Protection)
$$\text{USD\_Proceeds\_Put} = FC\_AMT \times \max(S_T, K\_PUT) - (FC\_AMT \times PREM\_PUT)$$

---

## Section 6: Sensitivity & Scenario Analysis Plan

* **Settlement Range:** Test ending spot rate ($S_T$) from $-5\%$ ($1.1068$) to $+5\%$ ($1.2233$) relative to $S0\_in$ in
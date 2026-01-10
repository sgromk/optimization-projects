# Optimization Projects

This repository contains a collection of three advanced optimization projects focused on financial risk management, retail operations, and marketing strategy. These projects leverage Linear Programming (LP), Mixed-Integer Programming (MIP), and Quadratic Programming (QP) to solve high-stakes business decision-making problems using the Gurobi Optimizer.

## Projects Overview

### 1. CVaR Portfolio Optimization
This project addresses the limitations of Mean-Variance optimization by focusing on tail risk. Using Conditional Value-at-Risk (CVaR), the model minimizes the expected loss in the worst $\alpha$% of cases, providing better protection against market crashes than standard deviation-based models.

* **Objective**: Construct a risk-optimal portfolio of 20 stocks for the 2020 trading year based on 2019 historical data.
* **Formulation**: A Linear Programming (LP) model that minimizes CVaR subject to a minimum target daily return.
* **Key Explorations**:
    * **$\beta$ Sensitivity**: Analyzed how different confidence levels (0.90 vs. 0.99) affect asset concentration and tail risk exposure.
    * **Dynamic Re-optimization**: Implemented a monthly re-balancing strategy with stability constraints (limiting weight changes to 5% per month) to adapt to the 2020 market volatility.
* **Results**: The monthly re-optimized portfolio reduced average monthly CVaR by approximately 30% compared to a static buy-and-hold strategy.

### 2. Joint Price and Inventory Optimization
This project integrates two traditionally separate business decisions—pricing and supply chain—into a single mathematical framework. It moves beyond the standard Newsvendor model by making demand a function of price.

* **Objective**: Simultaneously determine the optimal selling price and the optimal production quantity to maximize expected profit under demand uncertainty.
* **Formulation**: 
    1.  Derived a demand-price relationship using Simple Linear Regression on historical data.
    2.  Developed a Quadratic Programming (QP) model to solve the joint optimization problem.
* **Sensitivity Analysis**: Utilized Bootstrap-based sampling to test the stability of optimal price and quantity decisions against sampling variability.
* **Results**: The integrated approach yielded a 179% increase in expected profit over a traditional model that optimized quantity based on a fixed historical price.

### 3. Marketing Budget Allocation
This project tackles the complexity of multi-channel marketing where ROI is not constant but changes across different spending tiers (investment brackets).

* **Objective**: Allocate a $10M budget across multiple digital and print platforms to maximize total return while adhering to complex business rules.
* **Formulation**:
    * **Mixed-Integer Programming (MIP)**: Employed binary variables to enforce tiered investment logic (e.g., Tier 2 cannot be activated until Tier 1 is fully funded) and minimum spend requirements (e.g., if a platform is used, it must receive at least $500k).
* **Strategic Constraints**: Included platform caps, channel-specific distribution requirements, and monthly reinvestment cycles.
* **Cross-Evaluation**: Compared the MIP against a standard LP to assess performance on out-of-sample ROI data.
* **Results**: The MIP model provided a more robust allocation strategy, showing significantly lower ROI degradation (lower "gap") when applied to unseen performance data compared to the simpler LP model.

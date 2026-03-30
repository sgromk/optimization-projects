# Airline Ticket Pricing & Overbooking Optimization

Dynamic programming solution to the airline revenue management problem: given a flight 365 days out, find the joint pricing and overbooking policy that maximizes expected discounted profit.

## Problem

An airline sells coach and first-class tickets over a 365-day horizon. Each day it chooses a price for each cabin, which determines the probability a ticket sells. On departure day, passengers show up stochastically — if more coach passengers show up than seats exist, the airline must either upgrade them to available first-class seats ($50/passenger) or bump them off the flight entirely ($425/passenger). The goal is to find the pricing and overbooking policy that maximizes expected profit net of these costs, discounted at 17% annually.

Two overbooking policy types are compared:

- **Hard cap** — the airline sets a fixed maximum number of coach tickets to sell above seating capacity. Once that cap is reached, no further coach tickets are offered.
- **No-sale option** — the cap is replaced with a daily choice: the airline can also elect to withhold coach tickets on any given day, allowing it to stop selling based not just on how many tickets have been sold, but on how many days remain.

## Approach

The problem is framed as a finite-horizon Markov decision process. The state is (day, coach tickets sold, first-class tickets sold). Actions are the joint choice of coach price and first-class price. The value function is solved via backward induction over all 365 days and all reachable states.

Key modeling details:

- First-class and coach demand are independent, except that when first-class sells out, coach demand increases by 4 percentage points (spillover effect)
- Terminal costs on departure day are computed analytically using the binomial distribution over show-up outcomes
- A seasonal demand variant multiplies each day's base probabilities by (0.75 + t/730), capturing the empirical pattern of accelerating bookings as departure approaches

The optimal overbooking level under each policy type is found by solving the full DP across a range of hard caps and selecting the one that maximizes expected profit.

## Key Findings

Overbooking is unambiguously profitable. The optimal hard-cap policy (9 seats above capacity) yields roughly $300 more in expected discounted profit than a conservative 5-seat cap, with the curve peaking sharply and declining as excess overbooking costs overwhelm the marginal revenue gain.

The no-sale option provides a modest but consistent improvement over the hard-cap policy at any given cap level — approximately $144 in expected profit when both are evaluated at the same 30-seat cap. The benefit is structural: since the original decision set is a strict subset of the expanded one, the no-sale policy can never underperform.

Sensitivity analysis (±15 percentage points on each demand probability independently) shows that coach demand at the lowest price point ($300, base 65%) is by far the most influential assumption. A 15pp downward shock to that single parameter reduces expected profit by several hundred dollars, while equivalent shocks to first-class demand are nearly inconsequential. The recommendation to overbook is robust across the full range of perturbations tested.

Forward simulation (2,000 independent seasons) confirms that coach overbooking occurs on effectively every flight under both policies, with passengers bumped on roughly 72–75% of flights. Profit volatility is driven almost entirely by show-up randomness rather than by the choice of overbooking policy — the two policies produce nearly identical profit distributions in practice.

## Methods

Python · NumPy · SciPy · dynamic programming (backward induction) · Monte Carlo forward simulation · sensitivity analysis · `concurrent.futures` for parallelized DP sweeps

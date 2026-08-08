# EV Bus Depot Charging Optimiser

A cost-minimising charging scheduler for an electric bus depot, built to show how much a depot could save by shifting from naive (uncontrolled) charging to price-aware scheduling — while still fully meeting fleet readiness requirements. 

## Background

This project began as a technical exercise for a Virtual Power Plant (VPP) trading role interview, using a bus depot dataset. It has since been rebuilt as an independent personal project to demonstrate the approach end-to-end.

**Note:** the depot data used here is anonymised/synthetic and does not represent any specific company's operational data.

## The problem

Electric bus depots are large, flexible loads. Buses return to depot in a wave from around 17:30, charge overnight, and need to be ready to leave again from the early hours of the morning. A naive charging strategy — plug in and charge at maximum rate as soon as a bus arrives — concentrates demand right when it's most likely to coincide with high wholesale prices, including on system stress event days.

This project asks: how much could a depot save by scheduling that same charging demand more intelligently, without changing how much energy each bus receives?

## Working principle

- **Energy neutrality** — the total energy consumption of the optimised profile exactly matches the original (naive) profile, so fleet readiness is never compromised: every bus gets the same total charge, just at different times.
- **Price-aware optimisation** — the profile is optimised assuming perfect knowledge of in-day outturn prices (MIDP), reallocating charging within the available window to minimise total cost. This is a best-case benchmark rather than a forecast-driven, real-time model.

## Constraints

| Parameter | Value |
|---|---|
| Maximum import capacity | 1,500 kWh per settlement period |
| Charging window | 17:30 – 04:30 |
| Settlement period length | 30 minutes |

## Methodology

1. Start from a naive charging load profile (buses charge on arrival, capped at the import limit).
2. Pull in-day outturn prices (MIDP) for a chosen day — here, a real GB system stress event day, to highlight the savings available when it matters most.
3. Re-optimise the load profile within the charging window and import cap, minimising total cost subject to the energy-neutrality constraint.
4. Export the optimised schedule for comparison against the naive baseline.

## Output

The optimiser returns an optimised half-hourly charging schedule, exported in a format that drops straight into Excel — making it easy to chart the naive vs. optimised profiles side by side against the price curve.

**The MIDP outturn on December 8th 2025 ( potential system stress event was indicated by NESO) is used to compare the cost of charging a fleet of roughly 50 electric buses on 60 kW DC depot chargers — a fairly typical mid-size urban depot, showing a savings of c. £300 in a single run.**

## Tech stack

Python — pandas, numpy, cvxpy, matplotlib

## Disclaimer

This is a personal/portfolio project for demonstration purposes. It is not connected to, or endorsed by, any organisation involved in the original exercise that inspired it.

MIDP data - https://bmrs.elexon.co.uk/market-index-prices
Charging data is synthesized assuming a linear ramp as buses trickle in over the return wave, hitting a plateau at the 1500 kWh cap indicating the naive charging peak, and eventually tapering off overnight.


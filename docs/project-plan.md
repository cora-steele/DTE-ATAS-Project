# Project plan

## Candidate models

1. Adaptive Large Neighbourhood Search (ALNS)
2. CP-SAT
3. Mixed-Integer Linear Programming (MILP)
4. Greedy baseline

Use a rolling horizon to incorporate on-demand bookings.

## General design parameters

- One airport hub and approximately ten city destinations
- Number of landing sites and chargers at each location
- Fleet size
- Customer volume
- Trip mix: airport-to-city trips and a smaller volume of city-to-city trips
- Charging times and battery consumption
- Booking mix: pre-booked versus on-demand requests

## Constraints

### Hard constraints

- Maintain a minimum reserve charge.
- Do not allow take-off without a guaranteed landing site.
- Respect pad and charger capacity at each site.

### Soft constraints

- Passenger waiting time
- Energy efficiency
- Empty repositioning flights

## Data generation

Generate parameterised random distributions for:

- Customer volume, including peak periods
- Delays, such as flight delays and time required to clear customs
- Destinations
- Pre-booked and on-demand requests

Where possible, calibrate the distributions using helicopter flight patterns.

## Evaluation

- Service quality: passenger waiting and journey times
- Efficiency: energy consumption and fleet utilisation
- Safety: zero violations of hard constraints

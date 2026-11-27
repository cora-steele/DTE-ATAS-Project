# Air-taxi model: centralised version

This is a rough sketch of how the centralised model could look, as a starting point for discussion rather than a fixed design. The thinking is that one operator sees every aircraft, vertiport and request, and each request is treated as one flight. Time could move in 1-minute steps, with the model planning about an hour ahead, acting on the first few minutes, and then re-planning as new requests come in.

## Possible decision variables

- $x_{a,r,t}$ = 1 if aircraft $a$ takes request $r$, leaving at minute $t$
- $y_{a,i,j,t}$ = 1 if aircraft $a$ flies empty from vertiport $i$ to $j$
- $z_{r}$ = 1 if request $r$ is declined
- $g_{a,v,t}$ = 1 if aircraft $a$ is parked at vertiport $v$; $h_{a,v,t}$ = 1 if it is plugged in
- $b_{a,t}$ = battery energy left (kWh); $c_{a,t}$ = energy charged in minute $t$ (kWh)

Here $f_{a,i,j,t}$ is shorthand for any flight (with or without passengers) from $i$ to $j$ starting at $t$, and $\rho_{r}$ is the time request $r$ is ready to go.

## Things that could be worked out beforehand

Keeping these outside the solver keeps the model linear:

- **Flight time:**

  $$
  \tau_{ij} = 2t_{taxi} + 2t_{vert} + \frac{d_{ij}}{v}
  $$

  (taxi and takeoff/landing and cruise time)

- **Flight energy** (hover for take-off and landing, cruise in between):

  $$
  E_{ij} = 2P_{hover}t_{vert} + P_{cruise}\frac{d_{ij}}{v}
  $$

- **Safety reserve on landing at $j$** (20 min of cruise plus reaching the nearest other vertiport):

  $$
  R_{j} = 20P_{cruise} + E_{j,alt(j)}
  $$

## Constraints worth including

The constraint equations are formulated using Claude for the given idea and plan given In the third coloumn the reasoning

### Serve or decline

**How it might be written**

$$
\sum_{a,t}x_{a,r,t} + z_{r} = 1
$$

**Why:** Each request is flown once or turned down

### Waiting window

**How it might be written**

$x_{a,r,t} = 0$ unless $\rho_{r} \leq t \leq \rho_{r} + W_{r}$

**Why:** Passengers shouldn't wait too long; priority could get a shorter W ( 0 means waiting)

### Aircraft movement

**How it might be written**

$$
g_{a,v,t} = g_{a,v,t-1} + {arr}_{a,v,t} - {dep}_{a,v,t}
$$

**Why:** An aircraft is always parked somewhere or flying

### Turnaround

**How it might be written**

$$
{dep}_{a,v,t} \leq g_{a,v,t-s}, \qquad s = 1,\ldots,T_{turn}
$$

**Why:** Time to unload, board and check

### Battery

**How it might be written**

$$
b_{a,t} = b_{a,t-1} + c_{a,t} - \sum_{i,j}E_{ij}f_{a,i,j,t}
$$

**Why:** Charging adds energy, a flight removes it

### Charging speed

**How it might be written**

$$
c_{a,t} \leq P_{ch}\Delta t \sum_{v}h_{a,v,t}
$$

$$
c_{a,t} \leq \Delta t(P_{ch} - k(b_{a,t-1} - 0.8C))
$$

**Why:** Full speed up to about 80%, then slower [2]; k sets how fast it tapers; only while plugged in

### Safety

**How it might be written**

$$
b_{a,t} \geq B_{\min} + \sum_{i,j}R_{j}f_{a,i,j,t}
$$

**Why:** Don't take off unless you can land with a reserve

### Vertiport limits

**How it might be written**

$$
\sum_a({dep}_{a,v,t} + {arr}_{a,v,t}) \leq {Pads}_{v}
$$

$$
\sum_a g_{a,v,t} \leq {Stands}_{v}, \qquad \sum_a h_{a,v,t} \leq {Chargers}_{v}
$$

**Why:** Limited pads, parking stands and chargers

## A possible objective

One option is to do it in two steps, so no money values are needed. First, minimise waiting:

$$
S = \sum_r(w_r(t_{dep,r} - \rho_r) + D_r z_r)
$$

Call the best value $S^{*}$. Then minimise the carbon from charging while keeping service close to that best:

$$
\min \sum_{a,t}\kappa_t c_{a,t} \qquad \text{subject to} \qquad S \leq S^{*} + \varepsilon
$$

Varying $\varepsilon$ would show the trade-off between service and emissions.

## Rough starting values

| Item | Suggested value | Where from |
| --- | --- | --- |
| Operating day, time step | 07:00 to 19:00, 1 min | [1] |
| Vertiports, aircraft | about 5 and 6 | Similar sizes in [1] |
| Requests | about 10 per aircraft per day, peaks near 08:30 and 17:00 | [1] |
| Turnaround | about 10 min | [1] |
| Speed, seats | 240 km/h, 4 | Guess and google review likely value |
| Hover, cruise power | 360 kW, 120 kW | Guess and google review likely value |
| Battery, minimum | 150 kWh, 10% | Guess and google review likely value (tailored to this application vehicle) |
| Charger | 300 kW, slowing above 80% | Guess, shape from [2] |

With these, a 20 km trip would take about 9 min and use about 22 kWh, which is roughly 5 min of charging. Worth testing other values too.

## References

**[1]** Bennaceur, Delmas, Hamadi (2022), Passenger-centric urban air mobility: fairness trade-offs and operational efficiency, Transportation Research Part C. *For the general air-taxi setting and starting values.*

**[2]** Montoya, Guéret, Mendoza, Villegas (2017), The electric vehicle routing problem with nonlinear charging function, Transportation Research Part B. *For the idea of a charging curve that slows near full, written as straight-line pieces.*

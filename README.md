# DTE-ATAS-Project

Research project on planning and operating an urban air-taxi service. The work
currently focuses on defining the optimisation problem, its constraints, and
suitable modelling approaches before implementation begins.

## Current scope

The initial scenario considers one airport hub, a small network of city
destinations, a limited aircraft fleet, and a mix of pre-booked and on-demand
passenger requests. The model must coordinate aircraft movements, landing and
charging capacity, battery reserves, and passenger waiting times.

Candidate solution approaches include:

- Adaptive Large Neighbourhood Search (ALNS)
- Constraint Programming with CP-SAT
- Mixed-Integer Linear Programming (MILP)
- A greedy baseline for comparison

The working assumption is that on-demand bookings will be handled with a
rolling planning horizon.

## Repository structure

```text
.
├── docs/
│   ├── README.md          # Documentation index
│   ├── project-plan.md    # Scope, constraints, data, and evaluation plan
│   └── research/          # Problem formulation and design notes
├── .gitignore
└── README.md
```

Start with the [project plan](docs/project-plan.md), then use the
[documentation index](docs/README.md) to find the detailed research notes.

## Project status

The repository is in the problem-definition stage. No implementation or
reproducible experiment setup has been added yet. Once the formulation is
stable, source code, tests, and experiment configuration should be introduced
as separate top-level directories.

## Latest meeting — 27 November 2026

- **Cora and Shuyu** will lead implementation and bring a practical perspective to
  the modelling decisions.
- **Moeez and Iker** will lead the problem formulation and mathematical modelling.

The team agreed to work over the weekend of 28–29 November and meet at some
point during the weekend, either online or in person, to discuss progress.

The deliverable has two parts:

1. A clear problem formulation, with a rationale for how the problem is structured.
2. A practical prototype that demonstrates the approach in code.

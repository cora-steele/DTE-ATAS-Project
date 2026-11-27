# 1. Transport Network Design

#### **What is it?**

Area that studies how to design a transport network to satisfy the mobility needs of a population efficiently, given the available resources and restrictions.

### **Which mathematical areas are involved?**

Graph Theory, Mathematical Optimisation, Flux Theory.

### **Which problems are there?**

Thinking in terms of a network graph $G=(V,E)$, where $V$ represents the set of nodes (locations or eVTOL ports) and $E \subseteq V \times V$ represents the set of edges (routes or connections):

- **Network Design**: What is the optimal geometry or structure of the network/graph.
- **Network Flow**: What are the optimal fluxes that circulate inside the network.
- **Routing**: Which are the routes/edges used.
- **Scheduling**: When are actions made.
- **Facility Location**: Where are the different nodes.
- **Capacity Planning**: How much capacity is associated with each node.

### **On the mathematical representation**

Assume the previous graph $G=(V,E)$. For each $e = (i,j) \in E$ we can define the following parameters:

| **Parameters**     | **Meaning**                      |
| ------------------ | -------------------------------- |
| $d_{ij}$           | Distance                         |
| $\tau_{ij}$        | Travel time                      |
| $c_{ij}$           | Marginal cost per transportation |
| $F_{ij}$           | Fixed cost for connection        |
| $u_{ij}$           | Transport capacity               |
| $\varepsilon_{ij}$ | Consumo energético por vehículo  |
For each $i \in V$ we can define the following parameters:

| **Parameters**     | **Meaning**                      |
| ------------------ | -------------------------------- |
| $d_{ij}$           | Distance                         |
| $\tau_{ij}$        | Travel time                      |
| $c_{ij}$           | Marginal cost per transportation |
| $F_{ij}$           | Fixed cost for connection        |
| $u_{ij}$           | Transport capacity               |
| $\varepsilon_{ij}$ | Consumo energético por vehículo  |
### **Some ideas**

- The network design that we should assume at first is a fully connected graph $G=(V,E)$ in which the nodes $V$ represent the locations or eVTOL ports, and the bidirectional edges $E$ represent the routes that they can take (we can go to any location and come back from each)
	- An advanced design would be a hybrid structure in which we have few connections to some subgraphs and unique/centralised connections with some special kind of nodes known as "hubs"
	- The connection to subgraphs means that there most be some kinds of "local networks" (such as districts, cities, or any subunit of space) that are connected through some few connections (so not all locations are fully connected among each other) so that we can make a distinction between long-distance trips (which require more energy consumption and other characteristics) and short-distance trips. This might become useful from a business perspective but also from a energy/design efficiency perspective.
	- The unique/centralised connections with the special nodes known as "hubs" respond to the need of having a location in which eVTOLs are not accessed by clients, but storaged, repaired or others. This is a typical structure by most taxi services. This might make sense from the management POV but also for advanced settings in which some eVTOLs must be "retired" from locations in which clients are for different reasons.
- A
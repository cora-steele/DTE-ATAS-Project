# Possible models

1) ALNS 
2) CP-SAT
3) MILP
4) greedy baseline 

Rolling horizon for on demand bookings 


### General design parameters:
- 1 airport hub, ~10 city desinations - number of landing sites / chargers per location 
- Fleet size 
- customer volume 
- desitnations - just airport to city or smaller volume city to city aswell 
- charging times and battery usage 
- booking types, volume of pre-bookings vs on demand 

### constraints:
- hard 
    - minimum reserve charge 
    - no take off wihtout guarneteed landing site 
    - pad and charger capcity at each site 
- soft 
    - waiting time 
    - energy efficeny 
    - empty repositioning flights 

### data generation: random paramaetised distibutions for 
- customer volume with peak times 
- delays i.e. flight delays, time to clear customs etc 
- destination 
- pre-booked and on demand 

potentially informed via helicopter flight patterns 

### Evaluation:
- service time 
- efficeny - energy usage, fleet usage 
- saftey - no violation of hard constraints 
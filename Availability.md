
A system's readiness and accessibility to users at any given point of time is referred to as its Availability.
Redundancy, fault tolerance and effective recovery techniques are usually used to achieve high scalability.


> [!NOTE] Availability is expressed as the following formula:
> Availability (%) : Uptime / (Uptime + Downtime)


### Availability Tiers

Availability is often expressed in `nines`. Refer to the table below:
![[Pasted image 20260104143244.png]]

Each additional `nine` represents an order of magnitude improvement in availability. Example : 99.99% signifies a **10 times improvement** in uptime compared to 99.9%


**<u>Strategies to improve availability</u>**:

1. Redundancy: Use redundant servers or components so that in the event of a failure another instance can take over without any problems. Data centers, hardware redundancy are some examples
   
2. Load Balancing
   
3. Disaster Recovery: Having a comprehensive plan in place to recover the system in case of natural disasters
   
4. [[Scalability]]

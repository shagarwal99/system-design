
It is the capacity of a system to support growth or maintain performance when faced with an increased volume of work. It should be able to maintain improve its performance with the workload rises without creating the need for a re-architecture

How to achieve scalability?

- <u>Vertical Scaling</u> : Here we add more resources to the system like more RAM, better CPU etc
- <u>Horizontal Scaling</u> : We distribute the load, by employing more server or instances to the application and distributing the tasks among them
- <u>Microservices</u>: We break our application in multiple independent instances and we only scale those instances who are in need
- <u>Serverless</u> : Another approach is using services like AWS Lambda which handle all the scaling for us. It is usually great for unpredictable workloads

Below are the components that can be used to increase scalabilty:

1. <u>Load Balancer</u>: It distributes incoming load across servers to ensure no single resource is over-worked. There are several Load Balancing techniques that can be used to achieve it
   
2. <u>Caching</u>: It involved storing frequently accessed data in memory to reduce the time need to get the result by eliminating the need to access the original source of data.
   
3. [<u>Database Replication</u>](Database-Replication): It is the process of copying data from one database to another in real time
   
4. <u>Database Sharding / Database Partitioning</u>
   
5. <u>CDNs(Content Delivery Networks)</u>: They can improve scalability by caching and delivering content from servers that are geographically closer to the users, reducing the latency and improving performance
   
6. <u>Queuing Systems</u>: They can improve scalability by decoupling components and allowing requests to be processed asynchronously
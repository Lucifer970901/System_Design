# How to start with Distributed system?

## Vertical Scaling

Vertical scaling, also known as "scaling up" (or "scaling down"), is a method of increasing the capacity of a system by adding more resources to a single existing server or node.

Think of it as making one machine more powerful.

### **Key Characteristics**:

* **Action**: You increase the power of the current machine.
* **Resources Added**: Typically involves upgrading components like:
    * CPU (more or faster cores)
    * RAM (more memory)
    * Storage (faster or larger disk drives)
    * Network speed

Example: Upgrading a virtual machine (VM) instance from a small size with 2 vCPUs and 8GB of RAM to a larger size with 8 vCPUs and 32GB of RAM.

### **When to Use**:

* For applications not designed for distributed systems (like some monolithic or legacy applications).
* When the workload is predictable and fits within the physical limits of a single machine.
* When simplicity and low initial complexity are priorities.

---

## Horizontal scaling

Horizontal scaling, also known as "scaling out" (or "scaling in"), is a method of increasing the capacity of a system by adding more identical servers (or nodes) to a pool of resources.

Instead of making one machine more powerful, you add new machines to distribute the workload among them.

### **Key Characteristics**:

* **Action**: You increase the number of machines.
* **Resources Added**: New, typically commodity, servers or instances are added to a cluster.
* **Work Distribution**: A load balancer is essential to distribute incoming requests evenly across all the available servers.
* **Analogy**: If vertical scaling is like replacing one delivery truck with a bigger, faster one, horizontal scaling is like adding more standard-sized trucks to your fleet.

### **When to Use**:

* For high-availability and fault tolerance (if one server fails, others take over).
* For applications with unpredictable or rapidly growing workloads (e.g., e-commerce during a major sale).
* For distributed systems like microservices and cloud-native applications.
* To achieve virtually limitless capacity, as you can theoretically keep adding more servers.

---
## Resilience 

Resilience is the ability of a system to **handle failures gracefully** and **continue functioning** even when parts of it break, are unavailable, or face unexpected stress.

It's about making sure your application can "take a punch" and stay on its feet. A resilient system is designed to anticipate failure and automatically recover from it, minimizing downtime and data loss.

The basic principles for building a resilient application:

1. **Redundancy is Key (Have Backups)**
Don't put all your eggs in one basket. If a server, database, or network component fails, you need another one ready to immediately take over.
    
    Example: Running your application on multiple servers across different geographic availability zones (Horizontal Scaling).

2. **Isolate Failures (Contain the Damage)**
A failure in one part of the system shouldn't bring down the whole thing. Use bulkheads (like on a ship) to prevent a localized issue from spreading.
    
    Example: If your payment service is slow, don't let it block users from viewing product pages. Implement timeouts so a slow service request doesn't stall the whole user experience.

3. **Fail Fast and Recover Automatically (Be Quick)**
When a component fails, the system should quickly detect it and either fix it (e.g., restart the failed service) or route traffic around it.
    
    Pattern: Health checks constantly monitor a service. If a check fails, the load balancer stops sending traffic to that unhealthy instance.

4. **Degrade Gracefully (Do Less, But Keep Going)**
If a non-critical component fails, the system should shed that functionality rather than crashing. The core service must continue.
    
    Example: If the "Recommended Products" microservice fails, the e-commerce site should still allow the user to browse and purchase products, maybe just showing a blank space or a default list instead of crashing. This is a Circuit Breaker pattern in action.

5. **Test for Failure (Practice the Disaster)**
You must actively test how your system responds to things breaking. This is often called **Chaos Engineering**.

    Example: Tools like Chaos Monkey randomly shut down servers in a production environment to ensure the remaining servers automatically handle the sudden increase in load.

---

## Monolithic Architecture

Monolithic architecture is the traditional and simplest way to build an application. It is a single, unified, tightly-coupled codebase where all components—the user interface, business logic, and data access layer—are bundled together and deployed as one large, indivisible unit.

The term "monolith" means a single, large block of stone, which perfectly describes this architecture: it's one large, solid application.

### **Key Characteristics**
* **Structure**: Everything is in one code repository and deployed as one executable file.
* **Coupling**: Tightly coupled—a change in one small part can affect the entire system.
* **Database**: Typically uses a single, shared database for all functions.
* **Scaling**: Uses Vertical Scaling (scaling up) by upgrading the single server's resources (CPU, RAM).


### **Pros of Monolithic Architecture (simple to start)**

* Simple Development and easy deployment
* Simplified Debugging
* Lower Initial Cost
* Faster Initial Speed

### **Cons of Monolithic Architecture (Hard to Grow)**

* Scaling Inefficiency
* Technology Lock-in
* Slow Development Cycle
* Low Fault Isolation
* Code Complexity

### **When to Choose a Monolith**

The monolithic architecture is often the best choice for:

* **Startups/MVPs (Minimum Viable Products)**: Where speed to market and simplicity are critical.
* **Small, Simple Applications**: Projects that are not expected to scale massively or grow complex.
* **Small, Co-located Teams**: Where the communication overhead is low.

---

## Microservice Architecture

Microservice architecture is an approach to developing a single application as a collection of small, independent services that are built around specific business capabilities and communicate with each other using lightweight mechanisms, often through APIs (Application Programming Interfaces).

Think of it as transforming a single, massive, interdependent machine into a fleet of small, specialized robots, each responsible for one job.

* **Business-Capability Focused**:	Each microservice is designed to solve a specific business problem (e.g., "Order Processing," "User Accounts," "Product Catalog"), rather than being a layer of a technical function (like "Presentation Layer").
* **Independent & Autonomous**:	Services can be developed, deployed, and scaled independently of one another. A team can update their service without affecting or needing coordination with other teams.
* **Decentralized Data Management**	Each service often has its own private data store (database), ensuring loose coupling. Services communicate via APIs, not by directly accessing another service's database.
* **Technology Heterogeneity**:	Teams have the freedom to choose the best technology stack (language, framework, database) for a specific service's needs, rather than being restricted to one technology across the entire application.
* **Design for Failure (Resilience)**:	If one service fails, the overall application can remain functional (possibly with degraded capability), as the failure is isolated and does not crash the entire system.


### **Major Benefits**

* **Faster Development and Agility**: Small, autonomous teams can focus on their service and deploy updates quickly, enabling Continuous Integration/Continuous Delivery (CI/CD).
* **Scalability**: You can apply horizontal scaling to only the high-demand services (e.g., scale the "Search" service without scaling the "Billing" service), which is more efficient and cost-effective.
* **Flexibility and Innovation**: Teams can use the best tool for the job and adopt new technologies for a specific service without affecting the rest of the system.
* **Resilience and Isolation**: Failure in one service is contained, preventing cascading failures across the entire application.


### **Microservices vs. Monolithic Architecture**

The microservice architecture is typically adopted as an evolution away from the traditional monolithic architecture.

| Feature|  Monolithic Architecture | Microservice Architecture |
| :--- | :--- | :--- |
| Structure | A single, tightly coupled, unified code base. | A collection of small, loosely coupled services. | 
| Deployment | The entire application must be built and deployed as a single unit. | Each service is deployed independently. |
| Scaling | Vertical scaling (scale the whole app). | Horizontal scaling (scale only the services that need it).|
|Technology | All components typically use the same language/framework. | Allows for mixed technology stacks. |
| Fault Tolerance | Low. A bug in one module can bring down the entire system. | High. Failure is isolated to a single service.|

---

## Distributed systems

A distributed system is a collection of independent computers (nodes) that communicate with each other over a network and coordinate their actions to achieve a single, common goal.

To the end-user, the system often appears as a single, powerful entity, even though the work is being processed simultaneously across multiple machines.

### **Key Characteristics**

Distributed systems are defined by the architectural goals they aim to achieve, primarily by using horizontal scaling:

* **Scalability**: The ability to handle growing demands (more users, more data) by simply adding more nodes (Horizontal Scaling), rather than upgrading a single machine.

* **Fault Tolerance/Resilience**: The system can continue to operate even if one or more individual nodes fail. This eliminates the single point of failure found in monolithic (single-server) systems.

* **Concurrency**: Multiple nodes can process the same or different tasks simultaneously, significantly increasing throughput and performance.

* **Availability**: Services remain accessible to users for a high percentage of the time, achieved through redundancy (having multiple copies of data/services).

* **Transparency**: The complexity of the underlying network, node communication, and coordination is hidden from the end-user and often from the application developer.


### **Comparison with Centralized Systems**

| Feature | Distributed System | Centralized (Monolithic) System | 
| :--- | :--- | :--- |
| Structure | Multiple, independent computers (nodes) | Single, powerful computer/server |
| Scaling | Horizontal (add more machines) | Vertical (upgrade one machine) |
| Failure | Fault-Tolerant (other nodes take over) | Single Point of Failure (system crashes) | 
| Cost | Uses commodity hardware (cheaper servers) | Requires specialized, high-end hardware | 

---

## Partitioning (Data Partitioning)

it is the process of dividing a large dataset or table into smaller, more manageable, independent segments called partitions.

This technique is fundamental to achieving scalability, performance, and manageability for systems dealing with large volumes of data (Big Data) or high traffic (Distributed Systems).


### **Why Partitioning is Essential**

| Goal | Benefit Achieved | 
| :--- | :--- |
| Scalability | Enables Horizontal Scaling by distributing data and query load across multiple servers/nodes, overcoming the capacity limits of a single machine. | 
| Performance | Reduces the amount of data that must be scanned for any given query, leading to faster query response times (Partition Pruning). | 
| Manageability | Simplifies maintenance tasks like backups, archiving, and indexing, which can be performed on individual partitions instead of the massive original table. | 
| Fault Isolation | If one partition or the server hosting it fails, the rest of the data (on other partitions/servers) remains available. | 


### **Main Partitioning Strategies**

There are two primary ways to divide a large table, each serving a different purpose:

1. **Horizontal Partitioning (Sharding)**

    * **How it Works**: The table is split by rows. Each partition (often called a shard) contains a subset of the rows but all of the original columns.

    * **Goal**: To distribute the data and workload across multiple database servers (horizontal scaling).

2. **Vertical Partitioning**

    * **How it Works**: The table is split by columns. Each partition contains all the rows but only a subset of the columns.

    * **Goal**: To optimize read/write performance by separating frequently accessed columns from rarely accessed ones, reducing I/O and cache requirements.


### **Partitioning vs. Sharding**

The terms are often used interchangeably, but in system design, the distinction is subtle:

**Partitioning** is the logical division of data. It can ***occur within a single database instance*** (e.g., vertical split or range partitioning on a single server).

**Sharding** is a specific form of horizontal partitioning where the partitions (shards) are ***distributed across multiple physical database servers***. Sharding is the key technique for achieving massive horizontal scaling.

---

## ⚖️ Load Balancer 

A load balancer is a device or software program that acts as a "traffic cop" for your servers. Its main job is to efficiently distribute (balance) incoming network traffic across a group of backend servers (a server farm or pool).

It's essential for achieving horizontal scaling and high availability in distributed systems.


### **How It Works and Why It's Needed**

Imagine you have a popular website with thousands of visitors per second:

* **Incoming Request**: A user's web request (e.g., loading a page) first hits the load balancer.

* **Traffic Direction**: The load balancer uses a specific algorithm (like Round Robin or Least Connections) to decide which server in the pool is the best choice for handling that request right now.

* **Health Check**: Before sending the traffic, the load balancer constantly performs health checks to ensure all backend servers are **alive and responding**. If a server fails, the load balancer automatically stops sending traffic to it.


### **Load Balancing Algorithms**
Load balancers use various methods to decide where to send traffic.

* **Round Robin**: Sends ***requests to servers sequentially***, in order (Server 1, then Server 2, then Server 3, then back to Server 1, etc.). Simple and fair.

* **Least Connections**: Sends the ***request to the server that currently has the fewest active connections***. This is better when servers handle different workloads or tasks.

* **IP Hash**: Uses a hash function based on the client's IP address. This ensures a ***specific client is always directed to the same server***, which is useful for "***sticky***" sessions (where state is stored on the server).
    

### **Load Balancer Types**

*    * **L4 (Layer 4) Load Balancer**: Works at the Transport Layer (TCP/UDP). It makes decisions based primarily on IP addresses and ports. 

    * **L7 (Layer 7) Load Balancer**: Works at the Application Layer (HTTP/HTTPS). It can make more intelligent routing decisions based on the content of the request, such as the URL, cookies, or HTTP headers.

---

## 🤝 Decoupling the System 

Decoupling (or loose coupling) is a fundamental concept in system design that means making sure ***the different parts, services, or components of an application know as little as possible about each other***.

Think of it as having specialized teams at a company: the Marketing team knows they need a product from the Engineering team, but they don't need to know how Engineering builds it, what tools they use, or when they take coffee breaks. They just interact through a well-defined interface (like a Product Spec).

### **The Core Idea: Independence**
When a system is decoupled, it means:

* **Change Isolation**: If you change something in Component A, Component B doesn't automatically break. The change is isolated to one area.

* **Independent Development**: Teams can develop, update, and deploy their components without having to coordinate or stop the work of other teams.

* **Flexibility**: You can replace an old component with a new one entirely (e.g., swapping out one database for another) with minimal impact on the rest of the system.


### **To achieve Decoupling**

1. **Via APIs/Interfaces (Synchronous)**
Instead of calling Component B's internal function directly, Component A calls a formal API Endpoint (like using a common plug/socket). The API is the contract. As long as the contract doesn't change, the internal code can change freely.

2. **Via Message Queues (Asynchronous)**
This is the most powerful form of decoupling. Instead of Component A waiting for Component B to finish a task, A simply drops a message into a queue and immediately moves on.

---


## How to start with Distributed system?

### Vertical Scaling

Vertical scaling, also known as "scaling up" (or "scaling down"), is a method of increasing the capacity of a system by adding more resources to a single existing server or node.

Think of it as making one machine more powerful.

**Key Characteristics**:

* **Action**: You increase the power of the current machine.
* **Resources Added**: Typically involves upgrading components like:
    * CPU (more or faster cores)
    * RAM (more memory)
    * Storage (faster or larger disk drives)
    * Network speed

Example: Upgrading a virtual machine (VM) instance from a small size with 2 vCPUs and 8GB of RAM to a larger size with 8 vCPUs and 32GB of RAM.

**When to Use**:

* For applications not designed for distributed systems (like some monolithic or legacy applications).
* When the workload is predictable and fits within the physical limits of a single machine.
* When simplicity and low initial complexity are priorities.

---

### Horizontal scaling

Horizontal scaling, also known as "scaling out" (or "scaling in"), is a method of increasing the capacity of a system by adding more identical servers (or nodes) to a pool of resources.

Instead of making one machine more powerful, you add new machines to distribute the workload among them.

**Key Characteristics**:

* **Action**: You increase the number of machines.
* **Resources Added**: New, typically commodity, servers or instances are added to a cluster.
* **Work Distribution**: A load balancer is essential to distribute incoming requests evenly across all the available servers.
* **Analogy**: If vertical scaling is like replacing one delivery truck with a bigger, faster one, horizontal scaling is like adding more standard-sized trucks to your fleet.

**When to Use**:

* For high-availability and fault tolerance (if one server fails, others take over).
* For applications with unpredictable or rapidly growing workloads (e.g., e-commerce during a major sale).
* For distributed systems like microservices and cloud-native applications.
* To achieve virtually limitless capacity, as you can theoretically keep adding more servers.

---
### Resilience 

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

### Monolithic Architecture

Monolithic architecture is the traditional and simplest way to build an application. It is a single, unified, tightly-coupled codebase where all components—the user interface, business logic, and data access layer—are bundled together and deployed as one large, indivisible unit.

The term "monolith" means a single, large block of stone, which perfectly describes this architecture: it's one large, solid application.

**Key Characteristics**
* **Structure**: Everything is in one code repository and deployed as one executable file.
* **Coupling**: Tightly coupled—a change in one small part can affect the entire system.
* **Database**: Typically uses a single, shared database for all functions.
* **Scaling**: Uses Vertical Scaling (scaling up) by upgrading the single server's resources (CPU, RAM).

**Pros of Monolithic Architecture (simple to start)**

* Simple Development and easy deployment
* Simplified Debugging
* Lower Initial Cost
* Faster Initial Speed

**Cons of Monolithic Architecture (Hard to Grow)**

* Scaling Inefficiency
* Technology Lock-in
* Slow Development Cycle
* Low Fault Isolation
* Code Complexity

---
### Microservice Architecture

Microservice architecture is an approach to developing a single application as a collection of small, independent services that are built around specific business capabilities and communicate with each other using lightweight mechanisms, often through APIs (Application Programming Interfaces).

Think of it as transforming a single, massive, interdependent machine into a fleet of small, specialized robots, each responsible for one job.

* **Business-Capability Focused**:	Each microservice is designed to solve a specific business problem (e.g., "Order Processing," "User Accounts," "Product Catalog"), rather than being a layer of a technical function (like "Presentation Layer").
* **Independent & Autonomous**:	Services can be developed, deployed, and scaled independently of one another. A team can update their service without affecting or needing coordination with other teams.
* **Decentralized Data Management**	Each service often has its own private data store (database), ensuring loose coupling. Services communicate via APIs, not by directly accessing another service's database.
* **Technology Heterogeneity**:	Teams have the freedom to choose the best technology stack (language, framework, database) for a specific service's needs, rather than being restricted to one technology across the entire application.
* **Design for Failure (Resilience)**:	If one service fails, the overall application can remain functional (possibly with degraded capability), as the failure is isolated and does not crash the entire system.

**Major Benefits**

* **Faster Development and Agility**: Small, autonomous teams can focus on their service and deploy updates quickly, enabling Continuous Integration/Continuous Delivery (CI/CD).
* **Scalability**: You can apply horizontal scaling to only the high-demand services (e.g., scale the "Search" service without scaling the "Billing" service), which is more efficient and cost-effective.
* **Flexibility and Innovation**: Teams can use the best tool for the job and adopt new technologies for a specific service without affecting the rest of the system.
* ***Resilience and Isolation**: Failure in one service is contained, preventing cascading failures across the entire application.

**Microservices vs. Monolithic Architecture**

The microservice architecture is typically adopted as an evolution away from the traditional monolithic architecture.

| Feature|  Monolithic Architecture | Microservice Architecture |
| :--- | :--- | :--- |
| Structure | A single, tightly coupled, unified code base. | A collection of small, loosely coupled services. | 
| Deployment | The entire application must be built and deployed as a single unit. | Each service is deployed independently. |
| Scaling | Vertical scaling (scale the whole app). | Horizontal scaling (scale only the services that need it).|
|Technology | All components typically use the same language/framework. | Allows for mixed technology stacks.
| Fault Tolerance | Low. A bug in one module can bring down the entire system. | High. Failure is isolated to a single service.|

---
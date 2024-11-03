## General Approach for System Design interview


### **Step 1: Requirement Clarification**
**Purpose**: Fully understand the problem scope, objectives, constraints, and trade-offs to avoid miscommunication or feature creep.

**Details**:
1. **Functional Requirements**: Identify what the system must do.
   - **Questions**: What are the core features? What specific actions should the system enable?
   - **Example**: For designing a URL shortener, functional requirements would include URL shortening, redirection, and analytics on link usage.

2. **Non-Functional Requirements**: Identify performance, scalability, availability, and latency expectations.
   - **Questions**: What’s the acceptable latency? How many concurrent users should the system support?
   - **Example**: A real-time chat application should prioritize low latency and high availability.

3. **Constraints and Assumptions**: Identify any limitations or assumptions to help guide decisions.
   - **Questions**: Are there specific technology constraints (e.g., databases, frameworks)? What is the expected load?
   - **Example**: For an e-commerce recommendation system, assume high read rates and potentially large spikes during events like Black Friday.

4. **Trade-offs**: Identify areas where you might need to make trade-offs.
   - **Questions**: Would the team prioritize availability over consistency? How would the system scale under budget constraints?

---

### **Step 2: System Interface Definition**
**Purpose**: Define APIs and endpoints that the system will expose to external services or clients.

**Details**:
1. **Define API Endpoints**: Specify detailed functionality, input parameters, expected outputs, and any rate limits.
   - **Questions**: What specific data will each endpoint provide or consume? Are there security requirements for the endpoints?
   - **Example**: For a video streaming service, you might define endpoints such as `/getVideoDetails`, `/playVideo`, and `/uploadVideo`.

2. **Request/Response Models**: Describe the request and response format for each endpoint.
   - **Questions**: What data structures should be used for requests and responses? Are there specific serialization requirements (e.g., JSON, Protobuf)?
   - **Example**: In a social media app, a `/getUserProfile` endpoint may take `userID` as input and return user profile data in JSON format.

3. **Authentication and Authorization**: Define access control mechanisms.
   - **Questions**: Will the system use OAuth, JWT, or API keys? What permissions are required for each endpoint?
   - **Example**: For a banking API, user authentication may involve OAuth with role-based permissions.

4. **Error Handling**: Outline error codes and messages to handle different scenarios gracefully.
   - **Questions**: How will the API handle common errors (e.g., 404 for not found, 429 for rate limit exceeded)?
   - **Example**: A URL shortener might return a `404` for an invalid URL or a `400` for a malformed request.

---

### **Step 3: Back-of-the-Envelope Estimation**
**Purpose**: Conduct rough calculations to assess storage, bandwidth, and compute requirements, helping to validate the feasibility.

**Details**:
1. **Data Storage Needs**: Estimate how much data will be generated and stored over time.
   - **Questions**: What’s the average data size per entity? How many entities are expected per day/month/year?
   - **Example**: For a URL shortener, calculate based on the average URL length, expected number of URLs, and retention period.

2. **Traffic Estimates**: Estimate incoming requests per second, peak load, and network bandwidth.
   - **Questions**: What’s the expected QPS (queries per second)? Are there seasonal or peak load patterns?
   - **Example**: For a messaging app, if you expect 1 million active users, each sending 20 messages/day, estimate QPS to handle peak hours.

3. **Compute and Memory Requirements**: Calculate the estimated CPU and memory usage.
   - **Questions**: How much memory does each data object require? What are the computational requirements per request?
   - **Example**: For a recommendation engine, estimate memory based on the size of user profiles and item features.

---

### **Step 4: Defining Data Model**
**Purpose**: Outline a schema that efficiently supports the data requirements and access patterns.

**Details**:
1. **Entity Identification**: Define primary entities and their relationships.
   - **Questions**: What entities will the system manage? What are their relationships?
   - **Example**: In a ride-sharing app, entities might include `User`, `Ride`, `Driver`, and `Vehicle`.

2. **Data Schema**: Design schema for each entity (e.g., tables, documents).
   - **Questions**: What fields are required? What types and indexes will optimize retrieval?
   - **Example**: For an e-commerce system, a `Product` entity might include fields like `productID`, `name`, `price`, and `category`.

3. **Relationships and Foreign Keys**: Define relationships between entities for relational models or reference patterns in NoSQL.
   - **Questions**: Will entities be one-to-many, many-to-many? How should relationships be handled (join tables, references)?
   - **Example**: In a social media app, `User` and `Posts` would have a one-to-many relationship.

4. **Data Partitioning and Sharding**: If scalability is required, outline sharding or partitioning strategies.
   - **Questions**: What are logical ways to partition data (e.g., by user ID or region)?
   - **Example**: A multi-tenant SaaS app might partition user data by customer ID.

---

### **Step 5: High-Level Design**
**Purpose**: Develop an overview of the system's architecture, focusing on major components and their interactions.

**Details**:
1. **Component Breakdown**: Identify the main components and their functions.
   - **Questions**: What are the core services? How do they interact?
   - **Example**: For a video streaming service, key components include the Content Delivery Network (CDN), Encoder, and Video Metadata Service.

2. **Service Communication**: Define how services will communicate (REST, gRPC, messaging).
   - **Questions**: What protocols will be used for communication? Are there latency or reliability needs?
   - **Example**: In an IoT system, use MQTT for low-latency, lightweight communication.

3. **Data Flow**: Map the data flow from entry to storage, processing, and delivery.
   - **Questions**: What is the data flow for a request-response cycle? How will data move between services?
   - **Example**: For a news feed system, data flows from posts to indexing, to real-time feed generation.

4. **Load Balancing and Caching**: Plan for load balancing and caching strategies.
   - **Questions**: What data or requests can be cached? Where will load balancers be placed?
   - **Example**: For a web server, use a reverse proxy to load balance incoming requests.

---

### **Step 6: Detailed Design**
**Purpose**: Dive deeper into each component, defining algorithms, data structures, and internal details.

**Details**:
1. **Internal Workings of Components**: Explain each component’s internals in detail.
   - **Questions**: What algorithms will be used for processing? What optimizations are required?
   - **Example**: For a search engine, detail the indexing component, including inverted indexes, ranking algorithms, and caching layers.

2. **Scalability and Failover Mechanisms**: Define strategies for horizontal scaling and failover.
   - **Questions**: How will the system handle failures? Are components stateful or stateless?
   - **Example**: For a chat system, use stateful servers for message storage but set up replicas for failover.

3. **Data Consistency and Concurrency**: Identify consistency models and concurrency handling mechanisms.
   - **Questions**: Will data be strongly or eventually consistent? How will concurrency be managed?
   - **Example**: In a distributed database, implement leader-follower replication to ensure strong consistency for writes.

---

### **Step 7: Identifying and Resolving Bottlenecks**
**Purpose**: Recognize potential limitations and devise strategies to overcome them.

**Details**:
1. **Throughput and Latency Analysis**: Identify components or processes that may slow down under load.
   - **Questions**: What’s the bottleneck in request handling? Which components are most latency-sensitive?
   - **Example**: For a recommendation engine, model prediction latency could be a bottleneck, so consider model optimization.

2. **Scaling Strategies**: Use replication, sharding, and load balancing to scale components.
   - **Questions**: How will each component scale? What mechanisms will help distribute load?
   - **Example**: For a messaging system, shard user data based on geography to reduce latency.

3. **Optimization and Tuning**: Explore caching, indexing, and queuing solutions to optimize performance.
   - **Questions**: What queries or processes can be optimized? How can resource usage be minimized?
   - **Example**: For a news feed system, cache popular feeds and tune database indexes for quicker reads.

4. **Failover and Recovery**: Implement resilience strategies to ensure system continuity.
   - **Questions**: How will the system recover from partial failures? Are there automated retries or backups?
   - **Example**: In a financial transaction system, implement retries and database replication to prevent data loss.
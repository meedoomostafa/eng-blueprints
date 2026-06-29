# Distributed Systems: Course Index and Roadmap

This index serves as the master roadmap for organizing your notes for the Distributed Systems course, based on the lectures from the "Distributed Systems بالعربي" YouTube playlist by Ahmed Farghal.

## Why Learn Distributed Systems in This Order?

Distributed systems require thinking about concurrency, network failure, partial failure, and time in a completely different way than single-node systems:

1. **Why Distributed Systems**: Understanding the core trade-offs (scalability, availability, fault tolerance) and challenges compared to single-machine systems.
2. **State Machine Replication (SMR)**: The foundational pattern to build fault-tolerant services by replicating state across multiple nodes.
3. **Consensus (Raft)**: The algorithm that enables nodes to agree on a sequence of state transitions, even when some nodes fail or network partitions occur.
4. **Consensus Membership Management**: How to safely change the cluster membership (adding/removing nodes) without breaking consensus guarantees.
5. **Physical Clocks & Time**: Understanding how physical clocks drift in distributed systems and why relying on wall-clock time can lead to data loss.
6. **Logical & Vector Clocks**: How to track ordering and causality of events without relying on physical time.
7. **Consistency Models**: Understanding the spectrum of consistency guarantees (from strong/linearizability to eventual consistency) and their trade-offs.

## Curriculum and Notes Chain

### Topic 1: Foundations & Replication

- **Description**:
    Introduction to distributed systems challenges, motivation for distribution, and the core concept of State Machine Replication (SMR) to achieve fault tolerance.
    
- **Covered Videos**:
    - `Episode 1 | Why distributed systems?`
    - `Episode 2 | State Machine Replication`
    
- **Resources**:
    - **Reference Playlist**: [Distributed Systems بالعربي Playlist on YouTube](https://www.youtube.com/watch?v=s_p3I5CMGJw&list=PLald6EODoOJW3alE1oPAkGF0bHZkPIeTK)
    - **My Notes**: (notes links here)

### Topic 2: Consensus (Raft)

- **Description**:
    How a cluster of nodes agrees on a shared log. Detailed study of the Raft Consensus Algorithm, leader election, log replication, and cluster membership changes.
    
- **Covered Videos**:
    - `Episode 3 | Raft Consensus`
    - `Episode 4 | Raft membership management`
    
- **Resources**:
    - **Reference Playlist**: [Distributed Systems بالعربي Playlist on YouTube](https://www.youtube.com/watch?v=s_p3I5CMGJw&list=PLald6EODoOJW3alE1oPAkGF0bHZkPIeTK)
    - **My Notes**: [[topic-01-consensus]]

### Topic 3: Time, Ordering, and Causality

- **Description**:
    Understanding time and event ordering. Why physical time is unreliable (clock skew, NTP drift) and how logical clocks (Lamport Clocks) and Vector Clocks are used to track causality.
    
- **Covered Videos**:
    - `Episode 5 | Clocks`
    - `Episode 6 | Vector Clocks`
    
- **Resources**:
    - **Reference Playlist**: [Distributed Systems بالعربي Playlist on YouTube](https://www.youtube.com/watch?v=s_p3I5CMGJw&list=PLald6EODoOJW3alE1oPAkGF0bHZkPIeTK)
    - **My Notes**: (notes links here)

### Topic 4: Consistency & Real-world Systems

- **Description**:
    Analyzing the different models of consistency (Linearizability, Sequential, Causal, Eventual) and looking at how real-world distributed, resilient execution systems are designed.
    
- **Covered Videos**:
    - `Episode 7 | Consistency Models`
    - `Building Resilience: Designing a Distributed Durable Execution System from Scratch`
    
- **Resources**:
    - **Reference Playlist**: [Distributed Systems بالعربي Playlist on YouTube](https://www.youtube.com/watch?v=s_p3I5CMGJw&list=PLald6EODoOJW3alE1oPAkGF0bHZkPIeTK)
    - **My Notes**: (notes links here)

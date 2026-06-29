# Distributed Systems - Episode 1: Why Distributed Systems? (Ahmed Farghal)

> [!quote] The Golden Rule of Systems Design
> 
> **"Engineering is the art of trade-offs."**
> 
> In distributed systems, there is no single "correct" or "perfect" design. Every architectural decision is a deliberate choice to trade one property (e.g., latency, consistency, complexity, or cost) for another based on specific business and technical constraints.

## 1. Concurrency Limits & The Shift to Distributed Systems

- **Single Node Constraints**: Systems almost always start on a single node. When traffic grows, we first leverage concurrency and multithreading on the same machine to optimize resource utilization.
    
- **Hardware Bottlenecks**: Eventually, you hit physical hardware limits:
    
    - RAM address space limits.
        
    - Maximum network throughput capacity of a single network interface card (NIC).
        
    - CPU core and thermal limits.
        
- **IPC vs. Network Communication**:
    
    - On a **single machine**, Inter-Process Communication (IPC) is incredibly fast and offers high _atomicity_ (operations either succeed completely or fail completely with no middle ground).
        
    - In a **distributed system**, nodes must communicate over a network, introducing _asynchrony_, high latency, and _partial failures_.
        

## 2. Architectural Paradigms: Compute vs. State

- **Stateless Architecture**: Separating computation from state. Application servers only handle business logic. Since they do not hold persistent data, they can be horizontally scaled up or down effortlessly. The persistent state is offloaded to a database or a centralized cluster.
    
- **Stateful Architecture**: The state lives directly on the service nodes (e.g., Distributed Caches or Distributed Databases). This introduces the complex challenge of data distribution, solved primarily via two methods:
    
    1. **Replication**: Storing multiple full copies of the exact same data across different nodes.
        
        - _Purpose_: Achieves High Availability (if one node dies, others have the data) and Low Latency (placing data geographically closer to users to bypass the physical speed of light propagation limits in fiber optics).
            
    2. **Sharding / Partitioning**: Dividing a large dataset into smaller subsets and distributing those subsets across multiple nodes.
        
        - _Purpose_: Overcomes the storage capacity limits of a single drive and scales write throughput.
            

## 3. Consistency Models (The State Sync Dilemma)

When replication is used, keeping those copies synchronized introduces major trade-offs:

- **Strong Consistency (Linearizability)**: All nodes agree on the exact same state at the exact same moment. Any read operation performed after a write must return the newly written data.
    
    - _Trade-off_: Extremely high latency and lower availability during network hiccups.
        
- **Eventual Consistency**: Nodes are allowed to have temporarily divergent views of the data. However, if no new updates are made, all replicas will eventually converge and agree on the same value.
    
    - _Trade-off_: Fast writes and high availability, but clients might read stale data temporarily.
        

## 4. Failure Modes & Domains

- **Node Failures**:
    
    - **Crash-Stop / Fail-Stop**: A node abruptly halts execution (due to a crash, power outage, etc.) and stops responding entirely. This is the simplest failure mode to detect and handle.
        
    - **Gray Failures / Arbitrary Failures**: The node remains active but behaves abnormally. It might run incredibly slowly, drop specific packets, or return corrupt/invalid protocol responses.
        
- **Network Partitions (The Split-Brain Problem)**: A communication breakdown cuts the cluster into disconnected groups of nodes.
    
    - If both isolated groups continue to accept write operations independently, they will diverge irreversibly. To prevent this, systems use _consensus protocols_ to ensure only the majority group (the quorum) can proceed.
        

## 5. Foundational Distributed Problems

- **The Two Generals Problem**:
    
    - _The Scenario_: Two allied armies camp on opposite hills of an enemy city. They must agree on a unified time to attack, but their only way to communicate is by sending messengers through the enemy valley (an unreliable communication link where messengers can be captured).
        
    - _The Mathematical Proof_: It is mathematically impossible to reach 100% guaranteed consensus over an unreliable network. This is because every confirmation message itself requires a confirmation, leading to an infinite loop of acknowledgments.
        
    - _The Practical Solution_: Real-world systems rely on _probabilistic approaches_ (e.g., retrying up to a reasonable limit) or handle the failure gracefully at the application layer via rollbacks or compensating transactions.
        
- **The Byzantine Generals Problem**:
    
    - A more complex variation of consensus where not only is the network unreliable, but some nodes (generals) can be outright malicious, traitorous, or silent.
        
    - _Real-World Note_: Most enterprise distributed databases assume a non-byzantine model (they assume nodes may crash but won't actively lie or cheat), because the infrastructure is private and owned by a single trusted entity.
        

## 6. Consensus Protocols

- **Consensus** is the foundational process of getting a set of independent nodes to agree on a single value or state transition, even in the presence of node crashes and network failures.
    
- To safely build fault-tolerant distributed state machines, we rely on mathematically proven consensus algorithms, most notably:
    
    - **Paxos**: The academic, highly-generalized pioneer protocol.
        
    - **Raft**: A highly understandable, practically-designed consensus protocol widely adopted in modern infrastructure (used in etcd, Consul, etc.).
        

_Next Episode Focus: Consensus Algorithms in Action (Paxos, Raft, and Designing Distributed Storage)._
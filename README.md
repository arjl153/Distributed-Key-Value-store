# High Performance Distributed Key-Value Store

## Overview

Engineered a resilient Key-Value Store with fault tolerance and high availability, reducing downtime and data loss risks through ZooKeeper integration.

## Project Description

The project aimed to create a distributed key-value store using ZooKeeper, with physical nodes serving as the systems. The nodes were designated as follows: one running client code, others running server code, and one acting as the ZooKeeper server.

### Cluster Organization

- The first node contacting ZooKeeper becomes the master of the cluster.
- The second fastest node becomes the sub-master.
- All remaining nodes serve as slave nodes.

### Client Interaction with ZooKeeper

- When a client contacts ZooKeeper, it is informed of the cluster status (whether the master is alive or dead).
- If the cluster is alive, ZooKeeper returns the master node's IP address.
- If the cluster is dead, ZooKeeper returns only the status.

### High Availability

- High availability is achieved through duplication and failure tolerance.
- Each node's data is duplicated across other nodes.
- For example, if Node1 stores keys a-h and Node2 stores keys l-m, Node1 acts as Node2's backup and vice versa.
- Backup data is stored in a separate `bkpdata` file.

### Failure Handling

- When a node dies or is disconnected, ZooKeeper triggers the watch function, informing others of the node's death.
- If the failed node was the master, the sub-master promotes itself, and a new sub-master is created.
- The client receives the new master's IP address.
- The backup server now handles requests from the client.

### Node Recovery

- When a failed node comes back online, it syncs its `bkpdata` with the data of the node whose backup it acts as.
- For instance, if Node1 dies and its backup is in Node2, Node1 syncs its backup data with Node2's data to maintain the cluster's current status.
- Node1 then proceeds to sync its data file with Node2's `bkpdata` file.

## Implementation

The project code is organized as follows:

- **Client Code:** Responsible for client interactions with ZooKeeper and handling responses.
- **Server Code:** Implements the key-value store logic, with master, sub-master, and slave functionalities.
- **ZooKeeper Server:** Manages the cluster configuration and status.

## Usage

1. Set up the ZooKeeper server.
2. Deploy the client and server codes on respective nodes.
3. Upon client contact with ZooKeeper, receive information on the cluster status.
4. Ensure fault tolerance, high availability, and data duplication across nodes.
5. Handle node failures and recoveries seamlessly with ZooKeeper integration.

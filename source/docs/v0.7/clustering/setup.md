---
title: InfluxDB Cluster Setup
---

# Cluster Setup

InfluxDB can work with clusters. This can be usefull if you want to secure redundancy, high availability and scalability on queries.

## Creating an multiple-node InfluxDB cluster
If you are already running InfluxDB, stop the deamon and then delete the `data` directory.

### Parent Node
On the node/s that you will use as parent, configure the 
desired amount of replicas that you want to take. Just edit `config.toml` and set

```bash
[sharding]
  ...
  replication-factor = Your number of nodes
```
Is very recomendable to set the `hostname`. You need to set a valid value that is reachable
from the other nodes (you should do that on the other nodes too!)

```bash
hostname = "your.first.node.host.name"
```

Start the first node, now this node is the parent. 

### Children Nodes

On the other nodes, configure the same replication level as on the first node. Edit `config.toml` of each node and set

```bash
[sharding]
  ...
  replication-factor = Your number of nodes
```

Set the parent node/s

```bash
[cluster]
...
seed-servers = ["your.first.node.host.name:8090"]
```

The `8090` port is the default port for cluster connecting. Replication works with a protocol called Protobuf that runs in the port `8099`.

Start the children node/s. At this point, you should see all the nodes in the cluster section of the web GUI.

![Admin login](/images/docs/cluster_nodes.png)


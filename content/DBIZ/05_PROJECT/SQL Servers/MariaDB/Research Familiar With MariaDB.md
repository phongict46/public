---
stage: process
image: https://mariadb.org/wp-content/uploads/2023/05/Default_socail-card.png
tags: []
---
# **Research Familiar With MariaDB**
## **Summary information about MariaDB communication server product.**

### **Introduce:**
- MariaDB Community Server offers robust clustering capabilities primarily through MariaDB Galera Cluster. 

- This is a highly popular and widely used solution for achieving high availability and scalability with MariaDB databases.

### **Clustering Technology:**
 - **MariaDB Galera Cluster:**
	- **Virtually Synchronous Multi-Primary Replication:** 
		- This is the core of Galera. It means that transactions are applied to all nodes in the cluster before they are committed, ensuring that all nodes always have the same data. 
		- This virtually synchronous replication prevents data loss and minimizes replica lag.
    
	- **Active-Active Topology:** 
		- Unlike traditional master-slave replication where only one server handles writes, MariaDB Galera Cluster allows you to read from and write to _any_ node in the cluster. 
		- This significantly improves write scalability and overall performance.
    
	- **Automatic Membership Control:** 
		- If a node fails, it's automatically removed from the cluster. 
		- When it comes back online, it automatically rejoins and synchronizes with the rest of the cluster.
    
	- **Automatic Node Joining:** 
		- Adding new nodes to an existing cluster is largely automated. New nodes can easily join and receive a State Snapshot Transfer (SST) to catch up with the current data.
    
	- **True Parallel Replication:** 
		- Galera Cluster replicates at the row level, allowing for highly efficient parallel replication of transactions.
    
	- **Direct Client Connections:** 
		- Applications can connect directly to any node in the cluster, providing a native MariaDB experience.
    
	- **InnoDB Storage Engine Support:** 
		- Galera Cluster primarily supports the InnoDB storage engine, which is the recommended transactional storage engine for MariaDB.
    
	- **Linux Only:** 
		- MariaDB Galera Cluster is specifically designed for and supported on Linux operating systems.
    
	- **Powered by MariaDB Server and Galera wsrep provider library:** 
		- The functionality is embedded within standard MariaDB Server packages, requiring only the additional installation of the Galera wsrep provider library.
    

### **Benefits of MariaDB Galera Cluster (Community Edition):**

- **High Availability:** 
	- Eliminates single points of failure, as any node can take over if another fails.

- **No Data Loss:** 
	- Due to synchronous replication, transactions are committed on all nodes, preventing data loss in case of a node failure.

- **Read and Write Scalability:** 
	- Allows distributing both read and write workloads across multiple nodes.

- **No Replica Lag:** 
	- Ensures all nodes have up-to-date data.

- **Simpler Management (compared to manual replication setups):** 
		- Automatic failover and node joining simplify operations.


### **Other Clustering Considerations and Tools:** 

- **Standard Asynchronous/Semi-synchronous Replication:** 
	- Link refer: [link](https://mariadb.com/docs/server/ha-and-performance/standard-replication/semisynchronous-replication)
	- MariaDB also supports traditional master-slave (or master-master) replication, where data is copied from one server to another.
	- This provides read scalability and some redundancy but doesn't offer the same level of data consistency or automatic failover as Galera.

- **MaxScale (with limitations for Community):** 
	- Link refer: [link](https://mariadb.com/docs/maxscale)
	- MariaDB MaxScale is a database proxy that can provide features like load balancing, automatic failover, and read/write splitting. 
	- While the community edition of MariaDB allows using MaxScale, its free usage is typically limited to a small number of database nodes, making it less suitable for production deployments without an enterprise license.

- **ProxySQL:** 
	- Link refer: [link](https://proxysql.com/) 
	- Similar to MaxScale, ProxySQL is another popular open-source proxy that can be used in conjunction with MariaDB clusters for intelligent routing and load balancing.

- **ClusterControl:** 
	- Link refer: [link](https://mariadb.com/files/ClusterControl.pdf)
	- Tools like ClusterControl (by Severalnines) can help automate the deployment, management, monitoring, and scaling of MariaDB Galera Clusters, streamlining operations. 


### **Key Differences between Community and Enterprise for Clustering:**

- It's important to note that while MariaDB Community Server provides the core Galera Cluster functionality, MariaDB Enterprise Platform offers additional features and support for production-critical environments:

- **Enhanced Security:** 
	- Enterprise includes features like data-at-rest encryption for Galera's write-set cache, which is not available in Community.

- **Non-blocking DDLs:** 
	- Enterprise offers non-blocking DDL (Data Definition Language) statements for Galera, reducing the impact of schema changes on the cluster.

- **Enterprise Backup:** 
	- Improved backup solutions with optimized lock handling.

- **Long-Term Support (LTS):** 
	- Enterprise versions typically have longer maintenance periods.

- **Professional Support:** 
	- Enterprise provides 24/7 technical support with guaranteed SLAs.


### **Summary:**
 - MariaDB Community Server, powered by MariaDB Galera Cluster, provides a robust and open-source solution for building highly available and scalable database clusters, particularly for those comfortable with self-managing and leveraging community support. 
 
 - For mission-critical deployments and advanced features, MariaDB Enterprise Platform might be a more suitable choice.

### **MindMap for document:**
![[Pasted image 20250711101356.png]]
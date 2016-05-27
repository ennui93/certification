Databases
=================

DB 101
-----------
* Types
    * Relational: OLTP
        * Microsoft SQL Server
        * Oracle
        * MySql
        * PostgreSQL
        * MariaDB
        * Aurora
    * NoSql
        * DynamoDB
    * Warehouses: OLAP
        * Redshift
    * Cache
        * Elasticache - In-memory cache
            * Memcached
            * Redis
        
Relational
------------
* Backup
    * Automated
        * With retention period, 7 days by default
        * Stored in S3 with equal amount of storage
        * Deleted automatically when database deleted
        * For point in time recovery
    * Manual Snapshots
        * Remain after database is deleted
        * Can share with other AWS accounts
        * IO operations are suspended while taking a snapshot
    * During a defined window
    * Restores to a new RDS instance
    * Can Migrate to another type
* Encryption available for some types with KMS
* Multi-AZ
    * For DR, not for performance scaling
    * Cannot use secondary instance as a read node
    * Data replication is free
* Read Replicas
    * For performance improvement of read intensive work loads
    * Not for all Types
    * Data replication is free of charge
* Scale up
    * Take snapshot
    * Restore to a larger instance
* 6 TB max size of storage volume with Provisioned IOPS
    * MySQL/MariaDB
    * Oracle
    * PostgreSQL
* 4 TB max size
    * SQL Server
    
    

DynamoDB
-----------------
* Always on SSD
* In 3 AZ
* Scales without downtime
* Consistency
    * Eventually Consistent Reads (Default)
        * Eventual consistency after 1 second. Repeating a read after a short time should return updated data
    * Strongly Consistent Reads
        * Returns a result which reflects all successful writes
* Pricing
    * Provisioned Throughput Capacity
        * Writes $0.0065 per 10 units
        * Reads  $0.0065 per 50 units
    * Used storage
        * $0.25 per GB

Redshift
-----------
* Single Node
    * 160 GB of storage
* Multi-Node (MPP)
    * Leader Node
        * Manages client connections
    * Up to 128 Compute Nodes
* Columnar Data Storage
    * Faster
    * Better compression
* Pricing
    * 1 unit per node per hour
        * Not billed for leader nodes
    * Backups
    * Data transfer
* Encryption
    * Both at rest and in transit
* In one AZ
* Can restore to another AZ

Elasticache
----------
* In-memory cache service for
    * Read-heavy apps
    * Computation-heavy apps
* Supports
    * Memcached
        * Widely adopted object cache
        * No support for multi-AZ
    * Redis
        * Open source in-memory key-value store which supports sorted sets and lists
        * With master-slave replication and multi-AZ 

Aurora
-----------------
* Auto-scaled storage in 10 GB increments up to 64 TB
* Scalable compute up to 32 vCPUs and 244 GB RAM
* 2 copies of data in 3 AZ = 6 copies of data
* Designed to transparently handle the loss of up to 
    * 2 copies of data without affecting writes
    * 3 copies of data without affecting reads
* Replicas
    * Aurora replicas 
        * Up to 15 replicas
        * Automatic failover
    * MySql read replicas 
        * Up to 5 replicas
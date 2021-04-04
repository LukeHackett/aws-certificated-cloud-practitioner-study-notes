# The Core Database Services

AWS provides a number of managed database services. The Cloud Practitioner exam requires you to understand the following three different managed database services:

- Relational database service (RDS) 
- DynamoDB
- Redshift



## Amazon Relational Database Service

- RDS lets you provision a number of popular relational database management systems (RDBMSs) including Microsoft SQL Server, Oracle, MySQL, and PostgreSQL.
- Each RDS database instance cannot be secure shell (SSH) into, but they are connected to a virtual private cloud (VPC) of your choice.
- RDS instances use Elastic Block Service (EBS) volumes for storage.
- Multi-AZ deployments are supported, allowing you to have database instances in multiple Availability Zones.
- RDS can also perform manual or automatic EBS snapshots that you can easily restore to new RDS instances.

### Database Engines

- Amazon RDS supports the following six database engines:
  - MySQL
  - MariaDB
  - Oracle
  - PostgreSQL
  - Microsoft SQL Server
  - Amazon Aurora

- With the exception of Amazon Aurora, these database engines are either open source or commercially available products found in many data centre environments.
- Amazon Aurora is a proprietary database designed for RDS, but is compatible with MySQL and PostgreSQL databases.

### Licensing

- Depending on the database engine you choose, you must choose one of two licensing options.
  - **License included** — the license is included in the pricing for each RDS instance, MS SQL and Oracle include the license costs within the hourly fees.
  - **Bring your own license (BYOL)** - you must provide your own license to operate the database engine you choose (available only for Oracle databases)

### Instance Classes

- When deploying an RDS instance, you must choose a database instance class that defines the number of virtual CPUs (vCPU), the amount of memory, and the maximum network and storage throughput the instance can support. 
- There are three instances classes you can choose from: Standard, Memory Optimised, and Burstable Performance.
  - **Standard**
    - Designed for the majority of most applications.
    - Supports 2 - 96 vCPU and 8 - 384GB of memory.
  - **Memory Optimised**
    - Designed for applications with the most demanding database requirements, offering the most disk throughput and network bandwidth available.
    - Supports 4 - 128 vCPU and 122 - 3904GB of memory.
  - **Burstable Performance**
    - Designed for non-production workloads, that have minimal performance requirements, offering the lowest network bandwidth and disk throughput.
    - Supports 2 - 8 vCPU and 1 - 32GB of memory.

### **Scaling Vertically**

- Scaling vertically refers to changing the way resources are allocated to a specific instance. 
- After creating an instance, you can scale up to a more powerful instance class or you can save down to a less powerful instance class.

#### Storage

- The maximum throughput a volume can achieve is a function of both the instance class and the number of *input/output operations per second* (IOPS) the EBS volume supports.
- IOPS measure how fast you can read from and write to a volume - the higher the IOPS the faster the faster reads and writes will be.
- RDS offers three types of storage: general-purpose SSD, provi- sioned IOPS SSD, and magnetic
  - **General-Purpose SSD**
    - Designed for most databases.
    - Allocations of 20GB - 32TB are supported.
    - Higher storage levels support better read and write performances, and bursting is supported.
  - **Provisioned IOPS SSD**
    - Designed to allow you to set the IOPS that you want to achieve per instance.
    - Allocations of 20GB - 32TB are supported.
    - Provisioned IOPS SSD storage doesn’t offer bursting.
  - **Magnetic**
    - Magnetic storage is available for backward compatibility with legacy RDS instances. It doesn’t use EBS, and you can’t change the size of a magnetic volume after you create it.
    - Magnetic volumes are limited to 4 TB in size and 1,000 IOPS
- Note: You can increase the size of an EBS volume after creating it without causing an outage or degrading performance. You can’t, decrease the amount of storage allocated.
- You can also migrate from one storage type to another, but doing so can result in a short outage of typically a few minutes.

### Scaling Horizontally (Read Replicas)

- In a relational database, only the master database instance can write to the database. 
- A read replica helps with performance by removing the burden of read-only queries from the master instance, freeing it up to focus on writes.
- Hence, read replicas provide the biggest benefit for applications that need to perform a high number of reads

### High Availability with Multi-AZ

- Only the master database instance can perform writes, and hence if that goes down, the application will not be able to perform writes (only reads) until the master node is returned.
- To ensure that you always have a master database instance up and running, you can configure high availability by enabling the multi-AZ feature on your RDS instance.
- When multi-AZ is enabled, RDS creates an additional instance called a *standby database instance* that runs in a different Availability Zone than your primary database instance. 
- The primary instance replicates data to the secondary instance, so that if the primary instance fails, RDS will automatically fail over the secondary. 
- Failovers can take up-to 2 minutes, so your application will experience some interruption, but you won’t lose any data.
- With multi-AZ enabled, you can expect your database to achieve a monthly availability of 99.95 percent.


### Backup and Recovery

- RDS can take manual or automatic EBS snapshots of your instances, each being stored across multiple Availability Zones.
- If you ever need to restore from a snapshot, RDS will restore it to a new instance.
- You can take a manual snapshot at any time or you can configure automatic snapshots to occur at set intervals.
- RDS will retain automatic snapshots between 1 and 35 days, with a default of 7 days, while manual snapshots are retained until they are manually deleted.
- Enabling automatic snapshots also enables point-in-time recovery, a feature that saves your database change logs every 5 minutes. Combined with automated snapshots, this gives you the ability to restore a failed instance to within 5 minutes before the failure — losing no more than 5 minutes of data.

### Determining Your Recovery Point Objective

- How much data loss you can sustain in the event of a failure is called the *recovery point objective* (RPO).
- If you can tolerate losing an hour’s worth of data, then your RPO would be 1 hour.
- For an RPO of less than 5 minutes, you would also want to use multi-AZ to synchronously replicate your data to a secondary instance, while for RPOs higher than one hour snapshots would be more benefical.

### Snapshots

- Snapshots are good for letting you restore an entire database instance.
- If your database encounters corruption, such as malicious deletion of records, snapshots let you recover that data, even if the corruption occurred days ago.

#### Multi-AZ

- Multi-AZ is designed to keep your database up and running in the event of an instance failure
- This is achieved by ensuring data is synchronously replicated to a secondary instance.




## DynamoDB

- DynamoDB is Amazon’s managed non-relational database service. 
- It’s designed for highly transactional applications that need to read from or write to a database tens of thousands of times a second.
- DynamoDB is an ideal choice for applications that need to store a wide variety of data without having to know the nature of that data in advance.

### Items and Tables

- An item is analogous to a row or record in a relational database, each item is stored within a table.
- Each table is stored across one or more partitions, and partitions are replicated across multiple Availability Zones in a region, giving you a monthly availability of 99.99 percent.
- Each item must have a unique value for the primary key. 
- An item can also consist of other key-value pairs called *attributes*, with each item being able to store up to 400 KB of data.
- Every attribute must have a defined data type, which can be one of the following:
  - **Scalar** - a single value that can be a string, number, binary data or a boolean.
  - **Set** - a collection of multiple scalar values, with each value being unique within the set.
  - **Document** - a document of either an ordered list or an (unordered) map - often JSON documents are stored within the document type. DynamoDB can recognise and extract specific values nested within a document, allowing you to retrieve only the data you’re interested in without having to retrieve the entire document.

### Scaling Horizontally

- DynamoDB uses the primary key to distribute items across multiple partitions, which allows for consistency fast read and writes.
- The number of partitions DynamoDB allocates to your table depends on the number of write capacity units (WCU) and read capacity units (RCU) you allocate to your table.
- The higher the transaction volume and the more data you’re reading or writing, the higher your RCU or WCU values should be.
- Higher values cause DynamoDB to distribute your data across more partitions, increasing performance and decreasing latency. 
- As demand on your DynamoDB tables changes, you can change the number of RCU and WCU accordingly. 
- Alternatively, you can configure DynamoDB Auto Scaling to dynamically adjust the number of WCU and RCU based on demand.

### Queries and Scans

- Non-relational databases let you quickly retrieve items from a table based on the value of the primary key.
- Searching for a value in an attribute other than the primary key is possible, but it's slower.
- To locate all items with a Username that starts with the letter *p*, you’d have to perform a scan operation to list all items in the table.



## Amazon Redshift

- *Amazon Redshift* is a specialised type of managed relational database called a *data warehouse* that stores large amounts of structured data from other relational databases and allows you to perform complex queries and analysis against that data.
- For example, Redshift can combine data from financial, sales, and inventory databases into a single data warehouse and then analyse or generate reports on that data.
- To use Redshift, you create a cluster consisting of at least one compute node and up to 128 nodes.
- Using *dense compute* nodes, you can store up to 326 TB of data on magnetic disks, and with *dense storage* nodes you can store up to 2 PB of data on SSDs.
- The Redshift Spectrum is a feature of Redshift that lets you analyse data stored in S3 - although the data must be in a structured, and must the structure myst be defined so that Redshift can understand it.


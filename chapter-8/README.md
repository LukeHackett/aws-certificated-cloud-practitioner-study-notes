# The Core Storage Services




## Simple Storage Service (S3)

- Simple Storage Service (S3) lets you store and retrieve unlimited amounts of data. Because it’s part of the AWS ecosystem, data in S3 is available to other AWS services, including EC2, making it easy to keep storage and compute together for optimal performance.
- Many AWS services use S3 to store logs, retrieve data for processing, or to host static websites.

### Objects and Buckets

- S3 stores files (or *objects*) on disks in AWS data centres, which is different from EBS which stores blocks of raw data.
- You can store any type of file up to 5TB in size
- Each file's name is known as a key, and can be unto 1024 bytes long, and can consist of alphanumeric characters and some special characters, including `! - _ . * ( )`
- S3 stores objects in a container called a *bucket* that’s essentially a flat file system (folders can be created bu including a forward slash in the object name). Each bucket must have a name that is between 3 and 63 characters long, and the name *must be globally unique* across AWS (this is to ensure urls are mapped to the correct object and bucket).
- Data is technically only stored in one region, but can be replicated across multiple regions.
- You’re not charged for uploading data to S3, but you may be charged when you download data, as well as being charged based upon how much data you store and the storage class you use.

### S3 Storage Classes

- Objects that need to remain intact and free from inadvertent deletion or corruption are said to need high *durability*, which is the percent likelihood that an object will not be lost over the course of a year. The greater the durability of the storage medium, the less likely you are to lose an object.
- *Availability* is the percent of time an object will be available for retrieval. For instance, a patient’s medical records may need to be available around the clock, 365 days a year. Such records would need a high degree of both durability and availability.
- The level of durability and availability of an object depends on its storage class. S3 offers six different storage classes at different price points. 

| Storage Class              | Durability    | Availability | Availability Zones | Recommended Access Level | Example Pricing <br />(per GB/month)                         |
| -------------------------- | ------------- | ------------ | ------------------ | ------------------------ | ------------------------------------------------------------ |
| `STANDARD`                 | 99.999999999% | 99.99%       | \>2                | Frequent                 | $0.023                                                       |
| `STANDARD_IA`              | 99.999999999% | 99.9%        | \>2                | Infrequent               | $0.0125                                                      |
| `INTELLIGENT_TIERING`      | 99.999999999% | 99.9%        | \>2                | Frequent and Infrequent  | Frequent access tier: $0.023<br />Infrequent access tier: $0.0125 |
| `ONEZONE_IA`               | 99.999999999% | 99.5%        | 1                  | Infrequent               | $0.01                                                        |
| `GLACIER`                  | 99.999999999% | Varies       | \>2                | Infrequent               | $0.004                                                       |
| `REDUCED_REDUNDANCY (RRS)` | 99.99%        | 99.99%       | \>2                | Frequent                 | $0.024                                                       |

#### Frequently Accessed Objects

##### `STANDARD`

- This is the default storage class.
- It offers the highest levels of durability and availability.
- Objects are always replicated across at least three Availability Zones in a region.

##### `REDUCED_REDUNDANCY`

- This storage class is meant for data that can be easily replaced.
- It offers the lowest levels of durability and highest levels of availability.
- AWS recommends against using this storage class (but keeps it available for people who have processes that still depend on it).

#### Infrequently Accessed Objects

- Storage classesprefixed with `IA` offer millisecond-latency access and high durability but the lowest availability of all the classes.

##### `STANDARD_IA`

- This storage class is designed for important data that cannot be re-created.
- Objects are stored in multiple Availability Zones and have an availability of 99.9 percent.

##### `ONEZONE_IA`

- Objects stored using this storage class are kept in only one Availability Zone and consequently have the lowest availability of all the classes.
- The destruction of an Availability Zone could result in the loss of objects stored in that zone.
- Use this class only for data that you can re-create or have replicated elsewhere.

##### `GLACIER`

- The `GLACIER` class is designed for long-term archiving of objects that rarely need to be retrieved.
- Unlike the other storage classes, you can’t retrieve an object in real time, you must make a request for the data, the time in which this takes depends on the retrieval option you have selected — this ranges from 1 minute to 12 hours.

#### Frequently and Infrequently Accessed Objects

##### `INTELLIGENT_TIERING`

- This storage class automatically moves objects to the most cost-effective storage tier based on past access patterns.
- An object that hasn’t been accessed for 30 consecutive days is moved to the lower-cost infrequent access tier.
- Once the object is accessed, it’s moved back to the frequent access tier.
- In addition to storage pricing, you’re charged a monthly monitoring and automation fee.

### Access Permissions

- By default, objects you put in S3 are inaccessible to anyone outside of your AWS account
- S3 offers the following three methods of controlling who may read, write, or delete objects stored in your S3 buckets.
- You can use any combination of bucket policies, user policies, and access control lists — they are not mutually exclusive.
- **Bucket policies**
  - You can use bucket policies to grant access to all objects within a bucket or just specific objects.
  - You can also control which principals and accounts can read, write, or delete objects.
  - You can also grant anonymous read access to make an object, such as a webpage or image, available to everyone on the internet.
- **User policies**
  - You can use a User policy to grant IAM principals access to S3 objects.
  - You can’t use user policies to grant public (anonymous) access to an object.
- **Bucket and object access control lists**
  - Bucket and object access control lists (ACLs) are legacy access control methods that have mostly been superseded by bucket and user policies.
  - You can use bucket object ACLs to grant other AWS accounts and anonymous users access to your S3 resources.
  - Due to limitations of the bucket object ACLs, AWS recommends using bucket and user policies instead of ACLs whenever possible

### Encryption

- AWS does not encrypt data uploads by default
- S3 gives you the two options for encrypting objects — *server-side encryption* or *client-side encryption*.
- **Server-side encryption**
  - When you create an object, S3 encrypts the object and saves only the encrypted content. 
  - When you retrieve the object, S3 decrypts it and delivers the unencrypted object. 
  - Amazon manages the keys and therefore has access to your objects.
- **Client-side encryption**
  - You encrypt the data prior to uploading it to S3, and you must also decrypt the object when you retrieve it from S3.
  - If you lose the key used to encrypt an object, you won’t be able to decrypt it.
  - Organisations with strict security requirements may choose this option to ensure Amazon doesn’t have the ability to read their encrypted objects

### Versioning

- Versioning is disabled by default when you create a bucket. 
- You must explicitly enable versioning on a bucket to use it, and it applies to all objects in the bucket. 
- There’s no limit to the number of versions of an object you can store. 
- You can delete versions manually or automatically using object life cycle configurations.

### Object Lifecycle

- Because S3 can store practically unlimited amounts of data, it’s possible to run up quite a bill if you continually upload objects but never delete any.
- Object life cycle configurations can help you control costs by automatically moving objects to different storage classes or deleting them after a time.
- Object life cycle configuration rules are applied to a bucket and consist of *one or both* of the following types of actions:
- **Transition actions**
  - Transition actions move objects to a different storage class once they’ve reached a certain age
- **Expiration actions**
  - These can automatically delete objects after they reach a certain age.
  - If you have versioning enabled on a bucket, you can create expiration actions to delete object versions of a certain age.

It’s common to use both types of actions together. For example, if you’re storing web server log files in a bucket, you may want to initially keep each log file in `STANDARD` storage. Once the file reaches 90 days, you can have S3 transition it to `STANDARD_IA` storage. Then, after 365 days in STANDARD_IA storage, the file is deleted.




## S3 Glacier

- S3 Glacier offers long-term archiving of infrequently accessed data, such as backups that must be retained for many years.
- With Glacier, you store one or more files in an archive, which is a block of information. Although you can store a single file in an archive, the more common approach is to combine multiple files into a .zip or .tar file and upload that as an archive
- An archive can be anywhere from 1 byte to 40 TB.
- Glacier stores archives in a vault, which is a region-specific container (much like an S3 bucket) that stores archives. Vault names must be unique within a region but don’t have to be globally unique
- You can create and delete vaults using the Glacier service console. But to upload, download, or delete archives, you must use the AWS command line interface.
- Downloading an archive from Glacier is a two-step process that requires initiating a retrieval job and then downloading your data once the job is complete. 
- The length of time it takes to complete a job depends on the retrieval option you choose. 
- There are three retrieval options:
  - **Expedited** - retrievals usually complete within 1 to 5 minutes (unless there is high demand), although archive larger than 250MB may take longer, expedited retrievals are the most expensive.
  - **Standard** - retrievals usually complete within 3 to 5 hours, and is the default retrieval option. 
  - **Bulk** - retrievals usually complete within 5 to 12 hours, and is the cheapest option.



## AWS Storage Gateway

- AWS Storage Gateway is a virtual appliance that seamlessly moves data back and forth between your on-premises servers and AWS S3. It uses industry-standard storage protocols, making integration seamless.
- AWS Storage Gateway offers the following three virtual machine types for different use cases.

### File gateways

- A file gateway lets you use the Network File System (NFS) and Server Message Block (SMB) protocols to store data in Amazon S3.
- Data is stored on S3, but it’s cached locally, allowing for low-latency access.
- Because data is stored in S3, you can take advantage of all S3 features including versioning, bucket policies, life cycle management, encryption, and cross-region replication.

**Volume Gateways**

- Volume gateways offer S3-backed storage volumes that your on-premises servers can use via the Internet Small Computer System Interface (iSCSI) protocol.
- Volume gateways support the following two configurations:
  - **Stored volumes**
    - Storage Gateway stores all data locally and asynchronously backs it up to S3 as Elastic Block Store (EBS) snapshots
    - Stored volumes are a good option if you need uninterrupted access to your data
    - A stored volume can be from 1 GB to 16 TB in size
  - Cached volumes
    - The volume gateway stores all your data on S3, and only a frequently used subset of that data is cached locally.
    - A cached volume can range from 1 GB to 32 TB in size. 
    - This is a good option if you have a limited amount of local storage, but if there is an interruption in connectivity to AWS, then data may be inaccessible.
- Both configurations allow you to take manual or scheduled EBS snapshots of your volumes (EBS snapshots are always stored in S3).
- You can restore an EBS snapshot to an on-premises gateway storage volume or an EBS volume that you can attach to an EC2 instance.


**Tape Gateways**

- A tape gateway mimics traditional tape backup infrastructure.
- You simply configure your backup application to connect to the tape gateway via iSCSI.
- On the tape gateway, you create virtual tapes that can store between 100 GB and 2.5 TB each.
- When your backup software writes data to a virtual tape, the tape gateway asynchronously uploads that data to S3. Because backups can be quite large and take a long time to upload, the tape gateway keeps the data in cache storage until the upload is complete.



## AWS Snowball

- AWS Snowball is a hardware storage appliance designed to physically move massive amounts of data to or from S3, particularly when transferring the data over a network would take days or weeks.
- For example, suppose you want to migrate a 40 TB database to AWS. Such a transfer even over a blazing-fast 1 Gbps connection would still take more than 4 days!
- AWS will send you a Snowball device (for a fee), and you simply transfer your files to it and ship it back. When AWS receives it, AWS transfers the files from Snowball to one or more S3 buckets. 
- You’re not charged any transfer fees for importing files into S3.

### Hardware Specifications

- Snowball’s RJ45 and SFP+ network interfaces support speeds up to 10 Gbps
- Once you receive your Snowball, you can keep it for 10 days without incurring any additional costs ($15 per day after that).
- You’re allowed to keep Snowball for up to 90 days.
- You can use Snowball to export data from S3, but you’ll be charged out-bound S3 transfer rates.

### Security

- Snowball is contained in a rugged, tamper-resistant enclosure. It includes a trusted platform module (TPM) chip that detects unauthorised modifications to the hardware, software, or firmware. 
- AWS verifies that the TPM did not detect any tampering. If any tampering is detected by the TPM chip or if the device appears damaged, AWS does not transfer any data from it.
- Snowball uses two layers of encryption - SSL over ethernet, and the data is encrypted at rest (writing to the drive).
- As an added security measure, AWS erases your data from Snowball before sending it to another customer.

### Snowball Edge

- Snowball Edge is like Snowball but offers a wider variety of features such as a QSFP+ port, Local storage for S3 buckets, Compute power for EC2 instances, Lambda functions locally and File server functionality using the Network File System (NFS) protocol.
- You can cluster 5 to 10 Snowball Edge devices together to build a local, highly available compute or storage cluster. 

- There are three different device options to choose from, each optimised for a different application:
  - **Storage Optimised** - provides up-to 80TB of usable storage 24 vCPUs, and 32 GB of memory and a QSFTP+ network interface supporting transfer speeds of 40Gbps. 
  - **Compute Optimised** - provides 52 vCPUs and 208 GB of memory, as well as 39.5 TB of usable storage, plus 7.68 TB dedicated to compute instances. A QSFP+ network interface capable of speeds up to 100 Gbps is also included.
  - **Compute Optimised with GPU** - identical to the Compute Optimised option, except it includes an NVIDIA V100 Tensor Core GPUs, making it ideal for machine learning and high-performance computing applications.


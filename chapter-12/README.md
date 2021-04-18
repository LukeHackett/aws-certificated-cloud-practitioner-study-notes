# Common Use-Case Scenarios

- When running an application, you expect it to perform well and be reliable, secure, and cost-efficient.
- The AWS Well-Architected Framework codifies these principles and organizes them into the following five pillars:
  - Reliability
  - Performance efficiency
  - Security
  - Cost optimisation
  - Operational excellence



## The Well-Architected Framework

- The Well-Architected Framework is a set of principles that AWS recommends as a way of evaluating the pros and cons of designing and implementing applications in the cloud.

### Reliability

- *Reliability* means avoiding the complete failure of your application.
- Avoiding the complete failure of your application requires you to;
  - avoid the failure of the resources your application depends on (e.g misconfigured, overloaded, or shut down)
  - replace the failed resource with a healthy one (rather than try to fix the existing one)

### Performance efficiency

- *Performance efficiency* means getting the performance you desire without over provisioning capacity, but also without sacrificing reliability.

- You want enough resources to provide redundancy when a resource fails, and you also want to add resources as demand increases, as well as ensuring that at any given time, you have the right amount of resources for your given availability and performance requirements

- Performance and reliability are inextricably related - if a resource is overloaded, it’s likely that the application will perform poorly, and potentially bring the application down.

- Compute, storage and networking all affect the performance of an application - for example an application hosted within the United States will have high latency for users within Europe.

### Security

- *Security* is concerned with ensuring the confidentiality, integrity, and availability of data. 
- Only those people and systems that need access to data should have it, and that needs to be protected from unauthorised modifications.

- The following principles should always be followed:
  - Follow the principle of least privilege (for users and roles)
  - Avoid data loss by using backups and replication - for example EBS snapshots for EC2 instances and S3 versioning and replication
  - Enforce confidentiality by using encryption to protect data at rest as well as data transit
  - Track every activity that occurs on your AWS resources by enabling detailed logging

### Cost optimisation

- *Cost optimisation* isn’t about getting your costs as close to zero as possible, it's about using the cloud to meet your needs at the lowest possible cost.
- Using the AWS Cost Explorer and the Cost and Usage reports will inform you as to how you’re spending on AWS services and how those expenditures change over time.
- Use cost allocation tags to break down resource costs by owner, department, or application.
- One of the most effective ways to achieve cost optimisation is to pay only for the resources you need at any given time - for example turn off EC2 instances that are not being used.


### Operational excellence

- Operational excellence is fundamentally about automating the processes required to achieve and maintain the other four goals.
- As many organisations do not automate everything, operational excellence is more of a journey than a goal.
- The idea behind operational excellence is to incrementally improve and automate more activities for the purpose of strengthening the other pillars, examples include
  - **Reliability** - use elastic load balancing health checks to ensure users are also routed to a "healthy" EC2 instance.
  - **Performance efficiency** - EC2 Auto Scaling dynamic scaling policies to scale in and out automatically when demand increases and falls.
  - **Security** - use CodeBuild to automatically test new application code for security vulnerabilities, or use CloudFormation to automatically deploy fresh, secure infrastructure, rather than following a manual checklist
  - **Cost optimisation** - Automatically shut down or decommission unused resources, such as implementing S3 object life cycle configurations or automatically shutdown development EC2 instances at the end of the workday and restart them the following morning.
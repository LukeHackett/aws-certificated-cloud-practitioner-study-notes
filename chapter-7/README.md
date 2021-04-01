# The Core Compute Services



## Amazon Elastic Compute Cloud (EC2) Servers

### Amazon Machine Images

- An *image* is a software bundle that was built from a template definition and made available within a single AWS Region.
- The bundle can be copied to a freshly created storage volume that, once the image is extracted, will become a bootable drive that’ll turn the VM it’s attached to into a fully operational server.
- AMIs are available in multiple flavours, such as Ubuntu, Windows Server, or Amazon’s own Amazon Linux as well as popular software stacks such as OpenVPN, TensorFlow, or a Juniper firewall.
- AMIs are organised into four collections: the Quick Start set, any custom AMIs you might have created, the AWS Marketplace, and Community AMIs.

#### Quick Start

- AWS makes many popular AMIs easily available through the Quick Start tab on the Choose An Amazon Machine Image page.
- The available Linux distributions you’ll find here are mostly categorised as *long-term support* (LTS) releases.
- Windows Server and RHEL images will carry the normal licensing charges, which will be billed through your AWS account.
- There are also images optimised for deep learning, container hosting, .NET Core, and Microsoft SQL Server. 

#### AWS Marketplace and Community AMIs

- AWS Marketplace is a software store managed by Amazon where vendors make their products available to AWS customers as ready-to-run AMIs, CloudFormation stacks or containers
- Selecting a Marketplace AMI, will show its total billing cost, broken down into separate software license and EC2 infrastructure amounts
- You can also view and search through Marketplace offerings outside of the EC2 Launch Instance interface through the Marketplace site: https://aws.amazon.com/marketplace.
- In addition to vendors, end users can make and publish their own AMIs, to which can be found from within the Community AMIs tab. Given the informal sources of some Community AMIs, you must take responsibility for the security and reliability of AMIs you launch.

#### Creating Your Own AMIs

- It’s possible to convert any EC2 instance into an AMI by creating a snapshot from the EBS volume used with an instance and then creating an image from the snapshot. 
- Creating your own instance is often desired when a setup has been customised, and you would like to deploy that exact copy of the instance multiple times without have to setup each instance everytime.

### EC2 Instance Types

- An EC2 instance type is simply a description of the kind of hardware resources your instance will be using

- While, in general, the more vCPUs and memory you get, the better your instance will perform, that’s definitely not the whole story.

- vCPUs represent an instance type’s compute power compared to the number of processors on a physical machine.

- EC2 offers instance type families that are optimized for very different computing tasks, as shown below:
  | Family                | Instance Types                   |
  | --------------------- | -------------------------------- |
  | General purpose       | A1 T3 T2 M5 M5a M4 T3a           |
  | Compute optimised     | C5 C5n C4                        |
  | Memory optimised      | R5 R5a R4 X1e X1 High Memory z1d |
  | Storage optimised     | H1 I3 D2                         |
  | Accelerated computing | P3 P2 G3 F1                      |

- Note: the most up-to-date list of instances types can be found: https://aws.amazon.com/ec2/instance-types.

- If regulatory restrictions or company policy requires you to use only physically isolated instances, you can request either a ***dedicated instance*** or a ***dedicated host***. Both levels involve extra per-hour pricing and are available using on-demand, reserved, or spot models, but they have a few differences:

  - A ***dedicated instance*** is a regular EC2 instance that, instead of sharing a physical host with other AWS customers, runs on an isolated host that’s set aside exclusively for your account.
  - A ***dedicated host*** also gives you instance isolation but, in addition, allows a higher level of control over how your instances will be placed and run within the host environment. A dedicated host can also simplify and reduce the costs of running licensed software.

### Server Storage

Some EC2 instance types support only volumes from the Elastic Block Store (EBS), others get their storage from instance store volumes, and some can happily handle both.

#### Amazon Elastic Block Store

- The physical drive where an EBS volume actually exists may live quite a distance from the physical server that’s hosting your application — communication is performed over a super low-latency network connection spanning the data centre (rather than via SATA).
- Data stored within a EBS will survive shutdowns, restarts and system crashes
- EBS volumes can be encrypted
- EBS volumes can be moved around, mounted on other instances, or converted to AMIs

#### EC2 Instance Store Volumes

- Data is stored locally on the physical drives attached directly to the physical instance server, which increases data read and write speeds
- The downsides of instance store volumes include no encryption, no portability, and ephemeral data (not persisted between shutdowns, restarts and system crashes).

### EC2 Pricing Models

#### On-Demand Instances

- The most expensive pricing tier is on-demand, where you pay for every hour the instance is running, and is dependant upon the instance type and the region where it's running.
-  On-demand is great for workloads that need to run for a limited time without interruption, e.g. schedule an on-demand instance in anticipation of increased requests against your application.

#### Reserved Instances

- If your application needs to run uninterrupted for more than a month at a time, then you’ll usually be better off purchasing a reserved instance, with one and three year terms being widely available.
- When purchasing a reserved instance, you’re paying AWS (or other AWS customers who are selling reservations they are no longer using) for the *right* to run an EC2 instance at a specified cost during the reservation term.
- Reserved instances are paid for using an *All Upfront*, *Partial Upfront*, or *No Upfront* payment option — the more you pay up front, the less it’ll cost you overall.

#### Spot Instances

- For workloads that don’t need to run constantly and can survive unexpected shutdowns, the cheapest option will usually  involve requesting instances on the EC2 spot market.
- AWS makes unused compute capacity available at steep discounts — as much as 90% off the on-demand cost.
- The catch is that the capacity can, on two minutes’ notice, be reclaimed by shutting down your instance.
- Spot instances are not ideal when you require persistence and predictability, but can be great for certain classes of containerised big data workloads

## Simplified Deployments Through Managed Services

- To help lower the bar for entry into the cloud, some AWS services will handle much of the underlying infrastructure for you, allowing you to focus on your application needs. 
- The benefits of a managed service are sometimes offset by premium pricing — but it’s often well worth it.
- One step beyond a managed service — which handles only *one part* of your deployment stack for you (e.g. RDS) — is a managed *deployment*, where the *whole stack* is taken care of behind the scenes.

- Two AWS services created with managed deployments in mind are Amazon Lightsail and AWS Elastic Beanstalk.

### Amazon Lightsail

- Lightsail offers *blueprints* that, when launched, will automatically provision all the compute, storage, database, and network resources needed to make your deployment work (great for getting into cloud computing).
- You set the pricing level you’re after and add an optional script that will be run on your instance at startup, and AWS will take care of the rest.
- Lightsail bills at a flat rate (between $3.50 and $160 per month)
- Lightsail uses all the same tools — such as the AMIs and EC2 instances — to convert your plans to reality.
- Given that application stacks are packaged in blueprints, you won’t have the unlimited range of tools for your deployments that you’d get from EC2 itself.
- Blueprints include:
  - **Operating systems:** Amazon Linux, Ubuntu, Debian, FreeBSD, OpenSUSE, and Windows Server
  - **Applications:** WordPress, Magento, Drupal, Joomla, Redmine, and Plesk 
  - **Stacks:** Node.js, GitLab, LAMP, MEAN, and Nginx

### AWS Elastic Beanstalk

- Elastic Beanstalk is even simpler than Lightsail - all that’s expected from you is to define the application platform and then upload your code.
- You can choose between preconfigured environments (including Go, .NET, Java Node.js, and PHP) and a number of Docker container environments.

- Unlike Lightsail, Beanstalk generates costs according to how resources are consumed. You don’t get to choose how many vCPUs or how much memory you will use. Instead, your application will scale its resource consumption according to demand — keep this in mind, as such variations in demand will determine how much you’ll be billed each month.


## Deploying Container and Serverless Workloads

- Virtualised servers like EC2 instances tend to be resource-hungry, as they act like discrete, stand-alone machines running on top of a full-stack operating system. 
- Having 5 or 10 of those virtual servers on a single physical host involves some serious duplication because each one will require its own OS kernel and device drivers.

### Containers

- *Container technologies* such as Docker avoid a lot of that overhead by allowing individual containers to share the Linux kernel with the physical host.
- *Amazon Elastic Container Service* (ECS) or *Amazon Elastic Container Service for Kubernetes* (EKS) can be used to orchestrate swarms of Docker containers on AWS using EC2 resources.
- Both of those services manage the underlying infrastructure for you, allowing you to ignore the messy details and concentrate on administrating Docker itself.


### Serverless Functions

- The serverless computing model uses a resource footprint that’s even smaller than the one left by containers. 
- Not only do *serverless functions* not require their own OS kernel, but they tend to spring into existence, perform some task, and then just as quickly die within minutes, if not seconds.
- Lambda functions run only when triggered by a preset event
- If an hour or a week passes without a trigger, Lambda won’t launch a function (and you won’t be billed anything). 
- If there are a thousand concurrent executions, Lambda will scale automatically to meet the demand. 
- Lambda functions are short-lived: they’ll time out after 15 minutes.


# Automating Your AWS Workloads

- Automation allows common tasks to be repeated faster than doing them manually. 
- These tasks can be as simple as installing the latest security patches on an EC2 instance or as complex as creating a new AWS environment containing dozens of EC2 instances.
- Automation can be categorised into two approaches: 
  - **imperative** - focuses on the specific step-by-step operations required to carry out a task and is often using a scripting language such as bash
  - **declarative** - code is developed that declares the desired result of the task, rather than how to carry it out, examples include CloudFormation.
- Using code to define your infrastructure and configurations is commonly called the *infrastructure as code* (IaC) approach. 
- IaC is a fundamental element of automation in the cloud, as it allows for machines to execute the code, and thus reduces the  risk of human error inherent in carrying out the task.



## CloudFormation

- CloudFormation automatically creates and configures your AWS infrastructure from *templates* that defines the resources you want it to create and how you want those resources configured.

### Templates

- Templates use the proprietary CloudFormation language, which can be written in JSON or YAML format.
- Templates contain a description of the AWS resources you want CloudFormation to create, so they simultaneously function as infrastructure documentation.
- A single template can be used to build many similar environments, such as development, test and production environments.
- Templates can be configured to (optionally) take parameter inputs, allowing you to customise your resources without modifying the original template.

### Stacks

- A *stack* is a container that organises the resources described in the template, and is used to to collectively manage related resources - for example deleting a stack will remove all resources associated to the stack.
- All stack names must be unique within your account.
- Modifying a resource within a stack will cause CloudFormation to update just that resource (and any other related resources)

### Stack Changes

- A change set is a collection of changes that you have created.
- In addition to this, you can have CloudFormation generate a change set based on the template changes, and CloudFormation will let you review the specific changes it will make. You can then decide whether to execute those changes or leave everything as is.
- A stack policy can be created to prevent any unwanted changes to a stack - you can temporarily override the policy if you really do want to make a modification to the stack.
- Each stack contains a timestamped record of events that occur within the stack, including when resources were created, updated, or deleted. 
- CloudFormation stacks can be changed manually, but it's possible to have alerts raised when this happens - known as drift detection.

### CloudFormation vs AWS CLI

- AWS CLI can be used to create resources in a fast and automatic manner, but those resources won’t be kept in a stack.
- When using the CLI you need to make sure you correctly set the flags in order to perform the change, where as CloudFormation can workout the commands for you automatically.
- With the AWS CLI, it’s up to you to understand how to change each resource and to ensure that a change you make to one resource doesn’t break another.



## AWS Developer Tools

- The AWS Developer Tools are a collection of tools designed to help application developers develop, build, test, and deploy their applications onto EC2 and on-premises instances.



### CodeCommit

- CodeCommit is a git version control system that you can use to store source code, CloudFormation templates, documents, or any arbitrary files.

### CodeBuild

- CodeBuild is a fully managed build service that can compile, test your source code.
- You define your specific tasks in a build specification file that must be included with the source code. 
- CodeBuild can get source code from a CodeCommit, GitHub, or Bitbucket repository, or an S3 bucket.
- CodeBuild can perform multiple builds simultaneously. 
- Any artifacts that are created are stored in an S3 bucket, making them accessible to the rest of your AWS environment.
- CodeBuild performs builds in an isolated build environment that runs on a compute instance.
- AWS offers pre-configured build environments for Java, Ruby, Python, Go, Node.js, Android, .NET Core, PHP, and Docker, or alternatively you can create your own docker image.
- You can choose from the following three different compute types for your build environment:
  - `build.general1.small` - 3 GB of memory, 2 vCPU, Linux Only
  - `build.general1.medium` - 7 GB of memory, 4 vCPU, Linux and Windows
  - `build.general1.large` - 15 GB of memory, 8 vCPU, Linux and Windows

### CodeDeploy

- CodeDeploy can automatically deploy applications to EC2 instances, the Elastic Container Service (ECS), Lambda, and even on-premises servers.
- You must provide an application specification file that contains information about how to deploy the application.
- CodeDeploy can deploy applications, scripts, configuration files, and virtually any other file to an EC2 or on-premises instance - this requires the CodeDeploy agent to be installed.
- The agent allows the CodeDeploy service to copy files to the instance and perform deployment tasks on it.

- You can deploy to instances according to specifi c resource tags, based on Auto Scaling Group membership, or by just manually selecting them.
- CodeDeploy supports two different deployment types:
  - **In-place deployment** - deploys the application to the existing instances, and restarts the application after deployment (if required)
  - **Blue/Green deployment** - deploys the application to a new set of instances, and will automatically switch traffic to the new instances, removing the old instances afterwards if required.

### CodePipeline

- CodePipeline helps orchestrate and automate every task required to move software from development to production.
- It works by taking your source code and processing it through a series of stages, up to and including a deployment stage.
- Each stage consists of one or more actions, and these actions can occur sequentially or in parallel. There are six types of actions that you can include in a pipeline:
  - Source
  - Build
  - Test
  - Approval
  - Deploy
  - Invoke

- With the exception of the Approval action, each action is performed by a provider that depending on the action can be an AWS or third-party service:
  - **Source providers** - includes S3, CodeCommit, GitHub, and ECR
  - **Build and test providers** - includes CloudBees, Jenkins, and TeamCity
  - **Deployment providers** - includes CloudFormation, CodeDeploy, ECS, S3, Elastic Beanstalk, and OpsWorks Stacks
  - **Invoke** - only works with AWS Lambda
  - **Approval** - manual approvals can be inserted anywhere in the pipeline after the source stage



## EC2 Auto Scaling

- EC2 Auto Scaling automatically launches pre-configured EC2 instances, ensuring you have enough computing resources to meet user demand without over-provisioning.
- Auto Scaling works by spawning instances from either a launch configuration or a launch template.
- Launch templates are newer and can be used to spawn EC2 instances manually, even without Auto Scaling. You can also modify them after you create them. 
- Launch configurations can be used only with Auto Scaling - once you create a launch configuration, you can’t modify it.

### Auto Scaling Groups
Instances created by Auto Scaling are organized into an Auto Scaling group. All instances
in a group can be automatically registered with an application load balancer target group.

#### Desired Capacity

- When you configure an Auto Scaling group, you define a desired capacity - the number of instances that you want Auto Scaling to create.
- If you raise or lower the capacity, Auto Scaling launches or terminates instances to match.

#### Self-Healing

- If an instance fails or terminates, Auto Scaling re-creates a replacement. 
- Auto Scaling can use EC2 or ELB health checks to determine whether an instance is healthy.

### Scaling Actions

- Scaling actions control when Auto Scaling launches or terminates instances. 
- You control how many instances Auto Scaling launches or terminates by specifying a minimum and maximum
  group size.

#### Dynamic Scaling
- Auto Scaling launches new instances in response to increased demand using a process called scaling out . It can also scale in, terminating instances when demand ceases. 
- You can scale in or out according to a metric, such as average CPU utilisation of your instances, or based on the number of concurrent application users.

#### Scheduled Scaling

- Auto Scaling can scale in or out according to a schedule - this is particularly useful if your demand has predictable peaks and valleys.
- Predictive scaling is a feature that looks at historic usage patterns and predicts future peaks. It then automatically creates a scheduled scaling action to match. It needs at least one day’s worth of traffic data to create a scaling schedule.



## Configuration Management

- Configuration management is an approach to ensuring accurate and consistent configuration of your systems, and is primarily concerned with enforcing and monitoring the internal configuration state of your instances to ensure they’re what you expect.
- Configuration management tools use either imperative or declarative approaches, and can be used with EC2 and on-premises instances.

## Systems Manager

- Systems Manager uses the imperative approach to get your instances and AWS environment into the state that you want.

#### Command Documents

- Command documents are scripts that run once or periodically that get the system into the state you want.
- You can install software on an instance, install the latest security patches, or take inventory of all software on an instance.
- Systems Manager can run these periodically, or on a trigger, such as a new instance launch. 
- Systems Manager requires an agent to be installed on the instances that you want it to manage.

#### Configuration Compliance

- Configuration Compliance can show you whether instances are in compliance with your desired configuration state - for example is an instance up-to-date with the latest operating system security patches.

#### Automation Documents

- Systems Manager lets you perform many administrative AWS operations you would otherwise perform using the AWS Management Console or the AWS CLI - these are defined within an automation document.
- For example, you can use Systems Manager to automatically create a snapshot of an Elastic Block Store (EBS) volume, launch or terminate an instance.

#### Distributor

- The Systems Manager Distributor can deploy installable software packages to your instances.
- You create a .zip archive containing installable or executable software packages that your operating system recognises, put the archive in an S3 bucket, and tell Distributor where to find it.
- Distributor is especially useful for deploying a standard set of software packages that already come as installers or executables.

## Ops Works

- OpsWorks is a set of three different services that let you take a declarative approach to configuration management.
- OpsWorks uses two popular configuration management platforms that allow it to be a declarative configuration management system - [Chef](https://www.chef.io) and [Puppet Enterprise](https://puppet.com).

- AWS OpsWorks for Puppet Enterprise and AWS OpsWorks for Chef Automate are robust and scalable options that let you run managed Puppet or Chef servers on AWS. This is good if you want to use configuration management across all your instances.
- AWS OpsWorks Stacks provides a simple and flexible approach to using configuration management just for deploying applications.

### AWS OpsWorks for Puppet Enterprise and AWS OpsWorks for Chef Automate
- The high-level architectures of AWS OpsWorks for Puppet Enterprise and Chef Automate are similar. 
- They consist of at least one master server to communicate with your managed nodes — EC2 or on-premises instance — using an installed agent.
- You define the configuration state of your instances - such as OS applications - using Puppet modules or Chef recipes (these contain declarative code written in the platform's specific language).
- OpsWorks manages the servers, but you’re responsible for understanding and operating the Puppet or Chef software.

### AWS OpsWorks Stacks

- OpsWorks Stacks lets you build your application infrastructure in stacks using IaC.
- Each stack contains at least one layer, which is a container for some component of your application - for example a database layer containing a RDS instance or an application layer containing EC2 instances
- There are two basic types of layers that OpsWorks uses: OpsWorks layers and service layers.

#### OpsWorks layers

- An OpsWorks layer is a template for a set of instances. 
- It specifies instance-level settings such as security groups and whether to use public IP addresses. 
- It also includes an auto-healing option that automatically re-creates your instances if they fail.
- OpsWorks can also perform load-based or time-based auto scaling, adding more EC2 instances as needed to meet demand.
- OpsWorks uses the same declarative Chef recipes as the Chef Automate platform, but runs tasks the Chef Solo client that it installs on your instances automatically.

#### Service layers

- A stack can also include service layers to extend the functionality of your stack to include other AWS services.
- Service layers include the following layers:
  - Relational Database Service (RDS)
  - Elastic Load Balancing (ELB)
  - Elastic Container Service (ECS) Cluster


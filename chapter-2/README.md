# Understanding Your AWS Account

AWS supports more than 100 distinct services, which can each be deployed in hundreds or even thousands of permutations. This will require an understanding of how much each service will cost, as well as who has the authority to make the big decisions about firing up and shutting down account resources.

## The Free Tier

AWS offers a generous Free Tier for the first 12 months after opening a new account. Under the Free Tier, you can freely experiment with light versions of most AWS services without being billed.

Some of the more notable free usage levels are:

- Running a single Windows or Linux t2.micro EC2 instance for up to 750 hours per month
- Runing any number of EC2 instances (such as testing a high-availability deployment) so long as the total monthly hours of run time don’t exceed 750 hours
- Running a lightweight Amazon Relational Database Service (RDS) in any combination for up to 750 hours per month
- Storing up to 5 GB of data across any number of Simple Storage Service (S3) buckets
- 30 GB of either magnetic or solid-state drive (SSD) volumes from Amazon Elastic Block Storage (EBS)
- 500 MB/month of free storage on the Amazon Elastic Container Registry (ECR)
- 50 GB of outbound data transfers (per year) and 2 million HTTP(S) requests (per year) for Amazon CloudFront distributions
- One million API calls (per month) on Amazon API Gateway

If you go over the free tier limit, you'll be billed for the excess amount at the normal AWS billing rates.

There are actually *two* kinds of free services:

- Light implementations of most AWS services that are available through the first 12 months of a new AWS account
- Light usage of certain AWS services that are always available — even after your initial 12 months are up

Tracking your "Free Tier" usage can be done in two ways:

1. Waiting for emails from AWS, as by default, Free Tier–related email alerts are automatically sent whenever account activity approaches or has passed Free Tier limits.
2. Viewing all the "Free Services by Usage" table within the Billing Dashboard will give you a break down of all the services that are running, and how much they cost (as a percentage of the free tier limits)
Get requests into and out of S3 buckets also have their own limits.

Some AWS services can be useful even when consumed at levels so low that they don’t need to be billed. In such cases, AWS makes them available at those low levels over any time span. These use cases include the following:

- 10 custom monitoring metrics and 10 alarms on Amazon CloudWatch active at any given time
- 10 GB of data retrievals from Amazon Glacier per month
- 62,000 outbound email messages per month using Amazon Simple Email Service
- One million requests and 3.2 million seconds of compute time for AWS Lambda functions

*Note: AWS Free Tier benefits do not apply to resources launched in the GovCloud (US) region or through an AWS Organisations member account.*

## Product Pricing

As use more of the AWS infrastructure, you will need to keep an eye on costs within your organisation’s budget.

Each AWS service has it's own pricing web page, which can be found by adding `/pricing` to the end of the service's page, for example the AWS S3 service pricing can be found here: [https://aws.amazon.com/s3/pricing](https://aws.amazon.com/s3/pricing).

Each page will display tables highlighting the pricing levels per hour, month, or some other measured unit that’s appropriate to the service. Prices may very between AWS Regions, and different regions may offer differences across the global AWS infrastructure. Costs are often tiered, meaning that you pay less for each unit of what AWS produces that you consume when you “buy in bulk.”

AWS provides two modelling calculators to help you deduce what your total monthly costings will be when trying to deploy multiple services as part of a stack.

- The Simple Monthly Calculator helps you understand what *running* any combination of AWS resources will cost you
- The Total Cost of Ownership Calculator helps you compare what you’re currently paying for on-premises servers with what the same workload would cost on AWS

### AWS Simple Monthly Calculator

The Simple Monthly Calculator [https://calculator.s3.amazonaws.com/index.html](https://calculator.s3.amazonaws.com/index.html) can be used to calculate the costing for using various AWS Services. Choosing a service, will show a form, that can be filled in with the usages you are expecting. The calculator supports changing Regions, and adding multiple services.

On the right hand side of the page, a small collection of templates for common deployment scenarios can be found. The Disaster Recovery and Backup sample, for instance, estimates costs for running Linux database servers on EC2 in four geographically distinct regions and maintaining 500 GB of storage in Amazon S3.

When you’ve entered data for all the fields on all the service pages necessary to represent your plans, you can choose the Estimate tab at the top, and you’ll be shown a fully itemised breakdown of the estimated monthly cost of your resource stack. There is also an export to CSV option.

### AWS Total Cost of Ownership Calculator

The TCO Calculator can be found by visiting [https://aws.amazon.com/tco-calculator](https://aws.amazon.com/tco-calculator) page. The idea is that you enter the configuration specifications of your current on-premises deployment, and the calculator will produce a detailed, itemised estimate of what that workload is costing you in its current location and what it would cost on AWS.

The calculator incorporates estimates of capital infrastructure expenses including the amortised costs of servers, networking hardware, electricity, cooling, and maintenance, as well as the costs of an AWS business support plan.

## Service Limits

The total scope of the AWS cloud is vast, and the scalability of its design means that there shouldn’t be a practical ceiling on how much infrastructure any customer is able to access on demand.

However, AWS does impose limits on the scope of resources you can use. That’s why if you don’t properly plan ahead, your service requests might sometimes push your share of AWS resources past your account limit and fail.

For example, you’re allowed to run only 20 on-demand and 20 reserved instances of the EC2 m5.large instance type at any one time within a single AWS Region. This is due to the fact that all classes of resources should be reliably available to meet new demand and not all held by a few large customers. In addition to this, it can also protect customers from having automated processes accidentally (or maliciously) launch resources that generate unsustainable costs.

Most service limits can be adjusted by requesting a limit increase for your AWS your account.

You can find the complete and up-to-date list of service limits on this AWS documentation page [https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html).

## Billing and Cost Management

There are various tools found in your Billing Dashboard that can help with managing your account's finances. 

### The AWS Billing Dashboard

The main Billing & Cost Management Dashboard contains:

- A visual Spend Summary that displays your costs from the previous and current months so far
- A forecast of the costs you’ll likely face through the end of the month
- A Month-to-Date Spend By Service display from which choosing the Bill Details button will take you to an itemised breakdown of your current spending.
- Links to setting your payment methods and organisation’s tax information (if applicable)
- A list of all historical payment transactions

### AWS Budgets

An AWS budget is a tool for tracking a specified set of events so that when a preset threshold is approached or passed, an alert—perhaps (such as an email) is triggered.

There are *three* types of budgets:

1. **Cost budget** to monitor costs being incurred against your account
2. **Usage budget** to track particular categories of resource consumption
3. **Reservation budget** to help you understand the status of any active EC2, RDS, Redshift, or Elasticache reserve instances you might have

When setting up a budget, you first must set the terms of your budget (e.g. what are you tracking), and then you define how and when you want alerts sent.

### Monitoring Your Costs

- **Cost Explorer** [https://aws.amazon.com/aws-cost-management/aws-cost-explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer) lets you build graphs to visualise your account’s historical and current costs. The tool allows you to filter your account cost events by date, region, Availability Zone, instance type, platform, and others. CSV-formatted files based on customised views can also be downloaded.
- **Cost and usage reports** are accessed from the Reports link on the Billing Dashboard. Reports can be configured to include the full range of activity on your account, including what resources you have running and how much they’re costing you.
- **Cost Allocation Tags** are metadata identification elements representing a resource and its actions. Tags can be used to organise and track your resources, allowing you to visualise and better understand how resources are being used.

### AWS Organisations

AWS Organisations allows you to centralise the administration of multiple AWS accounts owned or controlled by a single company. This allows administrators to be able to control the allocation of resource permissions, security, and spending from a single location, as well as being able to manage payments.

Once accounts are linked, maintaining a secure profile and following best practices becomes even more critical than for standalone accounts. If a security breach of any one linked account occurs, then it runs the risk of spreading the vulnerability to everything running in *any* of your Organisation's accounts.
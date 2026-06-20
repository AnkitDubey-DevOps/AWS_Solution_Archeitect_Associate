## What is Serverless?
Serverless is a cloud computing model where developers focus only on writing and deploying code. They do not need to manage servers, operating systems, or infrastructure.

Although the term is "serverless," servers still exist. The difference is that the cloud provider (such as AWS) manages them on your behalf.

| Service           | Purpose                                 |
| ----------------- | --------------------------------------- |
| AWS Lambda        | Run code without managing servers       |
| DynamoDB          | Fully managed NoSQL database            |
| Cognito           | User authentication and authorization   |
| API Gateway       | Create and manage APIs                  |
| Amazon S3         | Store files and static website content  |
| SNS & SQS         | Messaging and queue services            |
| Kinesis Firehose  | Real-time data streaming                |
| Aurora Serverless | Auto-scaling relational database        |
| Step Functions    | Workflow orchestration                  |
| Fargate           | Run containers without managing servers |

# AWS Lambda

- AWS Lambda is a compute service that lets you run code without provisioning or managing servers.

- With AWS Lambda, you can run code for virtually any type of application or backend service — all with zero administration.

## AWS Lambda manages all the administration for you

1. Provisioning and capacity of the compute fleet that offers a balance of memory, CPU, network, and other resources.

2. Server and OS maintenance.

3. High availability and automatic scaling.

4. Monitoring fleet health.

5. Applying security patches.

6. Deploying your code.

7. Monitoring and logging your Lambda functions.

8. AWS Lambda runs your code on a high-availability compute infrastructure.

# AWS Lambda

- AWS Lambda runs your code on a high-availability compute infrastructure.

- AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second.

- You pay only for the compute time you consume. There is no charge when your code is not running.

- All you need to do is supply your code in the form of one or more Lambda functions to AWS Lambda, in one of the languages that AWS Lambda supports (currently Node.js, Java, Python, C#, Ruby, and Go), and the service can run that code on your behalf.

---

## AWS Lambda Application Lifecycle

Typically, the lifecycle for an AWS Lambda-based application includes:

1. Authoring code.
2. Deploying code to AWS Lambda.
3. Monitoring and troubleshooting.

---

## Limitations of AWS Lambda

This is in exchange for flexibility, which means:

- You cannot log into compute instances.
- You cannot customize the operating system.
- You cannot customize the language runtime.

If you want to manage your own compute resources, you can use:

- Amazon EC2
- AWS Elastic Beanstalk

# AWS Lambda vs EC2

| Feature | AWS Lambda | Amazon EC2 |
|----------|------------|------------|
| **Service Type** | Serverless compute service | Virtual machine (IaaS) |
| **Infrastructure Management** | AWS manages servers, scaling, and maintenance | You manage OS, patches, scaling, and infrastructure |
| **Provisioning** | No server provisioning required | Must launch and configure instances |
| **Scaling** | Automatic scaling based on incoming requests | Manual scaling or Auto Scaling Groups required |
| **Pricing Model** | Pay only for execution time and requests | Pay for instance uptime (running instances) |
| **Startup Time** | Can experience cold starts | Instance is continuously available once running |
| **Execution Duration** | Maximum 15 minutes per invocation | No execution time limit |
| **State Management** | Stateless by design | Can maintain state while instance is running |
| **Operating System Access** | No direct OS access | Full control over OS and environment |
| **Resource Limits** | Limited CPU, memory, and storage per function | Flexible CPU, memory, storage, and networking options |
| **Deployment Unit** | Function code | Virtual machine instance |
| **Best For** | Event-driven workloads, APIs, automation, scheduled jobs | Long-running applications, databases, custom software |
| **Maintenance Effort** | Very low | Higher operational overhead |
| **High Availability** | Built-in across AWS infrastructure | Requires configuration across multiple instances/AZs |
| **Monitoring** | Integrated with CloudWatch | Requires setup and monitoring configuration |

## When to Use AWS Lambda

Use AWS Lambda when:

- Running event-driven applications
- Building serverless APIs
- Processing files uploaded to S3
- Running scheduled tasks (cron jobs)
- Handling sporadic or unpredictable traffic
- Minimizing infrastructure management

## When to Use Amazon EC2

Use Amazon EC2 when:

- Running long-lived applications or services
- Hosting traditional web applications
- Needing full OS-level control
- Running custom software or agents
- Hosting databases or stateful workloads
- Requiring specialized hardware (GPU, high-memory instances)

### Benefits of AWS Lambda
1. Integrated with the whole AWS suite of services
2. Integrated with many programming languages
3. Easy monitoring through AWS CloudWatch
4. Easy to get more resources per functions (up to 10GB of RAM!)
5. Increasing RAM will also improve CPU and network!
6. Easy Pricing Pay per request and compute time
7. Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time

### AWS Lambda Pricing
1. AWS Lambda uses a pay-as-you-use pricing model.
2. Charges are based on:
                              Number of requests (invocations)
                              Function execution time (duration)
3. First 1 million requests per month are free.
4. After that, AWS charges approximately $0.20 per 1 million requests.
5. AWS provides 400,000 GB-seconds of free compute time every month.
6. Billing is calculated in milliseconds (ms).
7. Lambda is cost-effective because you pay only when your code runs.

### AWS Lambda Limits
#### Execution Limits
1. Memory allocation: 128 MB to 10 GB
2. Maximum execution time: 15 minutes (900 seconds)
3. Environment variables limit: 4 KB
4. Temporary storage (/tmp): 512 MB to 10 GB
5. Default concurrency limit: 1000 executions per region
#### Deployment Limits
1. Compressed deployment package (.zip): 50 MB
2. Uncompressed package size: 250 MB
3. Environment variables size: 4 KB

### What is Lambda Concurrency?

Lambda concurrency refers to the number of Lambda function instances that can run simultaneously at a given time.

1. AWS Lambda automatically creates multiple instances to handle requests.
2. Default concurrency limit is 1000 concurrent executions per region.
3. Concurrency helps Lambda scale automatically based on traffic.

### What is Throttling?

Throttling occurs when the number of incoming requests exceeds the available concurrency limit.

### What are Asynchronous Invocations?

In asynchronous invocation, AWS Lambda receives an event and immediately accepts it without waiting for the function to complete.

### What is a Cold Start?

When Lambda receives a request for the first time, AWS must:Create a container, Load code, Load libraries, Start the runtime

This takes some time.

This delay is called a Cold Start.

### What is Provisioned Concurrency?

Provisioned Concurrency keeps Lambda instances pre-initialized and ready to serve requests.

### Reserved Concurrency

Reserved Concurrency guarantees a specific number of concurrent executions for a Lambda function.

### Provisioned Concurrency

Provisioned Concurrency keeps Lambda environments already initialized and ready to process requests.

### Lambda Snapstart
Lambda SnapStart reduces startup time by restoring functions from a pre-initialized snapshot instead of initializing the runtime from scratch.

# AWS Lambda Part-1

## Important Terms used in AWS Lambda

### 1. Function

A function is a resource that you can invoke to run your code in AWS Lambda.

A function has:

- Code that processes events.
- A runtime that passes requests and responses between Lambda and the function code.

### 2. Runtime

Lambda runtimes allow functions written in different languages to run in the same base execution environment.

The runtime sits in between the Lambda service and your function code, relaying invocation events, context information, and responses between the two.

### 3. Event

An event is a JSON formatted document that contains data for a function to process.

### 4. Event Source / Trigger

An AWS service such as Amazon SNS, or a custom service, that triggers your function and executes its logic.

### 5. Downstream Resource

An AWS service, such as DynamoDB tables or S3 buckets, that your Lambda function calls once it is triggered.

### 7. Concurrency

The number of requests that your function is serving at any given time.

# AWS Lambda Part-2

## When Lambda Triggers

You can use AWS Lambda to run your code in response to:

- Events such as changes to data in an Amazon S3 bucket or an Amazon DynamoDB table.

- HTTP requests using Amazon API Gateway.

- With these capabilities, you can use Lambda to easily build data-processing triggers for AWS services like Amazon S3 and Amazon DynamoDB, process streaming data stored in Kinesis, or create your own backend that operates at AWS scale, performance, and security.

---

## Example of S3 Trigger

1. The user creates an object in an S3 bucket.

2. Amazon S3 detects the **Object Created** event.

3. Amazon S3 invokes your Lambda function using the permissions provided by the execution role.

4. Amazon S3 knows which Lambda function to invoke based on the event source mapping stored in the bucket notification configuration.

### Flow

```text
User
  |
  v
S3 Bucket (Object Created)
  |
  v
Amazon S3 detects event
  |
  v
AWS Lambda
  |
  v
Execution Role
  |
  v
Lambda Function
```

# AWS Lambda Part-2

## AWS Lambda Function Configuration

- A Lambda function consists of code and any associated dependencies.

- In addition, a Lambda function also has configuration information associated with it.

- Initially, you specify the configuration information when creating a Lambda function.

- Lambda provides an API for you to update some of the configuration data.

---

## Lambda Function Configuration Information

Lambda function configuration information includes the following key elements:

### Compute Resources

- You only specify the amount of memory you want to allocate for your Lambda function.

- AWS Lambda allocates CPU power proportional to the memory by using the same ratio as a general-purpose Amazon EC2 instance type, such as an M3 type.

- You can update the configuration and request additional memory in **64 MB increments**, from **128 MB to 3008 MB**.

- Functions larger than **1536 MB** are allocated multiple CPU threads.

# AWS Lambda Part-2

## Maximum Execution Timeout

- You pay for the AWS resources that are used to run your Lambda function.

- To prevent your Lambda function from running indefinitely, you specify a timeout.

- When the specified timeout is reached, AWS Lambda terminates your Lambda function.

- Default timeout is **3 seconds** and the maximum timeout is **900 seconds (15 minutes)**.

---

## IAM Role

- This is the role that AWS Lambda assumes when it executes the Lambda function on your behalf.

---

## AWS Lambda Function – Services It Can Access

Lambda functions can access:

- AWS services or non-AWS services.

- AWS services running in an AWS VPC  
  (e.g., Amazon Redshift, ElastiCache, RDS instances).

- Non-AWS services running on EC2 instances in an AWS VPC.

---

## VPC Access

- AWS Lambda runs your function code securely by default.

- However, to enable your Lambda function to access resources inside your private VPC, you must provide additional VPC-specific configuration information.

- This configuration includes:
  - VPC Subnet IDs
  - Security Group IDs

# AWS Lambda Part-2

## Different Ways to Invoke a Lambda Function

There are three ways to invoke a Lambda function:

1. **Synchronous Invoke (Push)**
2. **Asynchronous Invoke (Event)**
3. **Poll-Based Invoke (Pull-Based)**

---

## 1. Synchronous Invocation (Push)

Synchronous invocation is the most straightforward way to invoke your Lambda function.

- In this model, your function executes immediately when you perform the Lambda Invoke API call.
- The invocation flag specifies a value of **`RequestResponse`**.
- You wait for the function to process the event and return a response.

---

## 2. Asynchronous Invocation (Event)

For asynchronous invocation:

- Lambda places the event in a queue and returns a success response without additional information.
- Lambda queues the event for processing and returns a response immediately.
- You can configure Lambda to send an invocation record to another service such as:
  - Amazon SQS
  - Amazon SNS
  - AWS Lambda
  - Amazon EventBridge

### Services That Invoke Lambda Asynchronously

- Amazon S3
- Amazon SNS
- Amazon SES
- AWS CloudFormation
- Amazon CloudWatch Logs
- Amazon CloudWatch Events
- AWS CodeCommit
- AWS Config

### Asynchronous Invocation Flow

```text
Event Source
     |
     v
 Event Queue
     |
     v
 AWS Lambda Service
     |
     v
 Lambda Function
```

---

## 3. Poll-Based Invocation (Pull-Based)

The poll-based invocation model is designed to allow you to integrate with AWS stream and queue-based services with no code or server management.

Lambda polls the following services on your behalf, retrieves records, and invokes your function.

### Supported Services

- Amazon Kinesis
- Amazon SQS
- Amazon DynamoDB Streams

### Poll-Based Invocation Flow

```text
Amazon DynamoDB Streams / Kinesis / SQS
                 |
                 v
          AWS Lambda Service
                 |
                 v
           Lambda Function
```






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

## Difference between AWS Lambda vs EC2 

| EC2                              | Lambda                       |
| -------------------------------- | ---------------------------- |
| Server-based                     | Function-based               |
| Runs continuously                | Runs on demand               |
| Manual infrastructure management | Fully managed infrastructure |
| Manual or configured scaling     | Automatic scaling            |
| Pay for server uptime            | Pay for execution time       |

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




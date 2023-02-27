# EventBridge Non-Supported ECR API Call Notification

In this GitHub repository, you will get step by step guidance to deploy a Amazon CloudFormation (CFN) template to automatically create all the resources and settings required to enable SNS Notification for non-supported EventBridge Rule API Calls for Elastic Container Registry (ECR) Service. 

## Table of Contents

- [CloudFormation Template](#cloudformation-template)
  * [Architecture Diagram](#architecture-diagram)
  * [How to get-started?](#how-to-get-started)
  * [EventBridge Rule](#eventbridge-rule)
  * [SNS Topic](#sns-topic)
  * [Clean Up](#clean-up)
  * [License](#license)
     

## CloudFormation Template:

### Architecture Diagram:
![architecture-diagram](Architecture_Diagram.png)

### How to get-started?
To get-started, follow below steps:
1. Go into your CloudFormation Console and create stack **With new resources (Standard)**.
2. Upload template by selecting **Upload a template file**.
3. Specify the `Stack Name` and requested fields for EventBridge & SNS Configuration (Explore details about these in below sections). 
4. Post-configuring, go to next-page and leave all fields default.
5. In the next-page, click **Submit**.
6. Within few minutes, EventBridge Rule & SNS Topic will be created and you will receive subscription notification (accordingly which protocol you have selected in **Step 3**.

### EventBridge Rule:
The basis of **EventBridge** is to create rules that route events to a target. In this example, we are creating rule for a "AWS API Call via CloudTrail" Category which will be route Events for API Call triggered by eventSource: `ecr.amazonaws.com` for Target - **SNS Topic**.

Explore more about the parameters being asked in the Template:
1. `EventBridge Rule Name:` A rule can't have the same name as another rule in the same Region and on the same event bus.
2. `CTrail Event Name/API Call:` This is to be mentioned as per any API Call for ECR Service, which you can also explore at: [ECR API Operations](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html).

In each of the template, we are using Custom Event Pattern like below for Successful & Failure API Call:
- **Successful API Call:**
```
{
  "detail-type": ["AWS API Call via CloudTrail"],
  "source": ["aws.ecr"],
  "detail": {
    "eventSource": ["ecr.amazonaws.com"],
    "eventName": ["ReplicateImage"]
  }
}
```

- **Failure API Call:**
```
{
  "detail-type": ["AWS API Call via CloudTrail"],
  "source": ["aws.ecr"],
  "detail": {
    "eventSource": ["ecr.amazonaws.com"],
    "eventName": ["ReplicateImage"],
    "errorCode": [{
      "exists": true
    }]
  }
}
```

Checkout [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) for more details on Amazon EventBridge Service.

### SNS Topic:
**Amazon Simple Notification Service (Amazon SNS)** is a managed service that provides message delivery from publishers to subscribers (also known as producers and consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Clients can subscribe to the SNS topic and receive published messages using a supported endpoint type, such as Amazon Kinesis Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS).

Explore more about the parameters being asked in the Template:
1. `SNS Topic Name:` Enter any Topic Name such as _APISNSTopic_.
2. `SNS Protocol:` Based on your requirement, you can select SNS Protocol which will be used under **Subscription** creation.

Checkout [Amazon Simple Notification Service (SNS)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) for more details on Amazon SNS Service.

## Clean Up
Once you have deployed the CloudFormation resources and wish to delete from your accounts. Just navigate to the CloudFormation service on your AWS console, and then select the stack(s) that you'd like to remove and click on the `Delete` button. 

## License
This library is licensed under the MIT-0 License. See the LICENSE file.

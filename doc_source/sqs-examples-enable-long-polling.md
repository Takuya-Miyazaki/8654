--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Enabling long polling in Amazon SQS<a name="sqs-examples-enable-long-polling"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to enable long polling for a newly created queue\.
+ How to enable long polling for an existing queue\.
+ How to enable long polling upon receipt of a message\.

## The scenario<a name="sqs-examples-enable-long-polling-scenario"></a>

Long polling reduces the number of empty responses by allowing Amazon SQS to wait a specified time for a message to become available in the queue before sending a response\. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers\. To enable long polling, you must specify a non\-zero wait time for received messages\. You can do this by setting the `ReceiveMessageWaitTimeSeconds` parameter of a queue or by setting the `WaitTimeSeconds` parameter on a message when it is received\.

In this example, a series of Node\.js modules are used to enable long polling\. The Node\.js modules use the SDK for JavaScript to enable long polling using these methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html)

For more information about Amazon SQS long polling, see [Long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-enable-long-polling-prerequisites"></a>

To set up and run this example, you must first complete these tasks:

Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in JavaScript, so for consistency these examples are presented in JavaScript\. JavaScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Enabling long polling when creating a queue<a name="sqs-examples-enable-long-polling-on-queue-creation"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_longpolling_createqueue.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object containing the parameters needed to create a queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter\. Call the `CreateQueueCommand` method\. Long polling is then enabled for the queue\.

**Note**  
Replace and *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
import { CreateQueueCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = {
  QueueName: "SQS_QUEUE_NAME", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
};

const run = async () => {
  try {
    const data = await sqsClient.send(new CreateQueueCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_longpolling_createqueue.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_createqueue.js)\.

## Enabling long polling on an existing queue<a name="sqs-examples-enable-long-polling-existing-queue"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_longpolling_existingqueue.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object containing the parameters needed to set the attributes of the queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter and the URL of the queue\. Call the `SetQueueAttributesCommand` method\. Long polling is then enabled for the queue\.

**Note**  
Replace *SQS\_QUEUE\_URL* with the URL of the SQS queue, and *ReceiveMessageWaitTimeSeconds* with the number of seconds to wait before the message is received\.

```
// Import required AWS SDK clients and commands for Node.js
import { SetQueueAttributesCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = {
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
  QueueUrl: "SQS_QUEUE_URL", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
};

const run = async () => {
  try {
    const data = await sqsClient.send(new SetQueueAttributesCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_longpolling_existingqueue.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_existingqueue.js)\.

## Enabling long polling on message receipt<a name="sqs-examples-enable-long-polling-on-receive-message"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_longpolling_receivemessage.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object containing the parameters needed to receive messages, including a non\-zero value for the `WaitTimeSeconds` parameter and the URL of the queue\. Call the `ReceiveMessageCommand` method\.

**Note**  
Replace *SQS\_QUEUE\_URL* with the URL of the SQS queue, *MaxNumberOfMessages* with the maximum number of messages, and *WaitTimeSeconds* with the time to wait \(in seconds\)\.

```
// Import required AWS SDK clients and commands for Node.js
import { ReceiveMessageCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const queueURL = "SQS_QUEUE_URL"; // SQS_QUEUE_URL
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
  WaitTimeSeconds: 20,
};

const run = async () => {
  try {
    const data = await sqsClient.send(new ReceiveMessageCommand(params));
    console.log("Success, ", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_longpolling_receivemessage.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_receivemessage.js)\.
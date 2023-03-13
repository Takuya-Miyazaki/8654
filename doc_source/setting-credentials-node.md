--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Setting credentials in Node\.js<a name="setting-credentials-node"></a>

There are several ways in Node\.js to supply your credentials to the SDK\. Some of these are more secure and others afford greater convenience while developing an application\. When obtaining credentials in Node\.js, be careful about relying on more than one source, such as an environment variable and a JSON file you load\. You can change the permissions under which your code runs without realizing the change has happened\.

You can supply your credentials in order of recommendation:

1. Loaded from AWS Identity and Access Management \(IAM\) roles for Amazon EC2

1. Loaded from the shared credentials file \(`~/.aws/credentials`\)

1. Loaded from environment variables

1. Loaded from a JSON file on disk

1. Other credential\-provider classes provided by the JavaScript SDK

V3 provides a default credential provider in Node\.js\. So you are not required to supply a credential provider explicitly\. The default credential provider attempts to resolve the credentials from a variety of different sources in a given precedence, until a credential is returned from the one of the sources\. If the resolved credential is from a dynamic source, which means the credential can expire, the SDK will only use the specific source to refresh the credential\.

Here's the order of the sources where the default credential provider resolve credentials from:

1. Environment variables

1. The shared credentials file

1. Credentials loaded from the Amazon ECS credentials provider \(if applicable\)

1. Credentials loaded from AWS Identity and Access Management using the credentials provider of the Amazon EC2 instance \(if configured in the instance metadata\)

**Warning**  
We don't recommend hard\-coding your AWS credentials in your application\. Hard\-coding credentials poses a risk of exposing your access key ID and secret access key\.

The topics in this section describe how to load credentials into Node\.js\.

**Topics**
+ [Loading credentials in Node\.js from IAM roles for Amazon EC2](loading-node-credentials-iam.md)
+ [Loading credentials for a Node\.js Lambda function](loading-node-credentials-lambda.md)
+ [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)
+ [Loading credentials in Node\.js from environment variables](loading-node-credentials-environment.md)
+ [Loading credentials in Node\.js using a configured credential process](loading-node-credentials-configured-credential-process.md)
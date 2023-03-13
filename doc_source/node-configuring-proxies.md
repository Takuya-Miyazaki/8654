--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Configuring proxies for Node\.js<a name="node-configuring-proxies"></a>

If you can't directly connect to the internet, the SDK for JavaScript supports use of HTTP or HTTPS proxies through a third\-party HTTP agent\.

To find a third\-party HTTP agent, search for "HTTP proxy" at [npm](https://www.npmjs.com/)\.

To install a third\-party HTTP agent proxy, enter the following at the command prompt, where *PROXY* is the name of the `npm` package\. 

```
npm install PROXY --save
```

To use a proxy in your application, use the `httpAgent` and ` httpsAgent` property, as shown in the following example for a DynamoDB client\. 

```
import { DynamoDBClient } from '@aws-sdk/client-dynamodb';
import { NodeHttpHandler } from '@aws-sdk/node-http-handler';
import { HttpsProxyAgent } from 'hpagent';
const agent = new HttpsProxyAgent({ proxy: 'http://internal.proxy.com' });
const dynamodbClient = new DynamoDBClient({
    requestHandler: new NodeHttpHandler({
        httpAgent: agent,
        httpsAgent: agent
    }),
});
```

**Note**  
`httpAgent` is not the same as `httpsAgent`, and since most calls from the client will be to `https`, both should be set\.
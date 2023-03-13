--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Creating and calling service objects<a name="creating-and-calling-service-objects"></a>

The JavaScript API supports most available AWS services\. Each service in the JavaScript API provides a client class with a `send` method that you use to to invoke every API the service supports\. For more information about service classes, operations, and parameters in the JavaScript API, see the [API Reference](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/globals.html)\.

When using the SDK in Node\.js, you add the SDK package for each service you need to your application using `import`, which provides support for all current services\. The following example creates an Amazon S3 service object in the `us-west-1` Region\.

```
// Import the Amazon S3 service client
import {S3} from "@aws-sdk/client-s3"; 
// Create an S3 client in the us-west-1 Region
const s3Client = new S3.S3Client({
    region: "us-west-1"
  });
```

## Specifying service object parameters<a name="specifying-service-object-parameters"></a>

When calling a method of a service object, pass parameters in JSON as required by the API\. For example, in Amazon S3, to get an object for a specified bucket and key, pass the following parameters to the `GetObject` method\. For more information about passing JSON parameters, see [Working with JSON](working-with-json.md)\.

```
s3.getObject({Bucket: 'bucketName', Key: 'keyName'});
```

You can also call the `GetObjectCommand` method from the `S3Client`:

```
s3Client.send(new GetObjectCommand({Bucket: 'bucketName', Key: 'keyName'}));
```

For more information about Amazon S3 parameters, see [Class: S3](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/index.html) in the API Reference\.
--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Working with JSON<a name="working-with-json"></a>

JSON is a format for data exchange that is both human\-readable and machine\-readable\. Although the name JSON is an acronym for *JavaScript Object Notation*, the format of JSON is independent of any programming language\.

The AWS SDK for JavaScript uses JSON to send data to service objects when making requests and receives data from service objects as JSON\. For more information about JSON, see [json\.org](https://json.org)\.



JSON represents data in two ways:
+ As an *object*, which is an unordered collection of name\-value pairs\. An object is defined within left \(`{`\) and right \(`}`\) braces\. Each name\-value pair begins with the name, followed by a colon, followed by the value\. Name\-value pairs are comma separated\.
+ As an *array*, which is an ordered collection of values\. An array is defined within left \(`[`\) and right \(`]`\) brackets\. Items in the array are comma separated\.

Here is an example of a JSON object that contains an array of objects in which the objects represent cards in a card game\. Each card is defined by two name\-value pairs, one that specifies a unique value to identify that card and another that specifies a URL that points to the corresponding card image\.

```
var cards = [
  {"CardID":"defaultname", "Image":"defaulturl"},
  {"CardID":"defaultname", "Image":"defaulturl"},
  {"CardID":"defaultname", "Image":"defaulturl"},
  {"CardID":"defaultname", "Image":"defaulturl"},
  {"CardID":"defaultname", "Image":"defaulturl"}
];
```

## JSON as service object parameters<a name="json-as-parameters-passed"></a>

Here is an example of simple JSON used to define the parameters of a call to an AWS Lambda service object\.

```
const params = {
   FunctionName : "slotPull",
   InvocationType : "RequestResponse",
   LogType : "None"
};
```

The `params` object is defined by three name\-value pairs, separated by commas within the left and right braces\. When providing parameters to a service object method call, the names are determined by the parameter names for the service object method you plan to call\. When invoking a Lambda function, `FunctionName`, `InvocationType`, and `LogType` are the parameters used to call the `invoke` method on a Lambda service object\.

When passing parameters to a service object method call, provide the JSON object to the method call, as shown in the following example of invoking a Lambda function\.

```
  import { LambdaClient, 
   InvokeCommand } from "@aws-sdk/client-lambda";
  (async function() {
  const lambdaClient = new LambdaClient({ region: "us-west-2" });
  // create JSON object for service call parameters
  const params = {
    FunctionName : "slotPull",
    InvocationType : "RequestResponse",
    LogType : "None"
  };

  // create InvokeCommand command
  const command = new InvokeCommand(params);

  // invoke Lambda function
  try {
    const response = await lambdaClient.send(command);
    console.log(response);
  } catch (err) {
    console.err(err);
  }
})();
```
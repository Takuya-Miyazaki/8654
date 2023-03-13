--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Enforcing a minimum TLS version<a name="enforcing-tls"></a>

To add increased security when communicating with AWS services, configure the AWS SDK for JavaScript to use TLS 1\.2 or later\. 

**Important**  
The AWS SDK for JavaScript v3 automatically negotiates the highest level TLS version supported by a given AWS Service endpoint\. You can optionally enforce a minimum TLS version required by your application, such as TLS 1\.2 or 1\.3, but please note that TLS 1\.3 is not supported by some AWS Service endpoints, so some calls may fail if you enforce TLS 1\.3\.

Transport Layer Security \(TLS\) is a protocol used by web browsers and other applications to ensure the privacy and integrity of data exchanged over a network\.

## Verify and enforce TLS in Node\.js<a name="node-verify-enforce-tls"></a>

When you use the AWS SDK for JavaScript with Node\.js, the underlying Node\.js security layer is used to set the TLS version\.

Node\.js 12\.0\.0 and later use a minimum version of OpenSSL 1\.1\.1b, which supports TLS 1\.3\. The AWS SDK for JavaScript v3 defaults to use TLS 1\.3 when available, but defaults to a lower version if required\.

### Verify the version of OpenSSL and TLS<a name="verify-tls-version"></a>

To get the version of OpenSSL used by Node\.js on your computer, run the following command\.

```
node -p process.versions
```

The version of OpenSSL in the list is the version used by Node\.js, as shown in the following example\.

```
openssl: '1.1.1b'
```

To get the version of TLS used by Node\.js on your computer, start the Node shell and run the following commands, in order\.

```
> var tls = require("tls");
> var tlsSocket = new tls.TLSSocket();
> tlsSocket.getProtocol();
```

The last command outputs the TLS version, as shown in the following example\.

```
'TLSv1.3'
```

Node\.js defaults to use this version of TLS, and tries to negotiate another version of TLS if a call is not successful\.

### Enforce a minimum version of TLS<a name="enforce-tls-version"></a>

Node\.js negotiates a version of TLS when a call fails\. You can enforce the minimum allowable TLS version during this negotiation, either when running a script from the command line or per request in your JavaScript code\. 

To specify the minimum TLS version from the command line, you must use Node\.js version 11\.0\.0 or later\. To install a specific Node\.js version, first install Node Version Manager \(nvm\) using the steps found at [Node version manager installing and updating](https://github.com/nvm-sh/nvm#installing-and-updating)\. Then run the following commands to install and use a specific version of Node\.js\. 

```
nvm install 11
nvm use 11
```

------
#### [ Enforcing TLS 1\.2 ]

To enforce that TLS 1\.2 is the minimum allowable version, specify the `--tls-min-v1.2` argument when running your script, as shown in the following example\.

```
node --tls-min-v1.2 yourScript.js
```

To specify the minimum allowable TLS version for a specific request in your JavaScript code, use the `httpOptions` parameter to specify the protocol, as shown in the following example\.

```
const https = require("https");
const {NodeHttpHandler} = require("@aws-sdk/node-http-handler");
const {DynamoDBClient} = require("@aws-sdk/client-dynamodb");

const client = new DynamoDBClient({
    region: "us-west-2",
    requestHandler: new NodeHttpHandler({
        httpsAgent: new https.Agent(
            {
                secureProtocol: 'TLSv1_2_method'
            }
        )
    })
});
```

------
#### [ Enforcing TLS 1\.3 ]

To enforce that TLS 1\.3 is the minimum allowable version, specify the `--tls-min-v1.3` argument when running your script, as shown in the following example\.

```
node --tls-min-v1.3 yourScript.js
```

To specify the minimum allowable TLS version for a specific request in your JavaScript code, use the `httpOptions` parameter to specify the protocol, as shown in the following example\.

```
const https = require("https");
const {NodeHttpHandler} = require("@aws-sdk/node-http-handler");
const {DynamoDBClient} = require("@aws-sdk/client-dynamodb");

const client = new DynamoDBClient({
    region: "us-west-2",
    requestHandler: new NodeHttpHandler({
        httpsAgent: new https.Agent(
            {
                secureProtocol: 'TLSv1_3_method'
            }
        )
    })
});
```

------

## Verify and enforce TLS in a browser script<a name="browser-verify-enforce-tls"></a>

When you use the SDK for JavaScript in a browser script, browser settings control the version of TLS that is used\. The version of TLS used by the browser cannot be discovered or set by script and must be configured by the user\. To verify and enforce the version of TLS used in a browser script, refer to the instructions for your specific browser\.

------
#### [ Microsoft Internet Explorer ]

1. Open **Internet Explorer**\.

1. From the menu bar, choose **Tools** \- **Internet Options** \- **Advanced** tab\.

1. Scroll down to **Security** category, manually check the option box for **Use TLS 1\.2**\.

1. Click **OK**\.

1. Close your browser and restart Internet Explorer\.

------
#### [ Microsoft Edge ]

1. In the Windows menu search box, type *Internet options*\.

1. Under **Best match**, click **Internet Options**\.

1. In the **Internet Properties** window, on the **Advanced** tab, scroll down to the **Security** section\.

1. Check the **User TLS 1\.2** checkbox\.

1. Click **OK**\.

------
#### [ Google Chrome ]

1. Open **Google Chrome**\.

1. Click **Alt F** and select **Settings**\.

1. Scroll down and select **Show advanced settings\.\.\.**\.

1. Scroll down to the **System** section and click on **Open proxy settings\.\.\.**\.

1. Select the **Advanced** tab\.

1. Scroll down to **Security** category, manually check the option box for **Use TLS 1\.2**\.

1. Click **OK**\.

1. Close your browser and restart Google Chrome\.

------
#### [ Mozilla Firefox ]

1. Open **Firefox**\.

1. In the address bar, type **about:config** and press Enter\.

1. In the **Search** field, enter **tls**\. Find and double\-click the entry for **security\.tls\.version\.min**\.

1. Set the integer value to 3 to force protocol of TLS 1\.2 to be the default\.

1. Click **OK**\.

1. Close your browser and restart Mozilla Firefox\.

------
#### [ Apple Safari ]

There are no options for enabling SSL protocols\. If you are using Safari version 7 or greater, TLS 1\.2 is automatically enabled\.

------
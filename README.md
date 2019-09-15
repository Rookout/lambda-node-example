# Quickstart for Node + Rookout on AWS Lambda

A sample application for using Node + Rookout on AWS Lambda

Before following this guide we recommend reading the basic [Node + Rookout] guide


## Rookout Integration Explained

There are 3 simple steps to integrate Rookout into your existing Node application:

1. Add the npm dependency `rookout`

1. Wrap your lambda function with `rookout.wrap()`

1. Set the Rook's configuration as environment variables in the Lambda configuration


## Running on Lambda

**For this example we are using Amazon's [Node.js Function for an API with Lambda Proxy Integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html) hello world function**


1. Uploading your function : 
    - Zip Upload: In order to run your rookout wrapped function on Lambda, make sure the dependencies are downloaded and zip
    the folder (including node_modules).  
    `zip -r rookout_lambda_example.zip .`  
    [Download ready-to-upload zip](rookout_lambda_example.zip)
    
        **IMPORTANT:** _If you are building on a MacOS/Windows machine, npm will compile native binaries for this platform. AWS Lambda runs on Linux and thus needs the linux compiled binaries. The solution is doing `npm install` or `npm rebuild` on a Linux machine such as an EC2 instance and re-archive the zip for uploading to Lambda._

    - aws-cli -> Create a new Lambda function (It may take a few minutes due to uploading a large file size) :
        ```bash
        $ aws lambda create-function \
                    --region <REGION> \
                    --function-name rookout_lambda_example \
                    --zip-file fileb://rookout_lambda_example.zip \
                    --role <ROLE-ARN> \
                    --handler index.handler \
                    --runtime nodejs8.10 \
                    --environment Variables="{ROOKOUT_ROOK_TAGS=lambda,ROOKOUT_TOKEN=<org_token>}"
        ```  
        To create an API Gateway resource, refer to [AWS Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html)
        or create it through the AWS Lambda Console.
        **If you do not have access to aws-cli, you can do this from the [AWS console](https://console.aws.amazon.com/lambda/home/functions) and follow the [Amazon Documentation](https://docs.aws.amazon.com/lambda/latest/dg/get-started-create-function.html)**

    - **OR** Using Cloud9 IDE integrated tools


1. Set the Rook's agent configuration as environment variables in the Lambda configuration, fill the Environment Variables for :
    - `ROOKOUT_TOKEN` : Your Organization Token
    - `ROOKOUT_ROOK_TAGS` : lambda
    
    More information can be found in [our documentation](https://docs.rookout.com/docs/installation-agent-remote.html)

1. Go to [app.rookout.com](https://app.rookout.com) and start debugging !


## Rookout Integration Process

We have added Rookout to the original project by:
1. Installing rookout dependency : `npm install --save rookout` and adding it in the entry file `const rookout = require('rookout/lambda');`

1. Wrapping your function with the Lambda wrapper as such :  
`const rookout = require('rookout/lambda');`

```javascript
exports.handler = rookout.wrap((event, context, callback) => {
    ....
    ....
    ....
    callback(null, response);
});
```
    
1. Set Lambda environment `ROOKOUT_TOKEN` in order to connect
    

[Node + Rookout]: https://docs.rookout.com/docs/installation-node.html

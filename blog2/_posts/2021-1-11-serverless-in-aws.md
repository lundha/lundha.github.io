---
layout: post
title: Serverless in AWS
images:
  - url: /images/serverless-webapp.png
---

<img src="/images/serverless-webapp.png" width="800" height="400"/>


Working with software development can become a hassle when having to account for servers capacity, network traffic and different development environments. Moreover, capital expenses of acquiring and maintaing a server park is significant. A solution to these problems have been to use these servers as a service provided by a third part. 

Different levels of service are offered, depending on how much configurability, security and flexibility that are needed. The latest addition to these services are FaaS (Function as a service), which abstracts away all overhead configuration, leaving only the function calls to be configured. Serverless applications do not require you to provision, scale and manage any servers. 

AWS offers FaaS (function as a service) on different services, among these are Lambda, DynamoDB, Gateway API and S3. This post will utilize these four services to set up a data base and a static web site which can be used as a front end to collect data from the the data base. 

The architecture forthis serverless application can be found above. The client will communicate with the API through a static web site hosted in AWS S3. 

### Lambda function


The Lambda function is to be triggered from a call to the Gateway API. The handler will receive a key value and use it to make a query in the noSQL data base. This is done by using The DynamoDB DocumentClient which is a part of the AWS sdk. 

```
const AWS = require('aws-sdk');
var DocClient = new AWS.DynamoDB.DocumentClient();

var tableName = "DatabaseTable";

exports.handler = (event, context, callback) => {
    console.log(event.EmailID)

    var params = {
        TableName : tableName,
        Key:{
            "EmailID" : event.EmailID
        }
    }
    DocClient.get(params, function(err, data) {
      callback(err, data);  
    })
   
}
```
### Gateway API

Navigate to Gateway API and build a REST API. 

<img src="/images/gateway_api.png" width="587" height="180"/>

Create a new GET resource which will be connected to the Lambda function created earlier. The API will trigger the Lambda function when a call to the API has been made. 

<img src="/images/connect-api-lambda.png" width="400" height="250"/>

Further one need to map the values from the API call to a specified variable name. 

<img src="/images/mapping_template.png" width="300" height="280"/>

To enable the use of the gateway API on the static web site it is important to enable CORS. CORS is an acronym for Cross-Origin Resource Sharing. This enables the use of JavaScript to make authenticated GET and PUT requests.

Lastly, deploy the API.

### DynamoDB

The noSQL data base can be created with CloudFormation in AWS. CloudFormation is an Infrastructure as Code (IaC) service in AWS which enables infrastructure setup in text format, most common is YAML and JSON format. 
The following YAML file configures a DynamoDB table with the name DatabaseTablr and with EmailID as key value. 
```
Resources:
    ServerlessWebappDBTable:
        Type: AWS::DynamoDB::Table
        Properties:
          AttributeDefinitions:
            - AttributeName: EmailID
              AttributeType: S
          KeySchema:
            - AttributeName: EmailID
              KeyType: HASH
          ProvisionedThroughput:
            ReadCapacityUnits: 1
            WriteCapacityUnits: 1
          TableName: DatabaseTable

```

### Static web hosting
The static web site is written in HTML with the following function handling the API call. 
Upload the index.html file to a bucket in S3 and enable static website hosting with permissions described in bucket policy to host the web site in AWS. 
```
        $(document).ready(function() {

            var customerViewModel = function() {
            var self = this;
            self.firstName = ko.observable("");
            self.lastName = ko.observable("");
            self.emailId = ko.observable("");
            self.searchKey = ko.observable("");

            self.getCustomerDetails = function () {              
                $.ajax({
                    url: 'generated-aws-address/Dev/getcustomerdetailsbyemail',
                    cache: false,
                    type: 'GET',                   
                    data: { "EmailID": self.searchKey() },
                    success: function (data) {              
                        self.firstName(data.Item.FirstName)
                        self.lastName(data.Item.LastName),
                        self.emailId(data.Item.EmailID)
                    }
                });
            }
        }

            var viewModel = new customerViewModel();
            ko.applyBindings(viewModel);
         });
```

### Bucket policy 
The following policy will allow anyone to access the specified resource. 

```
{
    "Version": "2012-10-17",
    "Id": "",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::lundha-serverless-webapp/*"
        }
    ]
}
```

### Example

<img src="/images/example-result.png" width="480" height="345"/>

The image above shows the front end of the serverless function. 
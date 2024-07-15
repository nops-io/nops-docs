---
title: The Scheduler Slackbot "How To" Guide
keywords: setup, onboarding, essentials, sharesave, slackbot, developer, api
tags: [developer]
sidebar: mydoc_sidebar
permalink: developer-scheduler-slackbot.html
folder: Developer
series: [Essentials API]
weight: 3.0
---


### Prerequisites: ###

* Access to nOps Scheduler  
      
    Scheduler API doc:  
    [https://app.nops.io/svc/notifications/api/schema/swagger-ui/#/](https://app.nops.io/svc/notifications/api/schema/swagger-ui/#/)  
      
    nOps Slack Integration Lambda code in Github:   
    [https://github.com/nops-io/nops-rules-lambda/tree/master/scheduler/slack](https://github.com/nops-io/nops-rules-lambda/tree/master/scheduler/slack)

* nOps generated API key (we do not need to generate public and private keys for this SlackBot):
    * Log into the nOps console and from the **Settings** pane click the **API Key** option
    * Click on **Let’s Generate Your API Key**
    * You will see a confirmation dialog that contains your newly generated key.
    * Click the copy button on the right of the key, and save the key.You must download a copy of the key when it is generated as you will not be able to access it again.
* Access to AWS account. It is advisable to have an admin access

### Steps to create the Scheduler SlackBot:

1.  Login to nOps platform
2.  Go to Organization Settings > API Key > On the top right you will find a button “Generate New API Key” > Click to generate a new API Key and save it somewhere. We will need this later.
3.  Login to your AWS account Go to AWS Lambda > Create a aws chatbot that will integrate with the dedicated slack channel we setup to communicate with aws

**Use the following Lambda code to create the Scheduler SlackBot Lambda Function:**
```lambda
    import boto3
    import urllib3
    import json
    import os
    
    # Define the base URL for the RESTful API
    base_url = 'https://app.nops.io/svc/notifications/scheduler/nops_scheduler'
    
    # Define the function to make the POST request with access key
    def post_request(endpoint_url, request_body, access_key):
        # Set the API headers with the access key
        headers = {'Authorization': 'Bearer ' + access_key, "accept" : "application/json, text/plain, */*", "content-type": "application/json;charset=UTF-8"}
        encoded_data = json.dumps(request_body).encode('utf-8')
        http = urllib3.PoolManager()
    
        # Send the POST request to the API endpoint with the request body as JSON and headers
        response = http.request('POST', endpoint_url, body=encoded_data, headers=headers)
    
        # Return the response from the API
        return response
        
    # Define the main Lambda function to handle incoming chatbot requests
    def lambda_handler(event, context):
        # Get the input message and parse the arguments from the chatbot event
        schedule_id = event['schedule_id']
        schedule_action = event['schedule_action']
    
        # Get the access key for the API call from the Lambda environment variables
        access_key = os.environ['ACCESS_KEY']
    
        # Define the endpoint URL for the API based on the argument that parameterizes a portion of the REST URI
        endpoint_url = f"{base_url}/{schedule_id}/trigger?api_key={access_key}"
    
        # Define the request body for the API call based on the remaining arguments
        request_body = {
            "action_name": f"{schedule_action}"
        }
    
    
        # Make the POST request to the API with the endpoint URL, request body, and access key
        response = post_request(endpoint_url, request_body, access_key)
    
        # Build the response message to send back to the chatbot user
        response_message = f"The API response code: {response.status} (202 is success)"
    
        # Return the response message to the chatbot user
        return {
            "response_type": "in_channel",
            "text": response_message
        }
```
### **IAM Permissions to be attached to the Scheduler SlackBot Lambda Function:** ###

![](https://nops-help-site-assets.s3.amazonaws.com/images/solutions/dev-slackbot-lambda.png)


We need to set the ACCESS_KEY above in the Lambda configuration under _**‘Environment Variables’**_

4.  Once the chatbot is created, go to the Scheduler SlackBot Lambda Function channel and use the following command to invoke the Lambda function:

```
    @aws
     lambda invoke --payload {"schedule_id":"your_schedule_id", "schedule_action":"start"} --function-name invoke-scheduler --region us-west-2

```

> **Note:** You can use the following link to know “How to create an aws chatbot using Lambda”: [https://docs.aws.amazon.com/chatbot/latest/adminguide/chatbot-run-lambda-function-remotely-tutorial.html](https://docs.aws.amazon.com/chatbot/latest/adminguide/chatbot-run-lambda-function-remotely-tutorial.html)

{% include custom/series_related.html %}

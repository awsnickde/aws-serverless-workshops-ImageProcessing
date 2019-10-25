# Follow-up to ImageProcessing workshop
This repository contains additional exercises to the [AWS Serverless Workshop - Image Processing](https://github.com/aws-samples/aws-serverless-workshops/tree/master/ImageProcessing).

## Hints
If possible, please use `eu-west-1` region. This is necessary for the API Gateway script. (TBD)

## Exercise 1: Use API Gateway to push images to S3
This exercise includes a Cloud Formation template to create the API in front of S3 to upload images. Please note: uploading images does not trigger Step Functions execution and should be performed manually.

1. Navigate to Cloud Formation in your AWS account and click Create Stack.
2. Find the template in `api/apigateway-setup.yaml` and upload it to Cloud Formation.
3. Give the name for the stack and paste the S3 Bucket name that you created during the AWS Serverless Workshop - Image Processing where the photos are stored.
4. At the end of the 3rd step give AWS CloudFormation permission to "create IAM resources".
5. Navigate to API Gateway, find the newly create API and get the public URL from Stages -> "dev".
API is now ready to accept put requests.

### Sample HTML page
This HTML page uses API Gateway created before to upload your jpeg images into S3 Bucket.
1.  In the `api` folder of this repository find `index.html` and replace `<!!!API ID !!!>` with the correct API ID.
2. You can run it locally or try running with services like [jsbin](https://jsbin.com/) to testdrive it on your smartphone.

## Exercise 2: Publish message to SNS from 
This exercise guide how to setup direct integration with SNS without the use of Lambda functions (difficulty: moderate). The complete documentation can be found [here](https://docs.aws.amazon.com/step-functions/latest/dg/connect-sns.html).

Before we begin, there are several ways to trigger SNS notifications from Step Functions. One of them is to use Lambda function, the skeleton for this function is provided in the original workshop. The second option is to trigger SNS directly from Step Functions and this exercise will should you how to do it.

1. Open AWS Console and create new SNS topic, e.g. `PhotoDoesntMeetRequirementsTopic`. Copy the ARN of the new topic to the text editor.
2. Subscribe to the topic by opening it and clicking "Create subscription". Select email protocol and type your email. In few minutes you'll receive the email, please confirm the subscription using the link.
4. Open AWS Console and navigate to IAM. Go to Roles and find role starting with `wildrydes-step-module-resources-StateMachineRole`. Now we need to update this role, so that the State Machine can access SNS.
5. Click "add inline policy", in the new dialog switch to JSON tab and paste the following policy:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:Publish"
            ],
            "Resource": "*"
        }
    ]
}
``` 
Add the name like `snsPublish`
6. Go back to Step Functions to alter the state machine. Locate the `PhotoDoesNotMeetRequirement` and replace it with the following state
Please also replace `<!!! Topic ARN !!!>` with ARN from step 1.
```
      "PhotoDoesNotMeetRequirement": {
        "Type": "Task",
        "Resource": "arn:aws:states:::sns:publish",
        "Parameters": {
            "TopicArn": "<!!! Topic ARN !!!>",
            "Message.$": "$.errorInfo.Error"
          },
        
        "End": true
      },	
```
7. Create new execution with the same face to test your integarion works. In few minutes you'll receive an email with an error message.

## Excercise 3: Trigger State Machine execution when file is uploaded to S3.
(difficulty: high) If you feel ready for the challenge, please check the official [documentation](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-cloudwatch-events-s3.html) and try to implement the same for your solution.
# Follow-up to aws-serverless-workshops/ImageProcessing
This repository contains additional excercises to the AWS Serverless Workshop - Image Processing

## Hints
If possible, please use `eu-west-1` region. This is necessary for the API Gateway script. (TBD)

## API Gateway to push images to S3
1. Navigate to Cloud Formation in your AWS account and click Create Stack.
2. Find the template in `api/apigateway-setup.yaml` and upload it to Cloud Formation.
3. Give the name for the stack and paste the S3 Bucket name that you created during the AWS Serverless Workshop - Image Processing where the photos are stored.
4. At the end of the 3rd step give AWS CloudFormation permission to "create IAM resources".
5. Navigate to API Gateway, find the newly create API and get the public URL from Stages -> "dev".
API is now ready to accept put requests.

### Sample HTML page
This HTML page uses API Gateway created before to upload your jpeg images into S3 Bucket. Please note: uploading images does not trigger Step Functions execution and should be performed manually.
1.  In the `api` folder of this repository find `index.html` and replace `<!!!API ID !!!>` with the correct API ID.
2. You can run it locally or try running with services like [jsbin](https://jsbin.com/) to testdrive it on your smartphone.

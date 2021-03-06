# Promote New Environment to Production

This is an example of using CircleCI to promote a new environment to production and decommission the old environment in an automated process.

## Prerequests
1. AWS account
2. GitHub account
3. CircleCI account
4. To use AWS CLI in your jobs you need to add the following environment variables to the Project Settings in CircleCI:
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_DEFAULT_REGION (ex: us-west-2)

## Manual Steps
Few manual steps should be taken to "prime the pump". These manual steps will be automated later.

1. Create an S3 Bucket in your AWS account manually and make it public. If you need help with this, follow the instructions found in the [documentation](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html). 
2. Add a simple home page index.html to the created S3 bucket. It could be as simple as:
    ```
    <html>
    <body>
    <h1>Hello World!</h1>
    </body>
    </html>
    ```
3. Make sure you could browse to the home page. 
4. Create the first version of the AWS infrastructure using the following command:
    ```
    aws cloudformation deploy \
    --template-file cloudfront.yml \
    --stack-name production-distro \
    --parameter-overrides PipelineID="${S3_BUCKET_NAME}" \ # Name of the S3 bucket you created manually.
    ```

## Usage

1. Run your pipeline in CircleCI.
2. Verify the new version of your index.html is browsable using the CloudFront URL.
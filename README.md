# Context

**Documentation**: [https://www.serverless.com/framework/docs/](https://www.serverless.com/framework/docs/)

**Functions**

- Independent unit of deployment
- Code written & deployed to the cloud
- Performs a single job
- e.g.
    - Saving user to database
    - Process file in database
    - Perform scheduled task

**Events**

- Triggers **functions** from occurring
- e.g.
    - Endpoint request
    - CRON trigger
    - On upload

**Resources**

- Infrastructure **components** which **functions** use
- e.g.
    - AWS S3 Bucket
    - AWS SNS

**Services**

- Unit of organization
- e.g. project file
- Defines grouping of function, events, & resources

# Setup

1. Install global serverless **npm** package

```bash
npm install -g serverless
```

2. Run setup command

```bash
serverless
```

Extra: Upgrade package

```bash
npm update -g serverless
```

# Process

## Endpoint Set-up

Define endpoint in `serverless.yml` that will trigger your serverless function

```yaml
functions:
  hello:
    handler: handler.hello
    # Add the following lines:
    events:
      - http:
          path: hello
          method: post
```

## Deploy the Service

Deploy all changes within your service at the same time via **CloudFormation**

```bash
serverless deploy -v
```

- List of Options...

Can be found in `sls deploy` output

## Test the Service

**Target URL Endpoint**

```bash
curl -X POST https://TARGET_ENDPOINT.com/endpoint
```

**Invoke Service Function**

```bash
serverless invoke -f FUNCTION_NAME -l
```

**Fetch Function Logs**

```bash
serverless logs -f FUNCTION_NAME -t
```

## Removing the Service

Removes functions, events, & resources created

```bash
serverless remove
```

---

# AWS Specific Setup

## Credentials

1. Create AWS account; *if needed*
2. Create **IAM User** & **Access Key**
    - Stored in environment variables
3. Configure IAM permissions

## Using AWS Access Keys

**Method 1 (Quick Setup)**

Exported as *environment variables* to be accessible to *serverless* & *AWS SDK*

```bash
export AWS_ACCESS_KEY_ID=<your-key-here>
export AWS_SECRET_ACCESS_KEY=<your-secret-key-here>
# AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY are now available for serverless to use
serverless deploy

# 'export' command is valid only for unix shells. In Windows - use 'set' instead of 'export'
```

- Notes: If you are using a self-signed certificate you'll need to do one of the following...

**Method 2 (AWS Profiles)**
*Serverless Framework* provides a way to configure AWS profiles using `serverless config credentials`

```bash
serverless config credentials --provider aws --key AWS_ACCESS_ID --secret AWS_SECRET_KEY --profile test-profile-name
```

- List of Options...

## Create New Service

```bash
serverless create --template aws-nodejs --path test-service
```

- List of Options...
    - `--template` or `-t` from an available template **Required if none specified**
    - `--template-url` Creates service using custom template (*URL*) **Required if none specified**
    - `--path` Creates service in new folder
    - `--name` or `-n` Names services under `serverless.yml`
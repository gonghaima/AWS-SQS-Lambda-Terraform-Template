## [ AWS SQS Lambda Terraform Template](https://gonghaima.github.io/AWS-SQS-Lambda-Terraform-Template/)

This repository provides a Terraform template for setting up an AWS SQS queue integrated with an AWS Lambda function, including an Event Source Mapping to trigger the Lambda function from the SQS queue. The template is designed to be reusable and customizable for various use cases.

## Features

- **AWS SQS Queue**: A standard SQS queue for message queuing.
- **AWS Lambda Function**: A Lambda function triggered by messages in the SQS queue.
- **Event Source Mapping**: Configures the Lambda function to poll messages from the SQS queue.
- **IAM Roles and Policies**: Automatically sets up the necessary IAM roles and permissions for Lambda to access SQS.
- **Terraform Modules**: Organized as reusable Terraform modules for easy deployment and management.

## Prerequisites

Before using this template, ensure you have the following:

- **AWS Account**: An active AWS account with permissions to create SQS queues, Lambda functions, and IAM roles.
- **Terraform**: Installed on your local machine (version >= 0.12).
- **AWS CLI**: Configured with your AWS credentials.
- **Basic Knowledge**: Familiarity with AWS services (SQS, Lambda) and Terraform.

## Repository Structure

```plaintext
.
├── main.tf              # Main Terraform configuration file
├── variables.tf         # Input variables for customization
├── outputs.tf           # Output values for created resources
├── lambda_function.py   # Sample Lambda function code
├── modules/             # Terraform modules
│   ├── sqs/             # SQS module
│   └── lambda/          # Lambda module
└── README.md            # This file
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/gonghaima/AWS-SQS-Lambda-Terraform-Template.git
cd AWS-SQS-Lambda-Terraform-Template
```

### 2. Configure Terraform Variables

Edit the `variables.tf` file or create a `terraform.tfvars` file to customize the following variables:

- `region`: AWS region (e.g., `us-east-1`).
- `queue_name`: Name of the SQS queue.
- `lambda_function_name`: Name of the Lambda function.
- `lambda_handler`: Lambda handler (e.g., `lambda_function.lambda_handler`).
- `lambda_runtime`: Lambda runtime (e.g., `python3.9`).

Example `terraform.tfvars`:

```hcl
region              = "us-east-1"
queue_name         = "my-sqs-queue"
lambda_function_name = "my-lambda-function"
lambda_handler      = "lambda_function.lambda_handler"
lambda_runtime      = "python3.9"
```

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Plan and Apply

```bash
terraform plan
terraform apply
```

This will create the SQS queue, Lambda function, Event Source Mapping, and necessary IAM roles.

### 5. Test the Setup

- Send a message to the SQS queue using the AWS Management Console, AWS CLI, or SDK.
- Verify that the Lambda function is triggered by checking the CloudWatch logs.

### 6. Clean Up

To destroy the created resources:

```bash
terraform destroy
```

## Example Lambda Function

The `lambda_function.py` file contains a sample Python Lambda function that processes messages from the SQS queue:

```python
import json

def lambda_handler(event, context):
    for record in event['Records']:
        body = json.loads(record['body'])
        print(f"Received message: {body}")
    return {
        'statusCode': 200,
        'body': json.dumps('Successfully processed')
    }
```

## Customization

- **SQS Configuration**: Modify the `modules/sqs/main.tf` file to adjust queue settings like visibility timeout or message retention period.
- **Lambda Configuration**: Update `modules/lambda/main.tf` for additional environment variables, memory size, or timeout settings.
- **Additional Triggers**: Extend the template to include other event sources (e.g., SNS, S3).

## Notes

- Ensure the IAM role has sufficient permissions for Lambda to access SQS and CloudWatch Logs.
- The template uses a standard SQS queue. For FIFO queues, modify the `sqs` module accordingly.
- Monitor costs associated with SQS and Lambda usage.

## Contributing
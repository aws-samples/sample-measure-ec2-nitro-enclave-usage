
# AWS Nitro Enclave Usage Metering Solution

A solution for tracking, measuring, and billing AWS Nitro Enclave usage across multiple clients. This tool addresses the challenge of granular usage tracking and cost allocation for organizations running multiple Nitro Enclaves on EC2 instances.

## Overview

This solution automatically collects and processes AWS Nitro Enclave usage data, generating detailed reports for billing and metering purposes. It leverages AWS services including CloudWatch, Lambda, EventBridge, and S3 to create a scalable monitoring system.

## Features

- Automated collection of Nitro Enclave usage metrics
- Detailed tracking of enclave start/stop times
- Resource allocation monitoring (CPU, Memory)
- Per-client usage reporting
- Automated report generation and distribution
- Email notifications for new reports

## Architecture

![alt text](<Screenshot 2025-03-14 at 5.01.00 PM.png>)

The solution consists of:
- EC2 instances running Nitro Enclaves
- CloudWatch Logs for log collection
- Lambda function for log processing
- S3 bucket for report storage
- SNS for email notifications
- EventBridge for scheduling

## Prerequisites

- AWS Account with appropriate permissions
- EC2 instances from supported instance families
- Nitro-cli package installed
- CloudWatch agent configured
- IAM roles and permissions properly set up

## Installation

1. Download the CloudFormation template from this repository
2. Launch the template with required parameters:
   - Number of weeks for log analysis
   - Email address for notifications
   - S3 bucket prefix name

## Configuration

### CloudWatch Agent Configuration
```json
{
  "agent": {
    "run_as_user": "root"
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/nitro_enclaves/nitro_enclaves.log",
            "log_group_class": "STANDARD",
            "log_group_name": "/aws/nitro/enclave",
            "log_stream_name": "{instance_id}",
            "retention_in_days": 90
          }
        ]
      }
    }
  }
}
```

## Usage Reports

Sample output format:
```json
{
  "enclave_number": 25,
  "instance_id": "i-01d36da660a06d298",
  "instance_type": "t2.micro",
  "enclave_id": "i-01d36da660a06d298-enc192c49814d5d5f9",
  "start_time": "2024-10-25 16:52:34.176",
  "end_time": "2024-10-25 16:53:00.005",
  "duration": "0:00:25.829000",
  "run_args": {
    "eif_path": "hello.eif",
    "enclave_cid": null,
    "memory_mib": 512,
    "cpu_ids": null,
    "debug_mode": true,
    "attach_console": false,
    "cpu_count": 2,
    "enclave_name": "farshad1"
  }
}
```

## Cleanup

1. Delete the CloudFormation stack
2. Remove any separately created resources:
   - EC2 instances
   - S3 bucket
   - CloudWatch Log Group

## Cost Considerations

The solution uses several AWS services with associated costs:
- Amazon EventBridge (daily triggers)
- AWS Lambda (monthly executions)
- CloudWatch Logs Insights
- SNS (email notifications)
- S3 (report storage)

Refer to the AWS Pricing Calculator for detailed cost estimates based on your usage.

## Contributing

[Add contribution guidelines here]

## Support

[Add support information here]

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.


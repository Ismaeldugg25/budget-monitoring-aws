# Budget Monitoring with AWS Budgets and SNS

## Overview
A cloud cost monitoring system built on AWS that automatically sends email alerts when monthly spending reaches defined budget thresholds. Infrastructure is fully deployed using Terraform.

## Architecture
AWS Budgets monitors monthly spending → triggers SNS topic when thresholds are hit → SNS sends email notification to subscribed address. CloudWatch stores budget events for 30 days. KMS encrypts all log data.

## Services Used
- AWS Budgets
- Amazon SNS
- Amazon CloudWatch
- AWS KMS
- Terraform
- AWS CLI

## Resources Created
| Resource | Purpose |
|---|---|
| AWS Budget | Monitors monthly spending with 10EUR limit |
| SNS Topic | Notification channel for budget alerts |
| SNS Subscription | Delivers alerts to email |
| CloudWatch Log Group | Stores budget events for 30 days |
| KMS Key | Encrypts CloudWatch log data |

## Alert Thresholds
| Type | Threshold |
|---|---|
| Actual spending | 80% |
| Actual spending | 100% |
| Forecasted spending | 80% |

## Deployment Steps
1. Clone this repository
2. Create your own `terraform.tfvars` file:
```hcl
    environment         = "development"
    budget_limit_amount = 10
    notification_email  = "your-email@example.com"
```
3. Run `terraform init`
4. Run `terraform plan`
5. Run `terraform apply`
6. Confirm the SNS subscription email you receive
7. Test with `aws sns publish` command

## Important Notes
- `terraform.tfvars` is excluded from this repo via `.gitignore` to protect personal details
- Always run `terraform destroy` after testing to avoid unnecessary AWS charges
- AWS CLI must be configured with appropriate permissions (BudgetsFullAccess minimum)


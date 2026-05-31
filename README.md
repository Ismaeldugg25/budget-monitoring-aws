# Budget Monitoring with AWS Budgets and SNS

## Problem
Organizations struggle to control AWS spending without real-time visibility into cost trends and usage patterns. Without proactive cost monitoring, unexpected charges can accumulate rapidly, leading to budget overruns that impact business operations. Many teams discover cost spikes only after receiving monthly bills, making it impossible to take corrective action in time to prevent financial impact.

## Solution
AWS Budgets combined with SNS notifications provides automated cost monitoring and real-time alerts when spending approaches or exceeds predefined thresholds. This solution enables proactive cost management by sending email notifications at configurable percentage thresholds, allowing teams to take immediate action before budgets are exceeded.

## Architecture
AWS Budgets monitors monthly spending → triggers SNS topic when thresholds are hit → SNS sends email notification to subscribed address. CloudWatch stores budget events for 30 days. KMS encrypts all log data.
<img width="770" height="675" alt="image-6" src="https://github.com/user-attachments/assets/d8d4ae7d-7ab0-4a56-9ad2-df5eb4c8f737" />


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

## AWS Budgets
Multiple notification thresholds provide graduated alerting, enabling early warning at 80% of budget and final alerts at 100%. This configuration includes both actual spend notifications (for current costs) and forecasted notifications (for projected costs), providing comprehensive cost visibility and enabling proactive cost management. 

- Budget name: `monthly-cost-budget-2fab9e`
- Limit: 10 EUR (For learning purposes)
- Time unit: Monthly
- 3 alert thresholds configured: 80% actual, 100% actual, 80% forecasted
<img width="1592" height="499" alt="image" src="https://github.com/user-attachments/assets/0a185096-5cf6-498f-91a0-61f49454f745" />

<img width="1579" height="675" alt="image-2" src="https://github.com/user-attachments/assets/1ba551ec-1e0f-4dee-916d-203828acb09d" />

#### Terraform:

<img width="695" height="529" alt="image-3" src="https://github.com/user-attachments/assets/5f711a7a-70e3-46b6-91e6-2a55be16a19a" />

## Amazon SNS Topic
*Application-to-application messaging for microservices, distributed systems, and serverless applications.*

Amazon SNS provides a reliable, scalable messaging service that delivers budget alert notifications to email subscribers. Creating a dedicated topic for budget alerts ensures proper organization and allows for easy management of notification preferences, enabling reliable delivery even during high-volume periods.

Email subscriptions enable real-time delivery of budget alerts to stakeholders. SNS will send a confirmation email that must be confirmed to activate the subscription, ensuring that notifications reach the intended recipients and preventing unauthorized subscriptions.

- Topic name: `budget-alerts-2fab9e`
- Encryption enabled with **KMS**
- Email subscription confirmed and active
<img width="1599" height="548" alt="image-4" src="https://github.com/user-attachments/assets/8af387b6-1854-46da-a47d-c3baeae1b644" />

<img width="789" height="357" alt="image-5" src="https://github.com/user-attachments/assets/3e3c0c65-c27a-48f0-8cd5-291a6e0c7798" />

#### Terraform:
<img width="689" height="407" alt="image-7" src="https://github.com/user-attachments/assets/151e4ce5-c79a-44ac-9322-238e54759d5d" />

#### Test SNS Topic Subscription:
<img width="1132" height="322" alt="image-8" src="https://github.com/user-attachments/assets/8a2bfbfb-e16f-45d0-8dc0-e68ef9b7ad66" />

## CloudWatch and AWS KMS Key
- Log group: `/aws/budgets/monthly-cost-budget-2fab9e`
- Retention: 30 days
- Encrypted with custom KMS key
<img width="525" height="259" alt="image-9" src="https://github.com/user-attachments/assets/0ca97575-7dd5-46b0-8ff9-8170f7aef505" />


## Important Notes
- `terraform.tfvars` is excluded from this repo via `.gitignore` to protect personal details
- Always run `terraform destroy` after testing to avoid unnecessary AWS charges
- AWS CLI must be configured with appropriate permissions (BudgetsFullAccess minimum)


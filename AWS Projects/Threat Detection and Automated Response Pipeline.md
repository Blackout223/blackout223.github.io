# Scenario

SecurePay, a fast-growing fintech startup processing over 50,000 transactions daily, has recently expanded to AWS but has no visibility into what's happening inside their cloud environment. Their security team has no way of knowing if an attacker has compromised an IAM key, if someone logged into the root account at 3am, or if an S3 bucket containing customer financial records was quietly made public. With PCI-DSS compliance requirements looming and a recent industry-wide wave of cloud credential theft, SecurePay's CTO has issued an urgent mandate: build a threat detection and automated response system before their next compliance audit. As their Cloud Security Engineer, you'll build the system from scratch using AWS-native services.

# Solution

A fully automated cloud threat detection and incident response pipeline using AWS CloudTrail, Amazon GuardDuty, CloudWatch, EventBridge, SNS, and Lambda. The system continuously monitors all API activity across the AWS account, detects suspicious behaviour using machine learning and rule-based alerting, and automatically notifies the security team and in some cases remediates the threat within minutes of detection.

# Architecture Diagram

Below is an architecture diagram on how I am going to solve the problem
<img width="1440" height="1228" alt="image" src="https://github.com/user-attachments/assets/acee3270-670b-4c33-92c6-f504f5447e03" />

# Enable CloudTrail with S3 logging and CloudWatch Integration

CloudTrail is the foundation of everything that follows. It records every API call made in your AWS account, who made it, from where, at what time, and whether it succeeded.

I created an S3 bucket for log storage with versioning enabled 
`S3 -> Create bucket`
<img width="1533" height="1054" alt="image" src="https://github.com/user-attachments/assets/d840fea2-0335-41ef-aa17-1a4442e78455" />
<img width="1571" height="924" alt="image" src="https://github.com/user-attachments/assets/43aa95db-d79f-4686-bfca-f157288f4457" />

Once my bucket was created I needed to add a bucket policy so that CloudTrail will have the correct permissions to write logs into the bucket. To do this I went into my created bucket > permissions > Bucket policy
```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::securepay-cloudtrail-logs-992382586233"
        },
        {
            "Sid": "AWSCloudTrailWrite",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::securepay-cloudtrail-logs-992382586233/AWSLogs/992382586233/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        }
    ]
}
```
<img width="1464" height="799" alt="image" src="https://github.com/user-attachments/assets/514412bf-ca9e-4a24-9089-e5b3907f8a60" />

The bucket and policy has now successfully been created. 

Next step is to setup a CloudWatch Log Group for CloudTrail
`Cloudwatch > Log Management > Create Log Group`
<img width="875" height="864" alt="image" src="https://github.com/user-attachments/assets/eac43bdf-431a-45a1-bb8a-b2ab68c0c21a" />

Once the log group has been created, the trail needs to be setup
`CloudTrail > Trails > Create Trail`

As the bucket has already been created I used the existing S3 bucket option.
<img width="1421" height="821" alt="image" src="https://github.com/user-attachments/assets/b85c2850-455b-400d-9fcd-12771b52b7ea" />

On the optional settings I enabled CloudWatch Logs with the existing log group name and create a new IAM role
- Log group name - `/aws/cloudtrail/securepay-trail`
- IAM Role - `CloudTrail-CloudWatch-Role`

On the log events page after the trail attributes have been completed. I chose the following event types:
- Management events 
- Data events - Resource type with S3 and log all events
- Insights events - enabled API call/error rate 
<img width="1351" height="1136" alt="image" src="https://github.com/user-attachments/assets/2382dbf8-fe9b-4fb8-ae37-7a8867af4222" />
<img width="1395" height="436" alt="image" src="https://github.com/user-attachments/assets/dfc2de6b-a6b9-48c9-8e6a-01c67b3e36b6" />

The trail is successfully set up
<img width="2515" height="311" alt="image" src="https://github.com/user-attachments/assets/6b4ef706-3605-47e2-b025-1e66bf98ec19" />

After a few minutes, log files will start appearing under the bucket
<img width="2218" height="414" alt="image" src="https://github.com/user-attachments/assets/dce61a7d-0449-41a5-9ff1-38bfe557ba82" />

# Enabling Amazon GuardDuty

GuardDuty is AWS's managed threat detection service. It continuously analyses three data sources.  CloudTrail logs, VPC Flow Logs, and Route 53 DNS query logs  and applies ML models and threat intelligence feeds to identify malicious patterns.

I enabled GuardDuty as it provides a free 30 day trial. 

GuardDuty classifies findings into three severity tiers:
- **High (7.0–8.9):** Immediate action required. Examples: `UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration`, `Backdoor:EC2/C&CActivity`
- **Medium (4.0–6.9):** Investigate promptly. Examples: `Recon:EC2/PortProbeUnprotectedPort`, `Policy:S3/BucketPublicAccessGranted`
- **Low (0.1–3.9):** Informational. Examples: `Discovery:S3/MaliciousIPCaller`

GuardDuty comes with built-in threat intelligence that can be added on custom lists of known bad IP addresses. 

`GuardDuty > Lists > Add a threat intel list.`

I created a file called `threat-ips.txt` with IPs:
```
192.0.2.1
198.51.100.1
203.0.113.1
```

With this file I uploaded it to the S3 bucket that was created earlier and head back to GuardDuty to create the threat IP list
<img width="593" height="513" alt="image" src="https://github.com/user-attachments/assets/6be91cd9-ccf7-4476-a017-81d25bc0978a" />

GuardDuty will start cross-referencing all incoming traffic and API calls against those IPs immediately.

# Setting up SNS

 SNS is a highly available, durable, secure, fully managed pub/sub messaging service that enables you to decouple microservices, distributed systems, and event-driven serverless applications.

SNS was chosen for this pipeline because of its pub/sub architecture, meaning a single security finding can simultaneously notify multiple subscribers

When a GuardDuty finding triggers, the SNS email will contain the raw EventBridge event JSON. It's verbose but includes all the information you need: the finding type, the affected resource (IAM user, EC2 instance, or S3 bucket), the severity, the source IP address, and the time.

I will be creating a topic called `SecurePaySecurityAlerts` - Which will be used with my CloudWatch Alarms
<img width="1581" height="1149" alt="image" src="https://github.com/user-attachments/assets/6d664437-2550-4266-8028-ff6966f19f84" />

Once the topic has been created. I set up the subscription to send any notifications to my email
<img width="1526" height="630" alt="image" src="https://github.com/user-attachments/assets/934e3716-48f2-45f1-bc9e-6b0c8db5dc48" />

SNS has now fully been set up. Next step is to create the metric filters and alarms in CloudWatch

# Creating CloudWatch Metric filters and alarms

While GuardDuty handles ML-based detection, CloudWatch metric filters builds precise rule-based alerts for specific high-risk events that are defined.

I will be creating five alarms covering the most critical security events

### Metric filter for Root Account Usage
```sh
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/securepay-trail" \
  --filter-name "RootAccountUsage" \
  --filter-pattern "{ $.userIdentity.type = \"Root\" }" \
  --metric-transformations \
    metricName=RootAccountUsageCount,metricNamespace=SecurePaySecurityMetrics,metricValue=1,defaultValue=0 \
  --region eu-west-2
```

### Metric filter for Console Login Without MFA
```sh
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/securepay-trail" \
  --filter-name "ConsoleLoginWithoutMFA" \
  --filter-pattern "{ ($.eventName = \"ConsoleLogin\") && ($.additionalEventData.MFAUsed != \"Yes\") }" \
  --metric-transformations \
    metricName=ConsoleLoginNoMFACount,metricNamespace=SecurePaySecurityMetrics,metricValue=1,defaultValue=0 \
  --region eu-west-2
```

### Metric filter for IAM Policy Changes
```sh
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/securepay-trail" \
  --filter-name "IAMPolicyChanges" \
  --filter-pattern "{ ($.eventName = PutUserPolicy) || ($.eventName = PutGroupPolicy) || ($.eventName = PutRolePolicy) || ($.eventName = CreatePolicy) || ($.eventName = DeletePolicy) }" \
  --metric-transformations \
    metricName=IAMPolicyChangeCount,metricNamespace=SecurePaySecurityMetrics,metricValue=1,defaultValue=0 \
  --region eu-west-2
```

### Metric filter for S3 Bucket Policy Changes
```sh
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/securepay-trail" \
  --filter-name "S3BucketPolicyChanges" \
  --filter-pattern "{ ($.eventSource = s3.amazonaws.com) && (($.eventName = PutBucketAcl) || ($.eventName = PutBucketPolicy) || ($.eventName = DeleteBucketPolicy)) }" \
  --metric-transformations \
    metricName=S3BucketPolicyChangeCount,metricNamespace=SecurePaySecurityMetrics,metricValue=1,defaultValue=0 \
  --region eu-west-2
```

### Metric filter for Security Group Changes
```sh
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/securepay-trail" \
  --filter-name "SecurityGroupChanges" \
  --filter-pattern "{ ($.eventName = AuthorizeSecurityGroupIngress) || ($.eventName = AuthorizeSecurityGroupEgress) || ($.eventName = CreateSecurityGroup) || ($.eventName = DeleteSecurityGroup) }" \
  --metric-transformations \
    metricName=SecurityGroupChangeCount,metricNamespace=SecurePaySecurityMetrics,metricValue=1,defaultValue=0 \
  --region eu-west-2
```
<img width="2289" height="1149" alt="image" src="https://github.com/user-attachments/assets/daa83b0d-4689-410b-971e-76dff3159411" />

Now that the metric filters have been set up. Alarms need to be created for each metric by selecting the metric in CloudWatch > Create Alarm 

I used the same setting across the 5 alarms created

- **Threshold:** Greater than or equal to `1`
- **Period:** 5 minutes
- **Alarm name:** `CRITICAL-RootAccountUsage`
- **Notification:** Selecting `SecurePaySecurityAlerts` SNS topic

For the others:
- `ConsoleLoginWithoutMFA`> alarm name `HIGH-ConsoleLoginWithoutMFA`
- `IAMPolicyChanges` > alarm name `HIGH-IAMPolicyChange`
- `S3BucketPolicyChanges` > alarm name `HIGH-S3BucketPolicyChange`
- `SecurityGroupChanges` > alarm name `MEDIUM-SecurityGroupChange`
<img width="708" height="368" alt="image" src="https://github.com/user-attachments/assets/76bb76e7-0df1-4d46-8a4d-f2a05cfadc5c" />

# Configuring EventBridge to route GuardDuty Findings

CloudWatch alarms handle rule-based events from CloudTrail. EventBridge handles GuardDuty findings and routes them to multiple targets simultaneously. SNS for human alerting, Lambda for automated remediation, and Security Hub for centralised logging.

`EventBridge > Rules > Create Rule`

I create my rule called `GuardDuty-HighSeverity-Response` - Which will be used for high vulnerabilities. 

For triggering Events we use GuardDuty Finding and for the Targets SNS and Lambda (which will be set up later)

This is the event pattern that will be used:
```JSON
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [
      { "numeric": [">=", 7] }
    ]
  }
}
```
<img width="2182" height="770" alt="image" src="https://github.com/user-attachments/assets/15c790c4-4c73-49d0-9293-0654d92cbaa9" />

Next is to create the medium severity response. Which is the same exact steps but using a different event pattern
```JSON
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [
      { "numeric": [">=", 4, "<", 7] }
    ]
  }
}
```


# Writing the Lambda Auto-Remediation function

When GuardDuty detects that an IAM access key has been compromised, the Lambda function automatically deactivates that key, cutting off the attacker's access within seconds rather than waiting for a human to respond.

I created the Lambda execution role
`IAM > Roles > Create Role`

- **Trusted entity:** AWS service → Lambda
- **Permissions:** 
- `IAMFullAccess` - For Lambda to deactivate IAM keys
- `AWSLambdaBasicExecutionRole` - For Lambda to write its own logs to CloudWatch
- **Role name:** `SecurePay-Lambda-RemediationRole`
<img width="1747" height="874" alt="image" src="https://github.com/user-attachments/assets/10e3960a-ee07-4f09-9620-847d73c37f2c" />

After the IAM Role was created. I created the Lambda auto-remediation function (Lambda > Create Function) with the help of Claude creating the function

- **Function name:** `SecurePay-AutoRemediation`
- **Runtime:** Python 3.12
- **Execution role:** Use existing role → `SecurePay-Lambda-RemediationRole`

```python
import boto3
import json
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

iam = boto3.client('iam')
sns = boto3.client('sns')

SNS_TOPIC_ARN = 'arn:aws:sns:eu-west-2:992382586233:SecurePaySecurityAlerts'

def lambda_handler(event, context):
    logger.info(f"Received event: {json.dumps(event)}")

    try:
        detail = event.get('detail', {})
        finding_type = detail.get('type', 'Unknown')
        severity = detail.get('severity', 0)
        resource = detail.get('resource', {})
        access_key_details = resource.get('accessKeyDetails', {})

        username = access_key_details.get('userName')
        access_key_id = access_key_details.get('accessKeyId')

        # Only auto-remediate high severity IAM key findings
        if severity >= 7 and access_key_id and username:
            logger.info(f"High severity finding. Deactivating key {access_key_id} for user {username}")

            # Deactivate the compromised access key
            iam.update_access_key(
                UserName=username,
                AccessKeyId=access_key_id,
                Status='Inactive'
            )

            # Notify the team that auto-remediation occurred
            message = (
                f"AUTO-REMEDIATION EXECUTED\n\n"
                f"Finding type: {finding_type}\n"
                f"Severity: {severity}\n"
                f"Action taken: IAM access key {access_key_id} for user '{username}' "
                f"has been automatically DEACTIVATED.\n\n"
                f"Please investigate immediately and rotate credentials if needed.\n\n"
                f"Full finding:\n{json.dumps(detail, indent=2)}"
            )

            sns.publish(
                TopicArn=SNS_TOPIC_ARN,
                Subject='[AUTO-REMEDIATED] Compromised IAM Key Deactivated',
                Message=message
            )

            logger.info("Key deactivated and notification sent.")
        else:
            logger.info(f"Finding severity {severity} or no key details — no auto-remediation taken.")

    except Exception as e:
        logger.error(f"Remediation failed: {str(e)}")
        raise

    return {"status": "complete"}
```
<img width="1503" height="1084" alt="image" src="https://github.com/user-attachments/assets/0d0c6a55-b96d-4e2f-8238-1107bbc61983" />

By default Lambda functions timeout after 3 seconds. IAM API calls can occasionally take longer. I updated this to 30 seconds
<img width="1507" height="235" alt="image" src="https://github.com/user-attachments/assets/b99ae729-6502-4341-b55a-5509a9ae78fb" />

Next I headed back to EventBridge and added the Lambda function to the targets
<img width="2177" height="421" alt="image" src="https://github.com/user-attachments/assets/f9b80a89-9e9d-4401-9db2-c337d3983599" />

# Enabling AWS Security Hub

Security Hub aggregates findings from GuardDuty, CloudWatch, and other security tools into one dashboard, and scores your account against security standards like CIS AWS Foundations Benchmark and AWS Foundational Security Best Practices.

Security Hub can take up to 15–30 minutes to run its first assessment.
<img width="784" height="535" alt="image" src="https://github.com/user-attachments/assets/e0013529-6977-46c2-a74c-cbf25afb4782" />
# Simulating attack scenarios and verifying detection

I will be creating 4 simulation attacks:
- Console Login with No MFA
- Root Account Login
- S3 Bucket Made Public - GuardDuty Finding
- GuardDuty sample findings

### Console Login without MFA

I created a test IAM user with console access but no MFA configured.

`IAM > Users > Create user:`

- **Username:** `test-no-mfa-user`
- **Console access:** Enable, set a password
- **Permissions:** Attach `ReadOnlyAccess`
- **MFA:** Do not configure MFA
<img width="1721" height="246" alt="image" src="https://github.com/user-attachments/assets/5fcf1df0-07a4-4e71-82f5-20f61451773f" />

I opened an incognito tab and signed into the IAM user I had just created.

After 5 minutes my CloudWatch Alarm had gone into `In Alarm` state
<img width="1970" height="407" alt="image" src="https://github.com/user-attachments/assets/2de5c540-fb40-493c-bbc9-347a6bf1361d" />

### Root Account Login

Within 5 minutes, the `CRITICAL-RootAccountUsage` CloudWatch alarm went off when logging into root
<img width="1972" height="443" alt="image" src="https://github.com/user-attachments/assets/3bb71939-5999-4fc8-b74a-35f194eb5e41" />

### S3 Bucket Made Public

Here I create a random bucket and deliberately made it public
```sh
s3://securepay-test-public-bucket --region eu-west-2 

aws s3api delete-public-access-block \ 
--bucket securepay-test-public-bucket \ 
--region eu-west-2
```
<img width="2145" height="739" alt="image" src="https://github.com/user-attachments/assets/2dfcd634-bd63-4ae7-af8a-baae45e79c95" />

Within minutes, GuardDuty should generate a `Policy:S3/BucketPublicAccessGranted` finding - Which is rated a Low. This is also set off my CloudWatch Alarm
<img width="1153" height="515" alt="image" src="https://github.com/user-attachments/assets/170b9c18-7769-41d1-80a0-973ff2082a45" />

### GuardDuty Sample Findings

GuardDuty has a built-in sample findings generator that lets you test your entire alerting pipeline without actually performing any attack. 
`GuardDuty > Settings > Generate sample findings > Generate`

This creates one sample finding for every finding type GuardDuty can detect over 100 findings covering IAM credential theft, EC2 cryptocurrency mining, S3 data exfiltration, and more. They will start with `[SAMPLE]` in the title to show they are test data
<img width="1759" height="1058" alt="image" src="https://github.com/user-attachments/assets/5f272adf-7914-40ee-b259-86291ef01b67" />

Checking Lambda logs in my `/aws/lambda/SecurePay-AutoRemediation` log group. Lambda was executed 10 times for the high vulnerabilities that GuardDuty had generated with the samples 
<img width="2267" height="1117" alt="image" src="https://github.com/user-attachments/assets/6b5b6c13-570e-482f-8bab-7ce45b49ee94" />
<img width="2254" height="1014" alt="image" src="https://github.com/user-attachments/assets/725614cc-1fe4-4047-a34e-5e8d3b8884c9" />

# Conclusion

SecurePay had zero visibility into their AWS environment. An attacker could have stolen an IAM access key, logged into the console from a foreign country, exfiltrated customer financial data from S3, and opened backdoor ports on EC2 instances and the security team would have had no idea until a customer or regulator flagged it. I built a layered detection system that catches these exact threats in real time, automatically contains the most critical ones within seconds, and maintains a complete tamper-evident audit trail that satisfies compliance requirements. The difference between this and no monitoring is the difference between discovering a breach in 4 minutes versus 4 months.


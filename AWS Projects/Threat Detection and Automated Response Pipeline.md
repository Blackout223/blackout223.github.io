# Scenario

SecurePay, a fast-growing fintech startup processing over 50,000 transactions daily, has recently expanded to AWS but has no visibility into what's happening inside their cloud environment. Their security team has no way of knowing if an attacker has compromised an IAM key, if someone logged into the root account at 3am, or if an S3 bucket containing customer financial records was quietly made public. With PCI-DSS compliance requirements looming and a recent industry-wide wave of cloud credential theft, SecurePay's CTO has issued an urgent mandate: build a threat detection and automated response system before their next compliance audit. As their Cloud Security Engineer, you'll build the system from scratch using AWS-native services.

# Solution

A fully automated cloud threat detection and incident response pipeline using AWS CloudTrail, Amazon GuardDuty, CloudWatch, EventBridge, SNS, and Lambda. The system continuously monitors all API activity across the AWS account, detects suspicious behaviour using machine learning and rule-based alerting, and automatically notifies the security team and in some cases remediates the threat within minutes of detection.

# Architecture Diagram

Below is an architecture diagram on how I am going to solve the problem
![[Pasted image 20260411175704.png]]

# Enable CloudTrail with S3 logging and CloudWatch Integration

CloudTrail is the foundation of everything that follows. It records every API call made in your AWS account, who made it, from where, at what time, and whether it succeeded.

I created an S3 bucket for log storage with versioning enabled 
`S3 -> Create bucket`
![[Pasted image 20260412112408.png]]![[Pasted image 20260412112419.png]]

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
![[Pasted image 20260412113651.png]]

The bucket and policy has now successfully been created. 

Next step is to setup a CloudWatch Log Group for CloudTrail
`Cloudwatch > Log Management > Create Log Group`
![[Pasted image 20260412115824.png]]

Once the log group has been created, the trail needs to be setup
`CloudTrail > Trails > Create Trail`

As the bucket has already been created I used the existing S3 bucket option.
![[Pasted image 20260412120953.png]]

On the optional settings I enabled CloudWatch Logs with the existing log group name and create a new IAM role
- Log group name - `/aws/cloudtrail/securepay-trail`
- IAM Role - `CloudTrail-CloudWatch-Role`

On the log events page after the trail attributes have been completed. I chose the following event types:
- Management events 
- Data events - Resource type with S3 and log all events
- Insights events - enabled API call/error rate 
![[Pasted image 20260412123800.png]]
![[Pasted image 20260412123816.png]]

The trail is successfully set up
![[Pasted image 20260412123927.png]]

After a few minutes, log files will start appearing under the bucket
![[Pasted image 20260412124417.png]]
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
![[Pasted image 20260412130839.png]]

GuardDuty will start cross-referencing all incoming traffic and API calls against those IPs immediately.

# Setting up SNS

 SNS is a highly available, durable, secure, fully managed pub/sub messaging service that enables you to decouple microservices, distributed systems, and event-driven serverless applications.

SNS was chosen for this pipeline because of its pub/sub architecture, meaning a single security finding can simultaneously notify multiple subscribers

When a GuardDuty finding triggers, the SNS email will contain the raw EventBridge event JSON. It's verbose but includes all the information you need: the finding type, the affected resource (IAM user, EC2 instance, or S3 bucket), the severity, the source IP address, and the time.

I will be creating a topic called `SecurePaySecurityAlerts` - Which will be used with my CloudWatch Alarms
![[Pasted image 20260412192701.png]]

Once the topic has been created. I set up the subscription to send any notifications to my email
![[Pasted image 20260412192853.png]]

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
![[Pasted image 20260412221509.png]]

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
![[Pasted image 20260412221442.png]]
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
![[Pasted image 20260412202513.png]]

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
![[Pasted image 20260412205554.png]]

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
![[Pasted image 20260412210631.png]]

By default Lambda functions timeout after 3 seconds. IAM API calls can occasionally take longer. I updated this to 30 seconds
![[Pasted image 20260412211519.png]]

Next I headed back to EventBridge and added the Lambda function to the targets
![[Pasted image 20260412212517.png]]

# Enabling AWS Security Hub

Security Hub aggregates findings from GuardDuty, CloudWatch, and other security tools into one dashboard, and scores your account against security standards like CIS AWS Foundations Benchmark and AWS Foundational Security Best Practices.

Security Hub can take up to 15–30 minutes to run its first assessment.
![[Pasted image 20260412230831.png]]
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
![[Pasted image 20260412220025.png]]

I opened an incognito tab and signed into the IAM user I had just created.

After 5 minutes my CloudWatch Alarm had gone into `In Alarm` state
![[Pasted image 20260412222449.png]]

### Root Account Login

Within 5 minutes, the `CRITICAL-RootAccountUsage` CloudWatch alarm went off when logging into root
![[Pasted image 20260412223041.png]]
### S3 Bucket Made Public

Here I create a random bucket and deliberately made it public
```sh
s3://securepay-test-public-bucket --region eu-west-2 

aws s3api delete-public-access-block \ 
--bucket securepay-test-public-bucket \ 
--region eu-west-2
```
![[Pasted image 20260412223730.png]]

Within minutes, GuardDuty should generate a `Policy:S3/BucketPublicAccessGranted` finding - Which is rated a Low. This is also set off my CloudWatch Alarm
![[Pasted image 20260412224440.png]]

### GuardDuty Sample Findings

GuardDuty has a built-in sample findings generator that lets you test your entire alerting pipeline without actually performing any attack. 
`GuardDuty > Settings > Generate sample findings > Generate`

This creates one sample finding for every finding type GuardDuty can detect over 100 findings covering IAM credential theft, EC2 cryptocurrency mining, S3 data exfiltration, and more. They will start with `[SAMPLE]` in the title to show they are test data
![[Pasted image 20260412225041.png]]

Checking Lambda logs in my `/aws/lambda/SecurePay-AutoRemediation` log group. Lambda was executed 10 times for the high vulnerabilities that GuardDuty had generated with the samples 
![[Pasted image 20260412225841.png]]
![[Pasted image 20260412225900.png]]

# Conclusion

SecurePay had zero visibility into their AWS environment. An attacker could have stolen an IAM access key, logged into the console from a foreign country, exfiltrated customer financial data from S3, and opened backdoor ports on EC2 instances and the security team would have had no idea until a customer or regulator flagged it. I built a layered detection system that catches these exact threats in real time, automatically contains the most critical ones within seconds, and maintains a complete tamper-evident audit trail that satisfies compliance requirements. The difference between this and no monitoring is the difference between discovering a breach in 4 minutes versus 4 months.


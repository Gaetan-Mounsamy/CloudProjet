# CloudProjet
Projet Cloud AWS MOUNSAMY-TREHIN
## Sommaire  

- [Screenshots and explanation of the project on AWS](/AWS_Site)  
- [Screenshots of the project on QuickSight](/quicksight)  
- [Screenshots of the answers of the IAM quizz](/iam%20quizz)  
- [Screenshots of the answers of the network quizz](/network%20quizz)
- [Policies evaluation](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/README.md#policies-evaluation)

### Schema of our realisation :
![Schema](diagrammeprojet.png)

## Policies evaluation

Please evaluate below IAM policies

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2AndS3",
      "Effect": "Allow",
      "Action": [
        "ec2:RunInstances",
        "ec2:TerminateInstances",
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:ec2:us-east-1:123456789012:instance/*",
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

### Question: What actions are allowed for EC2 instances and S3 objects based on this policy? What specific resources are included?

#### Answer:
The actions allowed for EC2 instances based on this policy are `ec2:RunInstances` and `ec2:TerminateInstances`. For S3 objects, the actions allowed are `s3:GetObject` and `s3:PutObject`. The specific resources included are all instances in the `us-east-1` region under the AWS account `123456789012` and all objects in the `example-bucket`.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowVPCAccess",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-west-2"
        }
      }
    }
  ]
}
```

### Question: Under what condition does this policy allow access to VPC-related information? Which AWS region is specified?

#### Answer:
The policy allows access to VPC-related information (`ec2:DescribeVpcs`, `ec2:DescribeSubnets`, `ec2:DescribeSecurityGroups`) under the condition that the requested AWS region is `us-west-2`.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadWrite",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ],
      "Condition": {
        "StringLike": {
          "s3:prefix": ["documents/*", "images/*"]
        }
      }
    }
  ]
}
```

### Question: What actions are allowed on the "example-bucket" and its objects based on this policy? What specific prefixes are specified in the condition?

#### Answer:
The third policy allows the `s3:GetObject`, `s3:PutObject`, and `s3:ListBucket` actions on the "example-bucket" and its objects. The condition specifies that the actions are only allowed for objects with the prefixes "documents/" and "images/".

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowIAMUserCreation",
      "Effect": "Allow",
      "Action": "iam:CreateUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    },
    {
      "Sid": "AllowIAMUserDeletion",
      "Effect": "Allow",
      "Action": "iam:DeleteUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    }
  ]
}
```

### Question: What actions are allowed for IAM users based on this policy? How are the resource ARNs constructed?

#### Answer:
The fourth policy allows for the creation (`iam:CreateUser` action) and deletion (`iam:DeleteUser` action) of IAM users. The resource ARNs are constructed using the AWS account ID `123456789012` and the IAM username, which is dynamically replaced in the ARN with `${aws:username}`.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["iam:Get*", "iam:List*"],
    "Resource": "*"
  }
}
```

### Questions:

- Which AWS service does this policy grant you access to?
- Does it allow you to create an IAM user, group, policy, or role?
- Go to https://docs.aws.amazon.com/IAM/latest/UserGuide/ and in the left navigation expand Reference > Policy Reference > Actions, Resources, and Condition Keys. Choose Identity And Access Management. Scroll to the Actions Defined by Identity And Access Management list. Name at least three specific actions that the **iam:Get\*** action allows.
The answers to the questions are as follows:

#### Answer:
- This policy grants access to AWS IAM (Identity and Access Management) service.
- No, it doesn't allow you to create an IAM user, group, policy, or role. It only allows you to get and list IAM resources.
- The **iam:Get** action could allow actions such as `iam:GetUser`, `iam:GetGroup`, `iam:GetRole`, `iam:GetPolicy`, etc. These actions enable you to retrieve details about the corresponding IAM resources.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": ["t2.micro", "t2.small"]
        }
      },
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Action": ["ec2:RunInstances", "ec2:StartInstances"],
      "Effect": "Deny"
    }
  ]
}
```

### Questions:

- What actions does the policy allow?
- Say that the policy included an additional statement object, like this **example:**

#### Answer:
- The sixth policy doesn't allow any actions, it explicitly denies the `ec2:RunInstances` and `ec2:StartInstances` actions.
- If the policy included an additional statement that allows all EC2 actions (`"Action": "ec2:*"`), the `Deny` statement in the original policy would still take precedence and deny the `ec2:RunInstances` and `ec2:StartInstances` actions for instance types `t2.micro` and `t2.small`. This is because in AWS IAM, an explicit deny always overrides any allows.

```json
{
  "Effect": "Allow",
  "Action": "ec2:*"
}
```

- How would the policy restrict the access granted to you by this additional statement?
- If the policy included both the statement on the left and the statement in question 2, could you terminate an m3.xlarge instance that existed in the account?

#### Answer:
- If the policy included an additional statement that allows all EC2 actions (`"Action": "ec2:*"`), the `Deny` statement in the original policy would still take precedence and deny the `ec2:RunInstances` and `ec2:StartInstances` actions for instance types `t2.micro` and `t2.small`. This is because in AWS IAM, an explicit deny always overrides any allows.
- The policy from question 2 does not grant any permissions to terminate instances, it only allows describing VPCs, subnets, and security groups. Therefore, even if both policies were combined, you would not be able to terminate an `m3.xlarge` instance.

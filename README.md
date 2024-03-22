# Guide to setup self-managed dashboards in EC2 hosted container

## Prerequisite
A AWS managed OpenSearch domain without any authentication method enabled and is accompanied by the following domain access policy.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:ap-south-1:765423874566:domain/no-security/*"
    }
  ]
}
```


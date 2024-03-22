# Guide to setup self-managed dashboards in EC2 hosted container

## Prerequisite
A managed service OpenSearch domain without any authentication method enabled with the below domain access policy
```
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


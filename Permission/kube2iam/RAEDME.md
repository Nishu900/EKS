    Pre-check for debugging
    1. Assume policy in worker node role
    2. Trust policy
    3. Serviceaccount
    4. DaemonSet
    5. Role in deployment file to give permission
By using the --debug flag you can enable some extra features making debugging easier:



Assume policy in worker-node role 
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "sts:AssumeRole"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Action": [
        "ec2:DescribeRegions"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
  ]
}

Trust-relationship in application role 
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/kubernetes-worker-role"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}



How to give role to application 
        metadata:
          annotations:
            iam.amazonaws.com/role: role-arn
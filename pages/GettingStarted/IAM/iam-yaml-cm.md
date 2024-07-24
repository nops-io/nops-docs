---
title: Commitment Management Permissions
keywords: iam policies, iam, setup, onboarding, sharesave
tags: [onboarding, iam]
sidebar: mydoc_sidebar
permalink: iam-yaml-cm.html
folder: GettingStarted
series: [Onboarding, IAM, Copilot]
weight: 4.0
---

## Prerequisites ##


To take advantage of all nOps Commitment Management Program, **you must be subscribed to the nOps AWS Marketplace offering.** If you have previously subscribed, please see the instructions on [configuring Commitment Management](copilot-cm-configure.html) to learn how the policies are created.
    

## Policies and Permissions ##

1.  Within your **AWS Payer/Management account,** nOps will create a new hourly Cost/Usage Report(CUR), S3 bucket for the CUR and nOps ShareSave Payer cross-account role/policy :  
    1.  _Trusted Entity Type_: AWS Account
    2.  _Trusted Entity_: nOps
    3.  _Role name_: nops-sharesave-payer
    4.  _IAM Policy for the Role_: [IAM Policy](#cm-aws-payer-iam-policy)
    5.  _Automated Creation_: [YAML File](#commitment-management-yaml-file)

    
1.  Within the **CM linked account** used by nOps to buy/sell Reserved Instances and buy Compute Savings Plans.  
    1. _Trusted Entity Type_: AWS Account_Trusted 
    2. _Entity_: nOps
    3. _Role name_: nops-sharesave-ri
    4. _IAM Policy for the Role_: [IAM Policy](#cm-linked-iam-role)
    5. _Automated Creation_: NA – preloaded  
            

### Commitment Management YAML file ###

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: |
  nOps.io integration role for ShareSave accounts (updated September 12, 2022)
  For more information visit https://help.nops.io

Parameters:
  S3CurBucket:
    Description: Format  customername-sharesave
    Type: String
  ReportName:
    Description:  Format  customername-sharesave
    Type: String

Resources:
  CURBucketCreate:
    Type: "AWS::S3::Bucket"
    DeletionPolicy: Retain
    Properties:
      BucketName: !Ref "S3CurBucket"

  CURBucketPolicy:
    Type: "AWS::S3::BucketPolicy"
    DeletionPolicy: Retain
    DependsOn: CURBucketCreate
    Properties:
      Bucket: !Ref "S3CurBucket"
      PolicyDocument:
        Statement:
          - Action:
            - "s3:GetBucketAcl"
            - "s3:GetBucketPolicy"
            Effect: Allow
            Resource: !Join ["", ["arn:",!Ref AWS::Partition,":s3:::",!Ref "S3CurBucket"]]
            Principal:
              Service:
              - billingreports.amazonaws.com
          - Action:
            - "s3:PutObject"
            Effect: Allow
            Resource: !Join ["", ["arn:",!Ref AWS::Partition,":s3:::",!Ref "S3CurBucket","/*"]]
            Principal:
              Service:
              - billingreports.amazonaws.com

  CURCreate:
    Type: "AWS::CUR::ReportDefinition"
    DeletionPolicy: Retain
    DependsOn: CURBucketPolicy
    Properties:
      ReportName: !Ref "ReportName"
      RefreshClosedReports: True
      S3Bucket: !Ref "S3CurBucket"
      S3Prefix: sharesave
      S3Region: us-east-1
      TimeUnit: HOURLY
      ReportVersioning: OVERWRITE_REPORT
      AdditionalArtifacts:
        - REDSHIFT
      Compression: GZIP
      Format: textORcsv

  nOpsShareSaveRole:
    Type: "AWS::IAM::Role"
    DependsOn: CURBucketPolicy
    Properties:
      RoleName: nops-sharesave-payer
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              AWS:
                - arn:aws:iam::727378841472:root
            Action:
              - "sts:AssumeRole"
      Path: /
      Policies:
        - PolicyName: nops-sharesave-policy
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Sid: S3ViewCUR
                Effect: Allow
                Action:
                  - "s3:ListBucket"
                Resource: !Join ["", ["arn:",!Ref AWS::Partition,":s3:::",!Ref "S3CurBucket"]]
              - Sid: S3AccessCUR
                Effect: Allow
                Action:
                  - "s3:GetObject"
                  - "s3:PutObject"
                  - "s3:DeleteObject"
                Resource: !Join ["", ["arn:",!Ref AWS::Partition,":s3:::",!Ref "S3CurBucket","/*"]]
      ManagedPolicyArns:
        - "arn:aws:iam::aws:policy/AWSSupportAccess"

```

## Commitment Managment IAM Roles ##

### CM AWS Payer IAM Policy ###


```json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::<YOUR BUCKET NAME HERE>",
            "Effect": "Allow",
            "Sid": "S3ViewCUR"
        },
        {
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::<YOUR BUCKET NAME HERE>/*",
            "Effect": "Allow",
            "Sid": "S3AccessCUR"
        }
    ]
}

```

### CM Linked Account IAM Policy ###


```json

{
    "Version": "2012-10-17",
    "Statement": [
            {
        "Action": [
            "ec2:DescribeReservedInstances",
            "ec2:DescribeReservedInstancesListings",
            "ec2:DescribeReservedInstancesModifications",
            "ec2:DescribeReservedInstancesOfferings",
            "ec2:ModifyReservedInstances",
            "ec2:PurchaseReservedInstancesOffering",
            "ec2:CreateReservedInstancesListing",
            "ec2:CancelReservedInstancesListing",
            "ec2:GetReservedInstancesExchangeQuote",
            "ec2:AcceptReservedInstancesExchangeQuote",
            "rds:DescribeReservedDBInstances",
            "rds:DescribeReservedDBInstancesOfferings",
            "rds:PurchaseReservedDBInstancesOffering",
            "support:*",
            "SavingsPlans:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}

```


<br/><br/>

{% include custom/series_related.html %}
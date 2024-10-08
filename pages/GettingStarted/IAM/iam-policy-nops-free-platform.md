---
title: IAM permissions for the nOps platform
keywords: iam policies, iam, setup, onboarding
tags: [onboarding, iam]
sidebar: mydoc_sidebar
permalink: iam-policy-nops-free-platform.html
folder: GettingStarted
series: [Onboarding, IAM]
weight: 2.0
---

## IAM Policy Permissions for the nOps Platform ##

nOps requires safe, secure, and AWS-approved cross account access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what you want us to see in order to provide our services, no more, and we need you to give us permission first. 


**For AWS Payer/Management Account and AWS Linked accounts, nOps uses the following policies:**

1.  AWS managed [ReadOnlyAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/ReadOnlyAccess) policy, which is completely managed by AWS and is updated periodically as AWS adds new services.
2.  Since the AWS managed ReadOnlyAccess policy contains some read access to sensitive data, nOps uses an explicit deny list which can be easily update for your own security requires. – [Explicit Deny List](#explicit-deny)
3.  Lastly, few other policies that are necessary to create the Cost and Usage Report for Cost Visibility, Well-Architected Review and placeholders to support automating the setup for nOps Commitment Management Program. [CUR](#cur),  [S3](#s3bucket), [Well-Architected](#well-architected), [EventBridge](#eventbridge), and [Organization](#organizations)


**IAM Policy JSON** – [Policy – JSON](#IAM-Policy-JSON)

## What? Why? and How Much? ##
The following tables describe each permission within the IAM policy:

* First column: Permission name.
* Second column: What the permission is?
* Third column: Why the permission is important for nOps?
* Forth column: What kind of access the permission gives to nOps?What? Why? and How Much?The following tables describe each permission within the IAM policy:
    * First column: Permission name.
    * Second column: What the permission is?
    * Third column: Why the permission is important for nOps?
    * Forth column: What kind of access the permission gives to nOps?


|<span id="cur"> **CUR** </span>| **What** | **Why** | **Access (Full: Read, Limited: Write)** |
|-------|--------|---------|----------------------|
| DescribeReportDefinitions | Lists the AWS Cost and Usage Report available to this account. | Used for creating reports in billing bucket setup. | Read: All resource                       |
| PutReportDefinition | Creates a new report using the description that you provide. | Used for creating reports in billing bucket setup. | Write: All resource |



| <span id="eventbridge">**EventBridge**</span> | **What** | **Why** | **Access(Limited: Write)** |
| --- | --- | --- | --- |
| CreateEventBus | Creates a new event bus within your account. | Allows nOps to create EventBridge integrations for automation. Required for the Commitment Management program. | Write: All resources |



| <span id="organizations">**Organizations**</span> | **What** | **Why** | **Access (Limited: Write, Full: List, Read)** |
| --- | --- | --- | --- |
| InviteAccountToOrganization | Sends an invitation to another AWS account, asking it to join your organization as a member account. | Required for onboarding child accounts via CloudFormation stack during Automatic Setup and the Commitment Management program. | Write: All resources |


| <span id="S3">**S3**</span>  | **What** | **Why** | **Access (Limited: Read)** |
| --- | --- | --- | --- |
| HeadBucket | Allows you to determine if a bucket exists and you have permission to access it. | This permission allows nOps to see if the bucket for CUR already exists or do we need need to create one. | Read |
| HeadObject | The HEAD action retrieves metadata from an object without returning the object itself | This permission allows nOps to only see the metadata of a bucket without allowing nOps to see the bucket’s contents. | Read |

|  <span id="support">**Support**</span> | **What** | **Why** | **Access (Limited: Read)** |
| --- | --- | --- | --- |
| DescribeTrustedAdvisorCheckRefreshStatuses | Returns the refresh status of the AWS Trusted Advisor checks that have the specified check IDs. | Not used anymore. | Read: All resources |
| DescribeTrustedAdvisorCheckResult | Returns the results of the AWS Trusted Advisor check that has the specified check ID. | Not used anymore. | Read: All resources |
| DescribeTrustedAdvisorChecks | Returns information about all available AWS Trusted Advisor checks, including the name, ID, category, description, and metadata. | Not used anymore. | Read: All resources |


| <span id="well-architected">**Well-Architected** | **What** | **Why** | **Access (Full access)** |
| --- | --- | --- | --- |
| wellarchitected | Gives full access to Well-Architected. | nOps provides a full functionality dedicated for wellarchitected compliances and it requires full access of this component for managing cloud workloads. | Full access |

## Explicit Deny ##

The following is the list of services for which nOps explicitly denies the permission:

* [ACM (AWS Certificate Manager)](#acm)
* [API Gateway](#apigateway)
* [AppConfig](#appconfig)
* [AppFlow](#appflow)
* [AppStream](#appstream)
* [AppSync](#appstream)
* [Athena](#athena)
* [Backup](#backup)
* [Cassandra](#cassandra)
* [Chime](#chime)
* [Cloud9](#cloud9)
* [Cloud Directory](#clouddirectory)
* [CloudFront](#cloudfront)
* [CloudWatch](#cloudwatch)
* [CodeArtifact](#codeartifact)
* [CodeBuild](#codebuild)
* [CodeCommit](#codecommit)
* [CodeDeploy](#codedeploy)
* [CodeStar](#codestar)
* [Cognito](#cognito)
* [Comprehend](#compehend)
* [Config](#config)
* [Connect](#connect)
* [Data Pipeline](#datapipeline)
* [DAX (DynamoDB Accelerator)](#dax)
* [DeepComposer](#deepcomposer)
* [Device Farm](#devicefarm)
* [Direct Connect](#directconnect)
* [Discovery](#discovery)
* [DMS (Database Migration Service)](#dms)
* [DS (Directory Service)](#ds)
* [DynamoDB](#dynamodb)
* [EC2 (Elastic Compute Cloud)](#ec2)
* [ECR (Elastic Container Registry)](#ecr)
* [EKS (Elastic Kubernetes Service)](#eks)
* [Elastic Beanstalk](#elasticbeanstalk)
* [ES (Elasticsearch)](#es)
* [FIS (Fault Injection Simulator)](#fis)
* [FMS (Firewall Manager)](#fms)
* [Fraud Detector](#fraudetector)
* [GameLift](#gamelift)
* [GeoLocation](#geolocation)
* [Glue](#glue)
* [GuardDuty](#guardduty)
* [Inspector 2](#inspector)
* [Image Builder](#imagebuilder)
* [IoT RoboRunner](#iotrobo)
* [IoT SiteWise](#iotsite)
* [IVS (Interactive Video Service)](#ivs)
* [Kafka](#kafka)
* [Kendra](#kendra)
* [Kinesis](#kinesis)
* [KMS (Key Management Service)](#kms)
* [Lex](#lex)
* [Lambda](#lambda)
* [License Manager](#licensemanager)
* [Lightsail](#lightsail)
* [Logs](#logs)
* [ML (Machine Learning)](#ml)
* [Macie2](#macie)
* [Mobile Hub](#mobilehub)
* [Nimble](#nimble)
* [Polly](#polly)
* [Proton](#proton)
* [QLDB (Quantum Ledger Database)](#qldb)
* [RDS (Relational Database Service)](#rds)
* [Rekognition](#rekognition)
* [Resilience Hub](#resiliencehub)
* [RoboMaker](#robomaker)
* [S3 (Simple Storage Service)](#s3deny)
* [SageMaker](#sagemaker)
* [Schemas](#schemas)
* [SDB (SimpleDB)](#sdb)
* [Secrets Manager](#secrets)
* [Security Hub](#securityhub)
* [SES (Simple Email Service)](#ses)
* [Signer](#signer)
* [SMS (Server Migration Service)](#sms)
* [Snowball](#snowball)
* [SQS (Simple Queue Service)](#sqs)
* [S SM (Systems Manager Agent)](#ssm)
* [SSO (Single Sign-On)](#sso)
* [Storage Gateway](#storaregateway)
* [Support](#supportdeny)
* [TimeStream](#timestream)
* [Transcribe](#transcribe)
* [Transfer](#transfer)
* [WAF (Web Application Firewall)](#waf)
* [WorkMail](#workmail)



| <span id="acm">ACM (AWS Certificate Manager) </span> | What |
| --- | --- |
| acm-pca:Describe | Denies all Describe permissions in ACM-PCA. |
| acm-pca:Get | Denies all Get permissions in ACM-PCA. |
| acm-pca:List | Denies all List permissions in ACM-PCA. |
| acm:Describe | Denies all Describe permissions in ACM. |
| acm:Get | Denies all Get permissions in ACM. |
| acm:List | Denies all List permissions in ACM. |



| <span id="apigateway">API Gateway</span> | What |
| --- | --- |
| GET | Denies all Get permission for API Gateway. |


| <span id="appconfig">AppConfig</span> | What |
| --- | --- |
| GetConfiguration | Denies the permission to view details about a configuration. |


| <span id="appflow">AppFlow</span>  | What |
| --- | --- |
| DescribeConnector | Denies the permission to describe a connector registered in Amazon AppFlow. |
| ListConnector | Denies the permission to list connectors supported in Amazon AppFlow. |


| <span id="appstream">AppStream</span>  | What |
| --- | --- |
| DescribeDirectoryConfigs | Denies the permission to retrieve a list that describes one or more specified Directory Config objects for AppStream 2.0. |
| DescribeUsers | Denies the permission to retrieve a list that describes one or more specified users in the user pool. |
| DescribeSessions | Denies the permission to retrieve a list that describes the streaming sessions for a specified stack and fleet. |


| <span id="appsync">AppSync</span>  | What |
| --- | --- |
| Get | Denies the permission to read resources in this service. |
| List | Denies the permission to list resources in this service. |


| <span id="athena">Athena</span>  | What |
| --- | --- |
| Get | Denies the permission to read resources in this service. |
| List | Denies the permission to list resources in this service. |


| <span id="backup">Backup</span>  | What |
| --- | --- |
| GetBackupVaultAccessPolicy | Denies the permission to get backup vault access policy. |


| <span id="cassandra">Cassandra (Keyspaces)</span>  | What |
| --- | --- |
| Select | Denies the permission to SELECT data from table. |

| --- | --- |
| <span id="chime">Chime</span>  | What |
| --- | --- |
| Describe | Denies the permission to read resources in this service. |
| Get | Denies the permission to read resources in this service. |
| List | Denies the permission to list resources in this service. |


| <span id="cloud9">Cloud9</span>  | What |
| --- | --- |
| Describe | Denies the permission to read resources in this service. |
| Get | Denies the permission to read resources in this service. |
| List | Denies the permission to read resources in this service. |


| <span id="clouddirectory">Cloud Directory</span>  | What |
| --- | --- |
| Get | Denies the permission to read resources in this service. |
| List | Denies the permission to list resources in this service. |


| <span id="cloudfront">CloudFront</span>  | What |
| --- | --- |
| GetCloudFrontOriginAccessIdentity | Denies the permission to get the information about a cloud front origin access identity. |
| GetFieldLevelEncryption | Denies the permission to get the field-level encryption configuration information. |
| GetKeyGroupConfig | Denies the permission to get a key group configuration. |


| <span id="cloudwatch">CloudWatch</span>  | What |
| --- | --- |
| GetMetricData | Denies the permission to retrieve batch amounts of CloudWatch metric data and perform metric math on retrieved data. |
| GetMetricStream | Denies the permission to return the details of a CloudWatch metric stream. |
| ListMetricStreams | Denies the permission to return a list of all CloudWatch metric streams in your account. |


| <span id="codealert">CodeArtifact</span>  | What |
| --- | --- |
| GetAuthorizationToken | Denies the permission to generate a temporary authorization token for accessing repositories in a domain. |
| ReadFromRepository | Denies the permission to return package assets and metadata from a repository endpoint. |


| <span id="codebuild">CodeBuild</span>  | What |
| --- | --- |
| BatchGet | Denies the permission to all BatchGet permissions. |
| ListSourceCredentials | Denies the permission to return a list of SourceCredentialsInfo objects. |


| <span id="codecommit">CodeCommit</span>  | What |
| --- | --- |
| BatchGet | Denies the permission to all BatchGet permissions. |
| Get | Denies the permission to Get permissions. |
| GitPull | Denies the permission to pull information from an AWS CodeCommit repository to a local repo. |


| <span id="codedeploy">CodeDeploy</span>  | What |
| --- | --- |
| BatchGet | Denies the permission to all BatchGet permissions. |
| Get | Denies the permission to Get permissions. |


| <span id="codestar">CodeStar</span>  | What |
| --- | --- |
| DescribeUserProfile | Denies the permission to describe a user in AWS CodeStar and the user attributes across all projects. |
| ListUserProfiles | Denies the permission to list user profiles in AWS CodeStar. |


| <span id="cognito">Cognito</span>  | What |
| --- | --- |
| cognito-identity (Cognito Identity) | Denies the permission to access any resources in this service. |
| cognito-idp (Cognito User Pools) | Denies the permission to access any resources in this service. |
| cognito-sync (Cognito Sync) | Denies the permission to access any resources in this service. |


| <span id="comprehend">Comprehend</span>  | What |
| --- | --- |
| Describe | Denies the permission to Describe resources. |
| List | Denies the permission to List resources. |


| <span id="config">Config</span>  | What |
| --- | --- |
| BatchGetAggregateResourceConfig | Denies the permission to return the current configuration items for resources that are present in your AWS Config aggregator. |
| BatchGetResourceConfig | Denies the permission to return the current configuration for one or more requested resources. |
| SelectAggregateResourceConfig | Denies the permission to accept a structured query language (SQL) SELECT command and an aggregator to query configuration state of AWS resources across multiple accounts and regions, performs the corresponding search, and returns resource configuration matching the properties. |
| SelectResourceConfig | Denies the permission to accept a structured query language (SQL) SELECT command, performs the corresponding search, and returns resource configurations matching the properties. |


| <span id="connect">Connect</span>  | What |
| --- | --- |
| Describe | Denies the permission to Describe resources |
| Get | Denies the permission to Get resources. |
| List | Denies the permission to List resources. |


| <span id="datapipeline">Data Pipeline</span>  | What |
| --- | --- |
| DescribeObjects | Denies the permission to get the object definitions for a set of objects associated with the pipeline. |
| EvaluateExpression | Denies the permission to task runners to call EvaluateExpression, to evaluate a string in the context of the object. |
| QueryObjects | Denies the permission to query the specified pipeline for the names of the objects that match the specified set of conditions. |


| <span id="dax">DAX (DynamoDB Accelerator)</span>  | What |
| --- | --- |
| BatchGetItem | Denies the permission to return the attributes of one or more items from one or more tables. |
| GetItem | Denies the permission to the GetItem operation that returns a set of attributes for the item with the given primary key. |
| Query | Denies the permission to use the primary key of a table or a secondary index to directly access items from that table or index. |


| <span id="deepcomposer">DeepComposer</span>  | What |
| --- | --- |
| Get | Denies all Get permissions in DeepComposer. |
| List | Denies all List permissions in DeepComposer. |


| <span id="devicefarm">Device Farm</span>  | What |
| --- | --- |
| GetRemoteAccessSession | Denies the permission to retrieve the link to a currently running remote access session. |
| ListRemoteAccessSessions | Denies the permission to list the information of currently running remote access sessions. |


| <span id="directconnect">Direct Connect</span>  | What |
| --- | --- |
| Describe | Denies all Describe permissions in Direct Connect. |
| List | Denies all List permissions in Direct Connect. |


| <span id="discovery">Discovery</span>  | What |
| --- | --- |
| Describe | Denies all Describe permissions in Discovery. |
| Get | Denies all Get permissions in Discovery. |
| List | Denies all List permissions in List. |


| <span id="dms">DMS (Database Migration Service)</span>  | What |
| --- | --- |
| Describe | Denies all DEscribe permissions in DMS. |
| List | Denies the permission to list all tags for AWS DMS resources. |


| <span id="ds">DS (Directory Service)</span>  | What |
| --- | --- |
| Get | Denies all Get permission in Directory Service. |


| <span id="dynamodb">DynamoDB</span>  | What |
| --- | --- |
| GetItem | Denies permission to the GetItem operation that returns a set of attributes for the item with the given primary key. |
| BatchGetItem | Denies permission to return the attributes of one or more items from one or more tables. |
| Query | Denies permission to use the primary key of a table or a secondary index to directly access items from that table or index. |
| Scan | Denies the permission to return one or more items and item attributes by accessing every item in a table or a secondary index. |


| <span id="ec2">EC2 (Elastic Compute Cloud)</span>  | What |
| --- | --- |
| GetConsoleScreenshot | Denies the permission to retrieve a JPG-format screenshot of a running instance. |


| <span id="ecr">ECR (Elastic Container Registry)</span>  | What |
| --- | --- |
| ecr:BatchGetImage | Denies the permission to get detailed information for specified images within a specified repository. |
| ecr:GetAuthorizationToken | Denies the permission to retrieve a token that is valid for a specified registry for 12 hours. |
| ecr:GetDownloadUrlForLayer | Denies the permission to retrieve that download URL corresponding to an image layer. |
| ecr-public:GetAuthorizationToken | Denies the permission to retrieve a token that is valid for a specified registry for 12 hours. |


| <span id="eks">EKS (Elastic Kubernetes Service)</span>  | What |
| --- | --- |
| DescribeIdentityProviderConfig | Denies the permission to retrieve descriptive information about an Idp config associated with a cluster. |


| <span id="elasticbeanstalk">Elastic Beanstalk</span>  | What |
| --- | --- |
| DescribeConfigurationOptions | Denies the permission to retrieve descriptions of environment configuration options. |
| DescribeConfigurationSettings | Denies the permission to retrieve a description of the settings for a configuration set. |


| <span id="es">ES (OpenSearch Service)</span>  | What |
| --- | --- |
| ESHttpGet | Denies the permission to send HTTP GET request to the OpenSearch APIs. |


| <span id="fis">FIS (Fault Injection Simulator)</span>  | What |
| --- | --- |
| GetExperimentTemplate | Denies the permission to retrieve an AWS FIS Experiment Template. |


| <span id="fms">FMS (Firewall Manager)</span>  | What |
| --- | --- |
| GetAdminAccount | Denies the permission to retrieve the AWS Organization master account that is associated with AWS Firewall Manager as the AWS Firewall Manager administrator. |


| <span id="frauddetector">Fraud Detector</span>  | What |
| --- | --- |
| BatchGetVariable | Denies the permission to get a batch of variables. |
| Get | Denies all Get permission in Fraud Detector. |


| <span id="gamelift">GameLift</span>  | What |
| --- | --- |
| GetGameSessionLogUrl | Denies the permission to retrieve the location of stored logs for a game session. |
| GetInstanceAccess | Denies the permission to request remote access to a specified fleet instance. |


| <span id="geolocation">GeoLocation (Location)</span>  | What |
| --- | --- |
| ListDevicePositions | Denies the permission to retrieve a list of devices and their latest positions from the given tracker resource. |


| <span id="glue">Glue</span>  | What |
| --- | --- |
| GetSecurityConfiguration | Denies the permission to retrieve a security configuration. |
| SearchTables | Denies the permission to retrieve the tables in the catalog. |
| GetTable | Denies all GetTable permission in Glue. |


| <span id="guardduty">GuardDuty</span>  | What |
| --- | --- |
| GetIPSet | Denies the permission to retrieve GuardDuty IPSets |
| GetMasterAccount | Denies the permission to retrieve details of the GuardDuty administrator account associated with a member account. |
| GetMembers | Denies the permission to retrieve the member accounts associated with an administrator account. |
| ListMembers | Denies the permission to retrieve a list of GuardDuty member accounts associated with an administrator account. |
| ListOrganizationAdminAccounts | Denies the permission to list details about the organization delegated administrator for GuardDuty. |


| <span id="inspector">Inspector 2</span>  | What |
| --- | --- |
| GetConfiguration | Denies the permission to retrieve information about the Amazon Inspector configuration settings for an AWS account. |


| <span id="imagebuilder">Image Builder</span>  | What |
| --- | --- |
| GetImage | Denies the permission to get an EC2 image. |


| <span id="iotrobo">IoT RoboRunner</span>  | What |
| --- | --- |
| Get | Denies all Get permission in IoT RoboRunner. |


| <span id="iotsite">IoT SiteWise</span>  | What |
| --- | --- |
| ListAccessPolicies | Denies the permission to lit all access policies for an identity or a resource. |


| <span id="ivs">IVS (Interactive Video Service)</span>  | What |
| --- | --- |
| GetPlaybackKeyPair | Denies the permission to get the playback keypair information for a specified ARN. |
| GetStreamSession | Denies the permission to get information about the stream session on a specified channel. |


| <span id="kafka">Kafka (MSK)</span>  | What |
| --- | --- |
| GetBootstrapBrokers | Denies the permission to get connection details for the brokers in an MSK cluster. |


| <span id="kendra">Kendra</span>  | What |
| --- | --- |
| Query | Denies the permission to query documents and faqs. |


| <span id="kinesis">Kinesis</span>  | What |
| --- | --- |
| Get | Denies all Get permission in Kinesis. |


| <span id="kms">KMS (Key Management Service)</span>  | What |
| --- | --- |
| DescribeKey | Denies the control to the permission to view detailed information about an AWS KMS key. |
| GetPublicKey | Denies the control to the permission to download the public key of an asymmetric AWS KMS Key. |


| <span id="lex">Lex</span>  | What |
| --- | --- |
| Get | Denies all Get permission in Lex. |


| <span id="lambda">Lambda</span>  | What |
| --- | --- |
| GetFunctionConfiguration | Denies the permission to view details about the version-specific settings of an AWS Lambda function or version. |


| <span id="licensemanager">License Manager</span>  | What |
| --- | --- |
| GetGrant | Denies the permission to get a grant. |
| GetLicense | Denies the permission to get a license. |
| ListTokens | Denies the permission to list tokens. |


| <span id="lightsail">Lightsail</span>  | What |
| --- | --- |
| GetBucketAccessKeys | Denies the permission to get the existing access key IDs for the specified Amazon Lightsail bucket. |
| GetCertificates | Denies the permission to view information about one or more Amazon Lightsail SSL/TLS certificates. |
| GetContainerImages | Denies the permission to view the container images that are registered to your Amazon Lightsail container service. |
| GetKeyPair | Denies the permission to get information about a key pair. |
| GetRelationalDatabaseLogStreams | Denies the permission to get the log streams available for a relational database. |


| <span id="logs">Logs</span>  | What |
| --- | --- |
| GetLogEvents | Denies the permission to list log events from the specified log stream. |
| StartQuery | Denies the permission to schedule a query of a log group using CloudWatch Logs Insights. |


| <span id="ml">ML (Machine Learning)</span>  | What |
| --- | --- |
| GetMLModel | Denies the permission to return an MLModel that includes detailed metadata, and data source information as well as the current status of the MLModel. |


| <span id="macie">Macie2</span>  | What |
| --- | --- |
| GetAdministratorAccount | Denies the permission to retrieve information about the Amazon Macie administrator account for an account. |
| GetMember | Denies the permission to retrieve information about an account that’s associated with an Amazon Macie administrator account. |
| GetMacieSession | Denies the permission to retrieve information about the status and configuration settings for an Amazon Macie account. |
| SearchResources | Denies the permission to retrieve statistical data and other information about AWS resources that Amazon MAcie monitors and analyzes. |
| GetSensitiveDataOccurrences | Denies the permission to retrieve occurrences of sensitive data reported by a finding. |


| <span id="mobilehub">Mobile Hub</span>  | What |
| --- | --- |
| ExportProject | Denies the permission to export the project configuration. |


| <span id="nimble">Nimble Studio</span>  | What |
| --- | --- |
| GetStreamingSession | Denies the permission to get a streaming session. |


| <span id="polly">Polly</span>  | What |
| --- | --- |
| SynthesizeSpeech | Denies the permission to synthesize speech. |


| <span id="proton">Proton</span>  | What |
| --- | --- |
| GetEnvironmentTemplate | Denies the permission to describe an environment template. |
| GetServiceTemplate | Denies the permission to describe a service template. |
| ListServiceTemplates | Denies the permission to list service templates. |
| ListEnvironmentTemplates | Denies the permission to list environment templates. |


| <span id="qldb">QLDB (Quantum Ledger Database)</span>  | What |
| --- | --- |
| GetBlock | Denies the permission to retrieve a block from a ledger for a given BlockAddress. |
| GetDigest | Denies the permission to retrieve a digest from a ledger from a given BlockAddress. |


| <span id="rds">RDS (Relational Database Service)</span>  | What |
| --- | --- |
| Download | Denies all Download permission for RDS. |


| <span id="rekognition">Rekognition</span>  | What |
| --- | --- |
| CompareFaces | Denies the permission to compare faces in the source input images with each face detected in the target input image. |
| Detect | Denies all Detect permissions in Rekognition. |
| Search | Denies all Search permission in Rekognition. |


| <span id="resiliencehub">Resilience Hub</span>  | What |
| --- | --- |
| DescribeAppVersionTemplate | Denies the permission to describe the application version template. |
| ListRecommendationTemplates | Denies the permission to list recommendation templates. |


| <span id="robomaker">RoboMaker</span>  | What |
| --- | --- |
| GetWorldTemplateBody | Denies the permission to get the body of a world template. |


| <span id="s3deny">S3 (S3 Object Lambda)</span>  | What |
| --- | --- |
| s3-object-lambda:GetObject | Denies the permission to retrieve objects from Amazon S3. |


| <span id="sagemaker">SageMaker</span>  | What |
| --- | --- |
| Search | Denies the permission to search for SageMaker objects. |


| <span id="schemas">Schemas (EventBridgeSchemas)</span>  | What |
| --- | --- |
| GetDiscoveredSchema | Denies the permission to retrieve a schema for the provided list of sample events. |


| <span id="sbd">SDB (SimpleDB)</span>  | What |
| --- | --- |
| Get | Denies all Get permissions for SDB. |
| Select | Denies all Select permissions for SDB. |


| <span id="secrets">Secrets Manager</span>  | What |
| --- | --- |
| *   | Denies all permission in Secrets Manager. |


| <span id="securityhub">Security Hub</span>  | What |
| --- | --- |
| GetFindings | Denies the permission to retrieve a list of findings from Security Hub. |
| GetMembers | Denies the permission to retrieve the details of Security Hub member accounts. |
| ListMembers | Denies the permission to retrieve details about Security Hub member accounts associated with the administrator account. |


| <span id="ses">SES (SES v1, SES v2)</span>  | What |
| --- | --- |
| GetTemplate | Denies the permission to return the template object, which includes the subject line, HTML part, and text part for the template you specify. |
| GetEmailTemplate | Denies the permission to return the template object, which includes the subject line, HTML part, and text part for the template you specify. |
| GetContact | Denies the permission to return a contact from a contact list. |
| GetContactList | Denies the permission to return contact list metadata. |
| ListTemplates | Denies the permission to list the email templates present in your account. |
| ListEmailTemplates | Denies the permission to list all of the email templates for your account. |
| ListVerifiedEmailAddresses | Denies the permission to list all of the email addresses that have been verified. |


| <span id="signer">Signer</span>  | What |
| --- | --- |
| GetSigningProfile | Denies the permission to return information about a specific Signing Profile. |
| ListProfilePermissions | Denies the permission to list the cross-account permissions associated with a Signing Profile. |
| ListSigningProfiles | Denies the permission to list all Signing Profiles in your account. |


| <span id="sms">SMS (Pinpoint SMS Voice V2)</span>  | What |
| --- | --- |
| sms-voice:DescribeKeywords | Denies the permission to describe the keywords for a pool or origination phone number. |
| sms-voice:DescribeOptedOutNumbers | Denies the permission to describe the destination phone numbers in an opt-out list. |
| sms-voice:DescribePhoneNumbers | Denies the permission to describe the origination phone numbers in your account. |
| sms-voice:DescribePools | Denies the permission to describe the pools in your account. |


| <span id="snowball">Snowball</span>  | What |
| --- | --- |
| Describe | Denies all Describe permission for Snowball. |


| <span id="sqs">SQS (Simple Queue Service)</span>  | What |
| --- | --- |
| Receive | Denies all Receive permission in SQS. |


| <span id="ssm">S SM (Systems Manager)</span>  | What |
| --- | --- |
| ssm-contacts:* |     |
| ssm:DescribeParameters | Denies the permission to view details about a specified SSM parameter. |
| ssm:GetParameter | Denies all GetParameter permission in Systems Manager. |


| <span id="sso">SSO (Single Sign-On)</span>  | What |
| --- | --- |
| Describe | Denies all Describe permissions in SSO. |
| Get | Denies all Get permissions in SSO. |
| List | Denies all List permissions in SSO. |


| <span id="storagegateway">Storage Gateway</span>  | What |
| --- | --- |
| DescribeChapCredentials | Denies the permission to get an array of Challenge-Handshake Authentication Protocol (CHAP) credentials information for a specified iSCSI target, one for each target-initiator pair. |


| <span id="supportdeny">Support</span>  | What |
| --- | --- |
| DescribeCommunications | Denies the permission to return the communications and attachments for one or more AWS Support cases. |


| <span id="timestream">TimeStream</span>  | What |
| --- | --- |
| ListDatabases | Denies the permission to list databases in your account. |
| ListTables | Denies the permission to list tables in your account. |


| <span id="transcribe">Transcribe</span>  | What |
| --- | --- |
| Get | Denies all Get permission in Transcribe. |
| List | Denies all List permission in Transcribe. |


| <span id="transfer">Transfer</span>  | What |
| --- | --- |
| Describe | Denies all Describe permission in Transfer. |
| List | Denies all List permission in Transfer. |


| <span id="waf">WAF (WAF Regional)</span>  | What |
| --- | --- |
| waf-regional:GetChangeToken | Denies the permission to retrieve a change token to use in create, update, and delete requests. |


| <span id="workmail">WorkMail</span> | What |
| --- | --- |
| DescribeUser | Denies the permission to read details for a user. |
| GetMailUserDetails | Denies the permission to get the details of the user’s mailbox and account. |
| ListUsers | Denies the permission to list the organization’s users. |

IAM policy for nOps _Last Updated: 12/17/2022_


## **IAM Policy JSON** ##

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "cur:DescribeReportDefinitions",
                "cur:DeleteReportDefinition",
                "cur:PutReportDefinition",
                "ce:ListCostAllocationTags",
                "ce:UpdateCostAllocationTagsStatus",
                "organizations:InviteAccountToOrganization",
                "s3:ListBucket",
                "support:DescribeTrustedAdvisorCheckRefreshStatuses",
                "support:DescribeTrustedAdvisorCheckResult",
                "support:DescribeTrustedAdvisorChecks",
                "wellarchitected:*",
                "ce:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "acm-pca:Describe*",
                "acm-pca:Get*",
                "acm-pca:List*",
                "acm:Describe*",
                "acm:Get*",
                "acm:List*",
                "apigateway:GET",
                "appconfig:GetConfiguration*",
                "appflow:DescribeConnector*",
                "appflow:ListConnector*",
                "appstream:DescribeDirectoryConfigs",
                "appstream:DescribeUsers",
                "appstream:DescribeSessions",
                "appsync:Get*",
                "appsync:List*",
                "athena:Get*",
                "athena:List*",
                "backup:GetBackupVaultAccessPolicy",
                "cassandra:Select",
                "chime:Describe*",
                "chime:Get*",
                "chime:List*",
                "cloud9:Describe*",
                "cloud9:Get*",
                "cloud9:List*",
                "clouddirectory:Get*",
                "clouddirectory:List*",
                "cloudfront:GetCloudFrontOriginAccessIdentity",
                "cloudfront:GetFieldLevelEncryption*",
                "cloudfront:GetKeyGroupConfig",
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStream",
                "cloudwatch:ListMetricStreams",
                "codeartifact:GetAuthorizationToken",
                "codeartifact:ReadFromRepository",
                "codebuild:BatchGet*",
                "codebuild:ListSourceCredentials",
                "codecommit:BatchGet*",
                "codecommit:Get*",
                "codecommit:GitPull",
                "codedeploy:BatchGet*",
                "codedeploy:Get*",
                "codestar:DescribeUserProfile",
                "codestar:ListUserProfiles",
                "cognito-identity:*",
                "cognito-idp:*",
                "cognito-sync:*",
                "comprehend:Describe*",
                "comprehend:List*",
                "config:BatchGetAggregateResourceConfig",
                "config:BatchGetResourceConfig",
                "config:SelectAggregateResourceConfig",
                "config:SelectResourceConfig",
                "connect:Describe*",
                "connect:Get*",
                "connect:List*",
                "datapipeline:DescribeObjects",
                "datapipeline:EvaluateExpression",
                "datapipeline:QueryObjects",
                "dax:BatchGetItem",
                "dax:GetItem",
                "dax:Query",
                "deepcomposer:Get*",
                "deepcomposer:List*",
                "devicefarm:GetRemoteAccessSession",
                "devicefarm:ListRemoteAccessSessions",
                "directconnect:Describe*",
                "directconnect:List*",
                "discovery:Describe*",
                "discovery:Get*",
                "discovery:List*",
                "dms:Describe*",
                "dms:List*",
                "ds:Get*",
                "dynamodb:GetItem",
                "dynamodb:BatchGetItem",
                "dynamodb:Query",
                "dynamodb:Scan",
                "ec2:GetConsoleScreenshot",
                "ecr:BatchGetImage",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer",
                "ecr-public:GetAuthorizationToken",
                "eks:DescribeIdentityProviderConfig",
                "elasticbeanstalk:DescribeConfigurationOptions",
                "elasticbeanstalk:DescribeConfigurationSettings",
                "es:ESHttpGet*",
                "fis:GetExperimentTemplate",
                "fms:GetAdminAccount",
                "frauddetector:BatchGetVariable",
                "frauddetector:Get*",
                "gamelift:GetGameSessionLogUrl",
                "gamelift:GetInstanceAccess",
                "geo:ListDevicePositions",
                "glue:GetSecurityConfiguration*",
                "glue:SearchTables",
                "glue:GetTable*",
                "guardduty:GetIPSet",
                "guardduty:GetMasterAccount",
                "guardduty:GetMembers",
                "guardduty:ListMembers",
                "guardduty:ListOrganizationAdminAccounts",
                "inspector2:GetConfiguration",
                "imagebuilder:GetImage",
                "iotroborunner:Get*",
                "iotsitewise:ListAccessPolicies",
                "ivs:GetPlaybackKeyPair",
                "ivs:GetStreamSession",
                "kafka:GetBootstrapBrokers",
                "kendra:Query*",
                "kinesis:Get*",
                "kms:DescribeKey",
                "kms:GetPublicKey",
                "lex:Get*",
                "lambda:GetFunctionConfiguration",
                "license-manager:GetGrant",
                "license-manager:GetLicense",
                "license-manager:ListTokens",
                "lightsail:GetBucketAccessKeys",
                "lightsail:GetCertificates",
                "lightsail:GetContainerImages",
                "lightsail:GetKeyPair",
                "lightsail:GetRelationalDatabaseLogStreams",
                "logs:GetLogEvents",
                "logs:StartQuery",
                "machinelearning:GetMLModel",
                "macie2:GetAdministratorAccount",
                "macie2:GetMember",
                "macie2:GetMacieSession",
                "macie2:SearchResources",
                "macie2:GetSensitiveDataOccurrences",
                "nimble:GetStreamingSession",
                "polly:SynthesizeSpeech",
                "proton:GetEnvironmentTemplate",
                "proton:GetServiceTemplate",
                "proton:ListServiceTemplates",
                "proton:ListEnvironmentTemplates",
                "qldb:GetBlock",
                "qldb:GetDigest",
                "rds:Download*",
                "rekognition:CompareFaces",
                "rekognition:Detect*",
                "rekognition:Search*",
                "resiliencehub:DescribeAppVersionTemplate",
                "resiliencehub:ListRecommendationTemplates",
                "robomaker:GetWorldTemplateBody",
                "s3-object-lambda:GetObject",
                "sagemaker:Search",
                "schemas:GetDiscoveredSchema",
                "sdb:Get*",
                "sdb:Select*",
                "secretsmanager:*",
                "securityhub:GetFindings",
                "securityhub:GetMembers",
                "securityhub:ListMembers",
                "ses:GetTemplate",
                "ses:GetEmailTemplate",
                "ses:GetContact",
                "ses:GetContactList",
                "ses:ListTemplates",
                "ses:ListEmailTemplates",
                "ses:ListVerifiedEmailAddresses",
                "signer:GetSigningProfile",
                "signer:ListProfilePermissions",
                "signer:ListSigningProfiles",
                "sms-voice:DescribeKeywords",
                "sms-voice:DescribeOptedOutNumbers",
                "sms-voice:DescribePhoneNumbers",
                "sms-voice:DescribePools",
                "snowball:Describe*",
                "sqs:Receive*",
                "ssm-contacts:*",
                "ssm:DescribeParameters*",
                "ssm:GetParameter*",
                "sso:Describe*",
                "sso:Get*",
                "sso:List*",
                "storagegateway:DescribeChapCredentials",
                "support:DescribeCommunications",
                "timestream:ListDatabases",
                "timestream:ListTables",
                "transcribe:Get*",
                "transcribe:List*",
                "transfer:Describe*",
                "transfer:List*",
                "waf-regional:GetChangeToken",
                "workmail:DescribeUser",
                "workmail:GetMailUserDetails",
                "workmail:ListUsers"
            ],
            "Resource": "*",
            "Effect": "Deny"
        },
        {
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::[INSERT CUR S3 BUCKET]",
                "arn:aws:s3:::[INSERT CUR S3 BUCKET]/*"
            ],
            "Effect": "Allow"
        }
    ]
}

```

{% include custom/series_related.html %}
---
title: Container Insights Overview
tags: [agents_integrations]
keywords: CostContainer, integrations, agent, helm, containers, insights
sidebar: mydoc_sidebar
permalink: Container-Insights-Overview.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---



This architecture provides a **scalable and automated solution** for managing IAM roles in Amazon EKS clusters across multiple AWS regions. It leverages **AWS Lambda** to dynamically handle the creation, update, and deletion of IAM roles associated with the service account used by the Container Insights agent. These roles ensure that the agent has the necessary permissions to gather data and upload it to an **Amazon S3** bucket.


### Key Features
- **Automated Role Management**: Lambda functions dynamically manage IAM roles, ensuring each EKS cluster has the required permissions to interact with S3.
- **Multi-Region Support**: Easily extend the solution to EKS clusters across different AWS regions.
- **Flexibility**: The CloudFormation template supports scenarios where an IAM user is required, making the solution adaptable to environments without an OIDC identity provider.


### Deployment Workflow



1. **Deploy the CloudFormation Stack**: 
   - The stack automates the process of creating and managing IAM roles for EKS clusters, ensuring the proper roles are associated with the service account used by the Container Insights agent.
   
    ![Cloudformation](https://nops-help-site-assets.s3.amazonaws.com/images/Agents-Integrations/Cloud+formation+stack.svg)

    
    {% include note.html content="**Optional IAM User Support**: 
    - The template can also handle situations where an IAM user might be needed, making it suitable for environments lacking OIDC identity providers." %}
    
2. **Install the Container Insights Agent**: 
   - Once the stack is deployed, proceed with the installation of the agent within the cluster to begin data collection.

   ![Stack overview](https://nops-help-site-assets.s3.amazonaws.com/images/Agents-Integrations/high+level+diagram+container+insights.svg)




With this setup, your clusters are fully equipped to gather and upload data, providing comprehensive insights into your containerized workloads.

### How the Container Insights Agent Works

   ![Stack overview](https://nops-help-site-assets.s3.amazonaws.com/images/Agents-Integrations/container+insights+cronjobs+HLD.svg)


The agent operates through a series of cronjobs designed to collect and update data from Prometheus and OpenCost:

- **Hourly Cronjobs**:
    - Two cronjobs collect metrics from your cluster and upload Parquet files to the S3 bucket.

- **Daily Cronjob**: A daily cronjob runs to check and update, if necessary, the following components:
  - **Karpenops**
  - **Opencost**
  - **Container Insights cronjobs**
  - **Heartbeat**
  - **Datafetcher**


{% include note.html content="If your cluster includes NVIDIA GPU nodes, the **dcgm-exporter daemonsets** will be deployed on those nodes, exporting GPU-related metrics as well." %}


---

### CloudFormation Template Breakdown

#### 1. Parameters

The template accepts the following input parameters:

- **IncludeRegions**: 
  - A comma-separated list of AWS regions where the solution should operate. 
  - If left blank, the CloudFormation stack defaults to the region where it is being created.

- **RoleName**: 
  - The name of the IAM role to which the read policy will be attached. 
  - This role is created during the onboarding process for each AWS account into nOps.

- **CreateIAMUser**: 
  - A boolean flag (`true` or `false`) that determines whether to create an IAM user.
  - This is helpful for environments that don’t have an IAM OIDC provider for authentication.

---

#### 2. Conditions

The template uses conditions to control resource creation based on the `CreateIAMUser` parameter:

- **CreateIAMUserCondition**: 
  - If `CreateIAMUser` is set to `true`, an IAM user will be created.

- **CreateIAMRoleCondition**: 
  - If `CreateIAMUser` is set to `false`, IAM roles will be created instead.

---

#### 3. Resources

##### **S3 Bucket**

- **S3 Bucket**: 
  - Creates an S3 bucket named `nops-container-cost` suffixed with the account ID, with server-side encryption (AES-256) enabled.
  - This bucket is used to store data collected by the agent on each EKS cluster.
  - **aws:SecureTransport** is enforced through a Bucket Policy.

##### **IAM Policies and Roles**

- **ReadPolicy**: 
  - An IAM policy granting read-only access (`s3:GetObject`, `s3:ListBucket`) to the S3 bucket.
  - This policy is attached to the IAM role specified by the `RoleName` parameter, which is created during the initial AWS account onboarding process.

- **IAMUser**: 
  - This resource is conditionally created based on the `CreateIAMUserCondition`. 
  - If `true`, an IAM user named `nops-container-cost-s3` is created. This supports AWS accounts that lack an OIDC provider for authentication.
  - If this user is created, AWS credentials for the user must be generated and used during agent installation, as detailed in the documentation.

##### **Lambda Functions**

- **NOPSContainerCostRoleCreationFunction**: 
  - A Lambda function responsible for creating and updating IAM roles for EKS clusters across multiple regions. 
  - It is conditionally created if `CreateIAMRoleCondition` is `true`.
  - This function performs the following tasks:
    - Scans EKS clusters in the specified regions.
    - Creates or updates IAM roles associated with the OIDC identity provider for each EKS cluster.
    - Attaches an inline policy that grants interaction with the S3 bucket for each IAM role.
    - A custom CloudFormation resource triggers this function upon stack creation, update, or deletion.

- **NOPSContainerCostRoleManagementFunction**: 
  - A secondary Lambda function that periodically checks the existence of roles and clusters. 
  - It creates missing roles and cleans up orphaned ones. 
  - This function is triggered every two hours via a CloudWatch event rule.

##### **Lambda Execution Role**

- **NOPSContainerCostLambdaRole**: 
  - An IAM role that grants the Lambda functions permissions to manage IAM roles, interact with EKS clusters, and make STS (Secure Token Service) calls. 
  - This role includes all necessary permissions to handle roles and policies across multiple regions.

##### **CloudWatch Event Rule**

- **NOPSContainerCostScheduledCheckEventRule**: 
  - An EventBridge rule that triggers the `NOPSContainerCostRoleManagementFunction` every two hours. 
  - This ensures that IAM roles are correctly aligned with the current state of EKS clusters.

---

#### 4. Outputs

- **S3BucketSecureURL**: 
  - Outputs the secure HTTPS URL of the created S3 bucket.

- **BucketName**: 
  - Outputs the name of the S3 bucket created in the stack.

---
## FAQ

1. **Where can I get information about Security and Compliance?**  
   You can find detailed reports on our stack and policies in [this folder](https://drive.google.com/drive/folders/1pe2raNVzLIHw7cXzpf1s7uIRkiUk9yPL?usp=drive_link).

2. **Where can I find the CloudFormation template for inspection?**  
   You can find the template [here](https://nops-rules-lambda-sources.s3.us-west-2.amazonaws.com/container_cost/container-cost-setup-service-account.yaml).

3. **Which images are used in the deployment, and are they digitally signed?**  
   Yes, the following images are used in our full deployment, and all of them have been digitally signed in our public ECR:

   - [**container-insights-agent**](https://gallery.ecr.aws/nops/container-insights-agent)
   - [**k8s-heartbeat-agent**](https://gallery.ecr.aws/nops/k8s-heartbeat-agent)
   - [**k8s-data-fetcher-agent**](https://gallery.ecr.aws/nops/k8s-data-fetcher-agent)
   - [**karpenops**](https://gallery.ecr.aws/nops/karpenops)
   - [**opencost**](https://gallery.ecr.aws/nops/opencost)
   - [**alpine/k8s**](https://gallery.ecr.aws/nops/alpine/k8s)
   - [**datadog agent**](https://gallery.ecr.aws/nops/datadog/agent)
   - [**kube-state-metrics**](https://gallery.ecr.aws/nops/kube-state-metrics)
   - [**nvidia/dcgm-exporter**](https://gallery.ecr.aws/nops/nvidia/dcgm-exporter)
   - [**prometheus config-reloader**](https://gallery.ecr.aws/nops/prom/config-reloader)
   - [**prometheus node-exporter**](https://gallery.ecr.aws/nops/prom/node-exporter)
   - [**prometheus server**](https://gallery.ecr.aws/nops/prom/prometheus)

   These images are hosted in our public ECR to mitigate rate limit issues and are securely signed using AWS Signer. This ensures the integrity and authenticity of the images, supporting a reliable and trusted deployment pipeline.

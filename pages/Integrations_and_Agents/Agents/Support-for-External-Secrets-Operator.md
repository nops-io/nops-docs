---
title: Support for External Secrets Operator
keywords: CostContainer, integrations, agent, helm, containers, insights, secrets
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Support-for-External-Secrets-Operator.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---

You can use AWS Secrets Manager integration with [External Secrets Operator](https://external-secrets.io/latest/provider/aws-secrets-manager/) to create/sync the API Keys secret in your EKS cluster.

## Prerequisites
1. API Keys stored in AWS Secrets Manager
2. [External Secrets Operator](https://external-secrets.io/latest/) installed in the EKS Cluster
3. An already created [ClusterSecretStore](https://external-secrets.io/latest/api/clustersecretstore/) (Take note of the ClusterSecretStore name as it will be used in helm command)


## Steps to create secret in AWS Secrets Manager
    
1. **Navigate to AWS Secrets Manager service.** 
{% include note.html content="Make sure the secret is created in the same region as the EKS cluster" %}
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.52.52%E2%80%AFp.m..png)

2. **Click on store a new secret**

3. **Select Other type of secret**
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.53.50%E2%80%AFp.m..png)

4. **Add key/value for NOPS_API_KEY and DD_API_KEY respectively**
{% include note.html content="The values can be retrieved in the nOps platform under cluster configuration tab" %}
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.53.50%E2%80%AFp.m..png)
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.54.26%E2%80%AFp.m..png)

5. **Give the secret a name and click Next**
{% include note.html content="Take note of the secret name as it will be used in helm command" %}
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.55.04%E2%80%AFp.m..png)

6. **Click on store**
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.55.29%E2%80%AFp.m..png)

7. **Review new secret**
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+7.55.49%E2%80%AFp.m..png)

## Updated helm command
Once the secret in AWS secrets is created and the values for the API keys is in place you can update the helm command to use the integration with External Secrets Operator to create/sync the secret without explicitely passing the value in the helm command.

Example command for Karpenter enabled clusters:
```bash
helm upgrade -i nops-kubernetes-agent oci://public.ecr.aws/nops/kubernetes-agent \
  --namespace nops --create-namespace \
  --set containerInsights.enabled=true \
  --set containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=arn:aws:eks:us-east-1:123456789101:cluster/example-cluster  \ # Your target EKS cluster ARN
  --set containerInsights.env_variables.APP_AWS_S3_BUCKET=nops-container-cost-12345678101 \ # Your S3 Bucket name for Container Cost integration
  --set karpenops.enabled=true \
  --set karpenops.image.tag=1.23.2 \
  --set karpenops.clusterId=a+ABC1 \
  --set externalSecrets.enabled=true \ # Set it to true to enable External Secrets integration
  --set externalSecrets.secretStoreRef.name=<your_cluster_external_secret_store_name> \ # Prerequisite 3
  --set externalSecrets.data.apiKeys.remoteRef.key=<your_aws_secret_name> # Get it from step 4
```

Example command for ClusterAutoscaler enabled clusters:
```bash
helm upgrade -i nops-kubernetes-agent oci://public.ecr.aws/nops/kubernetes-agent \
  --namespace nops --create-namespace \
  --set containerInsights.enabled=true \
  --set containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=arn:aws:eks:us-east-1:123456789101:cluster/example-cluster  \ # Your target EKS cluster ARN
  --set containerInsights.env_variables.APP_AWS_S3_BUCKET=nops-container-cost-12345678101 \ # Your S3 Bucket name for Container Cost integration
  --set karpenops.enabled=false \
  --set externalSecrets.enabled=true \ # Set it to true to enable External Secrets integration
  --set externalSecrets.secretStoreRef.name=<your_cluster_external_secret_store_name> \ # Prerequisite 3
  --set externalSecrets.data.apiKeys.remoteRef.key=<your_aws_secret_name> # Get it from step 4
```
---
After a successful installation, you'll see that a ClusterExternalSecret was created as well a secret named **nops-api-keys** under the nops namespace with both API Keys.

```bash
kubectl -n nops get externalsecret
kubectl -n nops get secrets
```
![](https://nops-help-site-assets.s3.amazonaws.com/images/external-secrets-support/Screenshot+2024-08-26+at+8.38.53%E2%80%AFp.m..png)
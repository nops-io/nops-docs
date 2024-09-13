---
title: Configure nOps Kubernetes Agent
keywords: CostContainer, integrations, agent, helm, containers, insights
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Configure-nOps-Kubernetes-Agent-on-EKS.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---


{% include note.html content="If you have previously installed the container-insights (nops-k8s-agent) and want to migrate to the unified installation of agents, please refer to this [migration guide](https://help.nops.io/Migration-nOps-Kubernetes-Agent-on-EKS.html)." %}



The nOps Kubernetes Agent is required to fully utilize nOps features for your EKS clusters. It's a bundle of components that will enable you to leverage our Compute Copilot product and provide container visibility into the cluster.




## Prerequisites
1. You must have an active nOps account. If you do not have one, please register on <a href="https://app.nops.io/" target="_blank">nOps</a>
2. Make sure you have access to the Kubernetes cluster (recommended version v1.23.6 or later) to deploy the agent.
3. <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" target="_blank">aws cli</a>
4. <a href="https://helm.sh/" target="_blank">Helm</a>
5. <a href="https://kubernetes.io/docs/reference/kubectl/overview/" target="_blank">kubectl</a>
6. Unix-like terminal to execute the installation script.
7. Terraform if installing via [Infrastructure as Code (IaC)](https://github.com/nops-io/nops-docs/blob/main/pages/Integrations_and_Agents/Agents/Configure-nOps-Kubernetes-Agent-on-EKS.md#installation-using-insfrastructure-as-code).
8. Ensure that the EBS CSI Driver is installed with a Storage Class configured for the EKS cluster to dynamically create EBS gp2/gp3 volumes.
9. If you want to enable karpenOps agent, make sure Karpenter is installed in the cluster.

For karpenOps specific documentation, please click <a href="https://help.nops.io/copilot-eks-onboarding.html" target="_blank">here</a>.


## Steps to Configure nOps Kubernetes Agent
    
  - ![Organization Settings](https://nops-help-site-assets.s3.amazonaws.com/images/setup-containercost-buckets.gif)

 **Navigate to Container Cost Tab**
    - Go to your [Container Cost Integrations Settings](https://app.nops.io/v3/settings?tab=Integrations&subTab=Container-Cost).
2. **Setup Container Cost Integration**
    - Click the **Setup** button for the desired account. Ensure you are authenticated into that account.
    - This step will:
        - Create an S3 bucket in your AWS account.
        - Grant writing permissions to the agent for writing files to that bucket.
        - Use either your OIDC Identity Provider to create a service role or an IAM User with permissions to write to the S3 bucket.
        - Allow nOps to copy those files.
3. **Configure CloudFormation Stack**
    - On the CloudFormation stack creation page:
        - List the regions where your clusters for that specific account are located, separated by commas (e.g., `us-east-1,us-east-2,us-west-1,us-west-2`).
        - *(Optional)* If you don't have an IAM OIDC provider configured, you can create an IAM User to grant the agent permissions to write to the S3 bucket. To do this, set `CreateIAMUser` to `true`.
            If you choose this approach, you must create and store secret key credentials to replace values in the installation script.
        - Select the **I acknowledge that AWS CloudFormation might create IAM resources with custom names** checkbox.
        - Click the **Create stack** button.
4. **Create resources via Terraform *(Optional)***
    - If you prefer you can create the infrastructure using Terraform, use the instructions from the following [repo](https://github.com/nops-io/nops-containercost-S3-terraform).
5. **Check Integration Status**
    - After the creation is complete, return to the nOps platform.
    - Click the **Check Status** button to verify the integration status.
6. **Install Agent in your clusters**
    - Now in order to install the agent in your clusters, you must go to the EKS section in your nOps platform
    - Click on the cluster you want to install the agent,
    - Click on the Cluster Configuration tab 
    - Copy the command to install the agent for in that particular cluster (Make sure to be authenticated and having that cluster in current context of your kubectl)


    Example command for Karpenter enabled clusters:
    ```bash
      helm upgrade -i nops-kubernetes-agent oci://public.ecr.aws/nops/kubernetes-agent \
        --namespace nops --create-namespace \
        --set datadog.apiKey=realkeysonlyinprod \
        --set containerInsights.enabled=true \
        --set containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=arn:aws:eks:us-east-1:123456789101:cluster/example-cluster  \
        --set containerInsights.env_variables.APP_AWS_S3_BUCKET=nops-container-cost-12345678101 \
        --set karpenops.enabled=true \
        --set karpenops.image.tag=1.23.2 \
        --set karpenops.apiKey=*******************************a004eb \
        --set karpenops.clusterId=D/M7Yj
    ```

    Example command for ClusterAutoscaler enabled clusters:
    ```bash
      helm upgrade -i nops-kubernetes-agent oci://public.ecr.aws/nops/kubernetes-agent \
        --namespace nops --create-namespace \
        --set datadog.apiKey=realkeysonlyinprod \
        --set containerInsights.enabled=true \
        --set containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=arn:aws:eks:us-east-1:123456789101:cluster/example-cluster  \
        --set containerInsights.env_variables.APP_AWS_S3_BUCKET=nops-container-cost-12345678101 \
        --set karpenops.enabled=false
    ```
---
After a successful installation, you'll have our Compute Copilot (KarpenOps for Karpenter clusters) efficiently managing your node lifecycle, enhancing cost efficiency, and ensuring high availability. While EKS costs are often unclear, nOps helps by automatically finding waste through CPU and memory metrics. This lets you optimize resources and save money quickly.

{% include note.html content="As part of the installation a CRD is installed, ServiceMonitors." %}



## Installation via Terraform

### Requirements

| Name | Version |
|------|---------|
| [terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) | >= 1.3.2 |

### Providers

| Name | Version |
|------|---------|
[aws](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) | >= 5.58 |
[helm](https://registry.terraform.io/providers/hashicorp/helm/latest) | >= 2.7 |
[kubernetes](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs) | >= 2.20 |

### Usage

#### Helm Release

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "virginia"
}

data "aws_ecrpublic_authorization_token" "token" {
  provider = aws.virginia
}

provider "helm" {
  kubernetes {
    host                   = var.cluster_endpoint # Replace it with your cluster endpoint
    cluster_ca_certificate = base64decode(var.cluster_certificate_authority_data) # Replace it with your cluster certificate authority data
    exec {
      api_version = "client.authentication.k8s.io/v1beta1"
      command     = "aws"
      # This requires the awscli to be installed locally where Terraform is executed
      args = ["eks", "get-token", "--cluster-name", var.cluster_name] # Replace it with your cluster name
    }
  }
}

resource "helm_release" "nops_kubernetes_agent" {
  name                = "nops-kubernetes-agent"
  namespace           = "nops"
  create_namespace    = true
  
  repository          = "oci://public.ecr.aws/nops"
  repository_username = data.aws_ecrpublic_authorization_token.token.user_name
  repository_password = data.aws_ecrpublic_authorization_token.token.password
  description         = "Helm Chart for nOps kubernetes agent"
  chart               = "kubernetes-agent"
  version             = "0.0.70" # Ensure to update this to the latest/desired version: https://gallery.ecr.aws/nops/kubernetes-agent

  # Example to place Prometheus deployment in a on-demand node provisioned by Karpenter (THIS IS THE RECOMMENDED WAY TO RUN PROMETHEUS, Note: using double backslashes (\\) to escape the dot in karpenter.sh/capacity-type) 
  #set { 
  #  name = "prometheus.server.nodeSelector.karpenter\\.sh/capacity-type"
  #  value = "on-demand"
  #}
  
  set {
    name  = "datadog.apiKey"
    value = "<datadog_api_key>" # Get it from the nOps kubernetes agent onboarding process
  }
  
  set {
    name  = "containerInsights.enabled"
    value = "true" # Set it to true to install container insights agent to gain cost visibility in your EKS cluster
  }

  set {
    name  = "containerInsights.imageTag"
    value = "2.0.4" # Ensure to update this to the latest/desired version from [here](https://gallery.ecr.aws/nops/container-insights-agent)
  }

  set {
    name  = "containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN"
    value = "<your_eks_cluster_arn>" # Get it from the nOps kubernetes agent onboarding process
  }
  
  set {
    name  = "containerInsights.env_variables.APP_AWS_S3_BUCKET"
    value = "<your_s3_bucket_name>"  # Get it from the nOps kubernetes agent onboarding process
  }
  
  set {
    name  = "karpenops.enabled"
    value = "true" # Set it to true if you have karpenter running in your EKS Cluster and want the karpenOps agent to manage your EC2NodeTemplate/NodePool
  }
  
  set {
    name  = "karpenops.image.tag"
    value = "1.23.2" # Ensure to update this to the latest/desired version: https://gallery.ecr.aws/nops/karpenops
  }
  
  set {
    name  = "karpenops.apiKey"
    value = "<your_karpenops_api_key" # Get it from the nOps kubernetes agent onboarding process
  }
  
  set {
    name  = "karpenops.clusterId"
    value = "<your_karpenops_cluster_id>"  # Get it from the nOps kubernetes agent onboarding process
  }
}
```

#### Create Addon ([EKS Blueprints](https://github.com/aws-ia/terraform-aws-eks-blueprints-addon))

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "virginia"
}

data "aws_ecrpublic_authorization_token" "token" {
  provider = aws.virginia
}

provider "helm" {
  kubernetes {
    host                   = var.cluster_endpoint # Replace it with your cluster endpoint
    cluster_ca_certificate = base64decode(var.cluster_certificate_authority_data) # Replace it with your cluster certificate authority data
    exec {
      api_version = "client.authentication.k8s.io/v1beta1"
      command     = "aws"
      # This requires the awscli to be installed locally where Terraform is executed
      args = ["eks", "get-token", "--cluster-name", var.cluster_name] # Replace it with your cluster name
    }
  }
}

module "eks_blueprints_addon" {
  source = "aws-ia/eks-blueprints-addon/aws"
  version = "~> 1.0"
  chart               = "kubernetes-agent"
  chart_version       = "0.0.70" # Ensure to update this to the latest/desired version: https://gallery.ecr.aws/nops/kubernetes-agent
  
  repository          = "oci://public.ecr.aws/nops"
  repository_username = data.aws_ecrpublic_authorization_token.token.user_name
  repository_password = data.aws_ecrpublic_authorization_token.token.password
  description         = "Helm Chart for nOps kubernetes agent"
  namespace           = "nops"
  create_namespace    = true
  set = [
    # Example to place Prometheus deployment in a on-demand node provisioned by Karpenter (THIS IS THE RECOMMENDED WAY TO RUN PROMETHEUS, Note: using double backslashes (\\) to escape the dot in karpenter.sh/capacity-type)  
    #{
    #  name  = "prometheus.server.nodeSelector.karpenter\\.sh/capacity-type"
    #  value = "on-demand"
    #},
    {
      name  = "datadog.apiKey" # Get it from the nOps kubernetes agent onboarding process  
      value = "<datadog_api_key>" # Get it from the nOps kubernetes agent onboarding process
    },
    {
      name  = "containerInsights.enabled"
      value = "true" # Set it to true to install container insights agent to gain cost visibility in your EKS cluster
    },
    {
      name  = "containerInsights.imageTag"
      value = "2.0.4" # Ensure to update this to the latest/desired version from [here](https://gallery.ecr.aws/nops/container-insights-agent)
    },
    {
      name  = "containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN"
      value = "<your_eks_cluster_arn>" # Get it from the nOps kubernetes agent onboarding process
    },
    {
      name  = "containerInsights.env_variables.APP_AWS_S3_BUCKET"
      value = "<your_s3_bucket_name>" # Get it from the nOps kubernetes agent onboarding process
    },
    {
      name  = "karpenops.enabled"
      value = "true" # Set it to true if you have karpenter running in your EKS Cluster and want the karpenOps agent to manage your EC2NodeTemplate/NodePool
    },
    {
      name  = "karpenops.image.tag"
      value = "1.23.2" # Ensure to update this to the latest/desired version from [here](https://gallery.ecr.aws/nops/karpenops)
    },
    {
      name  = "karpenops.apiKey"
      value = "<your_karpenops_api_key" # Get it from the nOps kubernetes agent onboarding process
    },
    {
      name  = "karpenops.clusterId"
      value = "<your_karpenops_cluster_id>"  # Get it from the nOps kubernetes agent onboarding process
    }
  ]
}
```

#### Required Parameters

The following table lists required configuration parameters for the KarpenOps and Container Insights agents and their default values.

Parameter | Description | Default
--------- | ----------- | -------
`datadog.apiKey` | Datadog API Key. | `-`
`containerInsights.enabled` | Wheter to install Container Insights agent or not. | `true`
`containerInsights.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN` | EKS Cluster ARN. | `-`
`containerInsights.env_variables.APP_AWS_S3_BUCKET` | S3 Bucket name. | `-`
`karpenops.enabled` | Wheter to install KarpenOps agent or not. | `false`
`karpenops.apiKey` | KarpenOps agent API Key | `-`
`karpenops.clusterId` | KarpenOps Cluster ID | `-`

#### Optional Parameters

The following table lists the optional configuration parameters for the KarpenOps and Container Insights agents and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `externalSecrets.enabled` | External Secrets integration, more info [here](https://help.nops.io/Support-for-External-Secrets-Operator.html). | `false` |
| `externalSecrets.secretStoreRef.name` | Name of the ClusterSecretStore. | `-` |
| `externalSecrets.data.apiKeys.remoteRef.key` | Name of the secret in AWS Secrets Manager. | `-` |
| `autoUpdater.enabled` | Wether to enable the Auto Update CronJob that updates the latest image tag for KarpenOps, Container Insights and Opencost kubernetes resources, defaults to true | `true` |
| `autoUpdater.schedule` | Schedule to run the Auto Update CronJob, defaults to run every Monday at 12:00 AM | `0 0 * * 1` |
| `autoUpdater.repository` | Repository for the Auto Update container image | `public.ecr.aws/nops/alpine/k8s` |
| `autoUpdater.imageTag` | Image tag for the autoUpdater container image | `1.30.4` |
| `datadog.repository` | Repository for the Data Dog Agent container image | `public.ecr.aws/nops/datadog/agent` |
| `datadog.imageTag` | Image tag for the Data Dog Agent container image | `7.56.0` |
| `containerInsights.debug` | Debug mode. | `false` |
| `containerInsights.repository` | Repository for the nOps Container Insights Agent container image | `public.ecr.aws/nops/container-insights-agent` |
| `containerInsights.imageTag` | Image tag for the nOps Container Insights Agent container image | `2.0.4` |
| `opencost.loglevel` | Log level for nOps-cost. | `info` |
| `opencost.opencost.exporter.image.registry` | Registry for the Opencost Exporter container image | `public.ecr.aws` |
| `opencost.opencost.exporter.image.repository` | Repository for the Opencost Exporter container image | `nops/opencost` |
| `opencost.opencost.exporter.image.tag` | Image tag for the Opencost Exporter container image | `1.111.0` |
| `karpenops.image.repository` | Repository for the KarpenOps Agent container image | `public.ecr.aws/nops/karpenops` |
| `karpenops.image.tag` | Image tag for the KarpenOps Agent container image | `1.23.2` |
| `dcgmExporter.image.repository` | Repository for the DCGM Exporter container image | `public.ecr.aws/nops/nvidia/dcgm-exporter` |
| `dcgmExporter.image.tag` | Image tag for the DCGM Exporter container image | `3.3.6-3.4.2-ubuntu22.04` |
| `prometheus.ipv6_enabled` | Wether IPv6 is configured for the EKS cluster | `false` |
| `prometheus.deleteLogFile.repository` | Repository for the Busybox container image | `public.ecr.aws/docker/library/busybox` |
| `prometheus.deleteLogFile.imageTag` | Image tag for the Busybox container image | `1.36.1` |
| `prometheus.configmapReload.prometheus.image.repository` | Repository for the Prometheus Config Reloader container image | `public.ecr.aws/nops/prom/config-reloader` |
| `prometheus.configmapReload.prometheus.image.tag` | Image tag for the Prometheus Config Reloader container image | `v0.76.0` |
| `prometheus.kubeStateMetrics.image.registry` | Registry for the Kube State Metrics container image | `public.ecr.aws` |
| `prometheus.kubeStateMetrics.image.repository` | Repository for the Kube State Metrics container image | `nops/kube-state-metrics` |
| `prometheus.kubeStateMetrics.image.tag` | Image tag for the Kube State Metrics container image | `2.13.0` |
| `prometheus.nodeExporter.image.registry` | Registry for the Node Exporter container image | `public.ecr.aws` |
| `prometheus.nodeExporter.image.repository` | Repository for the Node Exporter container image | `nops/prom/node-exporter` |
| `prometheus.nodeExporter.image.tag` | Image tag for the Node Exporter container image | `v1.8.2` |
| `prometheus.nodeExporter.resources` | Node Exporter CPU and Memory limits and requests resources | `{}` |
| `prometheus.server.image.repository` | Repository for the Prometheus Server container image | `public.ecr.aws/nops/prom/prometheus` |
| `prometheus.server.image.tag` | Image tag for the Prometheus Server container image | `v2.54.0` |
| `prometheus.server.persistentVolume.storageClass` | StorageClass Name. | `gp2` |
| `prometheus.server.resources.requests.cpu` | Prometheus CPU resource requests. | `1000m` |
| `prometheus.server.resources.requests.memory` | Prometheus Memory resource requests. | `4Gi` |
| `prometheus.server.resources.limits.cpu` | Prometheus CPU resource limits. | `3000m` |
| `prometheus.server.resources.limits.memory` | Prometheus Memory resource limits. | `32Gi` |
| `prometheus.server.nodeSelector` | Node Selector labels to use for Prometheus deployment. | `{}` |

#### Prometheus Resources

Below is a table where you can see 3 options for Prometheus memory allocation depending on your cluster size (number of pods). Use it as a baseline and adjust it according to your needs. {% include note.html content="Prometheus memory limit default is 32Gi" %}

| No. of Pods    | Memory Request      | Memory Limits        |
|----------------|--------------------------------------------|---------------|
| 100 - 500      | 4Gi                 | 16Gi                  |
| 500 - 1000     | 8Gi                 | 24Gi                 |
| 1000 or more   | 16Gi                 | 32Gi                 |


## Frequently Asked Questions

1. **Will the agent installation affect my existing Prometheus setup?**
    **Answer:** No, the agent installs its own Prometheus instance in a separate namespace, as well running Node Exporter using a different port than the default one (9100) ensuring that it does not interfere with your current Prometheus deployment and Node exporter daemonset.
2. **How can I remove the agent?**

    **Answer:** 
      To remove the agent from your cluster you just need to follow these steps:
      ```bash
      # Delete nops-k8s-agent release
      helm uninstall nops-kubernetes-agent --namespace nops
      # Delete the namespaces
      kubectl delete namespace nops
      # CRD (ServiceMonitors) created by this chart is not removed by default and should be manually cleaned up if you want to.
      kubectl delete crd servicemonitors.monitoring.coreos.com
      ```
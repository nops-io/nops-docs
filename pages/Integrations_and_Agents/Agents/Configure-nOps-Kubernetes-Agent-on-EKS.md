---
title: Configure nOps Kubernetes Agent for Container Insights
keywords: CostContainer, integrations, agent, helm, containers, insights
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Configure-nOps-Kubernetes-Agent-on-EKS.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---
Installing the Container Cost Kubernetes agent is required to get access to container level visibility.
The way it works is by collecting metrics from the cluster and storing them into a S3 bucket on your AWS account which then is copied
over to nOps.
1. TOC
{:toc}
# Prerequisites
1. You must have an active nOps account. If you do not have one, please register on <a href="https://app.nops.io/" target="_blank">nOps</a>
2. Make sure you have access to the Kubernetes cluster (recommended version v1.23.6 or later) to deploy the agent.
3. <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" target="_blank">aws cli</a>
4. <a href="https://helm.sh/" target="_blank">Helm</a>
5. <a href="https://kubernetes.io/docs/reference/kubectl/overview/" target="_blank">kubectl</a>
6. Unix-like terminal to execute the installation script.
7. Terraform if installing via [IaC](https://github.com/nops-io/nops-docs/blob/main/pages/Integrations_and_Agents/Agents/Configure-nOps-Kubernetes-Agent-on-EKS.md#installation-using-insfrastructure-as-code).
8. Storage Class created for the EKS cluster (EBS gp2 or gp3)
# Steps to Configure nOps Kubernetes Agent
1. **Navigate to Container Cost Tab**
    - Go to your [Container Cost Integrations Settings](https://app.nops.io/v3/settings?tab=Integrations&subTab=Container-Cost).
    - ![Organization Settings](https://nops-help-site-assets.s3.amazonaws.com/images/integrations-container-cost.png)
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
    - ![CloudFormation Stack Parameters](https://nops-help-site-assets.s3.amazonaws.com/images/integration-container-cost-cf-parameters.png)
4. **Check Integration Status**
    - After the creation is complete, return to the nOps platform.
    - Click the **Check Status** button to verify the integration status.
5. **Install Agent**
    - Click on the **Install Agent** Button
    - **Select Options**
        - Before copying the script, you have the option to enable or disable:
            - Enable Debug Mode
            - Enable IPv6 
        - Check or uncheck these options based on your requirements.
    - **Modify the Installation Script**
        - Replace the placeholder `<<REPLACE-CLUSTER-ARN>>` with your actual cluster ARN in the following line:
            ```sh
            --set nopsAgent.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=<<REPLACE-CLUSTER-ARN>>
            ```
    - **Copy the Installation Script**
        - Click the **Copy Script** button to copy the generated installation script.
    - **Use On-Demand Node for Prometheus Server (Recommended)**
        - On-demand instances offer consistent performance, which is crucial for Prometheus when dealing with large volumes of metrics and queries, to use an on-demand node you can make use of labels for pod placement, the following example is using a [well-known Karpenter label](https://karpenter.sh/v0.37/concepts/scheduling/#well-known-labels) to schedule the pod.
            ```sh
            --set-string prometheus.server.nodeSelector."karpenter\.sh/capacity-type"=on-demand
            ```
        - If you have taints in place rememeber to use tolerations:
            ```sh
            --set-string prometheus.server.nodeSelector."karpenter\.sh/capacity-type"=on-demand \
            --set prometheus.server.tolerations[0].key="example-key" \
            --set prometheus.server.tolerations[0].operator="Equal" \
            --set prometheus.server.tolerations[0].value="example-value" \
            --set prometheus.server.tolerations[0].effect="NoSchedule"
            ```
    - **Add IAM User Credentials (If Applicable)**
        - If you chose to use an IAM User instead of an IAM Role, you need to add the following lines to the script with your access key ID and secret access key:
            ```sh
            --set nopsAgent.secrets.useAwsCredentials=true \
            --set nopsAgent.secrets.awsAccessKeyId=<<REPLACE-YOUR-ACCESS-KEY-ID>> \
            --set nopsAgent.secrets.awsSecretAccessKey=<<REPLACE-YOUR-SECRET-ACCESS-KEY>>
            ```
    - **Additional Parameters**
        - You can pass additional parameters described [here](https://help.nops.io/Configure-nOps-Kubernetes-Agent-on-EKS.html#optional-parameters):
    - **Execute the Installation Script**
        - Open your Unix-like terminal.
        - Change/Switch context to your desired cluster.
        - Execute the modified command by pasting it into the terminal and pressing **Enter**.
After running the script, the nOps Kubernetes agent will be installed on your cluster, allowing you to gain detailed visibility into your container costs.
---
After a successful installation, you'll gain clear visibility into your workloads. While EKS costs are often unclear, nOps helps by automatically finding waste through CPU and memory metrics. This lets you optimize resources and save money quickly.

_Note: As part of the installation a CRD is installed, ServiceMonitors_

# Removing Agent
To remove the agent from your cluster you just need to follow these steps:
```bash
# Delete nops-k8s-agent release
helm uninstall nops-k8s-agent --namespace nops-k8s-agent
# Delete the namespaces
kubectl delete namespace nops-k8s-agent
kubectl delete namespace nops-cost
kubectl delete namespace nops-prometheus-system
# CRD (ServiceMonitors) created by this chart is not removed by default and should be manually cleaned up if you want to.
kubectl delete crd servicemonitors.monitoring.coreos.com
```

# Installation using Insfrastructure as Code #

## Terraform ##
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

## Usage

### Helm Release

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "virginia"
}

data "aws_ecrpublic_authorization_token" "token" {
  provider = aws.virginia
}

provider "kubernetes" {
  host                   = var.cluster_endpoint # Replace it with your cluster endpoint
  cluster_ca_certificate = base64decode(var.cluster_certificate_authority_data) # Replace it with your cluster certificate authority data
  exec {
    api_version = "client.authentication.k8s.io/v1beta1"
    command     = "aws"
    # This requires the awscli to be installed locally where Terraform is executed
    args = ["eks", "get-token", "--cluster-name", var.cluster_name] # Replace it with your cluster name
  }
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

### If this is the first time you install nOps agent you will need to create this namespace beforehand, if you come from previous script installation you can ommit this resource ###
resource "kubernetes_namespace" "nops_cost" {
  metadata {
    name = "nops-cost"
  }
}

### If this is the first time you install nOps agent you will need to create this namespace beforehand, if you come from previous script installation you can ommit this resource ###
resource "kubernetes_namespace" "nops_prometheus_system" {
  metadata {
    name = "nops-prometheus-system"
  }
}

resource "helm_release" "nops_container_insights" {
  name                = "nops-container-insights"
  namespace           = "nops-k8s-agent"
  create_namespace    = true
  
  repository          = "oci://public.ecr.aws/nops"
  repository_username = data.aws_ecrpublic_authorization_token.token.user_name
  repository_password = data.aws_ecrpublic_authorization_token.token.password
  chart               = "container-insights"
  version             = "0.8.52" #ensure to update this to the latest/desired version: https://gallery.ecr.aws/nops/container-insights
  
  set {
    name  = "nopsAgent.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN"
    value = "<your_eks_cluster_arn>"
  }

  set {
    name  = "nopsAgent.env_variables.APP_AWS_S3_BUCKET"
    value = "<your_s3_bucket_name>"
  }

  depends_on = [
    kubernetes_namespace.nops_cost,
    kubernetes_namespace.nops_prometheus_system
  ]
}
```

### Create Addon ([EKS Blueprints](https://github.com/aws-ia/terraform-aws-eks-blueprints-addon))

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "virginia"
}

data "aws_ecrpublic_authorization_token" "token" {
  provider = aws.virginia
}

provider "kubernetes" {
  host                   = var.cluster_endpoint # Replace it with your cluster endpoint
  cluster_ca_certificate = base64decode(var.cluster_certificate_authority_data) # Replace it with your cluster certificate authority data
  exec {
    api_version = "client.authentication.k8s.io/v1beta1"
    command     = "aws"
    # This requires the awscli to be installed locally where Terraform is executed
    args = ["eks", "get-token", "--cluster-name", var.cluster_name] # Replace it with your cluster name
  }
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

### If this is the first time you install nOps agent you will need to create this namespace beforehand, if you come from previous script installation you can ommit this resource ###
resource "kubernetes_namespace" "nops_cost" {
  metadata {
    name = "nops-cost"
  }
}

### If this is the first time you install nOps agent you will need to create this namespace beforehand, if you come from previous script installation you can ommit this resource ###
resource "kubernetes_namespace" "nops_prometheus_system" {
  metadata {
    name = "nops-prometheus-system"
  }
}

module "eks_blueprints_addon" {
  source = "aws-ia/eks-blueprints-addon/aws"
  version = "~> 1.0"

  chart               = "container-insights"
  chart_version       = "0.8.52" #ensure to update this to the latest/desired version: https://gallery.ecr.aws/nops/container-insights
  repository          = "oci://public.ecr.aws/nops"
  repository_username = data.aws_ecrpublic_authorization_token.token.user_name
  repository_password = data.aws_ecrpublic_authorization_token.token.password
  description         = "Helm Chart for nOps container insights"
  namespace           = "nops-k8s-agent"
  create_namespace    = true

  set = [
    {
      name  = "nopsAgent.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN"
      value = "<your_eks_cluster_arn>"
    },
    {
      name  = "nopsAgent.env_variables.APP_AWS_S3_BUCKET"
      value = "<your_s3_bucket_name>"
    }
  ]

  depends_on = [
    kubernetes_namespace.nops_cost,
    kubernetes_namespace.nops_prometheus_system
  ]
}
```

### Required Parameters

The following table lists required configuration parameters for the Container Insights Helm chart and their default values.

Parameter | Description | Default
--------- | ----------- | -------
`nopsAgent.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN` | EKS Cluster ARN. | `-`
`nopsAgent.env_variables.APP_AWS_S3_BUCKET` | S3 Bucket name. | `-`


### Optional Parameters

The following table lists the optional configuration parameters for Container Insights Helm chart and their default values.

Parameter | Description | Default
--------- | ----------- | -------
`nopsAgent.debug` | Debug mode. | `false`
`opencost.loglevel` | Log level for nops-cost. | `info`
`prometheus.server.nodeSelector` | Prometheus node selector labels to use. | `{}`
`prometheus.server.tolerations` | Prometheus node taints to tolerate. | `[]`
`prometheus.server.persistentVolume.storageClass` | StorageClass Name. | `gp2`
`prometheus.server.resources.requests.cpu` | Prometheus CPU resource requests. | `800m`
`prometheus.server.resources.requests.memory` | Prometheus Memory resource requests. | `2Gi`
`prometheus.server.resources.limits.cpu` | Prometheus CPU resource limits. | `1000m`
`prometheus.server.resources.limits.memory` | Prometheus Memory resource limits. | `6Gi`

### Prometheus Resources

Below is a table where you can see 3 options for Prometheus Memory assignation depending on your cluster size (number of pods), use it as a baseline and adjust it according to your needs.

| No. of Pods    | Memory Request      | Memory Limits        |
|----------------|--------------------------------------------|---------------|
| 50 - 100       | 2Gi                 | 4Gi                  |
| 200 - 500      | 4Gi                 | 8Gi                  |
| 500 - 1000     | 4Gi                 | 12Gi                 |


# Frequently Asked Questions
1. **Will the agent installation affect my existing Prometheus setup?**
    **Answer:** No, the agent installs its own Prometheus instance in a separate namespace, as well running Node Exporter using a different port than the default one (9100) ensuring that it does not interfere with your current Prometheus deployment and Node exporter daemonset.
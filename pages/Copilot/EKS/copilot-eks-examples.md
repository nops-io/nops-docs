---
title: EKS Provisioner Examples
keywords: savings, recommendations, sharesave, nks, karpenter, compute copilot, examples
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: copilot-eks-examples.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---

# Examples of good configuration templates for EKS Copilot #

The following are examples of versatile node templates and provisioner templates that work well to maximize savings with nOps.


## EKS Node Templates ##

Example 1
```yml
kind: AWSNodeTemplate
spec:
  tags:
    nops:nks:enabled: 'true'
  userData: |-
    #!/bin/bash
    mkdir -p ~ec2-user/.ssh/
    touch ~ec2-user/.ssh/authorized_keys
    cat >> ~ec2-user/.ssh/authorized_keys <<EOF
    {% raw %}{{insertFile "../my-authorized_keys" | indent 4  }}{% endraw %}
    EOF
    chmod -R go-w ~ec2-user/.ssh/authorized_keys
    chown -R ec2-user ~ec2-user/.ssh
  amiFamily: AL2
  subnetSelector:
    Name: >-
      SUBNETFOR us-east-1a,SUBNETFOR us-east-1b,SUBNETFOR us-east-1c,SUBNETFOR us-east-1d,
  metadataOptions:
    httpTokens: required
    httpEndpoint: enabled
    httpProtocolIPv6: disabled
    httpPutResponseHopLimit: 2
  blockDeviceMappings:
    - ebs:
        encrypted: true
        volumeSize: 100Gi
        volumeType: gp3
        deleteOnTermination: true
      deviceName: /dev/xvda
  securityGroupSelector:
    aws-ids: sg-abc123defexamplesg
metadata:
  name: nops-node-template-for-eks-001
apiVersion: karpenter.k8s.aws/v1alpha1
```



## EKS Provisioner Templates ##

Example 1

```yaml
kind: Provisioner
spec:
  labels:
    karpenter-controlled: nops
  weight: 40
  providerRef:
    name: nops-eks-karpenter-provisioner-001
  requirements:
    - key: karpenter.sh/capacity-type
      values:
        - on-demand
        - spot
      operator: In
    - key: karpenter.k8s.aws/instance-category
      values:
        - c
        - m
        - r
        - t
        - x
      operator: In
    - key: karpenter.k8s.aws/instance-generation
      values:
        - '3'
      operator: Gt
    - key: topology.kubernetes.io/zone
      values:
        - us-east-1a
        - us-east-1b
        - us-east-1c
        - us-east-1d
      operator: In
    - key: kubernetes.io/os
      values:
        - linux
      operator: In
    - key: kubernetes.io/arch
      values:
        - arm64
      operator: In
  consolidation:
    enabled: true
metadata:
  name: nops-eks-karpenter-1-provisioner-001
apiVersion: karpenter.sh/v1alpha5
```



{% include custom/series_related.html %}
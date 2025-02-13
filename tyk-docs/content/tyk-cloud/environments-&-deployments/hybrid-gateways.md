---
title: "Managing Hybrid Gateways"
date: 2021-03-29
tags: ["Tyk Cloud", "Hybrid", "Gateways"]
description: "How to configure Hybrid Gateways on Kubernetes with Helm"
menu:
  main:
    parent: "Environments & Deployments"
weight: 5
---

## Introduction

Tyk Hybrid allows you to run a flexible and scalable SaaS solution. With Tyk Hybrid, the Management layer is hosted and managed by Tyk in AWS with the Gateway(s) deployed locally and managed by you with your own Data Centre, Public or Private Cloud or even on your own machine.

Tyk's Hybrid option provides you with a Tyk-hosted Cloud deployment, with the ability to deploy local Gateway across multiple locations. The Tyk hosted portion will include the Dashboard and Developer Portal. It would also allow you to run Tyk Pump locally, to maintain analytics and metrics within your chosen DB.

The connection between Hybrid Gateways and Tyk Cloud is always initiated from the Hybrid Gateway, not Tyk Cloud. As an example, you, as a customer, don't need to start punching holes in firewalls for inbound connections from Tyk Cloud.




## Hybrid Gateways in a Kubernetes Cluster
This Helm Chart provides a method of adding Hybrid Gateways into your Kubernetes cluster.
The Hybrid Gateways can be connected to *Tyk Cloud* or to a *Tyk Self managed Control plane* (a.k.a *MDCB*/*Tyk Multi Data Centre Bridge (MDCB)*).

### Prerequisites
- Redis: It is required for all Tyk installations and must be installed in the cluster or reachable from inside K8s. 

- Tyk Cloud Account: You need to set up a Tyk Cloud account 
[Getting Started with Tyk Cloud]({{< ref "/content/tyk-cloud/getting-started-tyk-cloud/getting-started-tyk-cloud.md" >}}) (With CP deployment set-up)

### Installation

This is Tyk's official Helm Charts repository `https://helm.tyk.io/public/helm/charts/`.
*Tyk Hybrid* Helm Chart is under the name `tyk-helm/tyk-hybrid`
You can also find it in [ArtifactHub](https://artifacthub.io/packages/helm/tyk-helm/tyk-hybrid).
<div class="artifacthub-widget" data-url="https://artifacthub.io/packages/helm/tyk-helm/tyk-hybrid" data-theme="light" data-header="true" data-responsive="true"><blockquote><p lang="en" dir="ltr"><b>tyk-hybrid</b>: This chart deploys the open source Tyk Gateway with a Hybrid setup that connects to a management control plan. Tyk is a fully open source Enterprise API Gateway, supporting REST, GraphQL, TCP and gRPC protocols. Tyk Gateway is provided ‘Batteries-included’, with no feature lockout. It enables organisations and businesses around the world to protect, secure, and process APIs and well as review and audit the consumed apis.</p>&mdash; Open in <a href="https://artifacthub.io/packages/helm/tyk-helm/tyk-hybrid">Artifact Hub</a></blockquote></div><script async src="https://artifacthub.io/artifacthub-widget.js"></script>

If you are interested in contributing to our charts, suggesting changes, creating PRs or any other way,
please use [GitHub Tyk-helm-chart repo](https://github.com/TykTechnologies/tyk-helm-chart/tree/master/tyk-hybrid)

#### Installation

1. clone all the repo files:

```bash
helm repo add tyk-helm https://helm.tyk.io/public/helm/charts/
helm repo update
```

2. Before we proceed with installation of the chart we need to set some custom values. To see what options are configurable on a chart and save that options to a custom values.yaml file run:

```bash
helm show values tyk-helm/tyk-hybrid > values.yaml
```

3. For Tyk-hybrid chart we need to modify following values in your custom values.yaml file:

Launch the API Manager Dashboard.
Within the API Manager Dashboard:
- Select or create your Hybrid user to be used as the login from your hybrid gateways.


Add your dashboard users organisation ID in gateway.rpc.rpcKey value
Add your dashboard users API key in gateway.rpc.apiKey value
Add your connection string to allow the Hybrid gateway to connect to your control plane in gateway.rpc.connString. On the Tyk Cloud Console find this value in the endpoints panel for your control plane deployment.
Then we can install the chart using our custom values file:

```bash
helm install tyk-hybrid tyk-helm/tyk-hybrid -f values.yaml -n tyk
```






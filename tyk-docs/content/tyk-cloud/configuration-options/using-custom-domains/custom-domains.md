---
title: "Using Custom Domains"
date: 2020-07-15
tags: ["Tyk Cloud", "Configuration", "Domains"]
description: "How to use custom domains with Tyk Cloud"
weight: 4
menu:
  main:
    parent: "Configuration Options"
url: /tyk-cloud/using-custom-domains
---

## Introduction

You can set up Tyk Cloud to use a custom domain. Using custom domains is available on the following [plans](/docs/tyk-cloud/account-billing/plans/):

- Production
- Enterprise
- Enterprise HA
- Enterprise Global

You can use a custom domain for both your Control Planes and Edge Gateways.

### Custom Domains with Control Planes

Currently, you can only use one custom domain per Control Plane deployment.

The custom domain in this case ties to a Tyk Developer Portal. Please set up a CNAME DNS record such that it points to the "Portal" ingress as displayed on your Control Plane deployment page.

### Custom Domains with Edge Gateways

You can set multiple custom domains on an Edge Gateway.

In this instance please set up your CNAME DNS records such that they point to the only ingress displayed on your Edge deployment page.

Note: While you can set multiple custom domains for an Edge Gateway, a single custom domain cannot be used for multiple Edge Gateways.

## How to set up a Custom Domain

In this example we are going to set up a custom domain called `edge.corp.com` for an Edge deployment.

1. Create a CNAME DNS record `edge.corp.com` that points to your Edge ingress (e.g. `something-something.aws-euw2.cloud-ara.tyk.io`).
2. From your Edge deployment, select Edit from the Status drop-down.

![Edge drop-down](/docs/img/2.10/edge-dropdown.png)

3. Enter `edge.corp.com` in the Custom Domains field.

![Edge Custom Domain](/docs/img/2.10/edge_custom_domain.png)

4. Click Save and Re-deploy.

![Save and Re-Deploy](/docs/img/2.10/save_redeploy.png)

### How our Custom Domain functionality works

When you point your custom domain to your deployment, we use [Let\'s Encrypt\'s](https://letsencrypt.org/docs/challenge-types/#http-01-challenge) HTTP01 ACME challenge type, which verifies ownership by accessing your custom CNAME on your Control Plane or Edge deployment. For example - `something-something.aws-euw2.cloud-ara.tyk.io` above.

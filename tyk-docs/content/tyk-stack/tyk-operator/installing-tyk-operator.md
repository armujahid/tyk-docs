---
date: 2017-03-24T16:39:31Z
title: Installing Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Installing Tyk Operator

### Prerequisites

- Kubernetes v1.19+
- Kubernetes Cluster Admin rights for installing CustomResourceDefinitions
- Tyk Community Edition (CE) or Tyk Pro v3+

{{< note success >}}
**Note**  

Tyk Operator supports any Tyk installation whether it is on Tyk Cloud, Hybrid, or self-managed on VMs and K8s. You only need to make sure that the management URL is accessible by Tyk Operator.
{{< /note >}}

### Configuring Tyk

We assume you have already installed Tyk. If you don’t have it, check this page. Tyk Helm Chart is the preferred (and easiest) way to install Tyk on Kubernetes.

{{< note success >}}
**Note**  

Tyk Operator is tested as compatible with v3+ of theTyk Gateway and Tyk Dashboard.
{{< /note >}}

If you are using the Self Managed edition, you need to make sure your Tyk Gateway's `tyk.conf` has `policies.allow_explicit_policy_id` set to true as follows:

```
"policies": {
  "allow_explicit_policy_id": true
},
```

### Installing cert-manager

Tyk Operator uses the cert-manager to provision certificates for the webhook server. If you don't have cert-manager installed, you can follow this command to install it: 

```
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.8.0/cert-manager.yaml
```

Since Tyk Operator supports Kubernetes v1.19+, the minimum cert-manager version you can use is v1.8. 
If you run into the cert-manager related errors, please ensure that the desired version of Kubernetes version works with the chosen version of cert-manager by checking supported releases page and cert-manager documentation.
Please wait for the cert-manager to become available.

### Configuring Tyk Operator

Tyk Operator configurations are set via K8s secret. The default K8s secret name is `tyk-operator-conf.` You can use a different secret name by setting it at `envFrom` field of `values.yaml`. You can also override the configuration values through environment variables by setting `envVars` field in `values.yaml` when you install Operator through Helm. 

#### Connecting to your Tyk Gateway or Dashboard

Tyk Operator needs to connect to a Tyk deployment. It also needs to know whether it is talking to Open Source Gateway or Self Managed installation. 
You can see how to set up the connection for Tyk Open Source and Tyk Self Managed respectively:

#### Tyk CE

Tyk Operator looks for these Keys from `tyk-operator-conf` secret or from the environment variables to make a connection to the Tyk Gateway.

| Key | Example Value   | Description  |
|:-----|:----------------|:-------------|

---
date: 2017-03-24T16:39:31Z
title: Installing Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

{{< toc >}}


### Prerequisites

- Kubernetes v1.19+
- Kubernetes Cluster Admin rights for installing CustomResourceDefinitions
- Tyk Community Edition (CE) or Tyk Pro v3+

{{< note success >}}
**Note**  

Tyk Operator supports any Tyk installation whether it is on Tyk Cloud, Hybrid, or self-managed on VMs and K8s. You only need to make sure that the management URL is accessible by Tyk Operator.
{{< /note >}}

### Configuring Tyk

We assume you have already installed Tyk. If you don’t have it, check [this](https://tyk.io/docs/getting-started/installation/) page. Tyk Helm Chart is the preferred (and easiest) way to install Tyk on Kubernetes.

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
If you run into the cert-manager related errors, please ensure that the desired version of Kubernetes version works with the chosen version of cert-manager by checking [supported releases page](https://cert-manager.io/docs/installation/supported-releases/) and [cert-manager documentation](https://cert-manager.io/docs/installation/supported-releases/).

Please wait for the cert-manager to become available.

### Configuring Tyk Operator

Tyk Operator configurations are set via K8s secret. The default K8s secret name is `tyk-operator-conf.` You can use a different secret name by setting it at `envFrom` field of [`values.yaml`](https://github.com/TykTechnologies/tyk-operator/blob/master/helm/values.yaml). You can also override the configuration values through environment variables by setting `envVars` field in [`values.yaml`](https://github.com/TykTechnologies/tyk-operator/blob/master/helm/values.yaml) when you install Operator through Helm. 

#### Connecting to your Tyk Gateway or Dashboard

Tyk Operator needs to connect to a Tyk deployment. It also needs to know whether it is talking to Open Source Gateway or Self Managed installation. 
You can see how to set up the connection for Tyk Open Source and Tyk Self Managed respectively:

#### Tyk CE

Tyk Operator looks for these Keys from `tyk-operator-conf` secret or from the environment variables to make a connection to the Tyk Gateway.

| Key | Example Value   | Description  |
|:-----|:----------------|:-------------|
| `TYK_MODE` | `ce` | “ce” for Tyk Open Source mode. |
| `TYK_URL` | `http://gateway-control.tykce.svc:9696` | Management URL of Tyk Gateway (CE).|| `TYK_ORG` | `5e9d9544a1dcd60001d0ed20` | Operator user ORG ID.|
| `TYK_AUTH` | `2d095c2155774fe36d77e5cbe3ac963b` | Gateway Management API Key.|
| `TYK_TLS_INSECURE_SKIP_VERIFY`| `true` | Set to `“true”` if the Tyk URL is HTTPS and has a self-signed certificate. If it isn't set, the default value is `false`.|

For Tyk Open Source, you can create the Kubernetes Secret object `tyk-operator-conf` through `kubectl` by running this command:

```
kubectl create namespace tyk-operator-system

kubectl create secret -n tyk-operator-system generic tyk-operator-conf \
  --from-literal "TYK_AUTH=${TYK_AUTH}" \
  --from-literal "TYK_ORG=${TYK_ORG}" \
  --from-literal "TYK_MODE=${TYK_MODE}" \
  --from-literal "TYK_URL=${TYK_URL}" \
  --from-literal "TYK_TLS_INSECURE_SKIP_VERIFY=true"
```

After running the command, the values get automatically Base64 encoded:

```
kubectl get secret/tyk-operator-conf -n tyk-operator-system -o json | jq '.data'
{
  "TYK_AUTH": "NWFhOTIyMTQwMTA0NGYxYzcwZDFjOTUwMDhkMzllZGE=",
  "TYK_MODE": "cHJv",
  "TYK_ORG": "NWY5MmQ5YWQyZGFiMWMwMDAxM2M3NDlm",
  "TYK_URL": "aHR0cDovL2Rhc2hib2FyZC50eWtwcm8tY29udHJvbC1wbGFuZS5zdmMuY2x1c3Rlci5sb2NhbDozMDAw",
  "TYK_TLS_INSECURE_SKIP_VERIFY": "dHJ1ZQ=="
}
```

If you use Helm Chart to deploy Tyk Open Source, you can obtain the values for `TYK_AUTH` and `TYK_ORG` from Helm Chart [values.yaml](https://github.com/TykTechnologies/tyk-operator/blob/master/ci/helm/tyk-headless/values.yaml):

- `TYK_AUTH` corresponds to the value of the `secrets.APISecret`.
- `TYK_ORG` corresponds to the value of the `secrets.OrgID`.

Therefore, according to this particular [values.yaml](https://github.com/TykTechnologies/tyk-operator/blob/master/ci/helm/tyk-headless/values.yaml) configuration, if we assume Tyk is deployed to `tykce-control-plane` namespace, the following secret will be created to connect Tyk Operator to Tyk Gateway:

```
kubectl create secret -n tyk-operator-system generic tyk-operator-conf \
--from-literal "TYK_AUTH=foo" \
--from-literal "TYK_ORG=myorg" \
--from-literal "TYK_URL=http://gateway-svc-tyk-ce-tyk-headless.tykce-control-plane.svc.cluster.local:8080" \
--from-literal "TYK_TLS_INSECURE_SKIP_VERIFY=true" \
--from-literal "TYK_MODE=ce"
```

#### Tyk Self Managed

Tyk Operator looks for these keys from `tyk-operator-conf` secret or from the environment variables to make a connection to the Tyk Dashboard.

| Key | Example Value | Description |
|:------|:------------|:-------------|
| `TYK_MODE` | `pro` | “pro” for Tyk Self Managed mode.|
| `TYK_URL` | `http://dashboard.tykpro.svc:3000` | Management URL of your Tyk Dashboard.|
| `TYK_ORG` | `5e9d9544a1dcd60001d0ed20` | Operator user ORG ID.|
| `TYK_AUTH` | `2d095c2155774fe36d77e5cbe3ac963b` | Operator user API Key.|
| `TYK_TLS_INSECURE_SKIP_VERIFY` | `true` | Set to `“true”` if the Tyk URL is HTTPS and has a self-signed certificate. If it isn't set, the default value is `false`.|

There are 2 ways to install Tyk Self Managed, either using Helm or manually: 

- If you install Tyk Self Managed using Helm, `tyk-operator-conf` will have been created with the following keys: `TYK_AUTH, TYK_MODE, TYK_ORG`, and `TYK_URL` by default.

The following command shows how you can access the value of `TYK_ORG` assuming that Tyk Self Managed is installed in the `tyk` namespace:

```
kubectl get secrets -n tyk tyk-operator-conf --template={{.data.TYK_ORG}} | base64 -d
```

- If you install Tyk Self Managed manually, you can access `TYK_AUTH` and `TYK_ORG` values from the dashboard.

Under the Users page, you can click on any user to find associated values for that particular user.

`TYK_AUTH` corresponds to `Tyk Dashboard API Access Credentials` in the above image, indicated by `1`. 
`TYK_ORG` corresponds to `Organisation ID` in the above image, indicated by `2`.
You can follow the instructions for Tyk CE to create a `tyk-operator-conf` secret using `kubectl` command.

#### Other configurations

You can include additional fields in the secret or environment variables to control Tyk Operator. These configuration fields are optional.

Here are the configurations you can use:

| Key | Example Value | Description |
|:-----|:-------------|:------------|
| `WATCH_NAMESPACE` | `foo,bar` | Comma separated list of namespaces for Operator to operate on. The default is to all namespaces if not specified.|
| `WATCH_INGRESS_CLASS` | `customclass` | Define the ingress class Tyk Operator to watch for. Default is `tyk`|
| `TYK_HTTPS_INGRESS_PORT` | `8443` | Define the ListenPort for HTTPS ingress. Default is `8443`.|
| `TYK_HTTP_INGRESS_PORT` | `8080` | Define the ListenPort for HTTP ingress. Default is `8080`.|

#### Watching Namespaces

Tyk Operator is installed with cluster permissions. However, you can optionally control which namespaces it watches by setting the `WATCH_NAMESPACE` through `tyk-operator-conf` secret or the environment variable to a comma separated list of k8s namespaces:

- `WATCH_NAMESPACE=""` will watch for resources across the entire cluster.
- `WATCH_NAMESPACE="foo"` will watch for resources in the `foo` namespace.
- `WATCH_NAMESPACE="foo,bar"` will watch for resources in the `foo` and `bar` namespace.

For example:

```
kubectl create secret -n tyk-operator-system generic tyk-operator-conf \
  --from-literal "TYK_AUTH=${TYK_AUTH}" \
  --from-literal "TYK_ORG=${TYK_ORG}" \
  --from-literal "TYK_MODE=${TYK_MODE}" \
  --from-literal "TYK_URL=${TYK_URL}" \
  --from-literal "WATCH_NAMESPACE=foo,bar"
```

#### Watching custom ingress class

The value of the `kubernetes.io/ingress.class` annotation identifies Ingress objects to be processed.
Tyk Operator looks for the value `tyk` and will ignore all other ingress classes by default. If you want to override this default behaviour, you may do so by setting `WATCH_INGRESS_CLASS` through `tyk-operator-conf` or the environment variable .

For example:

```
kubectl create secret -n tyk-operator-system generic tyk-operator-conf \
  --from-literal "TYK_AUTH=${TYK_AUTH}" \
  --from-literal "TYK_ORG=${TYK_ORG}" \
  --from-literal "TYK_MODE=${TYK_MODE}" \
  --from-literal "TYK_URL=${TYK_URL}" \
  --from-literal "WATCH_INGRESS_CLASS=foo"
```

### Installing Tyk Operator and CRDs
#### Using Helm

You can install CRDs and Tyk Operator through Helm Chart by running the following command:

```
helm repo add tyk-helm https://helm.tyk.io/public/helm/charts/
helm repo update
helm install tyk-operator tyk-helm/tyk-operator -n tyk-operator-system
```

#### From Tyk Operator repository

You can install CRDs and Tyk Operator by checking out this GitHub repository.
Installing CRDs is as simple as checking out this repository and running `kubectl apply`:

```
kubectl apply -f ./helm/crds
customresourcedefinition.apiextensions.k8s.io/apidefinitions.tyk.tyk.io configured
customresourcedefinition.apiextensions.k8s.io/securitypolicies.tyk.tyk.io configured
customresourcedefinition.apiextensions.k8s.io/webhooks.tyk.tyk.io configured
```

Run the following command to deploy Tyk Operator:

```
$ helm install tyk-operator ./helm -n tyk-operator-system

NAME: tyk-operator
LAST DEPLOYED: Tue Nov 10 18:38:32 2020
NAMESPACE: tyk-operator-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
You have deployed the tyk-operator! See https://github.com/TykTechnologies/tyk-operator for more information.
```

### Upgrading Tyk Operator
#### Using Helm

You can upgrade CRDs and Tyk Operator through Helm Chart by running the following command:

```
helm upgrade -n tyk-operator-system tyk-operator tyk-helm/tyk-operator  --wait
```

#### From Tyk Operator repository

You need to checkout the tag if you want to upgrade to with running `git checkout tags/{.ReleaseTag}`.For example, if you are on `v0.6.0` and you wish to upgrade to `v0.6.1` , run the following command:
`git checkout tags/v0.6.1`

To upgrade CRDs, run the following command:
`kubectl apply -f ./helm/crds`

To upgrade helm release, run the following command:
`helm upgrade tyk-operator ./helm -n tyk-operator-system`

### Uninstall

If you think we did something wrong, you can create a [GitHub issue](https://github.com/TykTechnologies/tyk-operator/issues/new). We will try to improve your experience and others. To uninstall Tyk Operator, you need to run the following command:
`helm delete tyk-operator -n tyk-operator-system`
 

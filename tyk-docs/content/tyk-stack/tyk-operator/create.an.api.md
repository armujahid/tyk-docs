---
date: 2017-03-24T16:39:31Z
title: Create an API with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Create an API

Creating an API takes the same approach whether you are using Tyk Open Source or Self Managed. 

The steps to create an API are as follows:

1. Define APIDefinition resource in YAML format.
2. Create a Kubernetes resource based on your YAML file.
3. Tyk Operator can handle the creation of your API.

We are going to create an ApiDefinition described in the httpbin.yaml file, by running the  following command:

```
kubectl apply -f config/samples/httpbin.yaml
```

Or , if you don’t have the manifest with you, you can run the following command:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: ApiDefinition
metadata:
    name: httpbin
spec:
    name: httpbin
    use_keyless: true
    protocol: http
    active: true
    proxy:
        target_url: http://httpbin.org
        listen_path: /httpbin
        strip_listen_path: true
```

We can walk you through the ApiDefinition that we created. We have an ApiDefinition called `httpbin`, as specified in a `spec.name` field, which listens to `/httpbin` and proxies requests to http://httpbin.org, as it's specified under a `spec.proxy` field. Now, any requests coming to the `/httpbin` endpoint will be proxied to the target URL that we defined in `spec.proxy.target_url`, which is http://httpbin.org in our example.

To find out about available ApiDefinition objects in your cluster, you need to run the following command:

```
$ kubectl get tykapis
NAME      DOMAIN   LISTENPATH   PROXY.TARGETURL      ENABLED
httpbin            /httpbin     http://httpbin.org   true
```

We can see that our ApiDefinition has been created. Now let's verify that our API is working as expected.

{{< note success >}}
**Note**  

The verification step may vary based on your environment, such as the type of your Tyk installation and Kubernetes cluster.
- If you are using local Kubernetes cluster such as KinD and Minikube,
you can do port-forwarding to access resources/services within the cluster.
- If you are using Kubernetes clusters provided by cloud providers, you need to configure your cluster to make it accessible.

{{< /note >}}

For the scope of this example, we are using a local Kubernetes cluster. Port-forwarding details can be found in the official Kubernetes documentation.

### Tyk Open Source

The example of port forwarding for Tyk Open Source will be explained in the following paragraph.
If you have installed Tyk Open Source in the `<TYK_CE_NAMESPACE>` namespace, you will have the following services:

```
kubectl get svc -n <TYK_CE_NAMESPACE> 
NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
gateway-svc-tyk-ce-tyk-headless   ClusterIP   10.96.38.138    <none>        8080/TCP   22m
redis                             ClusterIP   10.96.254.227   <none>        6379/TCP   22m
```

To access your Tyk Gateway, you can use the following port-forwarding command:

```
kubectl port-forward service/gateway-svc-tyk-ce-tyk-headless -n <TYK_CE_NAMESPACE> 8080:8080
```

The Tyk Gateway is accessible from your local cluster's 8080 port (e.g., `localhost:8080`).
Since Tyk Open Source does not come with the Dashboard, you can list APIs using the Tyk Gateway API by running the following command:


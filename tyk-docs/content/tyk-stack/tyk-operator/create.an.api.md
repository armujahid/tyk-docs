---
date: 2017-03-24T16:39:31Z
title: Create an API with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

{{< toc >}}


Creating an API takes the same approach whether you are using Tyk Open Source or Self Managed. 

The steps to create an API are as follows:

1. Define APIDefinition resource in YAML format.
2. Create a Kubernetes resource based on your YAML file.
3. Tyk Operator can handle the creation of your API.

We are going to create an ApiDefinition described in the [httpbin.yaml](https://github.com/TykTechnologies/tyk-operator/blob/master/config/samples/httpbin.yaml) file, by running the  following command:

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

We can walk you through the ApiDefinition that we created. We have an ApiDefinition called `httpbin`, as specified in a `spec.name` field, which listens to `/httpbin` and proxies requests to [http://httpbin.org](http://httpbin.org), as it's specified under a `spec.proxy` field. Now, any requests coming to the `/httpbin` endpoint will be proxied to the target URL that we defined in `spec.proxy.target_url`, which is [http://httpbin.org](http://httpbin.org) in our example.

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
- If you are using local Kubernetes cluster such as [KinD](https://kind.sigs.k8s.io/) and [Minikube](https://minikube.sigs.k8s.io/docs/start/),
you can do port-forwarding to access resources/services within the cluster.
- If you are using Kubernetes clusters provided by cloud providers, you need to configure your cluster to make it accessible.

{{< /note >}}

For the scope of this example, we are using a local Kubernetes cluster. Port-forwarding details can be found in the official [Kubernetes documentation](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/).

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
Since Tyk Open Source does not come with the Dashboard, you can list APIs using the [Tyk Gateway API](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) by running the following command:

```
$ curl -H "x-tyk-authorization: {your-secret}" localhost:8080/tyk/apis/
```

Your Tyk Gateway API secret is stored in your `tyk.conf` file. The property is called `secret`. You will need to use this as a header called `x-tyk-authorization` to make calls to the Gateway API.
You can make a request to verify that your API is working:

```
$ curl -i localhost:8080/httpbin/get
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.77.0",
    "X-Amzn-Trace-Id": "Root=1-62161e8c-2a1ece436633f2e42129be2a"
  },
  "origin": "127.0.0.1, 176.88.45.17",
  "url": "http://httpbin.org/get"
}
```

### Tyk Self Managed

The example of port forwarding for Tyk Self Managed will be explained in the following paragraph.
If you have installed Tyk Self Managed in the `<TYK_PRO_NAMESPACE>` namespace, you will have the following services:

```
kubectl get svc -n <TYK_PRO_NAMESPACE>
NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
dashboard-svc-tyk-pro   NodePort    10.96.152.180   <none>        3000:30357/TCP   2d17h
gateway-svc-tyk-pro     NodePort    10.96.228.133   <none>        8080:31516/TCP   2d17h
mongo                   ClusterIP   10.96.12.192    <none>        27017/TCP        2d17h
redis                   ClusterIP   10.96.66.91     <none>        6379/TCP         2d17h
```

To access the Dashboard, you can use the following port-forwarding command:

```
kubectl port-forward service/dashboard-svc-tyk-pro 3000:3000 -n TYK_PRO_NAMESPACE
```

The Dashboard is accessible from your local cluster's 3000 port (e.g., `localhost:3000`).
If you head over to the Dashboard, you can see that an ApiDefinition called `httpbin` is created.

You can make a request to verify that your API is working:

```
$ curl -i localhost:8080/httpbin/get
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.77.0",
    "X-Amzn-Trace-Id": "Root=1-62161e8c-2a1ece436633f2e42129be2a"
  },
  "origin": "127.0.0.1, 176.88.45.17",
  "url": "http://httpbin.org/get"
}
```

### Kubernetes service as an upstream target

Tyk Operator allows accessing your Kubernetes service as an upstream proxy target.
You can set the `proxy.target_url` as a Kubernetes Service following [DNS for Services and Pods guideline](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/), so that the requests will be proxied to your service.
In general, Kubernetes Services have a `<service-name>.<namespace-name>`.svc.cluster.local DNS entry once they are created.
For example, if you have a service called `httpbin` in `default` namespace, you can contact `httpbin` service with `httpbin.default.svc` DNS record in the cluster, instead of IP addresses.
Please visit the official [Kubernetes documentation](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/) for more details.
Suppose you want to create a Deployment of [`httpbin`](https://hub.docker.com/r/kennethreitz/httpbin/) service using [`ci/upstreams/httpbin.yaml`](https://github.com/TykTechnologies/tyk-operator/blob/master/ci/upstreams/httpbin.yaml) file. You are going to expose the application through port `8000` as described under the Service [specification](https://github.com/TykTechnologies/tyk-operator/blob/master/ci/upstreams/httpbin.yaml#L10).
You can create Service and Deployment by either applying the manifest defined in our repository:
```
kubectl apply -f ci/upstreams/httpbin.yaml
```

Or, if you don’t have the manifest with you, you can run the following command:

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: httpbin
  labels:
    app: httpbin
spec:
  ports:
    - name: http
      port: 8000
      targetPort: 80
  selector:
    app: httpbin
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpbin
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpbin
      version: v1
  template:
    metadata:
      labels:
        app: httpbin
        version: v1
    spec:
      containers:
        - image: docker.io/kennethreitz/httpbin
          imagePullPolicy: IfNotPresent
          name: httpbin
          ports:
            - containerPort: 80
EOF
```

You need to wait while until all pods reach READY `1/1` and STATUS `Running` state.
Once the pod is ready, you can update your `httpbin` API's `target_url` field to proxy your requests to the Service that you created above.
You can check all services in the `<ns>` namespace as follows:

```
kubectl get service -n <ns>
```

You can update your `httpbin` as follows:

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
    target_url: http://httpbin.default.svc:8000
    listen_path: /httpbin
    strip_listen_path: true
```

You need to pay attention to the value of the `spec.proxy.target_url` field.
It is set to `http://httpbin.default.svc:8000` by following the convention described above (`<service_name>.<namespace>.svc:<service_port>`).
Now, if you send your request to the `/httpbin` endpoint of the Tyk Gateway, the request will be proxied to the `httpbin Service`:

```
curl -sS http://localhost:8080/httpbin/headers
{
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip", 
    "Host": "httpbin.default.svc:8000", 
    "User-Agent": "curl/7.68.0"
  }
}
```

As you can see from the response, the host that your request should be proxied to is `httpbin.default.svc:8000`.



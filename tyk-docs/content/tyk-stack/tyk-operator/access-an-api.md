---
date: 2017-03-24T16:39:31Z
title: Access an API with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Access an API

You might already have realised that our `httpbin` API is keyless. If you check the APIDefinition's specification, the `use_keyless` field is set to `true`.
Tyk keyless access represents completely open access for your API and causes Tyk to bypass any session-based middleware (middleware that requires access to token-related metadata). Keyless access will enable all requests through.
You can disable keyless access by setting `use_keyless` to false. You can update `httpbin` API to see it in action.
To update `httpbin` API, either you can update your `httpbin.yaml` file as follows:

```
apiVersion: tyk.tyk.io/v1alpha1
kind: ApiDefinition
metadata:
  name: httpbin
spec:
  name: httpbin
  use_keyless: false
  protocol: http
  active: true
  proxy:
    target_url: http://httpbin.org
    listen_path: /httpbin
    strip_listen_path: true
```

and you can apply the changes:

```
kubectl apply -f config/samples/httpbin.yaml

apidefinition.tyk.tyk.io/httpbin configured
```

Or you can do as follows:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: ApiDefinition
metadata:
    name: httpbin
spec:
    name: httpbin
    use_keyless: false
    protocol: http
    active: true
    proxy:
        target_url: http://httpbin.org
        listen_path: /httpbin
        strip_listen_path: true
EOF

apidefinition.tyk.tyk.io/httpbin configured
```

To access `httpbin` API, you need to include a key to the header.

{{< note success >}}
**Note**  

All supported authentication types by Tyk Operator are listed [here](https://github.com/TykTechnologies/tyk-operator/blob/master/docs/api_definitions.md#client-to-gateway-authentication).
{{< /note >}}

Authentication token is the default one if you have set `use_keyless` to false, you don’t need to specify any other Authentication mode:

```
curl -i localhost:8080/httpbin/get
HTTP/1.1 401 Unauthorized
Content-Type: application/json
X-Generator: tyk.io
Date: Thu, 03 Mar 2022 15:47:30 GMT
Content-Length: 46

{
    "error": "Authorization field missing"
}%
```

You need to generate a key to access the `httpbin` API. Generating a key depends on which type of Tyk installation you have.

### Tyk Open Source

To create an API Key, you will need the API ID that we grant the key access to. Therefore, creating the key is a very simple API call to the endpoint.

**Prerequisite:**

- Your API secret: This is the secret property of the tyk.conf file. Once you have this value, you can use it to access the Gateway API.
- API ID: You need to obtain an API ID. We describe the ApiDefinition resource that represents our httpbin API as following:

```
kubectl describe tykapis httpbin
Name:         httpbin
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  tyk.tyk.io/v1alpha1
Kind:         ApiDefinition
Metadata:
  ...
Spec:
  ...
  api_id:  ZGVmYXVsdC9odHRwYmlu
  ...
Status:
  api_id:  ZGVmYXVsdC9odHRwYmlu
Events:    <none>
```

Once you describe `httpbin` ApiDefinition, the API ID will be represented in the `status.api_id` field.

In our example, it is as follows:

- {API-NAME}: httpbin
- {API-ID}: ZGVmYXVsdC9odHRwYmlu

The below curl command will generate a key for our API:

```
curl -X POST -H "x-tyk-authorization: {API-SECRET}" \
  -s \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "allowance": 1000,
    "rate": 1000,
    "per": 1,
    "expires": -1,
    "quota_max": -1,
    "org_id": "1",
    "quota_renews": 1449051461,
    "quota_remaining": -1,
    "quota_renewal_rate": 60,
    "access_rights": {
      "{API-ID}": {
        "api_id": "{API-ID}",
        "api_name": "{API-NAME}",
        "versions": ["Default"]
      }
    },
    "meta_data": {}
  }' http://localhost:8080/tyk/keys/create | python -mjson.tool
{
"action": "added",
"key": "eyJvcmciOiIxIiwiaWQiOiIxZTNhMTNhNGU4MGQ0ZWIxOGMzNjhlNzkyMjY5ODBmYiIsImgiOiJtdXJtdXIxMjgifQ==",
"key_hash": "9d8b101625d77f93153f2eeb0d2ae365",
"status": "ok"
}
```

We have successfully created a key. Now we can try and use it:

```
curl -H "Authorization: Bearer eyJvcmciOiIxIiwiaWQiOiIxZTNhMTNhNGU4MGQ0ZWIxOGMzNjhlNzkyMjY5ODBmYiIsImgiOiJtdXJtdXIxMjgifQ==" localhost:8080/httpbin/get
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip",
    "Authorization": "Bearer eyJvcmciOiIxIiwiaWQiOiIxZTNhMTNhNGU4MGQ0ZWIxOGMzNjhlNzkyMjY5ODBmYiIsImgiOiJtdXJtdXIxMjgifQ==",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.77.0",
    "X-Amzn-Trace-Id": "Root=1-6221e1e5-084139824f22e88542ab9246"
  },
  "origin": "127.0.0.1, 176.42.143.200",
  "url": "http://httpbin.org/get"
}
```

### Tyk Self Managed

Select Keys from the `System Management` section and click `ADD KEY` as shown in the image below:
You have the option to create a new key either by selecting an existing Policy created for your API or by simply selecting your API. For this tutorial, you are going to use an API.

To select `httpbin` API:

- Scroll through your `API Name` list.
- Use the `Search` field.
- Select the `httpbin` API that was previously created.

You can leave all other options at their default settings.
Now, you need to add configuration details to set an expiry time. After the key expires, you can use the `Configuration` section indicated by `3` in the image above.

Once you click `CREATE KEY`, a key is successfully generated. Then, a pop-up will be displayed with the key.You must save this somewhere for future reference as it will not be displayed again.
Click `Copy to clipboard` and paste it into a text document.
We have successfully created a key. Now we can try and use it:

```
curl -H "Authorization: Bearer {Key ID}" localhost:8080/httpbin/get
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip",
    "Authorization": "Bearer {Key ID}",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.77.0",
    "X-Amzn-Trace-Id": "Root=1-6221de2a-01aa10dd56f6f13f420ba313"
  },
  "origin": "127.0.0.1, 176.42.143.200",
  "url": "http://httpbin.org/get"
}
```

Since you have provided a valid key along with your request, you do not get a `HTTP 401 Unauthorized` response.

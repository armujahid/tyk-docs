---
date: 2017-03-24T16:39:31Z
title: Migration with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Migration

If there are existing APIs that you want to link to a CRD, it's very easy to do so. You need to simply add the `api_id` from your API Definition to the YAML of your `ApiDefinition` type. Then, the Operator will take care of the rest.

Example:

1. From the existing API Definition, grab the following field:

```
"api_id": "5e0fac4845bb46c77543be28300fd9d7"
```

2. Simply add this value to your YAML, in the spec.api_id field:

```
apiVersion: tyk.tyk.io/v1alpha1
kind: ApiDefinition
metadata:
  name: my-existing-api
spec:
  api_id: 5e0fac4845bb46c77543be28300fd9d7
  name: existing API
  protocol: http
  active: true
  proxy:
    target_url: http://httpbin.org
    listen_path: /httpbin
    strip_listen_path: true
```

3. Then apply your changes:

```
$ kubectl apply -f config/samples/httpbin_protected.yaml
apidefinition.tyk.tyk.io/my-existing-api created
```

{{< note success >}}
**Note**  

The source of truth for the API definition is now the CRD, meaning it will override any differences in your existing API definition.
{{< /note >}}

Any keys, policies and analytics linked to this API will continue to operate the same. This is great for idempotency.

### Idempotency

Because of the ability to declaratively define the `api_id`, this gives us the ability to preserve Keys that are tied to APIs or policies which are tied to APIs.
Read the [section on idempotency](https://github.com/TykTechnologies/tyk-operator/blob/master/docs/concepts.md) for more information.

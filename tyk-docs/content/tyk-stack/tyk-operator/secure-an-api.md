---
date: 2017-03-24T16:39:31Z
title: Secure an API with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---



You can access a secured API by creating a key. A key can be created by specifying security policy. A security policy encapsulates several options that can be applied to a key. It acts as a template that can override individual sections of an API key (or identity) in Tyk.

If you have issued multiple keys and want to change access rights, rate limits or quotas, you will have to update all the keys manually. Security policy comes handy in this scenario. You just need to update the security policy linked to the keys once.

You can create an API and security policy for that API.
Security policy resources are currently only supported when using Tyk Pro mode. You can get round this by mounting the policy object as a volume into the gateway container.

You can do so either by applying manifest defined in our repository:

`kubectl apply -f docs/policies/ratelimit.yaml`

Or creating it by running the following command:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: ApiDefinition
metadata:
  name: httpbin
spec:
  name: httpbin protected
  protocol: http
  active: true
  proxy:
    target_url: http://httpbin.org
    listen_path: /httpbin
    strip_listen_path: true
  use_standard_auth: true
  auth_configs:
    authToken:
      auth_header_name: Authorization
---
apiVersion: tyk.tyk.io/v1alpha1
kind: SecurityPolicy
metadata:
  name: httpbin
spec:
  name: Rate Limit, Quota and Throttling policy
  state: active
  active: true
  access_rights_array:
    - name: httpbin
      namespace: default
      versions:
        - Default
  quota_max: 10
  quota_renewal_rate: 60
  rate: 5
  per: 5
  throttle_interval: 2
  throttle_retry_limit: 2
EOF
```

To ensure policy is created, you can run as follows:

```
$ kubectl get securitypolicy
NAME      AGE
httpbin   10s
```

You have successfully created the  `httpbin` security policy for your `httpbin` API. A Policy also sets global usage quota, rate limits and throttling.

You can see the fields you have set in the policy:

- name: The name of the security policy.
- active: Marks policy as active.
- state: It can have value `active, draft,deny`.
- access_right_array: The list of APIs security policy has access to.

Usage Quota fields:

- quota_max: The maximum number of allowed requests over a quota period.
- quota_renewal_rate: Time, in seconds, after which quota will be renewed.

Rate limiting fields:

- rate: The number of the requests to allow per period.
- per: Time in seconds.

Throttling fields:

- throttle_interval: Interval (in seconds) between each request retry.
- throttle_retry_limit: Total requests retry number.

Now you can create a key by using this security policy to access your API.
We are continuously adding support for new features which you can track [here](https://github.com/TykTechnologies/tyk-operator/blob/master/docs/policies.md).

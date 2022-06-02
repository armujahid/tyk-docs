---
date: 2017-03-24T16:39:31Z
title: Publish an API with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Publish an API

Assuming that you have created the httpbin example API using the steps above, you can easily publish it to your Tyk Portal by applying a few specifications:

- SecurityPolicy 
- APIDescription 
- PortalAPICatalogue 

### 1. Adding a security policy via Tyk Operator

When you publish an API to the Portal, Tyk actually publishes a way for developers to enrol in a policy, not into the API directly. Therefore, you must also apply a SecurityPolicy CRD before proceeding with the publishing.

To do that, you can use the following command:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: SecurityPolicy
metadata:
 name: standard-pol
spec:
 name: standard-pol
 active: true
 state: active
 access_rights_array:
 - name: httpbin
   namespace: default
   versions:
     - Default
EOF
```

The above command will create the most basic security policy possible and attribute it to the httpbin API that was previously created.

### 2. Creating an API description

The portal serves as a visual description of an API. Therefore, you need to let the Tyk Portal know (via Tyk Operator) some details about the API you want to publish.

For doing that, we can run the following command:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: APIDescription
metadata:
 name: standard-desc
spec:
 name: HTTPBIN API
 policyRef:
  name: standard-pol
  namespace: default
 docs: 
  doc_type: swagger
  Documentation: 
```

As you can see the API description is tied to the SecurityPolicy in the lines:

```
policyRef:
  name: standard-pol
```

Furthermore, in the "documentation" field we have the swagger API definition of httpbin encoded in base64. All documentation descriptions need to be encoded base64.

### 3. Publishing the API to the Tyk Portal

For this step all you need to do is to create a PortalAPICatalogue resource that ties everything together. In our case, the command we need to run is:

```
cat <<EOF | kubectl apply -f -
apiVersion: tyk.tyk.io/v1alpha1
kind: PortalAPICatalogue
metadata:
 name: test-httpbin-api
spec:
 apis:
 - apiDescriptionRef:
    name: standard-desc
    namespace: default
EOF
```

As you can see above, you just need to reference the name of the apiDescription. Then, Tyk Operator will handle everything for you.

After all finished, if you go to the default TykPortal implementation (generally located at tyk-portal.local), you should see something like this in the API Catalogue section:

If you click on View documentation, you should have:


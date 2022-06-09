---
date: 2017-03-24T16:39:31Z
title: Getting started with Tyk Operator
weight: 16
menu:
    main:
        parent: "Tyk Operator"
---

## Getting started with Tyk Operator

Tyk Operator extends Kubernetes API with Custom Resources. Tyk features such as:  API Definitions, Security Policies, Authentication, Authorization, Rate Limits, and others can be managed just like other native Kubernetes objects. Tyk Operator leverages all the features of Kubernetes like kubectl, security, API services, RBAC. You can use Tyk Custom Resources to manage and secure REST, TCP, gRPC, GraphQL, and SOAP services. You can even take an existing REST based API and define a GraphQL schema to tell the GraphQL execution engine how to map those REST responses into the GraphQL schema, without a line of code. We can walk you through how to describe the entire API management system declaratively, from the API definitions and the security policies to the developer portal configurations and published API service catalogue by using Tyk Operator. You can find all the available example manifest files in the [samples](https://github.com/TykTechnologies/tyk-operator/tree/master/config/samples) directory.

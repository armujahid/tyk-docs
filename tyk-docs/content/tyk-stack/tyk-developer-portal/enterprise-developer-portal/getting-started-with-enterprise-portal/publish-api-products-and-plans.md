---
title: "Publish Api Products and Plans"
date: 2022-02-08
tags: [""]
description: ""
menu:
  main:
    parent: "Getting Started With Enterprise Portal"
weight: 3
---

{{< note success >}}
**This version of our Portal is in Beta**

This documentation is for the new Tyk Enterprise Portal currently in private beta. If you are interested in getting access contact us at [support@tyk.io](<mailto:support@tyk.io?subject=Tyk Enterprise Portal Beta>)

{{< /note >}}

## Introduction

In this section, you will learn how to publish the API products and plans to the public-facing portal so that API Consumers can access them.

## Prerequisites

- A Tyk Self-Managed [installation]({{< ref "/content/tyk-on-prem/installation/installation.md" >}})
- Tyk Self-Managed [added as a provider]({{< ref "/content/tyk-stack/tyk-developer-portal/enterprise-developer-portal/getting-started-with-enterprise-portal/with-tyk-self-managed-as-provider.md" >}})
- [Created and imported API Products and Plans from Tyk]({{< ref "/content/tyk-stack/tyk-developer-portal/enterprise-developer-portal/getting-started-with-enterprise-portal/create-api-product-and-plan.md" >}})

## Part 1 - Publish an API product

Follow these steps below how to publish an API Product to a catalogue:

1. From the **API Product** section, Click an API product to open the details.
2. Edit the metadata as needed.

| Field                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                     | This is an internal reference to the provider.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Provider type (disabled) | This refers to the type of provider; however, the only supported provider at this stage is Tyk Self-Managed.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| URL                      | The URL refers to the provider host URL for Tyk. Within the Tyk instance, the URL can be simply copied.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Gateway URL              | The Gateway URL refers to the URL that the portal developers will send the queries and use the access credentials.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Secret                   | This text is really heavy and it needs to be simpler. The Secret can be fetched from the Tyk analytics Dashboard. The procedure is as follows:  Go to the Tyk Dashboard Navigate to Users. Select a user with the permissions you want to bring on to the portal. You can find the secret under API Access Credentials. (Optional) You can find the organisation id  under Organisation ID if your use case requires. Note: The Portal will share the same permissions that the user selected to provide the secret.                              |
| Organisation ID          | The org id is required in order to connect to Tyk as a provider. It can be found in the user profile within the Tyk Dashboard.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Policy tags              | This is an optional field that can be used to define which policies from Tyk will be imported to the portal. If a tag is defined here, it needs to also be defined in the Policy section in Tyk described in the next journey. If this field is left empty in both this provider section and in the policies within Tyk, then all policies will be imported from the Tyk instance. How to include the label in the policy section inside Tyk will be explained in the next part Publish API Products and plans to the public-facing portal. |

3. Select a catalogue to publish the API product. If you want to create a custom catalogue.
4. From the top navigation menu, click **Live portal**, then select **Catalogues** to view the published catalogue.

## Part 2 - Publish a plan

In order for developers to be able to request access to an API Product and retrieve credentials, a minimum of one plan needs to be available within the same catalogue as the API Product.

Follow these steps below to knowhow to publish a plan:

1. From the Plans section, select a plan to open the details.
2. Edit the metadata as needed

| Field                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                     | This is an internal reference to the provider.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Provider type (disabled) | This refers to the type of provider; however, the only supported provider at this stage is Tyk Self-Managed.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| URL                      | The URL refers to the provider host URL for Tyk. Within the Tyk instance, the URL can be simply copied.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Gateway URL              | The Gateway URL refers to the URL that the portal developers will send the queries and use the access credentials.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Secret                   | The Secret can be fetched from the Tyk-pro / tyk-analytics dashboard. The procedure is as follows:  Go to the Tyk Dashboard Navigate to Users. Select a user with the permissions you want to bring on to the portal. You can find the secret under API Access Credentials. (Optional) You can find the organisation id  under Organisation ID if your use case requires. Note: The Portal will share the same permissions that the user selected to provide the secret.                              |
| Organisation ID          | The org id is required in order to connect to Tyk as a provider. It can be found in the user profile within the Tyk Dashboard.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Policy tags              | This is an optional field that can be used to define which policies from Tyk will be imported to the portal. If a tag is defined here, it needs to also be defined in the Policy section in Tyk described in the next journey. If this field is left empty in both this provider section and in the policies within Tyk, then all policies will be imported from the Tyk instance. How to include the label in the policy section inside Tyk will be explained in the next part Publish API Products and plans to the public-facing portal. |

3. Click **Save changes**. The plan will now be available within selected catalogue(s).
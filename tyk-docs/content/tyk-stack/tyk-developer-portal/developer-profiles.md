---
date: 2017-03-24T17:33:13Z
title: Developer Profiles
menu:
  main:
    parent: "Tyk Portal Classic"
weight: 2 
url: /tyk-developer-portal/tyk-portal-classic/developer-profiles
aliases:
  - /tyk-developer-portal/developer-profiles
---

Users that are signed up to your portal are called "Developers", these users have access to a Dashboard page which show them their API usage over the past 7 days as well as the policy and quota limits on their relevant keys.

Developers can sign up to multiple APIs using the API catalogue.

Developer accounts belong to an organisation ID, so accounts cannot be shared across organisations in a Tyk Dashboard setup.

### Navigate to the Portal Developers Section

![Developer Menu](/docs/img/2.10/developers_menu.png)

#### Select Add Developer

![Developer Profile add](/docs/img/2.10/add_developer.png)

### Add Basic Details

![Developer Profile Create Details](/docs/img/2.10/add_developer_details.png)

### Developer Profile Overview

The first panel in a developer profile will show you an avatar (if they have a Gravatar-enabled email address), as well as the basic fields of their signup:

![Developer profile detail](/docs/img/2.10/developer_details.png)

### Developer Usage

The next panel will show you their apI usage as an aggregate for all the tokens that they have generated with their developer access:

![Developer usage graph](/docs/img/2.10/developer_graph.png)

### Developer Keys

In this panel, you will be able to see the various Keys the developer has access to, and the policies that are connected to the individual Key.

{{< note success >}}
**Note**  

From version 1.9, you can now apply multiple policies to an individual Key.
{{< /note >}}

To drill down into the specific usage patterns for each Key, click **ANALYTICS** for the Key.

![Developer Keys](/docs/img/2.10/developer_keys.png)

### Add a New Key

To subscribe a developer to a new Key, from the Edit Developer screen, click **New Key**. From the pop-up screen, select one or more policies from the drop-down list and click **Request Key**.

 ![New Key Request](/docs/img/2.10/developer_new_key.png)

### Changing Developer Policy Keys

#### Step 1: View the Developer Profile

Browse to the developers list view and select the developer that you wish to manage.

![Developer profile detail](/docs/img/2.10/developer_details.png)

#### Step 2: View Keys List

This sections shows you the Keys and the policies connected to them. This view will always try to match the access level to a catalogue entry, if the policy assigned to a developer is not in the catalogue, the entry will read "(No Catalogue Entry)". We recommend that all policy levels are in your catalogue, even if they are not all live.

#### Step 3: Click Options

From the Options drop-down for the Key, select **Change Policy**.

![Keys Sections](/docs/img/2.10/developer_keys.png)

#### Step 4: Select the New Policy

Select a new policy to add to your Key from the **Policies** drop-down list. You can also remove existing policies connected to the Key.

![Change policy drop down list](/docs/img/2.10/developer_new_key.png)

#### Step 5: Save the Change

Click **CHANGE KEY POLICY** to save the changes.

### Developer OAuth Clients


### Edit the Developer Profile

All fields in the profile are editable. In this section you can select a field and modify that data for the developer. This will not affect any tokens they may have, but it will affect how it appears in their Developer Dashboard in your Portal.

![Developer edit form](/docs/img/2.10/edit_developer_details.png)

Developers can edit this data themselves in their accounts section.

### Search for a Developer

You can search for a developer (by email address) by entering their address in the Search field.

This option is only available from Dashboard v1.3.1.2 and onwards.

![Developer Profile Search](/docs/img/2.10/search_developers.png)

### Developer Edit Profile

Once logged in, a developer can edit their profile. Select **Edit profile** from the **Account** menu drop-down list.

![Manage Profile][9]

A developer can change the following:
* Email
* Change Password
* Name
* Telephone
* Country Location

### Reset Developer Password

If a developer has forgotten their password, they can request a password reset email from the Login screen.

![Login Screen][10]

1. Click **Request password reset**
2. Enter your email address and click **Send Password reset email**

![Email Reset][11]

You will be sent an email with a link to reset your Developer password. Enter your new password and click **Update**. You can then login with your new details.

{{< note success >}}
**Note**  

Your password must be a minimum of 6 characters.
{{< /note >}}

![Confirm password][12]





 [1]: /docs/img/dashboard/portal-management/developer_menu_2.5.png
 [2]: /docs/img/dashboard/portal-management/add_developer_2.5.png
 [3]: /docs/img/dashboard/portal-management/developer_details_2.5.png
 [4]: /docs/img/dashboard/portal-management/developer_overview_2.5.png
 [5]: /docs/img/dashboard/portal-management/developer_usage_2.5.png
 [6]: /docs/img/dashboard/portal-management/developer_subs_2.5.png
 [7]: /docs/img/dashboard/portal-management/developer_edit_2.5.png
 [8]: /docs/img/dashboard/portal-management/developer_search_2.5.png
 [9]: /docs/img/dashboard/portal-management/developer_manage_profile.png
 [10]: /docs/img/dashboard/portal-management/login_screen.png
 [11]: /docs/img/dashboard/portal-management/email_password_request.png
 [12]: /docs/img/dashboard/portal-management/password_confirmation.png
 [13]: /docs/img/dashboard/portal-management/developer_edit_2.5.png
 [14]: /docs/img/dashboard/portal-management/keys_dev_profile.png
 [15]: /docs/img/dashboard/portal-management/change_key_policy.png
 [16]: /docs/img/dashboard/portal-management/new_key_request.png 



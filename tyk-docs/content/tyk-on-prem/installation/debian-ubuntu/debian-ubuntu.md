---
date: 2017-03-22T15:46:41Z
Title: "Debian / Ubuntu "
tags: ["Tyk Stack", "Self Managed", "Installation", "Ubuntu", "Debian"]
description: "How to install the Tyk Stack on Ubuntu or Debian using Ansible or with shell scripts"
menu:
  main:
      parent: "Self-Managed Installation"
weight: 5
url: "/tyk-on-premises/debian-ubuntu"
aliases:
  - /getting-started/installation/with-tyk-on-premises/on-ubuntu/
  - /getting-started/installation/tyk-on-premises/on-ubuntu/
  - /tyk-on-premises/debian-ubuntu

---
{{< tabs_start >}}
{{< tab_start "Ansible" >}}
<br />
{{< note >}}
**Requirements**

[Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) is required to run the following commands. Instructions on how install Tyk with shell is in the <b>Shell</b> tab.
{{< /note >}}

## Getting Started
1. clone the [tyk-ansible](https://github.com/TykTechnologies/tyk-ansible) repositry

```bash
$ git clone https://github.com/TykTechnologies/tyk-ansible
```

2. `cd` into the directory
```.bash
$ cd tyk-ansible
```

3. Run initalization script to initialize environment

```bash
$ sh scripts/init.sh
```

4. Modify `hosts.yml` file to update ssh variables to your server(s). You can learn more about the hosts file [here](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

5. Run ansible-playbook to install the following:
- Redis
- MongoDB/SQL
- Tyk Dashboard
- Tyk Gateway
- Tyk Pump

```bash
$ ansible-playbook playbook.yml -t tyk-pro -t redis -t mongodb
```

You can choose to not install Redis or MongoDB by removing the `-t redis` or `-t mongodb` respectively. However Redis and MongoDB/SQL are a requirement and need to be installed for the Tyk Pro installation to run.

## Supported Distributions
| Distribution | Version | Supported |
| --------- | :---------: | :---------: |
| Debian | 10 | ✅ |
| Debian | 9 | ✅ |
| Debian | 8 | ❌ |
| Ubuntu | 20 | ✅ |
| Ubuntu | 18 | ✅ |
| Ubuntu | 16 | ✅ |
| Ubuntu | 14 | ❌ |

{{< tab_end >}}
{{< tab_start "Shell" >}}
<br />
{{< note >}}
**Requirements**

Before installing the Tyk components in the order below, you need to first install Redis and MongoDB/SQL.
{{< /note >}}

## Getting Started

{{< tabs_start >}}
{{< tab_start "MongoDB" >}}
#### Install MongoDB 4.0

You should follow the [online tutorial for installing MongoDb](https://docs.mongodb.com/v4.0/tutorial/install-mongodb-on-ubuntu/). We will be using version 4.0. As part of the Mongo installation you need to perform the following:

1. Import the public key
2. Create a list file
3. Reload the package database
4. Install the MongoDB packages
5. Start MongoDB
6. Check the `mongod` service is running

{{< tab_end >}}
{{< tab_start "SQL" >}}
#### Install SQL
See [SQL configuration]({{< ref "/content/planning-for-production/database-settings/sql-configuration.md" >}}) for details on installing SQL in a production environment.
{{< tab_end >}}
{{< tabs_end >}}

### Install Redis

```bash
sudo apt-get install -y redis-server
```

## Install Tyk Pro on Ubuntu

Installing Tyk on Ubuntu is very straightforward using our APT repositories, follow the guides and tutorials in this section to have Tyk up and running in no time.

The suggested order would be to install Tyk Dashboard, then Tyk Pump and then Tyk Gateway for a full stack.

- [Dashboard](/docs/getting-started/installation/with-tyk-on-premises/on-ubuntu/dashboard/)
- [Pump](/docs/getting-started/installation/with-tyk-on-premises/on-ubuntu/analytics-pump/)
- [Gateway](/docs/getting-started/installation/with-tyk-on-premises/on-ubuntu/gateway/)
{{< tab_end >}}
{{< tabs_end >}}

{{< note success >}}
**Note**  

For a production environment, we recommend that the Gateway, Dashboard and Pump are installed on separate machines. If installing multiple Gateways, you should install each on a separate machine. See [Planning for Production](/docs/planning-for-production/) For more details.
{{< /note >}}

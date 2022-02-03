---
date: 2017-03-22T17:04:48Z
title: "Ansible "
tags: ["Tyk Stack", "Self Managed", "Installation", "Ansible"]
description: "How to install the Tyk Stack using Ansible"
menu:
  main:
      parent: "Self-Managed Installation"
weight: 3
url: "/tyk-on-premises/ansible"
---
{{< note >}}
**Requirements**

[Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) is required to run the following commands. 
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
- MongoDB or one of our supported SQL Platforms]
- Tyk Dashboard
- Tyk Gateway
- Tyk Pump

```bash
$ ansible-playbook playbook.yml -t tyk-pro -t redis -t mongodb
```

You can choose to not install Redis or MongoDB/SQL by removing the `-t redis` or `-t mongodb` (**ADD SQL FLAG**) respectively. However Redis and MongoDB/SQL are a requirement and need to be installed for the Tyk Pro to run.

{{< note success >}}
**Note**  

For a production environment, we recommend that the Gateway, Dashboard and Pump are installed on separate machines. If installing multiple Gateways, you should install each on a separate machine. See [Planning for Production](/docs/planning-for-production/) For more details.
{{< /note >}}

## Supported Distributions
| Distribution | Version | Supported |
| --------- | :---------: | :---------: |
| Amazon Linux | 2 | ✅ |
| CentOS | 8 | ⚠️ |
| CentOS | 7 | ✅ |
| CentOS | 6 | ❌ |
| Debian | 10 | ✅ |
| Debian | 9 | ✅ |
| Debian | 8 | ❌ |
| RHEL | 8 | ⚠️ |
| RHEL | 7 | ✅ |
| RHEL | 6 | ❌ |
| Ubuntu | 20 | ✅ |
| Ubuntu | 18 | ✅ |
| Ubuntu | 16 | ✅ |
| Ubuntu | 14 | ❌ |

| Symbol | Description |
| :---------: | --------- |
| ✅ | Tested / Supported |
| ⚠️ | Tested / Not officially supported by Tyk |
| ❌️ | Untested / Not supported by tool |

## Variables
- `vars/mongodb.yml`

| Variable | default value | Comments |
| --------- | :---------: | --------- |
| bind_ip | `0.0.0.0` | Binding address of MongoDB |
| mongodb_version | `4.4` | MongoDB version |

Read more about MongoDB configuration [here](https://github.com/ansible-collections/community.mongodb).

- `vars/redis.yml`

| Variable | default value | Comments |
| --------- | :---------: | --------- |
| redis_bind_interface | `0.0.0.0` | Binding address of Redis |

Read more about Redis configuration [here](https://github.com/geerlingguy/ansible-role-redis).

- `vars/tyk.yml`

| Variable | Default | Comments |
| --------- | :---------: | --------- |
| dashboard_protocol | `http` | Dashboard server protocol |
| dashboard_host | | Dashboard server host if different than the hosts url |
| dashboard_port | `3000` | Dashboard server listening port |
| gateway_protocol | `http` | Gateway server protocol |
| gateway_host | | Gateway server host if different than the hosts url |
| gateway_port | `8080` | Gateway server listening port |
| redis_host | | Redis server host if different than the hosts url |
| redis_port | `6379` | Redis server listening port |
| redis_enable_cluster | `false` | Enable if redis is running in cluster mode |
| redis_enable_ssl | `false` | Enable if redis connection is secured with SSL |
| mongodb_host | | MongoDB server host if different than the hosts url |
| mongodb_port | `27017` | MongoDB server listening port |

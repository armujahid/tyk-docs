---
date: 2017-03-22T16:19:08Z
Title: "Red Hat (RHEL / CentOS) "
tags: ["Tyk Gateway", "Self Managed", "Installation", "Red Hat", "CentOS"]
description: "How to install the Tyk Stack on Red Hat or CentOS using Ansible or with shell scripts"
menu:
  main:
    parent: "Self-Managed Installation"
weight: 4
url: "/tyk-on-premises/redhat-rhel-centos"
aliases:
  - /getting-started/installation/with-tyk-on-premises/redhat-rhel-centos/
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
- MongoDB or SQL
- Tyk Dashboard
- Tyk Gateway
- Tyk Pump

```bash
$ ansible-playbook playbook.yml -t tyk-pro -t redis -t mongodb (**ADD SQL HERE**)
```

You can choose to not install Redis, MongoDB or SQL by removing the `-t redis` or `-t mongodb` (**ADD SQL HERE**)respectively. However Redis, MongoDB or SQL are a requirement and need to be installed for the Tyk Pro installation to run.

## Supported Distributions
| Distribution | Version | Supported |
| --------- | :---------: | :---------: |
| Amazon Linux | 2 | ✅ |
| CentOS | 8 | ⚠️ |
| CentOS | 7 | ✅ |
| CentOS | 6 | ❌ |
| RHEL | 8 | ⚠️ |
| RHEL | 7 | ✅ |
| RHEL | 6 | ❌ |

| Symbol | Description |
| :---------: | --------- |
| ✅ | Tested / Supported |
| ⚠️ | Tested / Not officially supported by Tyk |
| ❌️ | Untested / Not supported by tool |

{{< tab_end >}}
{{< tab_start "Shell" >}}
## Install Tyk Pro on Red Hat (RHEL) / CentOS

Installing Tyk on RHEL is very straightforward using our YUM repositories, follow the guides and tutorials in this section to have Tyk up and running in no time.

The suggested order would be to install Tyk Dashboard, then Tyk Pump and then Tyk Gateway for a full stack.

- [Dashboard](/docs/getting-started/installation/with-tyk-on-premises/redhat-rhel-centos/dashboard/)
- [Pump](/docs/getting-started/installation/with-tyk-on-premises/redhat-rhel-centos/analytics-pump/)
- [Gateway](/docs/getting-started/installation/with-tyk-on-premises/redhat-rhel-centos/gateway/)
{{< tab_end >}}
{{< tabs_end >}}

{{< note success >}}
**Note**  

For a production environment, we recommend that the Gateway, Dashboard and Pump are installed on separate machines. If installing multiple Gateways, you should install each on a separate machine. See [Planning for Production](/docs/planning-for-production/) For more details.
{{< /note >}}

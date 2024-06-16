+++
title = "Infrastructure as Code for Red Teamers"
date = 2024-06-15
[taxonomies]
tags = ["IaaC", "automation", "red team", "devops"]
+++

[Red Team Infrastructure Wiki](https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki)

Prior before starting a new red team engagement, there are plenty of infrastructure setup that is required to prepare for engagements. The process is always the same as you can see in the link above. There are a lot of moving pieces like setting up the C2 network, redirectors, proxies, phishing, domains, and etc. For every component, there is a custom configuration that may be required. This can be difficult to track and doing this all manually is a repetitive chore.

Modern sysadmin/devops teams encounter the same issues but they are smarter than us by using tools like [packer](https://www.packer.io/), [terraform](https://www.terraform.io/), and [ansible](https://www.ansible.com/). These set of tools help with the provisioning the infrastructure in a quick, iterative, and consistent fashion. There is too much info to dive deep into these three tools in this single post but here is the general idea.
* Packer can help with creating a custom "gold" image of which you can load up tools/scripts/settings preloaded so you don't need to.
* Terraform can help with provisioning the images that you created with packer by deploying on whatever platform you end up using, it can be cloud or on-premise hardware, it doesn't matter as long as there is a supported terraform script that interfaces with the platform's API.
* Finally with Ansible, it can help with logging into the newly setup VMs that is created by terraform via SSH and start executing commands that you generally perform when you first setup a host. For example, adding new users, setting up SSH keys, configuring apache modules, running package updates, and etc.

Learning and understanding these three tools does take some time but it will pay off in the end as you can also easily offload this task to anyone. Quite frankly, any red teamer worth their salt should know how their operational security and infrastructure works. The best way to learn is by writing IaaC scripts and track changes as you develop the core components. Take a peek at this simple [example](https://www.detectionlab.network/deployment/azure/) to get an fundamental idea of how components are put together.
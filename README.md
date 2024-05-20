# Ansible role: labocbz.install_kubernetes_cluster

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Docker](https://img.shields.io/badge/Tech-Docker-orange)
![Tag: Kubernetes](https://img.shields.io/badge/Tech-Kubernetes-orange)
![Tag: K8S](https://img.shields.io/badge/Tech-K8S-orange)
![Tag: Kubectl](https://img.shields.io/badge/Tech-Kubectl-orange)
![Tag: Kubeadm](https://img.shields.io/badge/Tech-Kubeadm-orange)
![Tag: Kubelet](https://img.shields.io/badge/Tech-Kubelet-orange)

An Ansible role to install and configure a Kubernetes cluster on your hosts.

This Ansible role simplifies the installation and configuration of Kubernetes using K3S. It detects and installs Docker if necessary, then configures K3S to start a manager node. Next, it retrieves join tokens for both manager and worker nodes, and configures the K3S cluster with specified network and DNS settings. The role also installs Helm for managing Kubernetes packages and deploys a control stack, such as Portainer or Rancher, to facilitate cluster management.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_kubernetes_cluster__is_manager: false

install_kubernetes_cluster__docker_script_version: "20.10"

install_kubernetes_cluster__data_dir: "/var/lib/rancher/k3s"
install_kubernetes_cluster__cluster_cidr: "10.42.0.0/16"
install_kubernetes_cluster__service_cidr: "10.43.0.0/16"
install_kubernetes_cluster__service_node_port_range: "30000-32767"
install_kubernetes_cluster__cluster_dns: "10.43.0.10"
install_kubernetes_cluster__cluster_domain: "cluster.local"
install_kubernetes_cluster__tls_sans:
  - "my.server-1.domain.tld"
  - "my.server-2.domain.tld"
  - "my.server-3.domain.tld"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_kubernetes_cluster__is_manager: false

inv_install_kubernetes_cluster__docker_script_version: "20.10"

inv_install_kubernetes_cluster__data_dir: "/var/lib/rancher/k3s"
inv_install_kubernetes_cluster__cluster_cidr: "10.42.0.0/16"
inv_install_kubernetes_cluster__service_cidr: "10.43.0.0/16"
inv_install_kubernetes_cluster__service_node_port_range: "30000-32767"
inv_install_kubernetes_cluster__cluster_dns: "10.43.0.10"
inv_install_kubernetes_cluster__cluster_domain: "cluster.local"

#inv_install_kubernetes_cluster__tls_sans:
#  - "my.server-1.domain.tld"
#  - "my.server-2.domain.tld"
#  - "my.server-3.domain.tld"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_kubernetes_cluster"
  tags:
    - "labocbz.install_kubernetes_cluster"
  vars:
    install_kubernetes_cluster__is_manager: "{{ inv_install_kubernetes_cluster__is_manager }}"
    install_kubernetes_cluster__docker_script_version: "{{ inv_install_kubernetes_cluster__docker_script_version }}"
    install_kubernetes_cluster__data_dir: "{{ inv_install_kubernetes_cluster__data_dir }}"
    install_kubernetes_cluster__cluster_cidr: "{{ inv_install_kubernetes_cluster__cluster_cidr }}"
    install_kubernetes_cluster__service_cidr: "{{ inv_install_kubernetes_cluster__service_cidr }}"
    install_kubernetes_cluster__service_node_port_range: "{{ inv_install_kubernetes_cluster__service_node_port_range }}"
    install_kubernetes_cluster__cluster_dns: "{{ inv_install_kubernetes_cluster__cluster_dns }}"
    install_kubernetes_cluster__cluster_domain: "{{ inv_install_kubernetes_cluster__cluster_domain }}"
    install_kubernetes_cluster__tls_sans: "{{ inv_install_kubernetes_cluster__tls_sans }}"
  ansible.builtin.include_role:
    name: "labocbz.install_kubernetes_cluster"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-01-08: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __
* Tested and validated on Docker DIND
* Removed docker socket local and port
* Validated on local, evenif portainer stack is not deployed (ImageInspectError)
* Added support for Rancher or Portainer (no customs)
* Fix risky shell

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [How to Setup Kubernetes Cluster on Debian 11](https://snapshooter.com/learn/linux/install-kubernetes)
* [K3S](https://k3s.io/)
* [Doc K3S](https://docs.k3s.io/)
* [Advanced K3S](https://docs.k3s.io/advanced)
* [Helm CLI Quick Start](https://ranchermanager.docs.rancher.com/getting-started/quick-start-guides/deploy-rancher-manager/helm-cli)
* [github.com/cert-manager](https://github.com/cert-manager/cert-manager/releases)

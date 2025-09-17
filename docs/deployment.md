## Depployment Guide

This document is outlines the basic steps to deploy the Red Hat Automotive Development Platform (RHADP). 

It is the TL;DR version. For in-depth documentation, start [here](README.md).

### Repository setup

Clone the repository:
```bash
git clone https://github.com/rhadp/rhadp-platform.git
cd rhadp-platform
```

In case you want to make changes to the Ansible playbooks, roles or to any of the configuration templates, `fork` the repository, instead of just cloning it.

### Environment preparation

Create a Python virtual environment, install Ansible and its dependencies:

```bash
python3.12 -m venv venv
source venv/bin/activate

# install the dependencies
pip install -r requirements.txt
```

### Inventory setup

Folder `inventory` contains examples of different platform configurations. Select a configuration example (e.g. inventory/platform.yml.example) and modify it to your needs.

Copy the inventory template:
```bash
cp inventory/platform.yml.example inventory/platform.yml
```

Edit the inventory file with your specific settings:
```bash
vi inventory/platform.yml
```

### Platform bootstrap

To deploy RHADP with all enabled features, run the following playbook:

```bash
ansible-playbook -i inventory/ 0_all.yml
```

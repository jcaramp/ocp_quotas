ocp_quotas
=========

This role implements tasks for set the Resource Quotas defined in an Openshift project.

Requirements
------------

The below requirements are needed on the host that executes this role.

Minimum Ansible version: 2.8.0

Ansible modules:

- k8s
- k8s_auth
- k8s_info / k8s_facts

Python modules:

- python >= 2.7
- openshift >= 0.6
- PyYAML >= 3.11
- urllib3
- requests
- requests-oauthlib

Role Variables
--------------

Roles needed to use this role:

Variable | Description | Required | Choices/***Defaults***
------------ | ------------- | ------------- | -------------
api_url | Openshift API URL | yes | -
ocp_username |  Openshift Username | no | -
ocp_password | Openshift Password | no | - 
ocp_token | Openshift Service Account Access Token | no | -
ocp_verify_ssl | Verify SSL | no | ***true***, false
project_name | Openshift Project Name | yes | -
quotas_size | Resource Quotas size depending on **`quotas`** variable definition | yes | ***medium***, large **\***

**\*** See file defaults/main.yml to know the default **`quotas`** variable and sizes defined ***(it can always be redefined)***.

Dependencies
------------

No dependencies.

Example Playbook
----------------

Using this variable example definition:

    # Default Resource Quotas definition
    quotas:
      medium:
        hard:
          requests.storage: "50Gi"
          requests.cpu: "2"
          requests.memory: 10Gi
          limits.cpu: "20"
          limits.memory: 24Gi
      large:
        hard:
          requests.storage: "50Gi"
          requests.cpu: "4"
          requests.memory: 20Gi
          limits.cpu: "40"
          limits.memory: 48Gi

This is an example using an API token to authenticate:

    - hosts: servers
      roles:
        - role: ocp_quotas
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_token: "{{ service_account_token }}"
            ocp_verify_ssl: false
            project_name: "example-project"
            quotas_size: "medium"

And this is an example using an user/password to authenticate:

    - hosts: servers
      roles:
        - role: ocp_quotas
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_username: "clusteradmin"
            ocp_password: "xxxxxxxxxxx"
            ocp_verify_ssl: true
            project_name: "example-project"
            quotas_size: "large"

Platforms
------------

Tested on:

- Red Hat Enterprise Linux 7.7
- Red Hat Openshift Container Platform 4.2

License
-------

GNU General Public License v3.0

Author Information
------------------

This role was written in 2020 by Jesús Carmona Ampuero
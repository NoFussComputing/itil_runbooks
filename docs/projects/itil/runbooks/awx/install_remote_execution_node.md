---
title: Install Remote Execution Node
description: Runbook to detail what to do to install a remote execution node.
date: 2024-02-26
template: itil_runbook.html
---

This runbook details the steps required to install an AWX remote execution node.

## Assumptions

- Ansible is installed.

    !!! tip
        ``` bash
        apt install -y --no-install-recommends python3-pip
        
        pip install ansible-core
        ```

- Remote execution node has already been setup within AWX


## Workflow

1. download config for the execution node from AWX `Instances -> <Instance Name> -> Install Bundle`

    > Or `curl -u <username>:<password> https://tower.nofusscomputing.com/api/v2/instances/<instance ID>/install_bundle/ -o bundle.tar.gz`

1. extract `tar -xzf bundle.tar.gz`

1. `cd <extracted dir that matches instance name>`

1. install dependencies `ansible-galaxy collection install -r requirements.yml`

1. run the playbook `ansible-playbook -i inventory.yml install_receptor.yml --extra-vars "ansible_connection=local"`

1. update `/etc/containers/registries.conf` with the following so the docker hub images resolve without having to specify registry `docker.io`

    ``` ini

    unqualified-search-registries = [ "docker.io" ]

    ```

1. remove extracted directory `rm -rf <extracted dir that matches instance name>`

1. remove archive `rm bundle.tar.gz`

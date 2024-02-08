---
title: New Computer
description: Runbook to detail what to do to add a new new computer to GLPI
date: 2023-11-20
template: itil_runbook.html
area: Automation
---


## New host workflow

1. Add host to GLPI _Service catalog item: new computer_

1. configure the host within the inventory

    - `hostconfig` variable

        !!! tip
            If host is to be exposed/used on the public internet set variable `host_external_ip` to the publicly accessible IP address.

    - edit `hosts.yaml` adding the host to **all** of the groups it is part of

1. run playbook `all.yaml` against the host

1. As soon as the playbook has completed login as the admin user

    1. change the password and save to the password manager.

        !!! tip
            use the password manager to generate a random and long password.

        !!! danger
            Failing to reset the admin users password removes console access to the machine. Without console access, fixing a missed-steak is no longer an option.

        !!! note
            You will be required to have your ssh-public cert signed by the host CA to access the machine.

1. Confirm that the host is displayed within grafana _node exporter_

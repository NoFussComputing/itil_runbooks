---
title: Adding a Custom CA Trusted Certificates
description: Runbook to detail what to do for a linux host to add a custom CA certificate to the hosts trusted certificates.
date: 2024-02-26
template: itil_runbook.html
---

This workflow details the steps required to add a CA certificate to the A hosts Trusted Certificates.


## Assumptions

The following assumptions are made for the usage of this runbook:

- You have `sudo` access

- The host where these commands to run from has `ca-certificates` package installed.

    - _Alpine Based OS_ `apk add ca-certificates`
    
    - _Debian based OS_ `apt install ca-certificates`


## Workflow


1. If the certificate is currently in `pem` format, convert it to an `x509` certificate.

    - `sudo openssl x509 -inform PEM -in /<certificate path>/ca_cert.pem -out /usr/local/share/ca-certificates/<dns name here>.crt`

        > This will convert `pem` certificate `/<certificate path>/ca_cert.pem` and save it to `/usr/local/share/ca-certificates/<dns name here>.crt`

1. If certificate not in path `/usr/local/share/ca-certificates/` or a sub-directory of, copy the certificate there

1. _Recommended_ flush the current trusted certificates `sudo update-ca-certificates --fresh`

1. Update the host trusted certificates with `sudo update-ca-certificates`

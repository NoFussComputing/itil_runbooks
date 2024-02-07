---
title: API Timeout
description: This runbook details fault finding a Kubernetes api timeout.
date: 2024-02-01
template: project.html
---

This runbook details the steps to fault find timeout errors when connecting to the api server. i.e. the following errors:

- `proxy io timeout`

- `curl -k https://kubernetes.default.svc/api/v1/namespaces` timeout


## Workflow

1. determine which node has the timeout:

    1. run command `curl -k https://172.16.100.1/api/v1/namespaces` on all nodes, where `172.16.100.1` is the ip address for kubernetes.default.svc.

        > the node that doesn't return anything within a few seconds is a good guess as the node with the issue

1. Stop the k3s service on the affected node `service k3s stop` for master node and `service k3s-agent stop` on a slave node

1. manually run the k3s binary which will output logs to stdout

    - Master Node: `k3s server`

    - Slave node: `k3s agent`

1. as the log scrolls through look for `[ERROR]` which will be colour coded red:

    - This error denotes a connection problem, likely firewall.

        ``` log
        ERRO[0141] Failed to connect to proxy. Empty dialer response  error="dial tcp <master node ip>:6443: connect: connection timed out"
        ERRO[0141] Remotedialer proxy error                      error="dial tcp <master node ip>:6443: connect: connection timed out"
        ```

        1. Fault find the firewall with the following, and from the affected node, which within a second or two should return a json unauthorized document:

            1. `curl -k https://127.0.0.1:6443/api/v1/namespaces` This command may fail

            1. `curl -k https://<internal IP>:6443/api/v1/namespaces`

            1. `curl -k https://<external IP>:6443/api/v1/namespaces`

            !!! tip
                It may also be helpful to run these commands on the node that the affected node was trying to connect to. This will aid in the fault finding, specifically the connection path.

        1. if any of the above http requests work, that should tell you where the connectivity problem is.

        1. review the firewall(s) between the affected nod and the master the node was trying to connect to and ensure that there is sufficient rules to allow the node to connect.

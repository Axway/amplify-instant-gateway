---
title: Troubleshooting guide
linkTitle: Troubleshooting guide
weight: 50
date: 2021-07-26
hide_readingtime: true
description: Troubleshoot the components in Amplify Instant Gateway.
---

The troubleshooting section includes details about issues you may come across during the use of the Amplify Instant Gateway, including error messages and why the message may have occurred, and suggested methods to resolve them.

## Deployment Failed

When a Virtual API is deployed in error and a status message of **Deployment Failed** is shown in the Deployments tab of the Virtual API Screen, then this is usually caused by a misconfiguration in the data plane or the governance plane. 

A regularly observered misconfiguration is a secret that is expected by one of the Virtual APIs and that is not present in the secret store. Be sure to check that all required secrets for all deployed APIs are present. Even if a secret is not used by the Virtual API that you are deploying, you could still see this error if the missing secret is expected by an already deployed Virtual API.

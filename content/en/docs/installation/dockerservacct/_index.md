---
title: Set up Service Account to download Docker images
linkTitle: Service Account for Docker images
weight: 20
date: 2022-01-06
hide_readingtime: true
---

## Prerequisites

* You have an organisation provisioned on the Amplify Platform.
* You have Axway CLI installed. See [Install Axway Central CLI](https://docs.axway.com/bundle/axway-open-docs/page/docs/central/cli_central/cli_install/index.html)

## Via Amplify Management APIs

To do

## Via Axway CLI

To do

## Via UI

1. Log into the Amplify platform
2. Navigate to Organization settings
3. Click on Service Accounts on the left hand menu
4. Click '+ Service Account' to create a new service account
5. Give the service account an identifable name. For example, 'ampgw-service-acct-for-docker-repo'
6. In the Method dropdown select 'Client Secret'
7. In the Credentials dropdown select 'Platform-generated Secret'
8. Save the service account
9. Copy the generated secret and store it securely. This secret will be used during Amplify Instant Gateway install process.
10. Copy the client id of the service account. This client id will be used during Amplify Instant Gateway install process.

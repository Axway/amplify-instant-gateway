---
title: Create a Virtual API
linkTitle: Create a Virtual API
weight: 10
date: 2021-07-26
hide_readingtime: false
description: >
  Create a Virtual API with or without an OAS specification.
---

A Virtual API is a representation of an API which can be invoked through Amplify Gateway which will route requests to the backend service. A Virtual API may be created from an initial backend specification (such as an OAS document) OR it may be manually populated where no backend specification exists.

## Prerequisites

* You have an organisation provisioned on the Amplify Platform.
* You have the appropriate permissions to allow you to create Virtual APIS. That is, you are a member of at least one team within the organisation with a minimum role of 'Developer'
* You are familiar with how to call an API (via Postman or CURL) if using the Amplify Management APIs
* You have downloaded the Axway CLI, if attempting to create the Virtual API via CLI. See [Install Axway Central CLI](https://docs.axway.com/bundle/axway-open-docs/page/docs/central/cli_central/cli_install/index.html)

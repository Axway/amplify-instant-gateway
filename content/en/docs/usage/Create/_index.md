---
title: Create a Virtual API
linkTitle: Create a Virtual API
weight: 10
date: 2021-07-26
hide_readingtime: false
description: >
  Create a Virtual API with or without an OAS specification.
---

A Virtual API is a container for API which can be invoked through the Amplify Gateway and which will route requests to backend service(s). A Virtual API may be created from an initial backend specification (such as an OAS document) OR it may be manually populated where no backend specification exists.

## Prerequisites

* You have an organisation provisioned on the Amplify Platform 
* You are familiar with how to call an API (via Postman or CURL) if using the Amplify APIs
* You have downloaded the Axway CLI, if attempting to create the Virtual API via CLI

## Creating a Virtual API with an OAS specification

If the backend API you wish to virtualize in Amplify has an OAS specification, you can provide that specification when creating the Virtual API.

### Creating a Virtual API via the Axway Amplify Management APIs

To create a vitual API via API call the following end-point

{code} POST /management/v1alpha1/virtualapis {code}

Passing the following json payload


### Creating a Virtual API via Axway CLI

### Creating a Virtual API via UI



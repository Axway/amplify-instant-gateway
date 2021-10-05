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

* You have an organisation provisioned on the Amplify Platform.
* You have the appropriate permissions to allow you to create Virtual APIS. That is, you are a member of at least one team within the organisation with a role of 'Developer'
* You are familiar with how to call an API (via Postman or CURL) if using the Amplify APIs
* You have downloaded the Axway CLI, if attempting to create the Virtual API via CLI. See [Install Axway Central CLI](https://docs.axway.com/bundle/axway-open-docs/page/docs/central/cli_central/cli_install/index.html)

## Creating a Virtual API with an OAS specification

If the backend API you wish to virtualize in Amplify has an OAS specification, you can provide that specification when creating the Virtual API.

### Creating a Virtual API via the Axway Amplify Management APIs

To create a virtual API via API call the following end-point

    POST /management/v1alpha1/virtualapis

If the followng json payload is provided then a new Virtual API called 'my-api' will be created with a title 'My API'

```json
{
	"group": "management",
	"apiVersion": "v1alpha1",
	"kind": "VirtualAPI",
	"name": "my-api",
	"title": "My API",
	"tags": [
		"My API",
		"Axway"
	],
	"spec": {
		"type": "REST",
		"description": "A simple API which returns a list of users",
		"api": {
			"oas": {
				"openapi": "3.0.0",
				"info": {
					"title": "My API",
					"description": "Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.",
					"version": "0.1.9"
				},
				"servers": [{
					"url": "http://my.api.example.com/v1",
					"description": "Optional server description, e.g. Main (production) server"
				}],
				"paths": {
					"/users": {
						"get": {
							"summary": "Returns a list of users.",
							"description": "Optional extended description in CommonMark or HTML.",
							"responses": {
								"200": {
									"description": "A JSON array of user names",
									"content": {
										"application/json": {
											"schema": {
												"type": "array",
												"items": {
													"type": "string"
												}
											}
										}
									}
								}
							}
						}
					}
				}
			}
		}
	}
}
```
In this example, the specification:

* Indicates that the Virtual API being created is a REST API.
* Provides a short description of what the API does.
* Provides the OAS 3 specification for the API. This specifcation will be parsed to create the paths that the gateway can route to. It will also be used to create a specification to be consumed by client application developers when the API is exposed via Amplify Gateway.

### Creating a Virtual API via Axway CLI

To create a virtual API via CLI, log on the Axway Central CLI and run the following command

    axway central create -f my-api.yaml

A yaml file called my-api.yaml should exist on teh file system with the following contents. Executing this command with the yaml file contents will create a new Virtual API called 'my-api' with the title 'My API'

```yaml
group: management
apiVersion: v1alpha1
kind: VirtualAPI
name: my-api
title: My API
tags:
  - My API
  - Axway
spec:
  type: REST
  description: A simple API which returns a list of users
  api:
    oas:
      openapi: 3.0.0
      info:
        title: My API
        description: >-
          Optional multiline or single-line description in
          [CommonMark](http://commonmark.org/help/) or HTML.
        version: 0.1.9
      servers:
        - url: 'http://my.api.example.com/v1'
          description: 'Optional server description, e.g. Main (production) server'
      paths:
        /users:
          get:
            summary: Returns a list of users.
            description: Optional extended description in CommonMark or HTML.
            responses:
              '200':
                description: A JSON array of user names
                content:
                  application/json:
                    schema:
                      type: array
                      items:
                        type: string
```
In this example, the specification:

* Indicates that the Virtual API being created is a REST API.
* Provides a short description of what the API does.
* Provides the OAS 3 specification for the API. This specifcation will be parsed to create the paths that the gateway can route to. It will also be used to create a specification to be consumed by client application developers when the API is exposed via Amplify Gateway.


### Creating a Virtual API via UI



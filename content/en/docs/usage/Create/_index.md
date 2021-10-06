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

### Via Amplify Management APIs

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

### Via Axway CLI

To create a virtual API via CLI, log on the Axway Central CLI and run the following command

    axway central create -f my-api.yaml

A yaml file called my-api.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will create a new Virtual API called 'my-api' with the title 'My API'

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


### Via UI

<Need to fill this in once I get a screenshot or 2>

## Creating a Virtual API with no OAS specification

If the backend API you wish to virtualize in Amplify does not have an OAS specification, you can create the Virtual API routes manually.

### Via Amplify Management APIs

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
		"description": "A simple API which returns a list of users"
	}
}
```

However, this virutal API cannot be used as is. When an OAS specification is provided, then the path routes will be parsed directly from the OAS specification. Where no OAS specification is provided, this information needs to be provided by calling another API to create another resource called a Virtual Service. 

    POST /apis/management/v1alpha1/virtualapis/{virtualapi}/virtualservices

where {virtualapi} is the name of the api created by calling the previous end-point. The following json payload should be included

```json
{
	"group": "management",
	"apiVersion": "v1alpha1",
	"kind": "VirtualService",
	"name": "my-api",
	"title": "my-api",
	"spec": {
		"route": [{
			"service": {
				"codec": "AUTO",
				"prefix": "/v1",
				"protocol": "http",
				"endpoints": [{
					"host": "my.api.example.com",
					"port": 80
				}]
			},
			"operations": [{
				"id": "my-api-get-users",
				"path": "/users",
				"method": "GET"
			}]
		}],
		"prefix": "/v1"
	}
}
```
In this example:

* The codec is set to 'AUTO' meaning that the gateway will negotiate with the backend service over how to communicate. If the backend can communicate over HTTP2 that protocol will be used by the gateway, otherwise the gateway will revert to communicating over HTTP1.1
* 'my.api.example.com' is the backend host server and '80' is the port. The backend can be called over http protocol. 
* The prefix to call the backend service is '/v1'.
* The backend api supports 1 operation - GET /users.
* The prefix to call the Virtual API through the gateway is '/v1'.

### Via Axway CLI

To create a virtual API via CLI, log on the Axway Central CLI and run the following command

    axway central create -f my-api.yaml

A yaml file called my-api.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will create a new Virtual API called 'my-api' with the title 'My API'

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
---
group: management
apiVersion: v1alpha1
kind: VirtualService
name: my-api
title: My API
metadata:
  scope:
    kind: VirtualAPI
    name: my-api
spec:
  route:
    - service:
        codec: AUTO 
        prefix: /v1
        protocol: http
        endpoints:
          - host: my.api.example.com
            port: 80
      operations:
        - id: my-api-get-users
          path: /users
          method: GET
  prefix: /v1
```
In this example:

* The codec is set to 'AUTO' meaning that the gateway will negotiate with the backend service over how to communicate. If the backend can communicate over HTTP2 that protocol will be used by the gateway, otherwise the gateway will revert to communicating over HTTP1.1
* 'my.api.example.com' is the backend host server and '80' is the port. The backend can be called over http protocol. 
* The prefix to call the backend service is '/v1'.
* The backend api supports 1 operation - GET /users.
* The prefix to call the Virtual API through the gateway is '/v1'.


### Via UI

It is not possible to create a Virtual API with no OAS specification via the UI. 


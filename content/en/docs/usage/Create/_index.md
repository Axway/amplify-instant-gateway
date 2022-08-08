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

## Creating a Virtual API with an OAS specification

If the API you wish to virtualize in Amplify has an OAS specification, you can provide that specification when creating the Virtual API.

### Via Amplify Management APIs

To create a virtual API via API call the following end-point

    POST /management/v1alpha1/virtualapis

If the followng json payload is provided then a new Virtual API called 'music' will be created with a title 'Musical Instruments'

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "VirtualAPI",
    "name": "music",
    "title": "Musical Instruments",
    "tags": [
        "music",
        "axway"
    ],
    "spec": {
        "api": {
            "oas": {
                "info": {
                    "title": "MusicalInstrumentsAPI",
                    "version": "2.0.4",
                    "description": "This is a sample Musical Instruments API."
                },
                "paths": {
                    "/instruments": {
                        "get": {
                            "tags": [
                                "instruments"
                            ],
                            "responses": {
                                "200": {
                                    "content": {
                                        "application/json": {
                                            "schema": {
                                                "type": "array",
                                                "items": {
                                                    "$ref": "#/components/schemas/instruments"
                                                }
                                            }
                                        }
                                    },
                                    "description": "The find all succeeded, and the results are available."
                                },
                                "401": {
                                    "content": {},
                                    "description": "This request requires user authentication, as configured by the server."
                                },
                                "404": {
                                    "content": {},
                                    "description": "No results were found."
                                },
                                "500": {
                                    "content": {},
                                    "description": "Something went wrong during the request; check out the logs on your server."
                                }
                            },
                            "description": "Retrieve all instruments",
                            "operationId": "FindInstruments"
                        }
                    },
                    "/instruments/{id}": {
                        "get": {
                            "tags": [
                                "instruments"
                            ],
                            "responses": {
                                "200": {
                                    "content": {
                                        "application/json": {
                                            "schema": {
                                                "$ref": "#/components/schemas/instruments"
                                            }
                                        }
                                    },
                                    "description": "The find succeeded, and the results are available."
                                },
                                "400": {
                                    "content": {},
                                    "description": "Bad request."
                                },
                                "401": {
                                    "content": {},
                                    "description": "This request requires user authentication, as configured by the server."
                                },
                                "404": {
                                    "content": {},
                                    "description": "No results were found."
                                },
                                "500": {
                                    "content": {},
                                    "description": "Something went wrong during the request; check out the logs on your server."
                                }
                            },
                            "parameters": [
                                {
                                    "in": "path",
                                    "name": "id",
                                    "schema": {
                                        "type": "string"
                                    },
                                    "required": true,
                                    "description": "The instrument ID"
                                }
                            ],
                            "description": "Find instrument by ID",
                            "operationId": "FindInstrumentByID"
                        }
                    },
                    "/instruments/query": {
                        "get": {
                            "tags": [
                                "instruments"
                            ],
                            "responses": {
                                "200": {
                                    "content": {
                                        "application/json": {
                                            "schema": {
                                                "type": "array",
                                                "items": {
                                                    "$ref": "#/components/schemas/instruments"
                                                }
                                            }
                                        }
                                    },
                                    "description": "The query succeeded, and the results are available."
                                },
                                "400": {
                                    "content": {},
                                    "description": "Bad request."
                                },
                                "401": {
                                    "content": {},
                                    "description": "This request requires user authentication, as configured by the server."
                                },
                                "404": {
                                    "content": {},
                                    "description": "No results were found."
                                },
                                "500": {
                                    "content": {},
                                    "description": "Something went wrong during the request; check out the logs on your server."
                                }
                            },
                            "parameters": [
                                {
                                    "in": "query",
                                    "name": "limit",
                                    "schema": {
                                        "type": "number",
                                        "default": 10
                                    },
                                    "description": "The number of records to fetch. The value must be greater than 0, and no greater than 1000."
                                },
                                {
                                    "in": "query",
                                    "name": "skip",
                                    "schema": {
                                        "type": "number",
                                        "default": 0
                                    },
                                    "description": "The number of records to skip. The value must not be less than 0."
                                },
                                {
                                    "in": "query",
                                    "name": "where",
                                    "schema": {
                                        "type": "string",
                                        "format": "json"
                                    },
                                    "description": "Constrains values for fields. The value should be encoded JSON."
                                },
                                {
                                    "in": "query",
                                    "name": "order",
                                    "schema": {
                                        "type": "string",
                                        "format": "json"
                                    },
                                    "description": "A dictionary of one or more fields specifying sorting of results. In general, you can sort based on any predefined field that you can query using the where operator, as well as on custom fields. The value should be encoded JSON."
                                },
                                {
                                    "in": "query",
                                    "name": "sel",
                                    "schema": {
                                        "type": "string",
                                        "format": "json"
                                    },
                                    "description": "Selects which fields to return from the query. Others are excluded. The value should be encoded JSON."
                                },
                                {
                                    "in": "query",
                                    "name": "unsel",
                                    "schema": {
                                        "type": "string",
                                        "format": "json"
                                    },
                                    "description": "Selects which fields to not return from the query. Others are included. The value should be encoded JSON."
                                },
                                {
                                    "in": "query",
                                    "name": "page",
                                    "schema": {
                                        "type": "number",
                                        "default": 1
                                    },
                                    "description": "Request page number starting from 1."
                                },
                                {
                                    "in": "query",
                                    "name": "per_page",
                                    "schema": {
                                        "type": "number",
                                        "default": 10
                                    },
                                    "description": "Number of results per page."
                                }
                            ],
                            "description": "Query instrument(s)",
                            "operationId": "QueryInstrument"
                        }
                    }
                },
                "openapi": "3.0.1",
                "servers": [
                    {
                        "url": "https://musicalinstruments.axway.com/music/v2"
                    }
                ],
                "components": {
                    "schemas": {
                        "ErrorModel": {
                            "type": "object",
                            "required": [
                                "code",
                                "message",
                                "request-id",
                                "success"
                            ],
                            "properties": {
                                "url": {
                                    "type": "string"
                                },
                                "code": {
                                    "type": "integer",
                                    "format": "int32"
                                },
                                "message": {
                                    "type": "string"
                                },
                                "success": {
                                    "type": "boolean",
                                    "default": false
                                },
                                "request-id": {
                                    "type": "string"
                                }
                            }
                        },
                        "instruments": {
                            "type": "object",
                            "properties": {
                                "type": {
                                    "type": "string",
                                    "description": "The type of instrument."
                                },
                                "price": {
                                    "type": "number",
                                    "description": "The price of the instrument."
                                },
                                "currency": {
                                    "type": "string",
                                    "description": "The price currency."
                                }
                            }
                        },
                        "ResponseModel": {
                            "type": "object",
                            "required": [
                                "request-id",
                                "success"
                            ],
                            "properties": {
                                "url": {
                                    "type": "string"
                                },
                                "code": {
                                    "type": "integer",
                                    "format": "int32"
                                },
                                "message": {
                                    "type": "string"
                                },
                                "success": {
                                    "type": "boolean"
                                },
                                "request-id": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "type": "REST",
        "description": "test"
    }
}
```

In this example, the specification:

* Indicates that the Virtual API being created is a REST API.
* Provides a short description of what the API does.
* Provides the OAS 3 specification for the API. This specifcation will be parsed to create the paths that the gateway can route to. It will also be used to create a specification to be consumed by client application developers when the API is exposed via Amplify Gateway.

### Via Axway CLI

To create a virtual API via CLI, log on the Axway Central CLI and run the following command

```markdown
axway central create -f music.yaml
```

A yaml file called music.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will create a new Virtual API called 'music' with the title 'Musical Instruments'

```yaml
apiVersion: v1alpha1
group: management
kind: VirtualAPI
name: music
title: Axway Musical Instruments
attributes:
  version: 1.0.0
tags:
  - music
  - axway
spec:
  description: A Sample Musical Instruments API
  type: REST
  api:
    oas:
        openapi: 3.0.1
        info:
          title: MusicalInstrumentsAPI
          description: This is a sample Musical Instruments API.
          version: 2.0.4
        servers:
        - url: https://musicalinstruments.axway.com/music/v2
        paths:
          /instruments/{id}:
            get:
              tags:
              - instruments
              description: Find instrument by ID
              operationId: FindInstrumentByID
              parameters:
              - name: id
                in: path
                description: The instrument ID
                required: true
                schema:
                  type: string
              responses:
                200:
                  description: The find succeeded, and the results are available.
                  content:
                    application/json:
                      schema:
                        $ref: '#/components/schemas/instruments'
                400:
                  description: Bad request.
                  content: {}
                401:
                  description: This request requires user authentication, as configured by
                    the server.
                  content: {}
                404:
                  description: No results were found.
                  content: {}
                500:
                  description: Something went wrong during the request; check out the logs
                    on your server.
                  content: {}
          /instruments:
            get:
              tags:
              - instruments
              description: Retrieve all instruments
              operationId: FindInstruments
              responses:
                200:
                  description: The find all succeeded, and the results are available.
                  content:
                    application/json:
                      schema:
                        type: array
                        items:
                          $ref: '#/components/schemas/instruments'
                401:
                  description: This request requires user authentication, as configured by
                    the server.
                  content: {}
                404:
                  description: No results were found.
                  content: {}
                500:
                  description: Something went wrong during the request; check out the logs
                    on your server.
                  content: {}
          /instruments/query:
            get:
              tags:
              - instruments
              description: Query instrument(s)
              operationId: QueryInstrument
              parameters:
              - name: limit
                in: query
                description: The number of records to fetch. The value must be greater than
                  0, and no greater than 1000.
                schema:
                  type: number
                  default: 10.0
              - name: skip
                in: query
                description: The number of records to skip. The value must not be less than
                  0.
                schema:
                  type: number
                  default: 0.0
              - name: where
                in: query
                description: Constrains values for fields. The value should be encoded JSON.
                schema:
                  type: string
                  format: json
              - name: order
                in: query
                description: A dictionary of one or more fields specifying sorting of results.
                  In general, you can sort based on any predefined field that you can query
                  using the where operator, as well as on custom fields. The value should
                  be encoded JSON.
                schema:
                  type: string
                  format: json
              - name: sel
                in: query
                description: Selects which fields to return from the query. Others are excluded.
                  The value should be encoded JSON.
                schema:
                  type: string
                  format: json
              - name: unsel
                in: query
                description: Selects which fields to not return from the query. Others are
                  included. The value should be encoded JSON.
                schema:
                  type: string
                  format: json
              - name: page
                in: query
                description: Request page number starting from 1.
                schema:
                  type: number
                  default: 1.0
              - name: per_page
                in: query
                description: Number of results per page.
                schema:
                  type: number
                  default: 10.0
              responses:
                200:
                  description: The query succeeded, and the results are available.
                  content:
                    application/json:
                      schema:
                        type: array
                        items:
                          $ref: '#/components/schemas/instruments'
                400:
                  description: Bad request.
                  content: {}
                401:
                  description: This request requires user authentication, as configured by
                    the server.
                  content: {}
                404:
                  description: No results were found.
                  content: {}
                500:
                  description: Something went wrong during the request; check out the logs
                    on your server.
                  content: {}
        components:
          schemas:
            ResponseModel:
              required:
              - request-id
              - success
              type: object
              properties:
                code:
                  type: integer
                  format: int32
                success:
                  type: boolean
                request-id:
                  type: string
                message:
                  type: string
                url:
                  type: string
            ErrorModel:
              required:
              - code
              - message
              - request-id
              - success
              type: object
              properties:
                code:
                  type: integer
                  format: int32
                success:
                  type: boolean
                  default: false
                request-id:
                  type: string
                message:
                  type: string
                url:
                  type: string
            instruments:
              type: object
              properties:
                type:
                  type: string
                  description: The type of instrument.
                price:
                  type: number
                  description: The price of the instrument.
                currency:
                  type: string
                  description: The price currency.
```

In this example, the specification:

* Indicates that the Virtual API being created is a REST API.
* Provides a short description of what the API does.
* Provides the OAS 3 specification for the API. This specifcation will be parsed to create the paths that the gateway can route to. It will also be used to create a specification to be consumed by client application developers when the API is exposed via Amplify Gateway.

### Via UI

Login to the platfrom, navigate to Central, select 'Virtual APIs' from the left hand navigation window. Click 'Add Virtual API'

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
  "name": "music",
  "title": "Musical Instruments",
  "tags": [
    "music",
    "axway"
  ],
  "spec": {
    "type": "REST",
    "description": "A Sample Musical Instruments API"
  }
}
```

However, this virutal API cannot be used as is. When an OAS specification is provided, then the path routes are parsed directly from the OAS specification. Where no OAS specification is provided, this information needs to be provided by calling another API to create another resource called a Virtual Service. 

    POST /apis/management/v1alpha1/virtualapis/{virtualapiName}/virtualservices

where {virtualapiName} is the name of the api created by calling the previous end-point. The following json payload should be included

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "VirtualService",
    "name": "music",
    "title": "music",
    "spec": {
        "route": [
            {
                "service": {
                    "codec": "AUTO",
                    "prefix": "/music/v2",
                    "protocol": "https",
                    "endpoints": [
                        {
                            "host": "musicalinstruments.axway.com",
                            "port": 443
                        }
                    ]
                },
                "operations": [
                    {
                        "id": "FindInstruments",
                        "path": "/instruments",
                        "method": "GET"
                    },
                    {
                        "id": "FindInstrumentByID",
                        "path": "/instruments/{id}",
                        "method": "GET"
                    },
                    {
                        "id": "QueryInstrument",
                        "path": "/instruments/query",
                        "method": "GET"
                    }
                ]
            }
        ],
        "prefix": "/music/v2"
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
name: music
title: Musical Instruments
tags:
  - music
  - axway
spec:
  type: REST
  description: A Sample Musical Instruments API
---
group: management
apiVersion: v1alpha1
kind: VirtualService
name: music
title: Musical Instruments
spec:
  route:
    - service:
        codec: AUTO
        prefix: /music/v2
        protocol: https
        endpoints:
          - host: musicalinstruments.axway.com
            port: 443
      operations:
        - id: FindInstruments
          path: /instruments
          method: GET
        - id: FindInstrumentByID
          path: /instruments/{id}
          method: GET
        - id: QueryInstrument
          path: /instruments/query
          method: GET
  prefix: /music/v2
```

In this example:

* The codec is set to 'AUTO' meaning that the gateway will negotiate with the backend service over how to communicate. If the backend can communicate over HTTP2 that protocol will be used by the gateway, otherwise the gateway will revert to communicating over HTTP1.1
* 'musicalinstruments.axway.com' is the backend host server and '443' is the port. The backend can be called over https protocol. 
* The prefix to call the backend service is '/music/v2'.
* The backend api supports 3 operations -
  * GET /instruments
  * GET /instruments/{id}
  * GET /instruments/query
* The prefix to call the Virtual API through the gateway is 'music/v2'.

### Via UI

It is not possible to create a Virtual API with no OAS specification via the UI.

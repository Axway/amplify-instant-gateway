---
title: Create a Virtual API with an OAS specification
linkTitle: With an OAS 3.x specification
weight: 10
date: 2021-07-26
hide_readingtime: false
description: >
  Create a Virtual API with an OAS specification.
---

If the API you wish to virtualize in Amplify has an OAS 3.x specification, you can provide that specification when creating the Virtual API.

## Via Amplify Management APIs

To create a virtual API via API call the following end-point

```markdown
POST /management/v1alpha1/virtualapis
```

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

## Via Axway CLI

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

## Via UI

Login to the platfrom, navigate to Central, select 'Virtual APIs' from the left hand navigation window. Click 'Add Virtual API'

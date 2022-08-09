---
title: Create a Virtual API without API Specification
linkTitle: With no API Specification
weight: 20
date: 2021-07-26
hide_readingtime: false
description: >
  Create a Virtual API with no API Specification.
---

If the backend API you wish to virtualize in Amplify Instant Gateway does not have an API specification, you can still create the Virtual API routes manually.

## Via Amplify Management APIs

To create a virtual API via API call the following end-point

```markdown
POST /management/v1alpha1/virtualapis
```

If the followng json payload is provided then a new Virtual API called 'my-api' will be created with a title 'My API'

```json
{
  "group": "management",
  "apiVersion": "v1alpha1",
  "kind": "VirtualAPI",
  "name": "my-api",
  "title": "My API",
  "tags": [
    "axway"
  ],
  "spec": {
    "type": "REST",
    "description": "An example of a virtual API with no API specification"
  }
}
```

However, this virutal API cannot be used as is. When an API specification is provided, then the  routes are parsed directly from the specification. Where no  specification is provided, this information needs to be provided by calling another API to create another resource called a Virtual Service.

```markdown
POST /apis/management/v1alpha1/virtualapis/{virtualapiName}/virtualservices
```

A json payload which reflects the shape of the backend API being called should be included

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "VirtualService",
    "name": "my-api",
    "title": "My API",
    "spec": {
        "route": [
            {
                "service": {
                    "codec": "AUTO",
                    "prefix": "/myapi",
                    "protocol": "https",
                    "endpoints": [
                        {
                            "host": "myapi.axway.com",
                            "port": 443
                        }
                    ]
                },
                "operations": [
                    {
                        "id": "users",
                        "path": "/users",
                        "method": "GET"
                    }
                ]
            }
        ],
        "prefix": "/myapi"
    }
}
```

In this example:

* The backend host server is configured as 'myapi.axway.com' and '443' is the port. The backend can be called over https protocol.
* The prefix to call the backend service is '/myapi'.
* The backend api supports 1 operation - GET /users.
* The prefix to call the Virtual API through the gateway is '/myapi'.

## Via Axway CLI

To create a virtual API via CLI, log on the Axway Central CLI and run the following command

```markdown
axway central create -f my-api.yaml
```

A yaml file called my-api.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will create a new Virtual API called 'my-api' with the title 'My API' and a virtual service that contains information about the routes.

```yaml
group: management
apiVersion: v1alpha1
kind: VirtualAPI
name: my-api
title: My API
tags:
  - axway
spec:
  type: REST
  description: An example of a virtual API with no API specification
---
group: management
apiVersion: v1alpha1
kind: VirtualService
name: my-api
title: My API
spec:
  route:
    - service:
        codec: AUTO
        prefix: /myapi
        protocol: https
        endpoints:
          - host: myapi.axway.com
            port: 443
      operations:
        - id: users
          path: /users
          method: GET
  prefix: /myapi
```

In this example:

* The backend host server is configured as 'myapi.axway.com' and '443' is the port. The backend can be called over https protocol.
* The prefix to call the backend service is '/myapi'.
* The backend api supports 1 operation - GET /users.
* The prefix to call the Virtual API through the gateway is '/myapi'.

## Via UI

It is not possible to create a Virtual API with no API specification via the UI.

---
title: Apply Governance Rules
linkTitle: Apply Governance Rules
weight: 20
date: 2021-07-26
hide_readingtime: false
description: >
  Apply Governance rules to a Virtual API.
---

Governance rules change the behavior of the API when deployed via Amplify Gateway. For example, security, CORS and timeouts can be configured for APIs which are deployed to an Amplify Gateway dataplane.

## Prerequisites

* You have created a Virtual API as outlined in [Create Virtual API](/docs/usage/create/index.html).

## Configuring HTTP/2

By default, Amplify Gateway will negotiate how to communicate with the backend api. If the backend api can support HTTP/2 then Amplify Gateway will communicate with the backend over HTTP/2. If the backend can only support HTTP/1.1 then all communication will take place over HTTP/1.1. It is possible to change this default configuration to specify that Amplify Gateway should communicate with the backend either exclusively over HTTP/1.1 or HTTP/2. You may wish to do this if you know the capabilities of the backend and do not wish to incur the overhead of hte negotiation. Backends which do not support TLS cannot use HTTP/2.

### Via the Amplify Management APIs

To configure HTTP/2 setting via API call the following end-point

    PUT /management/v1alpha1/virtualapis/{virtualapiName}/virtualservices/{virtualserviceName}

where {virtualapiName} is the name of the Virtual API you wish to configure and {virtualserviceName} is the name of its corresponding virtual service. If the following json payload is provided then the virtual service will be updated to communicate exclusively over HTTP/2. 

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
                    "codec": "HTTP2",
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
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstruments",
                        "path": "/instruments",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstrumentByID",
                        "path": "/instruments/{id}",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 QueryInstrument",
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

In this example, the specification:

* Has been updated to specify a codec of HTTP2 as opposed to AUTO (the default).
* All other values in the Virtual Service are unaltered. 

To test HTTP settings, release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)

### Via Axway CLI

To configure HTTP/2 setting via CLI, , log on the Axway Central CLI and run the following command

    axway central apply -f music-updateHTTPSettings.yaml

A yaml file called music-updateHTTPSettings.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will update the virtual service to communicate exclusively over HTTP/2.

```yaml
group: management
apiVersion: v1alpha1
kind: VirtualService
name: music
title: music
metadata:
  scope:
    kind: VirtualAPI
    name: music
spec:
  route:
    - service:
        codec: HTTP2
        prefix: /music/v2
        protocol: https
        endpoints:
          - host: musicalinstruments.axway.com
            port: 443
      operations:
        - id: MusicalInstrumentsAPI-2.0.4 FindInstruments
          path: /instruments
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 FindInstrumentByID
          path: /instruments/{id}
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 QueryInstrument
          path: /instruments/query
          method: GET
  prefix: /music/v2
```
In this example, the specification:

* Has been updated to specify a codec of HTTP2 as opposed to AUTO (the default).
* All other values in the Virtual Service are unaltered. 

To test HTTP settings, release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)

### Via UI

HTTP Settings cannot be configured via the UI currently.

## Configuring a backend connection timeout

By default, Amplify Gateway will wait 10 seconds to establish a connection to a backend api. This includes the time to do the TLS handshake if the backend connection supports TLS. You may wish to increase or decrease this timeout as appropriate.

### Via the Amplify Management APIs

To configure a backend connection timeout via API, call the following end-point

    PUT /management/v1alpha1/virtualapis/{virtualapiName}/virtualservices/{virtualserviceName}

where {virtualapiName} is the name of the Virtual API you wish to configure and {virtualserviceName} is the name of its corresponding virtual service. If the following json payload is provided then the virtual service will be updated to wait 12 seconds to establish a connection to the backend. 

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
                    "codec": "HTTP2",
                    "connectTimeout": 12,
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
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstruments",
                        "path": "/instruments",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstrumentByID",
                        "path": "/instruments/{id}",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 QueryInstrument",
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

In this example, the specification:

* Has been updated to specify a connection timeout of 12 seconds.
* All other values in the Virtual Service are unaltered. 

To test backend connection timeout, release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)

### Via Axway CLI

To configure a backend connection timeout via CLI, log on the Axway Central CLI and run the following command

    axway central apply -f music-updateConnectionTimeout.yaml

A yaml file called music-updateConnectionTimeout.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will update the virtual service to wait 12 seconds to establish a connection to the backend.

```yaml
group: management
apiVersion: v1alpha1
kind: VirtualService
name: music
title: music
metadata:
  scope:
    kind: VirtualAPI
    name: music
spec:
  route:
    - service:
        codec: HTTP2
        connectTimeout: 12
        prefix: /music/v2
        protocol: https
        endpoints:
          - host: musicalinstruments.axway.com
            port: 443
      operations:
        - id: MusicalInstrumentsAPI-2.0.4 FindInstruments
          path: /instruments
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 FindInstrumentByID
          path: /instruments/{id}
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 QueryInstrument
          path: /instruments/query
          method: GET
  prefix: /music/v2
```
In this example, the specification:

* Has been updated to specify a connection timeout of 12 seconds.
* All other values in the Virtual Service are unaltered. 

To test backend connection timeout, release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)

### Via UI

Backend connection timeout cannot be configured via the UI currently.

## Configuring Security

Amplify Gateway provides several mechanisms to secure your API traffic. API Traffice may be secured as follows:

* Passthrough - Delegate all security to the backend api. API consumers must provide the authentication mechanism mandated by the backend api. For example, if the backend api is secured via an API key then the api consumer must include that API Key when making the API call. Amplify Gateway will proxy the request to the backend api but will not enforce any security itself. 
* API Key - Amplify Gateway will require the api consumer to include an API Key when calling the API and will validate that API key at runtime. If the key is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the api key is not present or invalid the api request will be rejected by Amplify Gateway. 
* OAuth - Amplify Gateway will require the api consumer to include an OAuth token when calling the API and will validate that OAuth token at runtime. If the token is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the token is not present or invalid the api request will be rejected by Amplify Gateway. Please Note: Amplify Gateway only supports OAuth servers that issue tokens in JWT format. Opaque tokens are not currently supported. 
* JWT Bearer Token - Amplify Gateway will require the api consumer to include an OAuth token when calling the API and will validate that OAuth token at runtime. If the token is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the token is not present or invalid the api request will be rejected by Amplify Gateway.

## Configuring API Key Auth Rule

To specify that the Virtual API is to be secured via API key, the user must configure an API Key Auth rule and update the Virtual Service to use that Auth Rule.

### Via the Amplify Management APIs

To configure an API Key auth rule via API, call the following end-point

    POST /management/v1alpha1/virtualapis/{virtualapiName}/apikeyauthrules

where {virtualapiName} is the name of the Virtual API you wish to configure the API Key auth rule for. If the following json payload is provided then an API Key Auth rule will be created. 

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "APIKeyAuthRule",
    "name": "music-apikeyrule",
    "title": "Music - API Key Auth Rule",
    "spec": {
        "name": "my-api-key",
        "description": "API key will be found in a header called 'my-api-key'"
    }
}
```
In this example, the specification:

* Indicates that the api key will be located in a header named 'my-api-key'. Please Note: Amplify Gateway only supports providing API Keys as a header at the moment.
* A Description of the API Key auth rule. 

Now the Virtual Service needs to be updated to use the API Key via the following API call

    PUT /management/v1alpha1/virtualapis/{virtualapiName}/virtualservice/{virtualserviceName}

where {virtualapiName} is the name of the Virtual API and {virtualserviceName} is the name of the corresponding virtual service. If the following json payload is provided then the Virtual Service will be updated to use the API Key auth rule created previously.

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "VirtualService",
    "name": "music",
    "title": "music",
    "spec": {
        "auth": {
            "kind": "APIKeyAuthRule",
            "name": "music-apikeyrule"
        },
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
                    ],
                    "connectTimeout": 12
                },
                "operations": [
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstruments",
                        "path": "/instruments",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 FindInstrumentByID",
                        "path": "/instruments/{id}",
                        "method": "GET"
                    },
                    {
                        "id": "MusicalInstrumentsAPI-2.0.4 QueryInstrument",
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

To test API Key Auth rule

1. Release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)
2. Subscribe to the API in order to be able to consume it. This process will issue an API Key to the consumer. 

### Via Axway CLI

To configure an API Key auth rule via CLI, log on the Axway Central CLI and run the following command

    axway central create -f music-apiKeyAuthRule.yaml

A yaml file called music-apiKeyAuthRule.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will create the API Key rule.

```yaml
apiVersion: v1alpha1
group: management
kind: APIKeyAuthRule
name: music-apikeyrule
title: Music - API Key Auth Rule
metadata:
  scope:
    kind: VirtualAPI
    name: music
spec:
  description: API key will be found in a header called 'my-api-key'
  name: my-api-key
```
In this example, the specification:

* Indicates that the api key will be located in a header named 'my-api-key'. Please Note: Amplify Gateway only supports providing API Keys as a header at the moment.
* A Description of the API Key auth rule. 

Now the Virtual Service needs to be updated to use the API Key via the following CLI command

    axway central apply -f music-updateWithApiKeyAuthRule.yaml

A yaml file called music-updateWithApiKeyAuthRule.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will update the Virtual Service to use the API Auth rule created previously.

```yaml
group: management
apiVersion: v1alpha1
kind: VirtualService
name: music
title: music
metadata:
  scope:
    kind: VirtualAPI
    name: music
spec:
  auth:
    kind: APIKeyAuthRule
    name: music-apikeyrule
  route:
    - service:
        codec: AUTO
        prefix: /music/v2
        protocol: https
        endpoints:
          - host: musicalinstruments.axway.com
            port: 443
        connectTimeout: 12
      operations:
        - id: MusicalInstrumentsAPI-2.0.4 FindInstruments
          path: /instruments
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 FindInstrumentByID
          path: /instruments/{id}
          method: GET
        - id: MusicalInstrumentsAPI-2.0.4 QueryInstrument
          path: /instruments/query
          method: GET
  prefix: /music/v2
```

To test API Key Auth rule

1. Release and deploy the Virtual API as outlined in [Release Tag a Virtual API](/docs/usage/ReleaseTag/index.html) and [Deploy a Virtual API](/docs/usage/Deploy/index.html)
2. Subscribe to the API in order to be able to consume it. This process will issue an API Key to the consumer. 

### Via UI

API Key Auth rule cannot be configured via the UI currently.

## Configuring OAuth Auth Rule

To specify that the Virtual API is to be secured via OAuth, the user must configure an OAuth Auth rule and update the Virtual Service to use that Auth Rule.

### Via the Amplify Management APIs

### Via Axway CLI

### Via UI

OAuth auth rule cannot be configured via the UI currently.

## Configuring OAuth Auth Rule

To specify that the Virtual API is to be secured via JWT Bearer Token, the user must configure a JWT Auth rule and update the Virtual Service to use that Auth Rule.

### Via the Amplify Management APIs

### Via Axway CLI

### Via UI

JWT auth rule cannot be configured via the UI currently.
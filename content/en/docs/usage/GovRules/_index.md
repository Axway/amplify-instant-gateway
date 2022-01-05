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

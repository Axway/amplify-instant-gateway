---
title: Configuring Timeouts
linkTitle: Configuring Timeouts
weight: 20
date: 2022-01-06
hide_readingtime: false
description: >
  Configuring Timeouts for a Virtual API.
---

By default, Amplify Gateway will wait 10 seconds to establish a connection to a backend api. This includes the time to do the TLS handshake if the backend connection supports TLS. You may wish to increase or decrease this timeout as appropriate.

## Prerequisites

* You have created a Virtual API as outlined in [Create Virtual API](/docs/usage/create/index.html).

## Via the Amplify Management APIs

To configure a backend connection timeout via API, call the following end-point

```markdown
PUT /management/v1alpha1/virtualapis/{virtualapiName}/virtualservices/{virtualserviceName}
```

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

## Via Axway CLI

To configure a backend connection timeout via CLI, log on the Axway Central CLI and run the following command

```markdown
axway central apply -f music-updateConnectionTimeout.yaml
```

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

## Via UI

Backend connection timeout cannot be configured via the UI currently.

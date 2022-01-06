---
title: Configuring API Key Security
linkTitle: Configuring API Key Security
weight: 40
date: 2022-01-06
hide_readingtime: false
description: >
  Configure API Key Security for a Virtual API.
---

To specify that the Virtual API is to be secured via API key, the user must configure an API Key Auth rule and update the Virtual Service to use that Auth Rule.

## Prerequisites

* You have created a Virtual API as outlined in [Create Virtual API](/docs/usage/create/index.html).

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
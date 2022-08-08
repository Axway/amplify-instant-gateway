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

## Configuring Security

Amplify Gateway provides several mechanisms to secure your API traffic. API Traffic may be secured as follows:

* Passthrough - Delegate all security to the backend api. API consumers must provide the authentication mechanism mandated by the backend api. For example, if the backend api is secured via an API key then the api consumer must include that API Key when making the API call. Amplify Gateway will proxy the request to the backend api but will not enforce any security itself.
* API Key - Amplify Gateway will require the api consumer to include an API Key when calling the API and will validate that API key at runtime. If the key is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the api key is not present or invalid the api request will be rejected by Amplify Gateway.
* OAuth - Amplify Gateway will require the api consumer to include an OAuth token when calling the API and will validate that OAuth token at runtime. If the token is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the token is not present or invalid the api request will be rejected by Amplify Gateway. Please Note: Amplify Gateway only supports OAuth servers that issue tokens in JWT format. Opaque tokens are not currently supported.
* JWT Bearer Token - Amplify Gateway will require the api consumer to include an OAuth token when calling the API and will validate that OAuth token at runtime. If the token is valid for the consumer, then Amplify Gateway will proxy the request on to the backend api. If the token is not present or invalid the api request will be rejected by Amplify Gateway.

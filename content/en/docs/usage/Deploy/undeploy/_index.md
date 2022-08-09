---
title: Deploy a Virtual API to Amplify Gateway
linkTitle: Deploy a Virtual API to Amplify Gateway
weight: 30
date: 2021-07-26
hide_readingtime: false
description: >
  Deploy a Virtual API to an Amplify Gateway environment.
---

It is possible to undeploy or remove a Virtual API Release from running in an Amplify Gateway environment. Once the API is undeployed it can no longer be called in that environment.

## Via the Amplify Management APIs

To undeploy or remove a Virtual API Release that is currently running on an Amplify Gateway Environment via API, call the following end-point

```markdown
DELETE /management/v1alpha1/environments/{environmentName}/deployments/{deploymentName}
```

where {environmentName} is the name of the Amplify Gateway dataplane environment that the Virtual API is currently running on and {deploymentName} is the name of the deployment.

The above API call will only request that the API is undeployed from the data plane. While the request is waiting to be picked up by the data plane, the deployment status will be 'Pending'. Once it has been sucessfully undeployed from the data plane, the deployment resource will be removed and no longer show up in the list of deployments in the environment. To confirm the deployment status call the following API

```markdown
GET /management/v1alpha1/environments/{environmentName}/deployments/{deploymentName}
```

## Via Axway CLI

To undeploy or remove a Virtual API Release that is currently running on an Amplify Gateway Environment via CLI, log on the Axway Central CLI and run the following command

```markdown
axway central delete deployments <deploymentName> -s <environmentName>
```

where 'environmentName' is the name of the Amplify Gateway dataplane environment that the Virtual API is currently running on and 'deploymentName' is the name of the deployment.

The above CLI command will only request that the API is undeployed from the data plane. While the request is waiting to be picked up by the data plane, the deployment status will be 'Pending'. Once it has been sucessfully undeployed from the data plane, the deployment resource will be removed and no longer show up in the list of deployments in the environment. To confirm the deployment status use the following CLI command

```markdown
axway central get deployments -s <environmentName>   
```

## Via UI

It is currently not possible to undeploy a Virtual API via the UI.

---
title: Deploy a Virtual API to Amplify Gateway
linkTitle: Deploy a Virtual API to Amplify Gateway
weight: 30
date: 2021-07-26
hide_readingtime: false
description: >
  Deploy a Virtual API to an Amplify Gateway environment.
---

A Virtual API Release can be deployed to a specified Amplify Gateway environment so that the virtual api can be called via the Amplify Gateway dataplane running in that environment.

## Via the Amplify Management APIs

To deploy a Virtual API release to an Amplify Gateway Environment via API call the following end-point

```markdown
POST /management/v1alpha1/environments/{environmentName}/deployments
```

where {environmentName} is the name of the Amplify Gateway dataplane environment that you are deploying to. If the followng json payload is provided then provided then this will trigger a request to the data plane to deploy the Virtual API release.

```json
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "Deployment",
    "name": "music-deploy",
    "title": "Music - Deployment of release 0.1.0",
    "spec": {
        "virtualHost": "music.ampgw.com",
        "virtualAPIRelease": "music-0.1.0"
    }
}
```

In this example, the specification:

* Specifies the Virtual API Release to deploy.
* Describes the virtual host to use to call the API. A virtual host is mandatory. The end-point to use to call the API depends on the environment to which it is deployed.

The above API call will only request that the API is deployed to the data plane. While the deployment is waiting to be picked up by the data plane, it's status will be 'Pending'. Once it has been deployed sucessfully to the data plane, it's status will become 'Success'. To confirm the deployment status call the following API

```markdown
GET /management/v1alpha1/environments/{environmentName}/deployments/{deploymentName}
```

## Via Axway CLI

To deploy a Virtual API release to an Amplify Gateway Environment via CLI, log on the Axway Central CLI and run the following command

```markdown
axway central create -f music-deployment.yaml
```

A yaml file called music-deployment.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will trigger a request to the data plane to deploy the Virtual API release to the specified environment.

```yaml
apiVersion: v1alpha1
group: management
kind: Deployment
name: music-deploy
title: Music - Deployment of release 0.1.0
metadata:
  scope:
    kind: Environment
    name: amplify-gateway-env
spec:
  virtualAPIRelease: music-0.1.0
  virtualHost: music.ampgw.com
```

In this example, the specification:

* Specifies the Virtual API Release to deploy.
* Describes the virtual host to use to call the API. A virtual host is mandatory. The end-point to use to call the API depends on the environment to which it is deployed.

The above CLI command will only request that the API is deployed to the data plane. While the deployment is waiting to be picked up by the data plane, it's status will be 'Pending'. Once it has been deployed sucessfully to the data plane, it's status will become 'Success'. To confirm the deployment status use the following CLI command

```markdown
axway central get deployments -s <environmentName> 
```

## Via UI

Login to the platfrom, navigate to Central, select 'Virtual APIs' from the left hand navigation window. Select the Virtual API to deploy from the list. Click the Deploy button and select the Virtual API Release to deploy from the list. Enter the Virtual host to call the Virtual API on.

Please Note: if no Virtual API releases exist, create one as described in  [Release Tag a Virtual API via UI](/docs/usage/ReleaseTag/index.html).

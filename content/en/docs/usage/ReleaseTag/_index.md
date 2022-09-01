---
title: Release Tag a Virtual API
linkTitle: Release Tag a Virtual API
weight: 20
date: 2021-07-26
hide_readingtime: false
description: >
  Release Tag a Virtual API to create a Virtual API Release.
---

A Release Tag is used to create a versioned snapshot of the virtual api (for example versiion 1.0.1) at a given point in time. This snapshot is known as a Virtual API Release. The Release Tag is used to manage the Virtual API Release throughout its lifecycle.

## Prerequisites

* You have created a Virtual API as outlined in [Create Virtual API](/docs/usage/Create/index.html).
* You have applied whatever governance rules you require as outlined in [Apply Governance Rules](/docs/usage/GovRules/index.html).

## Release Tag a Virtual API

### Via the Amplify Management APIs

To create a Release Tag via API call the following end-point

```none
POST /management/v1alpha1/virtualapis/{virtualapiName}/releasetags
```

where {virtualapiName} is the name of the virtual api. For example, music. If the followng json payload is provided then a new Release Tag called 'music-release-1-minor' will be created. This in turn will cause a new Virtual API Release named music-0.1.0 to be created. The Virtual API release created will depend on what Virtual API releases currently exist and the release type of the release tag. For example, if music-1.0.0 already exists then creating a new release tag with a release type of Major will create music-2.0.0.

```yaml
{
    "group": "management",
    "apiVersion": "v1alpha1",
    "kind": "ReleaseTag",
    "name": "music-release-1-minor",
    "title": "music - release 1 - v0.1.0",
    "spec": {
        "description": "First release of music API. Includes a CORS rule and an API Key auth rule",
        "releaseType": "minor"
    }
}
```

In this example, the specification:

* Describes what is included in the release tag.
* Specifies the Release Type. Allowable values include minor, major or patch.

To confirm a Virtual API Releae was created as a result of the creation of a release tag, call the following API

```none
GET /management/v1alpha1/virtualapireleases
```

### Via Axway CLI

To create a Release tag via CLI, log on the Axway Central CLI and run the following command

```none
axway central create -f music-release-1-minor.yaml
```

A yaml file called music-release-1-minor.yaml should exist on the file system with the following contents. Executing this command with the yaml file contents will cause a new Release Tag called 'music-release-1-minor' to be created. This in turn will cause a new Virtual API Release named music-0.1.0 to be created. The Virtual API release created will depend on what Virtual API releases currently exist and the release type of the release tag. For example, if music-1.0.0 already exists then creating a new release tag with a release type of Major will create music-2.0.0.

```yaml
apiVersion: v1alpha1
group: management
kind: ReleaseTag
name: music-release-1-minor
title: music - release 1 - v0.1.0
metadata:
  scope:
    kind: VirtualAPI
    name: music
spec:
  description: First release of music API. Includes a CORS rule and an API Key auth rule
  releaseType: minor
```

In this example, the specification:

* Describes what is included in the release tag.
* Specifies the Release Type. Allowable values include minor, major or patch.

To confirm a Virtual API Releae was created as a result of the creation of a release tag, use the the following CLI command

```none
axway central get virtualapireleases
```

### Via UI

Login to the platfrom, navigate to Central, select 'Virtual APIs' from the left hand navigation window. Select the virtual API to create the release tag for from the list. Click on the New Release button.

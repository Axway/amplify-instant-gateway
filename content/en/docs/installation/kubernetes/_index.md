---
title: Install Amplify Gateway into a Kubernetes Cluster
linkTitle: Kubernetes
weight: 10
date: 2022-01-06
hide_readingtime: true
description: Install Amplify Gateway into a Kubernetes Cluster.
---

## Prerequisites

* You have an organisation provisioned on the Amplify Platform.
* You have created a service account in your organisation
* You have installed Helm
* You have installed a Kubernetes Cluster. If running on a laptop, Docker Desktop, Minikube, Rancher Desktop or K3s may be used. If installing into a cloud based cluster, AWS EKS or Azure AKS may be used. 

## Set up the Helm repository

To create the Helm repository run the following commands

```
helm repo add ampc-rel https://artifactory-phx.ecd.axway.int/artifactory/ampc-helm-release
helm repo update
```

To seach for the latest releases in the helm repository, run the following command

```
helm search repo ampgw
```

## Install Amplify Gateway

### Create the Kubernetes cluster

### Create the required Kubernetes secret

Amplify Gateway requires a specific secret in order to run. This secret must be named 'ampgw-secret' and should contain the following:

* Your organization id
* Your service account client id in that organization
* The public and private keys of your service account.
* A TLS certificate and private key which can be used to create virtual hosts on which to your APIs on Amplify Gateway




```
kubectl -- create secret generic ampgw-secret \
    --from-file serviceAccPrivateKey=private_key.pem \
    --from-file serviceAccPublicKey=public_key.pem \
    --from-file listenerPrivateKey=listener_private_key.pem  \
    --from-file listenerCertificate=listener_certificate.pem \
    --from-literal orgId=929039818019890 \
    --from-literal clientId=<service account client id>
```

### Customizing the Amplify Gateway Installation

By default, if you run the helm install with no customization, AMplify Gateway will apply the following defaults:

* The Amplify Gateway environment on the platform will have a logical name of 'amplify-gateway-env'
* Data Plane listener port will be 4443
* Redaction will be turned on for the traceability data
* The secret containing the TLS cert is called 'ampgw-secret'

The following is an example of the custom override file which applies the following customizations:

* The Amplify Gateway Environment on the platform has a logical name of 'my-env'
* Data plane lister port is will be 5555
* Turn off redaction for traceability data
* Set log levels to debug for the agents and envoy
* Use a custom external secret name
* Add a list of virtual hosts to bootstrap

```yaml
global:
  environment: my-env
  listenerPort: 5555
  # Below is the default name for the ExternalSecret resource created on the platform. If you do not specify this value, the ExternalSecret resource will be created with the default secret name 
  # 'ampgw-secret'
  tlsExternalSecretName: ampgw-tls-external-secret
  virtualHosts:
    # You can have 0..more virtual hosts, they will only be created if you have the listenerPrivateKey and listenerCertificate in the ampgw-secret.
    # Each entry will create a VirtualHost resource in Central that links to the same ExternalSecret which points to ampgw-secret in the data plane.
    - name: virtualhost-1
      domain: pets.ampgw.com
    - name: virtualhost-2
      domain: music.ampgw.com
 
ampgw-governance-agent:
  env:
    LOG_LEVEL: debug
 
ampgw-traceability-agent:
  env:
    LOG_LEVEL: debug
    TRACEABILITY_REDACTION_PATH_SHOW: "[{keyMatch:\".*\"}]"
    TRACEABILITY_REDACTION_QUERYARGUMENT_SHOW: "[{keyMatch:\".*\"}]"
    TRACEABILITY_REDACTION_REQUESTHEADER_SHOW: "[{keyMatch:\".*\"}]"
    TRACEABILITY_REDACTION_RESPONSEHEADER_SHOW: "[{keyMatch:\".*\"}]"
 
ampgw-secret-provider-k8s:
  env:
    LOG_LEVEL: debug

ampgw-proxy:
  env:
    LOGLEVEL: debug
```

### Run the Helm install

The following command will install the latest released version of Amplify Gateway

```
helm install ampgw ampc-rel/ampgw
```

If you want to customize your install, create an override yaml file, as outlined in the previous section, and run the install as follows

```
helm install ampgw ampc-rel/ampgw -f override.yaml
```

### Check whats running

```
kubectl get pods
kubectl describe pods <pod-name>
```

### Check the logs

```
kubectl -- logs --follow <pod-name>
```



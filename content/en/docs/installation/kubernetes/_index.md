---
title: Install Amplify Gateway into a Kubernetes Cluster
linkTitle: Install into Kubernetes cluster
weight: 10
date: 2022-01-06
hide_readingtime: true
description: Install Amplify Gateway into a Kubernetes Cluster.
---

## Prerequisites

* You have an organisation provisioned on the Amplify Platform.
* You have installed Helm
* You have installed a Kubernetes Cluster. If running on a laptop, Docker Desktop, Minikube, Rancher Desktop or K3s may be used. If installing into a cloud based cluster, AWS EKS or Azure AKS may be used.

## Set up Service Accounts

Two service accounts will be required to install Amplify Instant Gatway:

* Service Account used by Helm install to download docker images. See [Creating Service Account for downloading docker images](/docs/installation/dockerservacct/index.html).
* Service Account used by the governance agent to communicate with the Amplify plaform. See [Creating Service Account for Governance Agent](/docs/installation/govagentservacct/index.html)

Service accounts may be set up via CLI or UI

## Create the required Kubernetes secret

Amplify Gateway requires a specific secret in order to run. This secret must be named 'ampgw-secret' and should contain the following:

* Your organization id
* The client id of the service account used by the governance agent to communicate with the Amplify Platform
* The public and private keys of this service account
* A TLS certificate and private key which can be used to create virtual hosts on which to your APIs on Amplify Gateway

```markdown
kubectl -- create secret generic ampgw-secret \
    --from-file serviceAccPrivateKey=private_key.pem \
    --from-file serviceAccPublicKey=public_key.pem \
    --from-file listenerPrivateKey=listener_private_key.pem  \
    --from-file listenerCertificate=listener_certificate.pem \
    --from-literal orgId=<ORG_ID> \
    --from-literal clientId=<SERVICE_ACCOUNT_CLIENT_ID>
```

## Create the Kubernetes secret to allow access to Docker images

Helm requires access to the docker repository in order to download docker images. To enable this access you must create another kubernetes secret. This secret must be named 'regcred' and should contain the following:

* Your organization id
* The client id of the service account used to access the docker repository
* The client secret of this service account.

```markdown
kubectl create secret docker-registry regcred --docker-server=docker.repository.axway.com --docker-username=<SERVICE_ACCOUNT_CLIENT_ID> --docker-password=<CLIENT_SECRET>
```

## Set up the Helm repository

To configure the helm repository run the following command. The service account client id and client secret to use are the ones created in the [Creating Service Account for downloading docker images](/docs/installation/setupDockerRepoServAcct/index.html) section.

```markdown
helm repo add Axway https://helm.repository.axway.com --username=<SERVICE_ACCOUNT_CLIENT_ID> --password=<CLIENT_SECRET>
```

Update the repo index to make sure you have access to the latest charts

```markdown
helm repo update
```

To seach for the latest releases in the helm repository, run the following command

```markdown
helm search repo ampgw
```

## Install Amplify Gateway

## Customizing the Amplify Gateway Installation

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

```markdown
helm install ampgw ampc-rel/ampgw
```

If you want to customize your install, create an override yaml file, as outlined in the previous section, and run the install as follows

```markdown
helm install ampgw ampc-rel/ampgw -f override.yaml
```

### Check whats running

```markdown
kubectl get pods
kubectl describe pods <pod-name>
```

### Check the logs

```markdown
kubectl -- logs --follow <pod-name>
```

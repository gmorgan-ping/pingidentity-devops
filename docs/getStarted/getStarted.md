---
title: Get Started
---
# Get Started

You can quickly deploy Docker images of Ping Identity products. We use Kubernetes to deploy our Docker images in stable, network-enabled containers. Our Docker images are preconfigured to provide working instances of our products, either as single containers or in orchestrated sets.

!!! info "Deprecation Notice"
    The Ping Identity DevOps Getting Started Repository is shifting from docker-compose and Kustomize to all Kubernetes and Helm. This shift removes the support needed for these ineffective service and allows Ping Identity to provide greater focus on patterns that are more production-ready. If needed, older examples can be found in tags <2108.

This documentation is intented to help Ping Identity DevOps consumers learn how to use Ping Identity Docker images in Kubernetes, identify appropriate infrastructure configurations, and prepare for ongoing management of services.

Here is a suggested path for reading through this documentation:

  * [Prerequisites](#prerequisites) - General Prerequisites needed for success accross examples

  * [Deploy Simple Stack](#deploy-simple-stack)

  * 



## Prerequisites

* Egress (outbound) internet access
* cloned this repo: 
      ```
      mkdir -p ${HOME}/projects/devops
      cd ${HOME}/projects/devops
      git clone https://github.com/pingidentity/pingidentity-devops-getting-started.git
      ```
* You have [access to a kubernetes cluster](getStartedWithKubernetes.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [helm](https://helm.sh/docs/intro/install/)
* [Set Up Your Devops Environment](#set-up-your-devops-environment)

## Set Up Your Devops Environment

Evaluation licenses are pulled at container startup if a valid Ping Identity DevOps User and Key is provided. For more information, see [DevOps Registration](devopsRegistration.md). Prepare your user and key to be used by deployments via a Kubernetes secret with the name `devops-secret`.
      
      ```
       kubectl create secret generic devops-secret \
        --from-literal=PING_IDENTITY_DEVOPS_USER='<YOUR_PING_IDENTITY_DEVOPS_USER>' \
        --from-literal=PING_IDENTITY_DEVOPS_KEY='<YOUR_PING_IDENTITY_DEVOPS_KEY>' \
        --from-literal=PING_IDENTITY_ACCEPT_EULA=YES
      ```

Eventually, full licenses will be desired, this usage 

Add our [Helm chart repository](https://helm.pingidentity.com/)
      
```sh
helm repo add pingidentity https://helm.pingidentity.com/
```

## Deploy Simple Stack

Deploy the simple stack with:

```sh
helm upgrade --install myping pingidentity/ping-devops -f 10-kubernetes/01-simple-stack/values.yaml
```

Watch the release become healthy: 

```
watch -n 2 "kubectl get pods,services,ingresses"
```

!!! info "What is Healthy?"
      In Kubernetes it is assumed an application is healthy if the `READY` column shows a whole number (e.g. `1/1` or `2/2`)

Note the ingress resources. If your cluster has an nginx-ingress-controller like the one in [Deploy a Local Kubernetes Cluster](./getStartedWithKubernetes.md) you will see the address field populated:

```
NAME                         CLASS    HOSTS                                       ADDRESS     PORTS     AGE
myping-pingaccess-admin      <none>   myping-pingaccess-admin.ping-local.com      localhost   80, 443   10m
myping-pingaccess-engine     <none>   myping-pingaccess-engine.ping-local.com     localhost   80, 443   10m
myping-pingdataconsole       <none>   myping-pingdataconsole.ping-local.com       localhost   80, 443   10m
myping-pingdelegator         <none>   myping-pingdelegator.ping-local.com         localhost   80, 443   10m
myping-pingdirectory         <none>   myping-pingdirectory.ping-local.com         localhost   80, 443   10m
myping-pingfederate-admin    <none>   myping-pingfederate-admin.ping-local.com    localhost   80, 443   10m
myping-pingfederate-engine   <none>   myping-pingfederate-engine.ping-local.com   localhost   80, 443   10m
```

### Interact with Simple Stack

Once a product shows healthy, the corresponding host should be accessible via curl or the browser. 

```
curl -k https://myping-pingdirectory.ping-local.com/available-state
{ "availability-state":"AVAILABLE" }
```

If you cannot access the host url, then you can use [port forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) to access any service resource. 

Syntax for port-forward as a background process:

```
kubectl port-forward svc/<service-name> <localPort>:<k8sPort> &
```

```
kubectl port-forward svc/myping-pingdirectory 1443:443 &
#response
# [1] 1768909
# Forwarding from 127.0.0.1:1443 -> 1443
# Forwarding from [::1]:1443 -> 1443
```

This opens a port on you local machine to be used by APIs or via browser

```
curl -k https://localhost:1443/available-state
Handling connection for 1443
{ "availability-state":"AVAILABLE" }
```

Since the port-forward was created with as a [background process](https://linuxize.com/post/how-to-run-linux-commands-in-background/) using `&`. You can create mupltiple port-forwards at once. To close the port forward, identify it with the `jobs` command, then `kill` the job number:

```
jobs
[1]  - running    kubectl port-forward svc/myping-pingdirectory 1443:443
[2]  + running    kubectl port-forward svc/myping-pingdirectory 1636:636
kill %1 %2
[1]  - 1768909 terminated  kubectl port-forward svc/myping-pingdirectory 1443:443
[2]  + 1789180 terminated  kubectl port-forward svc/myping-pingdirectory 1636:636
```

Login information for Product Admin Consoles: 

!!! info "Port-Forward Reminder"
      If you don't have access to the ingress URLs and want to access via `https://localhost:<port>` you must first start the [port-forward connection](#interact-with-simple-stack)


  | Product | Connection Details |
  | --- | --- |
  | [PingFederate](https://localhost:9999/pingfederate/app) | <ul> <li>Ingress URL: [https://myping-pingfederate-admin.ping-local.com/pingfederate/app](https://myping-pingfederate-admin.ping-local.com/pingfederate/app)</li><li>Port-Forward URL: [https://localhost:9999/pingfederate/app](https://localhost:9999/pingfederate/app)</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |
  | [PingDirectory](https://localhost:8443/console) | <ul><li>Ingress URL: [https://myping-pingdataconsole.ping-local.com/console](https://myping-pingdataconsole.ping-local.com/console)</li><li>Port-Forward URL: [https://localhost:8443/console](https://localhost:8443/console)</li><li>Server: pingdirectory:1636</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |
  | [PingAccess](https://localhost:9000) | <ul><li>Ingress URL: [https://myping-pingaccess-admin.ping-local.com](https://myping-pingaccess-admin.ping-local.com)</li><li>Port-Forward URL: [https://localhost:9000](https://localhost:9000)</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |
  | [Delegated Administration](https://localhost:6443/delegator/) | <ul><li>Ingress URL: [https://myping-pingdelegator.ping-local.com/delegator/](https://myping-pingdelegator.ping-local.com/delegator/)</li><li>URL: [https://localhost:6443/delegator/](https://localhost:6443/delegator/)</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |
  | Apache Directory Studio for PingDirectory |<ul> <li> **requires port-forward</li><li> LDAP Host: localhost</li><li>LDAP Port: 1636</li><li>LDAP BaseDN: dc=example,dc=com</li><li>Root Username: cn=administrator</li><li>Root Password: 2FederateM0re</li></ul> |

## Cleanup

Uninstall your helm release: 

```
helm uninstall myping
```

The Ping Identity applications that use persistent storage have dynamically provisioned [Persistent Volume Claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/).
Delete the associated Persistent Volume Claims. 

```
kubectl delete pvc --selector=app.kubernetes.io/instance=myping
```

Due to use of [dynamic provisioning](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#dynamic), deletion of the Persistent Volume Claim will trigger deletion of the bound Persistent Volume and leave your environment clean.

## Next Steps

Depending on preference explore one of these two critical sections:

  * **Deployment** - Here you will find guidance on what we have identified as the common questions asked by every consumer looking to have a successful produciton deployment. 
  * **Container Anatomy** - A detailed look at what occurs when any PingIdentity Docker image is deployed. This section is deep dive in understanding the patterns implemented across all PingIdentity images to standardize user interaction with any PingIdentity software in DevOps.
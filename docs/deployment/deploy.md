---
title: Orchestrate Deployments with Kubernetes
---
# Orchestrate Deployments with Kubernetes

!!! warning "Kustomize and docker-compose Deprecation"
    We are deprecating support for Kustomize and docker-compose examples in favor of our [helm chart](https://helm.pingidentity.com). Documentation may be impacted during this transition. 


Kubernetes and the Ping Identity [Helm charts](https://helm.pingidentity.com) enable you to quickly deploy demo examples and prepare for production scenarios. Use of this chart and feedback is highly encouraged. As such, the Ping DevOps Program will focus all new work on Kubernetes (provider agnostic) and Helm.

<div class="iconbox" onclick="window.open('https://helm.pingidentity.com','');">
    <img class="assets" src="../../images/logos/helm.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://helm.pingidentity.com" target=”_blank”>Helm Charts Repo</a>
    </span>
</div>

The Deployment topic aims to give consumers guidance on common questions that arise when working towards a successful production deployment. Many sections in the Deployment topic can be read independently as needed, but there are some core concepts that are relevant to every deployment.

As you read through this section you will find snippets of values.yaml code relevant to each topic. Your goal should be to compile any snippets that matter to you into your own, complete values.yaml, this is covered further on [Building values.yaml](../get-started/k8sHelmBasics#building-values.yaml). 


Critical sections to consider: 

  * Core Concepts - relevant to every consumer 
    * Server Profiles
    * [Container Anatomy]
    * [Building values.yaml](../get-started/k8sHelmBasics#building-values.yaml)
  * Operating Patterns - configuration management options per product. Topics like networking, disaster recovery, and secrets management depend on having a solid operating pattern. 

  

---
title: Working with DevOps Images
---
# Working with DevOps Images

After you've deployed a set of our DevOps images using the full-stack server profile in [Get Started](../get-started/getStarted.md), you can move on to deployments using server profiles that might more closely reflect use cases you want to test out. You can:

* Continue working with the full-stack server profile in your local `pingidentity-devops-getting-started/11-docker-compose/03-full-stack` directory.
* Try our other server profiles in your local `pingidentity-devops-getting-started` directory to quickly deploy typical use cases.
* Clone the [`pingidentity-server-profiles`](https://github.com/pingidentity/pingidentity-server-profiles) repository to your local `${HOME}/projects/devops` directory and learn about the setup of specific product configurations.

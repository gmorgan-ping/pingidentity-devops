---
title: Orchestrate Deployments with Kubernetes
---
# Orchestrate Deployments with Kubernetes

Kubernetes and the Ping Identity [Helm charts](https://helm.pingidentity.com) enable you to quickly deploy demo examples and build production scenarios. Use of this chart and feedback is highly encouraged. As such, the Ping DevOps Program will focus all new work on Kubernetes (provider agnostic) and Helm.

<div class="iconbox" onclick="window.open('https://helm.pingidentity.com','');">
    <img class="assets" src="../../images/logos/helm.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://helm.pingidentity.com" target=”_blank”>Helm Charts Repo</a>
    </span>
</div>

The Deployment topic provides guidance on common questions that arise when working towards a successful production deployment. Many sections in the Deployment topic can be read independently as needed, but there are some core concepts that are relevant to every deployment.

As you read through this section you will find snippets of values.yaml code relevant to each topic. Your goal should be to compile any snippets that matter to you into your own, complete, `values.yaml`, this is covered further on [Building values.yaml](../get-started/k8sHelmBasics#building-values.yaml). 

Critical sections to consider: 

  * Core Concepts - relevant to every consumer 
    * [Server Profiles](serverProfiles.md)
    * [Container Anatomy](../reference/containerAnatomy.md) - 
    * [Building values.yaml](../getStarted/k8sHelmBasics#building-values.yaml)
  * Operating Patterns - configuration management options per product. Topics like networking, disaster recovery, and secrets management depend on having a solid operating pattern. 

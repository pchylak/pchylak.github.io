---
layout: post
title: 'Microsoft Azure - Containers'
tags: 
  - Microsoft Azure
date: 2023-06-09
categories: 
  - Blog
image: /assets/img/posts/2023/MicrosoftAzure-Containers.png
hidden: false
---
* toc
{:toc .large_only}

Microsoft Azure Services overview.
<!--more-->

### Introduction

Containerization is a technology that enables the deployment and running of applications in isolated environments known as containers. This is a lightweight solution for delivering applications along with their dependencies and configurations. The popularity of containers in IT environments has grown significantly since the introduction of Docker in 2013. Today, containers are an integral part of modern IT architectures and align very well with the DevOps philosophy.

### Azure Container Instances

Azure Container Instances (ACI) is a service provided by Microsoft Azure. It allows you to run containerized applications without the need to manage the infrastructure. ACI offers serverless environment for quick and easy deployment of containers without managing virtual machines and some container orchestration solution. Container Groups are feature that allows You to deploy multiple containers that share the same lifecycle and network into Azure single instance. That means containers in one group will start and stop together and it's scheduled on single host.

### Azure Kubernetes Service

Azure Kubernetes Service (AKS) is a Microsoft implementation of popular open-source platform Kubernetes (K8s) that is a container orchestration platform. It siplifiy deployment and management of containerized applications on large scale. AKS orchestrates a cluster of virtual machines, schedules containers, traks the health of resources in cluster.

AKS is provided as a platform service, meaning Microsoft will manage key components like Scheduler, API Server and Controler Manager. As a customer/user You can manage nodes, podes and other elements.

|![AKS Azure](/assets/img/posts/2023/control-plane-and-nodes.png)|
| -- |
| Image source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/aks/concepts-clusters-workloads) |

### Azure Container Apps

Azure Container Apps is a application platform based on Kubernetes and open-source technologies ([Dapr](https://dapr.io/), [KEDA](https://keda.sh/), [envoy](https://www.envoyproxy.io/)) - it provides possibilities to deploy containers without managing orchestrator infrastructure. Unlike the Azure Container Instance You have access to Kubernetes features without direct access to Kubernetes API.

### Azure App Service

Azure App Service is a fully managed hosting for Your applications, it's optimized for websites and web applications. This service provides possibility to deploy app from code or container.

### Azure Container Registry

Azure Container Registry (ACR) is a private Docker registry provided by Microsoft Azure. It allows to store, build and manage container images. ACR enables simplify of container lifecycle management by centralizing storage of private container images.

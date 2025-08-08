---
layout: post
title: 'IaC - Environment preparation - Part 1'
tags: 
  - Infrastructure as a Code
  - Microsoft Azure
date: 2022-03-26
categories: 
  - Blog

image: /assets/img/posts/2022/FeaturedImage2.jpg
---
* toc
{:toc .large_only}

# Introduction

Why to use Infrastructure as a Code?

My first think is: to reduce natural human mistakes. Second is that I'm little lazy and I like to use some automations in my work, so scripts, bicep and ARM templates are very helpful in this aspect.

Every resource can be defined by template, image or script - when we need to create or update resources we are using prepared templates and fill only parameter values.
In public cloud one mistake can cost a lot of money so... let's start.

If You are starting with Infrastructure as a Code, You'll need ready environment to work with templates, images, etc.
The main goal of this article is creating that environment.
This part describes how to configure Windows 10/11 OS client.

# Configuration

In first step I'll use chocolatey as package provider for Windows 10/11. After installation we need to restart Powershell Console or Terminal.

```powershell
 Set-ExecutionPolicy Bypass -Scope Process -Force; 
 [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; 
 iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1')) 
```

Second step is installation of Visual Studio Code, Azure Cli and Docker

```powershell
choco install vscode -y
choco install azure-cli -y
choco install docker-desktop -y
```

Next step is installing Visual Studio Code extensions for Bicep, Terraform, Powershell

```powershell
code --install-extension ms-azuretools.vscode-bicep; 
code --install-extension ms-azuretools.vscode-azureterraform 
code --install-extension ms-vscode.azure-account 
code --install-extension ms-vscode.powershell 
code --install-extension redhat.vscode-yaml
code --install-extension ms-azuretools.vscode-docker 
```

After installation You can start work with templates, images, scripts.

# Updates

Everything You can update by using:

```powershell
choco update [Package Name]
```

# Links

[Github Repo {_ext}](https://github.com/pchylak/inCloud.blog/blob/main/IaaC/WindowsEnvScript.md)
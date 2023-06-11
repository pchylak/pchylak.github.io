---
layout: post
title: 'IaaC - Azure Bicep - First steps'
tags: [Infrastructure as a Code, Microsoft Azure, Azure Bicep]
featured_image_thumbnail: 
featured_image: assets/images/posts/2022/FeaturedImage3.jpg
---

Infrastructure as a Code series. In this series, I'll try to present all aspects of working with IaaC, starting with preparation of the work environment. 
<!--more-->

# Benefits of using Azure Bicep

- Simple syntax - if You are using JSON teplates You'll see that Bicep is easier to read. 
- Repeatable results - overtime You will use Your template You'll see that results are deployed in a consistent manner.
- Modularity - You don't need to build one big template - it can be split to modules that covers set of resources.
- Native integration with Azure
- No state or state files to manage
- It's totally free

# Tools

- [Prepare Your environment](http://cloudinsights.blog/2022/03/26/iaac-environment-part1/)
- [Bicep playground {_ext}](https://bicepdemo.z22.web.core.windows.net/)

# First template

Our first template will contain: 
- Resource group
- Azure Virtual Network

main.bicep:
```
//scope of template
targetScope = 'subscription'

//parameters section
param location string 
param rgName string
param vnetName string
param vnetAddrSpace string
param snet1AddrSpace string 
param snet2AddrSpace string
param snet3AddrSpace string  

//modules section
module resourceGroup1 'rg.bicep' = {
  name: rgName
  params: {
    location: location
    rgName: rgName
  }
}

//reference to existing resource
resource resGrp 'Microsoft.Resources/resourceGroups@2021-04-01' existing = {
  name: rgName
}

module vNetwork 'vnet.bicep' = {
  name: rgName
  scope: resGrp
  params: {
    location: location
    vnetName: vnetName
    vnetAddrSpace: vnetAddrSpace
    snet1AddrSpace: snet1AddrSpace
    snet2AddrSpace: snet2AddrSpace
    snet3AddrSpace: snet3AddrSpace
  }
}
```

rg.bicep:
```
//scope of template
targetScope = 'subscription'

//parameters section
param rgName string
param location string

//resources section
resource resGrp 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: rgName
  location: location
}
```

net.bicep:
```
//parameters section
param vnetName string
param vnetAddrSpace string
param snet1AddrSpace string 
param snet2AddrSpace string
param snet3AddrSpace string  
param location string

//variables section
var snet2Name = 'AzureBastionSubnet'
var snet3Name = 'GatewaySubnet'
var snet1Name = 'default'

//resources section
resource vnet 'Microsoft.Network/virtualNetworks@2019-11-01' = {
    name: vnetName
    location: location
    properties: {
      addressSpace: {
        addressPrefixes: [
          vnetAddrSpace
        ]
      }
      subnets: [
        {
          name: snet1Name
          properties: {
            addressPrefix: snet1AddrSpace
          }
        }
        {
          name: snet2Name
          properties: {
            addressPrefix: snet2AddrSpace
          }
        }
        {
          name: snet3Name
          properties: {
            addressPrefix: snet3AddrSpace
          }
        }
      ]
    }
}
```

How to run deployment? 

Azure Cli:
```
az deployment sub create \
    --name firstBicepDeployment \
    --subscription 'xxxx-yyyyy-zzzz-yyyy'
    --template-file main.bicep
    --parameters param1=value1 param2=value2
    --location westeurope
```

Powershell:
```
New-AzSubscriptionDeployment `
    -name firstBicepDeployment `
    -subscription 'xxxx-yyyy-zzzz-yyyy' `
    -templatefile main.bicep `
    -param1 value1 
    -param2 value2
```

in this example I used: 

```
az deployment sub create \
    --name firstBicepDeployment \
    --subscription 'xxxx-yyyyy-zzzz-yyyy'
    --template-file main.bicep
    --parameters location=westeurope rgName=azr-tst-rg vnetName=azr-tst-vnet-001 vnetAddrSpace=10.0.0.0/16 snet1AddrSpace=10.0.0.0/24 snet2AddrSpace=10.0.253.0/24 snet3AddrSpace=10.0.254.0/24
    --location westeurope
```

# Converting

If we have JSON template from work environment we can decompile it to Bicep template. 

```
az bicep decompile --file myfile.json
```

This command will create myfile.bicep file in directory where it was run. There can be some warnings and errors that You will need to fix after decompiling. 

# Useful links

[JSON decompile to Bicep {_ext}](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/decompile?tabs=azure-cli)<br/>
[What is Bicep? {_ext}](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep)<br/>
[GitHub Repo {_ext}](https://github.com/pchylak/inCloud.blog/tree/main/IaaC/AzureBicepIntro)
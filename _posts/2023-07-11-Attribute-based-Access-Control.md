---
layout: post
title: 'Microsoft Azure - Attribute-based access control'
tags: 
  - Microsoft Azure
date: 2023-07-11
categories: 
  - Blog

image: /assets/img/posts/2022/FeaturedImage3.jpg
---

* toc
{:toc .large_only}

Microsoft Azure Services overview.
<!--more-->

### Introduction

Inspired by June meeting of Microsoft Azure Users Group Poland I decided to try to use Attribute-based access control on storage account. Honesly I missed announcement about this feature so it was little suprise for me.

|![ABAC](/assets/img/posts/2023/condition-access-multiple.png)|
| -- |
|Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-overview)|

First at all let's look at the definition of ABAC:

"Attribute-based access control (ABAC) is an authorization system that defines access based on attributes associated with security principals, resources, and the environment of an access request."

Sounds like a solution to help protect your storage account keys and prevent SAS token generation. Let's take a look at this feature in tests.

### Configuration

ABAC works only on blob storage and queue storage data so we need to assign one of those roles to storage account or on resource group level:

* Storage Queue Data Reader
* Storage Queue Data Contributor
* Storage Queue Data Message Processor
* Storage Queue Data Message Sender
* Storage Blob Data Reader
* Storage Blob Data Contributor
* Storage Blob Data Owner

When we choose one of roles above there will appear new tab in form:

|![Form](/assets/img/posts/2023/conditions-tab.png)|

In next step we need to create condition with privileges based on role that we assign before. Condition can be created as a code:

|![Condition as code](/assets/img/posts/2023/condition-code.png)|
| -- |
|Condition as a Code|

or in visual designer:

|![Condition as visual designer](/assets/img/posts/2023/condition-visual-designer.png)|
| -- |
|Condition in visual designer|

Action is a single permission that we want to assign to identity.

Expression is additional attribute that we can use - it based on resource, principal or environment, so we can assign permissions using even AAD custom security attributes or we can be sure that access to data is granted by using private link or private endpoint.

Expression sources:

* Environment
  * Subnet
  * Private endpoint
  * Private link
  * UTC now
* Principal
  * Custom security attributes from AAD
* Resource
  * Account name
  * Container name
  * Blob path

Here are example of Azure Active Directory custom security attributes:

|![AAD Custom attributes](/assets/img/posts/2023/aad-custom-attributes.png)|
| -- |
|Azure AD custom security attributes|

### Test

In my environment I was created conditions like above. First test was connection with account with attribute 'isManager' set to True.

|![Test result 1](/assets/img/posts/2023/storage-test-result-1.png)|
| -- |
|Test result 1|

As a second test, I changed AAD custom security attribute 'isManager' to False on testing account.

|![Test result 2](/assets/img/posts/2023/storage-test-result-2.png)|
| -- |
|Test result 2|

Looks and works well, I can imagine projects where I can use it.

### Limitations

* to use principal or user attributes in conditions You need Azure AD Premium P1 or P2 plan
* remember that all changes requires minimum 5 minutes to be executed
* in visual designer we can create only 5 expressions

### Summary

I think this is a very helpful feature when we need to separate a lot of projects/applications in one storage account.
We don't need to share access keys or SAS tokens with anyone. That meens we don't need to rotate it when someone is finish work with data.

### Links

Full documentation is available [here](https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-overview).

Examples of conditions are [here](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-auth-abac-examples?tabs=portal-visual-editor)

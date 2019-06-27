---
layout: post
title: Azure DevOps - Xamarin.iOS buildtool versions
description: ""
modified: 2019-06-27
tags: [Azure Devops]
share: false
image:
  path: /images/abstract-5.jpg
  feature: abstract-5.jpg
---

Xamarin.iOS task handles buildtool version a bit different, than other Azure DevOps Task.  
Basically, there's a shells script on each macOS hosted VM, with which one can select the build tool vesion.  

This example sets the Xamarin SDK Version (mono version resp.) to a given version:
```yaml
steps:
- script: sudo $AGENT_HOMEDIRECTORY/scripts/select-xamarin-sdk.sh 5_18_1 
  displayName: 'Select the Xamarin SDK version'
```  

For more information visit [https://go.microsoft.com/fwlink/?linkid=871629](https://go.microsoft.com/fwlink/?linkid=871629)

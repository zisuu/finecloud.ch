---
title: "Automatically Update the Hidden Dependencies in Your DevContainer"
meta_title: "Automatically Update the Hidden Dependencies in Your DevContainer"
description: "Renovate is able to update the DevContainer feature versions, but it is not able to update the hidden dependencies in the .devcontainer/devcontainer.json file, like for example the node and npm versions in this example."
date: 2025-03-11T19:33:00
categories: ["Docker", "DevContainer", "Renovate", "Dependency Management"]
author: "David"
tags: ["devcontainer", "docker", "renovate", "dependency management"]
draft: false
---

Renovate is able to update [the DevContainer feature versions,](https://docs.renovatebot.com/modules/manager/devcontainer/) 
but it is not able to update the hidden dependencies in the `.devcontainer/devcontainer.json` file, like for example the 
node and npm versions in this example:

```json
"features": {
  "myociregistry.local/devcontainer/features/node:1.1.0": {
  "version": "18.20.5",
  "npmVersion": "10.5.0"
  }
},
```

To update these dependencies, you can use define a `customManagers` in the Renovate configuration file:

```json
  "customManagers": [
    {
      "customType": "regex",
      "description": "Update version fields in devcontainer.json",
      "fileMatch": ["(^|/)devcontainer\\.json$"],
      "matchStrings": [
        "// renovate: datasource=(?<datasource>[a-z-]+?)(?: depName=(?<depName>.+?))?
        packageName=(?<packageName>.+?)\\s+\"\\w*[Vv]ersion\":\\s*\"(?<currentValue>[^\"]+)\""
      ]
    }
  ],
```

To make this work we need to add a comment to the `devcontainer.json` file with the following format:

```json
"features": {
  "myociregistry.local/devcontainer/features/node:1.1.0": {
    // renovate: datasource=node depName=node packageName=node
    "version": "18.20.5",
    // renovate: datasource=npm depName=npm packageName=npm
    "npmVersion": "10.5.0"
  }
},
```
With that in place, Renovate will be able to update the `node` and `npm` versions in the `devcontainer.json` file.

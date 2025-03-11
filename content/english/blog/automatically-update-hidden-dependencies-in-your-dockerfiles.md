---
title: "Automatically Update the Hidden Dependencies in Your Dockerfiles"
meta_title: "Automatically Update the Hidden Dependencies in Your Dockerfiles"
description: "Learn how to keep your dependencies in your dependencies up to date with Renovate."
date: 2024-09-13T13:52:00
categories: ["Docker", "Renovate", "Dependency Management"]
author: "David"
tags: ["docker", "renovate", "dependency management"]
draft: false
---

You have probably already heard about Renovate - the fantastic tool that helps you to keep your dependencies up to date.
But today, we want to look at an edge case: how to keep your dependencies in your dependencies up to date (also called hidden dependencies)?

Atlassian provides a Confluence image. If you want to run this with an Oracle database, you have to inject the Oracle JDBC driver yourself. But how can you ensure that you automatically always will have the most recent OJDBC driver in your Confluence image?

```dockerfile
FROM atlassian/confluence:8.5.15-ubuntu-jdk17

# Download the ojdbc driver
RUN curl -fSL "https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc10/19.23.0.0/ojdbc10-19.23.0.0.jar" \
    -o "${CONFLUENCE_INSTALL_DIR}/confluence/WEB-INF/lib/ojdbc10.jar"
```

First of all, let's extract the semantic version tag from the URL into a separate ARG so that Renovate only needs to bump the version there:

```dockerfile
FROM atlassian/confluence:8.5.15-ubuntu-jdk17

ARG OJDBC_VERSION=19.24.0.0

# Download the ojdbc driver
RUN curl -fSL "https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc10/${OJDBC_VERSION}/ojdbc10-${OJDBC_VERSION}.jar" \
    -o "${CONFLUENCE_INSTALL_DIR}/confluence/WEB-INF/lib/ojdbc10.jar"
```

Next, we will add a comment directly above the ARG OJDBC version, which we will later use to tell Renovate what dependency type and package this version tag is about. It's a maven package with the package name "com.oracle.database.jdbc:ojdbc10":

```dockerfile
FROM atlassian/confluence:8.5.15-ubuntu-jdk17

# renovate: datasource=maven depName=com.oracle.database.jdbc:ojdbc10 packageName=com.oracle.database.jdbc:ojdbc10
ARG OJDBC_VERSION=19.24.0.0

# Download the ojdbc driver
RUN curl -fSL "https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc10/${OJDBC_VERSION}/ojdbc10-${OJDBC_VERSION}.jar" \
    -o "${CONFLUENCE_INSTALL_DIR}/confluence/WEB-INF/lib/ojdbc10.jar"
```

You could write a custom regex manager in the Renovate config that updates this version tag. But what if you have multiple ARG or ENV version tags in multiple Dockerfiles within the same repo? You probably don't want to write a regex config for each case.

Let's imagine you have something like this in your Dockerfile:

```dockerfile
# renovate: datasource=github-tags depName=node packageName=nodejs/node versioning=node
ENV NODE_VERSION=20.10.0
# renovate: datasource=github-releases depName=composer packageName=composer/composer
ENV COMPOSER_VERSION=1.9.3
# renovate: datasource=docker packageName=docker versioning=docker
ENV DOCKER_VERSION=19.03.1
# renovate: datasource=npm packageName=yarn
ENV YARN_VERSION=1.19.1
```

All you now need is the following regex configuration in your `renovate.json` config to detect and bump all of these version tags:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:recommended"
  ],
  "separateMinorPatch": true,
  "customManagers": [
    {
      "customType": "regex",
      "description": "Update _VERSION variables in Dockerfiles",
      "fileMatch": ["(^|/|\\.)Dockerfile$", "(^|/)Dockerfile\\.[^/]*$"],
      "matchStrings": [
        "# renovate: datasource=(?&lt;datasource&gt;[a-z-]+?)(?: depName=(?&lt;depName&gt;.+?))? packageName=(?&lt;packageName&gt;.+?)(?: versioning=(?&lt;versioning&gt;[a-z-]+?))?\\s(?:ENV|ARG) .+?_VERSION=(?&lt;currentValue&gt;.+?)\\s"
      ]
    }
  ]
}
```

Now, you could even set up auto-merge for these Renovate updates. Make sure you have some merge checks before you enable this. At least you should ensure that the Docker image can still be built before a new Renovate pull request gets auto-merged.

✨ Happy renovating! ✨


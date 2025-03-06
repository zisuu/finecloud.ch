---
title: "Third-party GitHub Actions"
meta_title: "Third-party GitHub Actions"
description: "Today I came across these steps to guide our decision-making process, before using a 3rd Part GitHub Action."
date: 2024-04-08T20:50:00
categories: ["Tools"]
author: "David"
tags: ["github", "security"]
draft: false
---

Today I came across these steps to guide our decision-making process, before using a 3rd Part GitHub Action:

1. For simple tasks, avoid external GitHub Actions because the risk might outweigh the value. Maybe a simple curl could do it as well? 😉
2. Use GitHub Actions from Verified Creators because they follow a strict security review process.
3. Use the latest version of a GitHub Action because it might contain security fixes.
4. Think about GitHub Actions like dependencies: they need to be maintained and updated. Dependabot or Renovate can help here.
5. Think about disabling or limiting GitHub Actions for your organization(s) in Settings.
6. Have a PR process with multiple reviewers to avoid adding a malicious GitHub Action.

This article was updated on April 8, 2024.

### Tags
- [github](https://www.finecloud.ch/tags/github/)
- [security](https://www.finecloud.ch/tags/security/)

### Related Posts
- [GitHub classic vs. fine-grained Personal Access Tokens](https://www.finecloud.ch/github-classic-vs-fine-grained-personal-access-tokens.html) - July 31, 2024
- [GitHub Codespace](https://www.finecloud.ch/github-codespace.html) - April 5, 2024
---
title: "GitHub Classic vs. Fine-grained Personal Access Tokens"
meta_title: "GitHub Classic vs. Fine-grained Personal Access Tokens"
description: "A comparison of classic and fine-grained personal access tokens (PATs) on GitHub."
date: 2024-08-05T13:52:00
categories: ["GitHub", "Security"]
author: "David"
tags: ["github", "security", "personal access tokens"]
draft: false
---

**What are PATs?** Personal access tokens are an alternative to using passwords for authentication to GitHub when using the [GitHub API](https://docs.github.com/en/rest/overview/authenticating-to-the-rest-api) or the [command line](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#using-a-personal-access-token-on-the-command-line).

Personal access tokens are intended to access GitHub resources on your behalf. **To access resources on behalf of an organization, or for long-lived integrations, you should use a GitHub App. For more information, see "[About creating GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/setting-up-a-github-app/about-creating-github-apps)"**

**GitHub recommends that you use fine-grained personal access tokens instead of personal access tokens (classic) whenever possible. ([source](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#types-of-personal-access-tokens))**

## What are classic PATs?

Personal access tokens (classic) are less secure. **However, some features currently will only work with personal access tokens (classic):**

- Only personal access tokens (classic) have write access for public repositories that are not owned by you or an organization that you are not a member of.
- Outside collaborators can only use personal access tokens (classic) to access organization repositories that they are a collaborator on.
- A few REST API endpoints are only available with a personal access tokens (classic). To check whether an endpoint also supports fine-grained personal access tokens, see the documentation for that endpoint, or see "[Endpoints available for fine-grained personal access tokens](https://docs.github.com/en/rest/overview/endpoints-available-for-fine-grained-personal-access-tokens)".

If you choose to use a personal access token (classic), keep in mind that it will grant access to all repositories within the organizations that you have access to, as well as all personal repositories in your personal account.

As a security precaution, GitHub automatically removes personal access tokens that haven't been used in a year. To provide additional security, we highly recommend adding an expiration to your personal access tokens.

## What are fine-grained PATs?

Fine-grained personal access tokens have several security advantages over personal access tokens (classic):

- Each token can only access resources owned by a single user or organization.
- Each token can only access specific repositories.
- Each token is granted specific permissions, which offer more control than the scopes granted to personal access tokens (classic).
- Each token must have an expiration date.
- Organization owners can require approval for any fine-grained personal access tokens that can access resources in the organization.

## What happens when the user who generated them becomes inactive and loses access to the resource?

Both fine-grained personal access tokens and personal access tokens (classic) are tied to the user who generated them and will become inactive if the user loses access to the resource.

## What are the risks to consider with PATs?

- PATs could be leaked if they get into the wrong hands and are used externally.
- Org. Admins don't see users classic PATs with Org. access.

## Can a classic PAT (owned by a regular GH user) be used to access and manipulate a GH internal Repo from externally without any other auth requirement?

Yes. You need to create a classic PAT, select all permissions and then try to access an internal GitHub Test Repo from an external Computer without any VPN/internal access or SSO auth session. You will be able to pull all the Repo content and write everywhere where the owner of the PAT has access, e.g., create Issues, close PRs, etc.

## Can a fine-grained PAT (owned by a regular GH User) be used to access and manipulate a GH internal Repo from externally without any other auth requirement?

Per default, user-owned fine-grained PATs have only read access to public github.com repos. To select access to an internal GH Repo, the Organization must own the PAT, which is only possible if the token request gets approved by an Org. Admin.

## Can a fine-grained PAT (owned by the Org) be used to access and manipulate a GH internal Repo externally without any other auth requirement?

Once the requested PAT got approved by an Org Admin, yes, this is possible. But the token has a maximum lifetime of 1 year. If the user of the PAT changes something on the permissions, the review workflow is again triggered, and an Org Admin must approve the permission change before it is applied.

## How can Org. Admins restrict classic PATs access to the Org?

Organization owners/admins can prevent personal access tokens (classic) from accessing resources owned by the organization. Personal access tokens (classic) can still read public resources within the organization.

![GitHub Token](https://www.finecloud.ch/media/posts/104/gh-token.png)

[https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens](https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens)

## How can you manage classic PATs of the Users as an Org Admin?

You can't, because you don't see them. Only the User who created the classic PAT can see and manage it.

## How can I restrict Org access of fine-grained PATs as an Org Admin?

![GitHub Token](https://www.finecloud.ch/media/posts/104/gh-token-2.png)

[https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens](https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens)

## How can I manage fine-grained PATs of the Users as an Org Admin?

You can't manage/see the fine-grained PATs the User Account owns. Once the Org owns the PAT, you can see and manage them.

If the owner is set to the User, the PAT is invisible/unable to manage, but this also means that token only has access to public repos:

![GitHub Token](https://www.finecloud.ch/media/posts/104/SCR-20240805-qyyf.png)

If you select the Organization as the Resource owner of the PAT instead, a fine-grained personal access token request will be made, which needs to be approved by an Org. Admin before it can be used.

An overview of all Org. owned PATs can be found under this URL; if you are an Org. Admin: [https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens/active](https://github.com/organizations/<your-gh-org-name>/settings/personal-access-tokens/active)

## Overview of Classic vs. Fine-grained PATs

| | Classic PAT | Fine-grained PAT |
| --- | --- | --- |
| write access for public repositories that are not owned by you or an organization that you are not a member of | ✅ | ❌ |
| Outside collaborators can access organization repositories that they are a collaborator on. | ✅ | ❌ |
| Can be used with all REST API endpoints | ✅ | ❌ |
| Feature release status | stable | beta |
| Tokens must have an expiration date | ❌ | ✅ |
| Token can inherit all permissions of the User, incl. all repositories within the organizations that the user has access to without any approval/review | ✅ | ❌ |
| Token permissions can be defined set on repository level (fine-grained) | ❌ | ✅ |
| Organization owners can prevent token from accessing resources owned by the organization | ✅ | ✅ |
| Organization owners can require approval for each token that can access the organization (e.g. internal Repo) from externally without any other auth requirement | ❌ | ✅ |

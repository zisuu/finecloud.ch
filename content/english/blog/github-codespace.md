---
title: "GitHub Codespace"
meta_title: "What is a Codespace in GitHub?"
description: "What is a Codespace? A codespace is a development environment that's hosted in the cloud. You can customize your project for GitHub Codespaces by committing configuration files to your repository (also known as configuration-as-code), which creates a repeatable codespace configuration for all users of your project."
date: 2024-04-05T21:02:00
categories: ["Software Development", "GitHub"]
author: "David"
tags: ["codespace", "git", "github", "software development"]
draft: false
---

## What is a Codespace?

A codespace is a development environment that's hosted in the cloud. You can customize your project for GitHub Codespaces by committing configuration files to your repository (also known as configuration-as-code), which creates a repeatable codespace configuration for all users of your project. Each codespace you create is hosted by GitHub in a Docker container that runs on a virtual machine. You can choose the type of machine you want to use depending on the resources you need.

## How can I create a Codespace?

There are four ways to create a Codespace:

- From a GitHub template or any template repository on GitHub.com to start a new project.
- From a branch in your repository for new feature work.
- From an open pull request to explore work-in-progress.
- From a commit in a repository's history to investigate a bug at a specific point in time.

The easiest way to start a Codespace is by clicking the green Code button and then choosing open in a Codespace:

![Codespace Screenshot](https://www.finecloud.ch/media/posts/102/SCR-20240405-stqz.png)

## What happens when I create a Codespace?

When you create a GitHub Codespace, four processes occur:

1. VM and storage are assigned to your Codespace.
2. A container is created.
3. A connection to the Codespace is made.
4. A post-creation setup is made.

## What you can customize

There are many ways you can customize your Codespace. Let's review each one.

- **Settings Sync**: You can synchronize your Visual Studio Code (VS Code) settings between the desktop application and the VS Code web client.
- **Dotfiles**: You can use a dotfiles repository to specify scripts, shell preferences, and other configurations.
- **Rename a Codespace**: When you create a Codespace, it's assigned an autogenerated display name. If you have multiple Codespaces, the display name helps you to differentiate between Codespaces. You can change the display name for your Codespace.
- **Change your shell**: You can change your shell in a Codespace to keep the setup you're used to. When you're working in a Codespace, you can open a new terminal window with a shell of your choice, change your default shell for new terminal windows, or install a new shell. You can also use dotfiles to configure your shell.
- **Change the machine type**: You can change the type of machine that's running your Codespace, so that you're using resources appropriate for the work you're doing.
- **Set the default editor**: You can set your default editor for Codespaces in your personal settings page. Set your editor preference so that when you create a Codespace or open an existing Codespace, it opens to your default editor.
  - Visual Studio Code (desktop application)
  - Visual Studio Code (web client application)
  - JetBrains Gateway - for opening Codespaces in a JetBrains IDE
  - JupyterLab - the web interface for Project Jupyter
- **Set the default region**: You can set your default region in the GitHub Codespaces profile settings page to personalize where your data is held.
- **Set the timeout**: A Codespace will stop running after a period of inactivity. By default this period is 30 minutes, but you can specify a longer or shorter default timeout period in your personal settings on GitHub. The updated setting applies to any new Codespaces you create, or to existing Codespaces the next time you start them.
- **Configure automatic deletion**: Inactive Codespaces are automatically deleted. You can choose how long your stopped Codespaces are retained, up to a maximum of 30 days.

## Difference between Codespaces and GitHub.dev editor

### When should I use GitHub Codespaces and when should I use GitHub.dev?

GitHub.dev is a good fit if you only want to navigate some files and source code repositories from GitHub, and maybe make and commit small code changes.

On the other hand, if you want to run a bunch of tests with your code, or build a heavy application you better use GitHub Codespaces. It has compute associated with it so you can build your code, run your code, and have terminal access. GitHub.dev doesn't have compute in it. With GitHub Codespaces, you get the power of a personal Virtual Machine (VM) with terminal access, the same way you could use your local environment, just in the cloud.

|                       | GitHub.dev | GitHub Codespaces |
|-----------------------|------------|-------------------|
| **Cost**              | Free       | Free monthly quota of usage for personal accounts |
| **Availability**      | Available to everyone on GitHub.com | Available to everyone on GitHub.com |
| **Startup**           | GitHub.dev opens instantly with a key-press and you can start using it right away without having to wait for configuration or installation | When you create or resume a Codespace, the Codespace is assigned a VM, and the container is configured based on the contents of a devcontainer.json file. This setup takes a few minutes to create the development environment. |
| **Compute**           | There's no associated compute, so you can't build and run your code or use the integrated terminal. | With GitHub Codespaces, you get the power of a dedicated VM to run and debug your application. |
| **Terminal access**   | None       | GitHub Codespaces provides a common set of tools by default, meaning that you can use the Terminal exactly as you would in your local environment. |
| **Extensions**        | Only a subset of extensions that can run on the web appear in the extensions view and can be installed | With GitHub Codespaces, you can use most extensions from the Visual Studio Code Marketplace. |

## Using Codespace images

One of the biggest benefits of codespaces is that GitHub ships images with the most recent and common tools you need to develop. Have a look at this Repository: [https://github.com/devcontainers/images/tree/main/src/universal](https://github.com/devcontainers/images/tree/main/src/universal)

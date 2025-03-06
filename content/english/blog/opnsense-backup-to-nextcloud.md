---
title: "Backup OPNsense to Nextcloud"
meta_title: "Backup OPNsense to Nextcloud"
description: "What is OPNsense? OPNsense® is an open source, easy-to-use and easy-to-build FreeBSD based firewall and routing platform. OPNsense includes most of the features available in expensive commercial firewalls, and more in many cases. It brings the rich feature set of commercial offerings with the benefits…"
date: 2022-11-02T19:12:00
categories: ["Network", "Tools"]
author: "David"
tags: ["backup", "firewall", "linux", "network", "nextcloud", "tools"]
draft: false
---

## What is OPNsense?

[OPNsense®](https://opnsense.org) is an open source, easy-to-use and easy-to-build FreeBSD based firewall and routing platform.

OPNsense includes most of the features available in expensive commercial firewalls, and more in many cases. It brings the rich feature set of commercial offerings with the benefits of open and verifiable sources.

## Why OPNsense?

The feature set of OPNsense includes high-end features such as forward caching proxy, traffic shaping, intrusion detection and easy OpenVPN client setup. The latest release is based on a recent FreeBSD for long-term support and uses a newly developed MVC-framework based on Phalcon. OPNsense’s focus on security brings unique features such as the option to use LibreSSL instead of OpenSSL (selectable in the GUI).

The robust and reliable update mechanism gives OPNsense the ability to provide important security updates in a timely fashion.

## Backup your OPNsense configuration to your Nextcloud

In OPNsense you can backup your configuration directly and automatically to Nextcloud, using the backup feature. Every backup will be encrypted with the same algorithm used in the manual backup so it’s quite easy to restore to a new installed machine.

After set-up, the backup feature will run a first backup of the OPNsense configuration file. Then, if the configuration is subsequently changed, a new backup will be run. Only one backup is run per day after configuration changes.

### 1. Create a new user

Click on the user icon top right and click “Users”. In the new page, enter a username and a password into the boxes and click create to create a new user.

### 2. Create an Access Token

Close the modal dialog and remove the default files. Then open the Settings menu (also in the menu top right). Switch to security and generate an App password.

![Access Token](https://www.finecloud.ch/media/posts/69/Screenshot-2022-11-03-at-09.19.23.png)

Copy and store the generated password.

### 3. Install Nextcloud Backup Plugin

Go to *System ‣ Firmware ‣ Plugins* and install the *os-nextcloud-backup* plugin.

![Install Plugin](https://www.finecloud.ch/media/posts/69/Screenshot-2022-11-03-at-09.24.39.png)

### 4. Connect OPNsense with Nextcloud

Scroll to the Nextcloud Section in *System ‣ Configuration ‣ Backups* and enter the following values:

| Field               | Value                                      |
|---------------------|--------------------------------------------|
| Enable              | checked                                    |
| URL                 | Base URL of your Nextcloud installation like [https://cloud.example.com](https://cloud.example.com) |
| User                | your chosen username                       |
| Password            | paste your app password from step 2        |
| Encryption Password | define a Password to encrypt the config file |
| Backup Directory    | a name consisting of alphanumeric characters (keep default) |

![Connect OPNsense](https://www.finecloud.ch/media/posts/69/Screenshot-2022-11-03-at-09.32.01.png)

### 5. Verify the Configuration Upload

When everything worked, you will see the newly created directory after saving the settings:

![Verify Upload](https://www.finecloud.ch/media/posts/69/Screenshot-2022-11-03-at-09.38.41.png)

If you open it, you will see at least a single backed up configuration file:

![Configuration File](https://www.finecloud.ch/media/posts/69/Screenshot-2022-11-03-at-09.39.22.png)
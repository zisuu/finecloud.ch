---
title: "Threema Safe Backup to Nextcloud"
meta_title: "Threema Safe Backup to Nextcloud"
description: "Did you know that Threema allows you to backup your Threema app to a custom Threema Safe server? You can, for example, use your Nextcloud to keep your Threema backups safe."
date: 2022-10-13T07:42:00
categories: ["Tools"]
author: "David"
tags: ["android", "backup", "nextcloud", "tools", "threema"] 
draft: false
---

Did you know that Threema allows you to backup your Threema app to a custom Threema Safe server? You can, for example, use your Nextcloud to keep your Threema backups safe.

### Configure Nextcloud

I recommend creating a new Nextcloud user account only for the purpose of the Threema backup. If this account gets in the wrong hands, all other Nextcloud files are still safe. You can share your Threema backup folder with your regular Nextcloud user account if you want to have direct access to your Threema backups.

1. Create a new Nextcloud user, e.g. `threema-safe`, and generate a complex password with your favorite password tool.
2. Sign in with the newly created Nextcloud user and delete the example files. Create a new folder `threema-safe` with a subfolder `backups`.
3. In the `threema-safe` folder, create a config file (yes, no file-ending!) with the following content:
    ```json
    {
        "maxBackupBytes": 52428800,
        "retentionDays": 180
    }
    ```
4. If you have 2FA active (I hope you do), you need to generate a new app password. Go to *Settings -> Security -> Devices & sessions* and enter an app name like `threema-safe` and click "Create new app password", save the generated password in your password tool.

### Configure Threema app

1. Open your app, on the menu select "Backups".
2. Enable Threema Safe.
3. Choose expert settings, uncheck "use default server".
4. Provide your Nextcloud WebDAV URL: `https://yourserver.domain.com/remote.php/dav/files/threema-safe/threema-safe/`
5. Press "ok".
6. Generate and enter a strong password to protect your Threema Safe backup and click "Next". Your first backup should now be generated.

### Verify

You should now see a newly generated backup in your `threema-safe` folder. To share your backups with your personal Nextcloud user, you can now select the `threema-safe` folder in Nextcloud and press the "share" icon, add your username and you're done. Happy backing!

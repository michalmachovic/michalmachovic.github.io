---
layout: post
date:   2019-05-02 07:13:26 +0200
title:  "Magento2: Unlock admin account"
category: magento2
tags: [magento2, admin]
---

If you cant login to admin and see error `You did not sign in correctly or your account is temporarily disabled.`, you can try following:
<br />

`sudo php bin/magento admin:user:create --admin-user="USERNAME" --admin-password="PASSWORD" --admin-email="EMAIL" --admin-firstname="FIRSTNAME" --admin-lastname="LASTNAME"`

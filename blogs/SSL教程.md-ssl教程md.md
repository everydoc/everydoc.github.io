---
title: SSL教程.md
date: 2021-11-05 23:32:52.353
updated: 2021-11-05 23:32:52.353
url: https://halo.imjcker.com/archives/ssl教程md
categories: 
tags: 
---

# SSL教程



来源：https://cnodejs.org/topic/5be29f7c21d75b74609f4fbf



```shell
./certbot-auto certonly  -d *.imjcker.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```





```shell
[root@imjcker ~]# certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: imjcker.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 
Requesting a certificate for imjcker.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/imjcker.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/imjcker.com/privkey.pem
This certificate expires on 2021-12-31.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for imjcker.com to /etc/nginx/nginx.conf
Congratulations! You have successfully enabled HTTPS on https://imjcker.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
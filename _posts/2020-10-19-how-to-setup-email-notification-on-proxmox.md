---
layout: post
title:  "How you can set up the email notification on Proxmox"
date:   2020-10-19
category: [ blog, proxmox ]
---

Step by step guide on how to setup email on your [Proxmox](https://www.proxmox.com/en) host, in order to reseive email alerts.

<!--more-->

### Guide:

1. Install the authentication library

```console
apt-get install libsasl2-modules
```

2. If Gmail has 2FA enabled, go to [App Passwords](https://security.google.com/settings/security/apppasswords) and generate a new password just for Proxmox.

3. Create a password file

```console
nano /etc/postfix/sasl_passwd
```

4. Insert your login details (substitute `youremail@mail.com` and `yourpassword`).

```console
smtp.gmail.com youremail@mail.com:yourpassword
```

5. Save the password file

6. Create a database from the password file.

```console
postmap hash:/etc/postfix/sasl_passwd
```

7. Protect the text password file.

```console
chmod 600 /etc/postfix/sasl_passwd
```

8. backup the postfix configuration file.

```console
cp /etc/postfix/main.cf /etc/postfix/main.cf.backup
```

9. Edit the postfix configuration file.

```console
nano /etc/postfix/main.cf
```

10. Add/change the following (certificates can be found in /etc/ssl/certs/)

```console
relayhost = smtp.gmail.com:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_security_options =
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_tls_CAfile = /etc/ssl/certs/Entrust_Root_Certification_Authority.pem
smtp_tls_session_cache_database = btree:/var/lib/postfix/smtp_tls_session_cache
smtp_tls_session_cache_timeout = 3600s
```

11. Reload the updated configuration

```console
postfix reload
```

### Testing
```console
echo "test message" | mail -s "test subject" youremail@gmail.com
```

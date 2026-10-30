---
layout: default
title: Gotta Catch'em All - TryHackMe Writeup
---

[← Back to TryHackMe write-ups](/thm)

# Gotta Catch'em All

**Difficulty:** Easy  
**Room:** Based on the original Pokemon series. Can you obtain all the Pokemon in this room?

## Recon

I ran an nmap scan:

```
nmap -sV -sC -T4 10.10.70.161
```

The results came back quickly and showed ports 22 (SSH) and 80 (HTTP) open.

![nmap results](/THM/images/gotta/01.png)

## Discovery

Port 80 served a default Apache page. Nothing looked out of place until I checked the page source, which contained what looked like SSH credentials: the username `pokemon` and the password `hack_the_pokemon`. There was also an HTML comment telling me to check the console for an extra surprise, which turned out to be a list of Pokemon.

![Page source](/THM/images/gotta/02.png)

![Console output](/THM/images/gotta/03.png)

## Foothold

I tried the credentials over SSH and successfully logged in as `pokemon`.

![SSH login](/THM/images/gotta/04.png)

Running `ls` showed several folders. In the Desktop directory I found an interesting file called `P0kEmOn.zip`.

![P0kEmOn.zip on the Desktop](/THM/images/gotta/05.png)

I unzipped it with `unzip P0kEmOn.zip`, which created a `P0kEmOn` directory containing `grass-type.txt`. Reading it with `cat` showed what appeared to be hex.

![Hex in grass-type.txt](/THM/images/gotta/06.png)

I put this into CyberChef, which decoded it automatically and gave me the first flag.

![Decoded in CyberChef](/THM/images/gotta/07.png)

**Find the Grass-Type Pokemon:** `PoKeMoN{Bulbasaur}`

## Water-type

I guessed the next flag would have a similar file name, so I used `locate water-type.txt`. It was in `/var/www/html`, the web directory. Reading the file showed the flag was encoded.

![water-type.txt](/THM/images/gotta/08.png)

I tried hash-identifier, but it found no hash.

![hash-identifier](/THM/images/gotta/09.png)

CyberChef couldn't decode it automatically this time. I realised it was a cipher rather than a hash, so I used an online cipher identifier, which suggested ROT13.

![Cipher identifier](/THM/images/gotta/10.png)

Back in CyberChef I used the ROT13 recipe with the amount set to 14 and got the second flag.

![ROT decode in CyberChef](/THM/images/gotta/11.png)

**Find the Water-Type Pokemon:** `Squirtle_SqUaD{Squirtle}`

## Fire-type

I used the same approach with `locate fire-type.txt` and found it in an unusual directory, `/etc/why_am_i_here?/fire-type.txt`. The contents were base64, which was easy to spot from the `==` padding at the end.

![fire-type.txt](/THM/images/gotta/12.png)

I decoded it with `cat fire-type.txt | base64 -d` to get the third flag.

![base64 decode](/THM/images/gotta/13.png)

**Find the Fire-Type Pokemon:** `P0k3m0n{Charmander}`

## Privilege escalation

For the final flag I went back to `/home` and found another user, `ash`, along with the last flag file. I couldn't read it because I wasn't root.

![roots-pokemon.txt](/THM/images/gotta/14.png)

I also got permission denied when trying to enter ash's directory. To look for a way into ash's account, I searched the files I could read for any mention of ash with `grep -r "ash" . 2>/dev/null`. This revealed his password, and I logged in as ash.

![grep results](/THM/images/gotta/15.png)

![Logged in as ash](/THM/images/gotta/16.png)

I could then read `roots-pokemon.txt` and get the final flag.

![Final flag](/THM/images/gotta/17.png)

**Who is Root's Favorite Pokemon?** `Pikachu!`

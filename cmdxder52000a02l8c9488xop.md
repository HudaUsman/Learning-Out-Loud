---
title: "TryHackMe: Corridor CTF Writeup"
datePublished: Mon Aug 04 2025 17:14:47 GMT+0000 (Coordinated Universal Time)
cuid: cmdxder52000a02l8c9488xop
slug: tryhackme-corridor-ctf-writeup

---

Corridor is a TryHackMe room designed to demonstrate Insecure Direct Object Reference. It encourages us to analyze URL endpoints and note the hexadecimal values. Let’s dive into it.

Once the machine is up, let’s run a quick **nmap** scan.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754324381546/2bbf3683-f56e-47c0-97ec-522cd52aa76c.png align="center")

Only http is open. Let’s visit the IP address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754324499932/36ef0c82-5ed8-45b7-9a28-48ca0ec6384b.png align="center")

It takes us to a page like this. Let’s run **Gobuster** — it’s a great tool for discovering hidden directories and pages that aren't directly linked on the website.

**Note:**

Gobuster is a tool used for brute-forcing URLs, helping us discover hidden directories and files on web servers. It’s useful when websites don’t directly link to important pages.

Usage**:** `gobuster dir -u http://<target-ip>/ -w /usr/share/wordlists/dirb/common.txt`

We didn’t find anything there. Now let us check the source code of the page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754325097709/a0accc2d-7e0d-4663-8b4e-84282d5acbc5.png align="center")

We can find few hexadecimal values there. These might be the hashes hinted in the beginning of the room. Upon close inspection and confirmation from Hash Type Identifier — an online tool used to identify hashes — we can confirm that they are **MD5** hashes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754325402371/a648288b-8a5f-4438-9951-74256d2e0a7e.png align="center")

In the hints it was given that the hashes follow a pattern. So let’s crack the hash.

I pasted all the hashes in CrackStation — a free hash cracker.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754325811529/5a484706-d7bc-4a25-8a7e-a35f837b9f3f.png align="center")

And here — we have our result. It is indeed following a pattern.

Let’s try navigating through the website by appending the hashes at the end of the URL.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754325982302/6af343f8-d734-48d9-bdd0-9dea18b36972.png align="center")

Every time it took us to the same room. Since it followed this pattern - let us try appending the hashes of next numbers i.e. 14, 15, 16…

I generated the hashes from 14 to 30 using this python script. (We can also use online tools like CyberChef)

`import hashlib`

`for i in range(14, 30): h =` [`hashlib.md`](http://hashlib.md)`5(str(i).encode()).hexdigest() print(f"{i}: {h}")`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754326533314/68f1df82-d0b5-4d3f-ad0f-00dae420efb1.png align="center")

I tried appending the hashes one by one at the end of the URL and it didn’t work out.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754326702965/e987cb9b-a3af-4267-9a3c-1d0ee75eb926.png align="center")

After a few tries, I started getting confused — am I on the wrong path? The room didn’t seem very complex but I was missing something. Then I realized I haven’t tried for zero. Then I put in the hash for zero and YESSS — we found the flag there.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754327089966/1a5d662a-f122-44db-9f9c-f484a7797168.png align="center")

This was my first write-up, and Corridor turned out to be one of the easiest TryHackMe rooms I’ve completed so far. Despite its simplicity, it was a great hands-on introduction to **Insecure Direct Object Reference (IDOR)** vulnerabilities and reinforced the importance of URL-based access control.

By paying attention to subtle patterns in hash values and experimenting with different inputs, we uncovered the hidden flag — a small but satisfying win!
---
layout: post
title: Tryhackme Writeup for Chill Hack
---

Hello, Welcome Back Today I'm doing a write-up for the tryhackme room *Brooklyn Nine Nine* . This room is relatively easy and their are 2 way of solving it
so no pressure (っ＾▿＾)۶🍸🌟🍺٩(˘◡˘ ) have a drink

-------------------------------------------------------------------------------------------------------------------------------

First we do a normal nmap scan to check what services are up. (I try using threader3000 but it doesn't work for this room)


![nmap](https://github.com/CoolGuyWithTech/coolguywithtech.github.io/blob/master/_posts/Attachment/Chill%20Hack/Pasted%20image%2020211101160717.png)

So we that FTP, SSH and HTTP is open
<br>

I first check the website and check the source page and found out this 

![[website.png]]

It mention stenography for the picture *brooklyn99.jpg*

This is the other way of solving the room, so let's skip it for now

-------------------------------------------------------------------------------------------------------------------------------
**Jake Route**

<br>
Next, we login to the ftp server with anonymous login and we found a *Note_to_Jake.txt* so after using to **get** command to download the .txt 


![[note to jake.png]]

we found out that jake have a weak password, and possible username jake,holt and amy

So i try to use medusa to cracks jake ssh password

![[Attachment/Brooklyn99/medusa.png]]

And found the password

![[jake password.png]]

The password is `987654321`, Classic Jake
<br>
So i use pwncat to ssh into jake account 

![[pwncat.png]]

There is nothing in jake directory so i try to access holt and amy directory and in holt directory we found the user flag and a nano.save

![[Attachment/Brooklyn99/user flag.png]]

<br>
I try to cat the nano.save file but i don't have permission, so i thought using linpeas to see what i can use to PE but i then i remember to do some manually enumeration first

So i use sudo -l and find that we can use less with sudo 

![[less.png]]

So we go to gtfobins and see what we can find

![[gtfo.png]]

With that we got root and the root flag

![[Attachment/Brooklyn99/root flag.png]]

-------------------------------------------------------------------------------------------------------------------------------
**Holt Route**

Hope you haven't finish your drink, cause there is another way to solve this room  (•◡•) /
<br>
So remember the *brooklyn99.jpg* ? we try to use exiftool, binwalk and steghide to see if there is anything hiding

Exiftool and binwalk return nothing to interesting

![[exiftool.png]]

![[Attachment/Brooklyn99/binwalk.png]]

But on steghide we found extra info in the file but it needs a passphrase

![[steghide.png]]

So we use the tool stegcracker
https://github.com/Paradoxis/StegCracker

To crack the passphrase of the file

![[stegcracker.png]]

We found out the password is `admin`

and this reveal holt ssh password

![[holt password.png]]

lol

Next, i try sudo -l and found that holt can run nano

![[nano.png]]
<br>
There are 2 ways to get root flag with nano

First Way :

`sudo /bin/nano /root/root.txt` and view the flag
![[nano flag.png]]

Second Way :

So we go gtfobin

![[gtfo nano.png]]

I try so many times and it wouldn't work but in the end, i look how other people did it  

| Steps | Explanation                                                 |
| ----- | ----------------------------------------------------------- |
| 1     | type sudo nano                                              |
| 2     | press Ctrl+R                                                |
| 3     | press Ctrl+X                                                |
| 4     | type `reset; sh 1>&0 2>&0`                                  |
| 5     | press Enter                                                 |
| 6     | You will see a # and the screen will be a bit funny looking | 
| 7     | type clear and enter                                        |
| 8     | Boom!!! Root                                                |

Funny Screen : 

![[sudo nano.png]]
Type Clear and press Enter
![[clear.png]]



-------------------------------------------------------------------------------------------------------------------------------
So that's it for this Room its easy and fun hope you guys enjoy it as much as i do. Fluffy Signout~~ ʕ•́ᴥ•̀ʔっ

#Brooklyn99

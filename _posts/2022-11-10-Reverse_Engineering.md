---
layout: post
title: Reverse Engineering on Tryhackme
---

Hello~~ This is my first time doing a reverse engineering type room so if there are anything that i can improve please tell me.


So for the first crackme1.bin you could use strings to figure out the password

![[Pasted image 20211118174242.png]](https://i.imgur.com/PXqMFqW.png)

<br>


But you could also use ltrace to look at the password as well

![[Pasted image 20211118174335.png]](https://i.imgur.com/3yE22P1.png)

We can see the program is comparing the input with `hax0r`

<br>

Or another way is using ghidra, which is a little more tricker 

We go to the main function of the program and see what we can find

We found that there are 2 values and 1 of them or getting strcmp, there is another value as well, `local_18` which also contain a value. And having the philosophy of why put extra code into the program, so the 2 values must be the apart of the password.

![[Pasted image 20211118174740.png]](https://i.imgur.com/ieqpxuS.png)

And since assembly is in little Indian which means characters are in reverse order, so we must reverse the order of the values

![[Pasted image 20211118175922.png]](https://i.imgur.com/8MlyWtW.png)

With that, task 1 is finish


<br>

## Crackme2

So for the second task, i also did the same thing which is using strings and ltrace but it doesn't work


![[strings crackme2.png]](https://i.imgur.com/VvWiKjQ.png)

![[ltrace crackme2.png]](https://i.imgur.com/6ot1VVP.png)

So i launch ghidra and see what i can find

We can see that the program is comparing the input with a hex value

![[ghidra crackme2.png]](https://i.imgur.com/6uIN4jp.png)

So i go to cyberchef and see if its text, but the output is nothing special

![[cyberchef crackme2.png]](https://i.imgur.com/KJ2etAG.png)

So i just google the hex number into decimal and found `4988`

![[google crackme2.png]](https://i.imgur.com/5huWiss.png)

And we are done with crackme2

<br>

## Crackme3

I go the program through strings and ltrace and found nothing


![[strings crackme3.png]](https://i.imgur.com/gaPnz5C.png)

![[ltrace crackme3.png]](https://i.imgur.com/DtITx7d.png)

So i look it in ghidra and found the same code as task 1

![[ghidra crackme3.png]](https://i.imgur.com/IWz2ud9.png)

Which i just combine and reverse the output of the hex number and found `azt`

![[cyberchef crackme3.png]](https://i.imgur.com/bSbUDQh.png)

<br>


------------------------------------------
With that all task in of reverse engineering is done, the room itself is very fun and the challenge is not really hard which is very welcoming for beginners. All and all i hope you guys enjoy it as well. 

Fluffy Signout~~ ʕ•́ᴥ•̀ʔっ


Hello everyone, today i'm doing the jumpjumpjump crackme from [cbm-hackers](https://crackmes.one/user/cbm-hackers), hope you enjoy. 

Here the link for the crackme challenge : https://crackmes.one/crackme/5c1a939633c5d41e58e005d1

<br>

---
## Static Analysis

![](https://i.imgur.com/kmsE0Fd.png)


We need to input a string, and the system will tell us if the string is correct or wrong


<br>

![](https://i.imgur.com/RU8xVW6.png)

![](https://i.imgur.com/PSDXxTr.png)

Running ltrace doesnt give us much information, as well as strings, we need to put it in ghidra to find out what is happening

<br>



![](https://i.imgur.com/jdNFjBj.png)

Heres the code in ghidra. We need to clean up the code

<br>

![](https://i.imgur.com/r44wJ3Y.png)


After abit of time, i manage to clean the code.

<br>

Execution Flow:
1. Read input from user, 10 characters (input < 11).
2. if longer than 10 character, print: too long...sorry no flag for you!!!
3. Counting each ascii value of each character.
4. If the counter is equal to 1000, then the flag will be print.

After knowing what the program do, we can write a python file to create a string that matches the condition

---
## Solving

![](https://i.imgur.com/gqkrxVS.png)

From the ascii table we know that printable character starts from decimal 33 to 126, so we just need to create a script that could create the flag that is 9 character or less and will be equal to 1000 in value.

<br>


```python
import random

arr = []

total = 1000
counter = 0

while True:
	if counter == 9 :
		print(arr)
		print(sum(arr))
		break
	elif counter == 8:
		if total > 126:
			counter = 0
			total = 1000
			arr.clear()
		else:
			arr.append(total)
			print(arr)
			print(sum(arr))
			break
	tmp = random.randint(33, 126)
	total -= tmp
	counter += 1
	arr.append(tmp)

print("".join([chr(arr[x]) for x in range(len(arr))]))
```

I use this python script to create random flag to input

<br>


![](https://i.imgur.com/mlaG4QT.png)


![](https://i.imgur.com/WVRsqZo.png)

The flag doesn't work, so i try to troubleshot my script, after debugging it multiple times the the output is still wrong, around this time i realize that theres's nothing wrong with my script and i need to dig deeper with gdb.

---
## Troubleshooting

To ensure that there wasn't a problem with special characters, i run the binary in gdb and look for the ascii value count  

<br>

![](https://i.imgur.com/3zOFVkr.png)

Breakpoint at comparison

![](https://i.imgur.com/t5CHG4V.png)

Value of rbp-0x14

![](https://i.imgur.com/w2DSyJA.png)

<br>


I set a breakpoint on the comparison instruction and view the value of `$rbp-0x14`, and found out that the value was 0x3f2 which eqaute to 1010, which is very weird, so i started to test with other values

<br>


All of them return the same `0x3f2`, so we have a additional +10 in the system, checking with the ascii table, decimal 10 correspond to line feed, so my theory is that when entering our flag, the system also counted the line feed, so we have a additional +10 on the value. So we just need to minus 10 on the python script and it should be good.

<br>

---
## Solving....Again

```python
import random

arr = []

total = 990
counter = 0

while True:
	if counter == 9 :
		print(arr)
		print(sum(arr))
		break
	elif counter == 8:
		if total > 126:
			counter = 0
			total = 990
			arr.clear()
		else:
			arr.append(total)
			print(arr)
			print(sum(arr))
			break
	tmp = random.randint(33, 126)
	total -= tmp
	counter += 1
	arr.append(tmp)

print("".join([chr(arr[x]) for x in range(len(arr))]))
```

![](https://i.imgur.com/GInZV4c.png)

After tweaking the script, we successfully got the flag.

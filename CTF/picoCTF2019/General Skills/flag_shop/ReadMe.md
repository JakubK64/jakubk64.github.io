# flag_shop

### Description
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF%202019/General%20Skills/flag_shop/Task.PNG)

### Solution
We have simple program and its source code i C language. We can buy our flag here, but we don't have enough money.

When we analize source code we can see, that there is only one place when we can affect to "account_balance" (variable which contains our account ballance). When we try to buy "not real flag" we can input how many flag we want to buy. Program only checks if this number is grater than 0 and when it is -> total_cost = 1000*number_flags (simple multiplication to generate total cost of this transaction).

Next thing what program do is check if total_cost is less or equal with account_balance - for obvious reason. If it is program will do: account_balance = account_balance - total_cost - again, for obvious reason. But if we make total_cost as negative number our account balance will grow up instead of decrease.
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF%202019/General%20Skills/flag_shop/Solution2.PNG)

So we do it, increase our account balance and then buy flag in normal way:
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF%202019/General%20Skills/flag_shop/Solution1.PNG)

#### *Flag: picoCTF{m0n3y_bag5_325fcd2e}*

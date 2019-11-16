# whats-the-difference

### Description
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF%202019/General%20Skills/whats-the-difference/Task.png)

### Solution
We have 2 jpg files - kitters.jpg and cattos.jpg. We know, that we need to find difference between them.
First of all let's check difference between those 2 files using:
```unix
diff kitters.jpg cattos.jpg
```

Now when we are sure that there is some difference between them let's compare them. Simple "cmp" command will give us only 3 columns with data (decimal, octal). In secend column we can see that the decimal numbers have range similar to ASCII. Let's check it and try do deocde it using "gawk" command which allow us to use simple printf to print on screen converted data:

![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF%202019/General%20Skills/whats-the-difference/Solution.PNG)

We get the flag!

#### *Flag: picoCTF{th3yr3_a5_d1ff3r3nt_4a_bu773r_4nd_j311y_aslkjfdsalkfslkflkjdsfdszmz10548}*

# Aca-Shell-A

### Description
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/task.png)


### Solution

*Task have limited time, when we aren't enough fast we'll get disconected and we need to start everything from beginning*

When we connect to server we get something like welcome message. First of all we of course need to check what contain home directory here using:
```unix
ls -l
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution1.png)

As we can see we have 5 directories. When we check it we'll see that at this moment first 4 are empty. We need to change direction to "Secret" directory and check what is contain:
```unix
cd secret
ls -l
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution2.png)

We can find here a few file, 5 with name starts with "intel" and rest starts with "profile_".
We also get message to sabotage intel files.
We start doing it using:
```unix
rm intel_1
```

After delete first intel file we get message that we can "use file with exploit". We need to write on screen "Drop it in!"
```unix
echo 'Drop it in!'
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution3.png)

We get message that we get some file in "the only place we can execute from".
We need to go back to home directory and check which subdirectory can it be. Most likely it is "executable" directory. 
```unix
cd ..
ls -l
```

We go inside this directory and check if is there any file.
```unix
cd executables
ls -l
```

We can find here file named "dontLookHere":
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution4.png)

We need to execute this file, so we can do it using:
```unix
./dontLookHere
```

We get probably some data in base 16 system:
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution5.png)

We get message that we probably have our password in this data. Then we need to quick print our username on screen without using "echo" command. The easiest way to do it is to use:
```unix
whoami
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution6.png)

After that we get information that we have what we are looking for and we need to copy file called TopSecret from tmp directory to password directory. We use:
```unix
cp /tmp/TopSecret passwords
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution7.png)

Now we get information that system will shutdown in 10 seconds but we still need to read copied file. We fast use:
```unix
cd ..
cd passwords
```
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution8.png)

We find here our file, now we need to open and read it. We use:
```unix
cat TopSecret
```

Here we have some letter with our flag at the end of it:
![alt text](https://github.com/JakubK64/CTF-writeups/blob/master/picoCTF/Basics/Aca-Shell-A/solution9.png)

#### *Flag: picoCTF{CrUsHeD_It_dddcec58}*


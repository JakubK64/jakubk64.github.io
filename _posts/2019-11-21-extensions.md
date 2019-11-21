---
layout: post
title: EXTENSIONS (picoCTF2019)
published: true
tags: pico picoCTF Forensics extensions picoCTF2019
---

### Description

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Forensics/extensions/Task.png }}\CTF\picoCTF2019\Forensics\extensions\Task.png)

### Solution
We get .txt file. When we try to read it (in my case using "cat" in linux console) we can see that in contains some unreadable data. What is most important in first line we can find "PNG"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Forensics/extensions/Solution1.PNG }}\CTF\picoCTF2019\Forensics\extensions\Solution1.PNG)

Let's try to change .txt format to .png using simple:
```unix
mv flag.txt flag.png
```

No when we open png file we can see our flag. We can get it using 
```unix
gocr flag.png > tmp.txt
```
or just write it down from screen 

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Forensics/extensions/Solution2.PNG }}\CTF\picoCTF2019\Forensics\extensions\Solution2.PNG)

#### *Flag: picoCTF{now_you_know_a bout_extensions}*

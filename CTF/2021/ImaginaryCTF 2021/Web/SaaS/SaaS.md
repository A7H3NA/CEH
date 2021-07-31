# SaaS(Web) - Writeup

## Description

````
**Description**
Welcome to Sed as a Service! Now you can filter lorem ipsum to your heart's desire!

**Attachments**
https://imaginaryctf.org/r/C279-app.py
http://saas.chal.imaginaryctf.org

**Author**
Eth007
````

## answer

When you access the specified URL, you will see a web page where you can use the sed command to replace characters.

![SaaS_001-1.jpg](https://github.com/A7H3NA/CEH/blob/master/CTF/2021/ImaginaryCTF%202021/Web/SaaS/SaaS_001-1.jpg)


The input characters of the sed command will filter out the characters registered in the blacklist.

![SaaS_002-1.jpg](https://github.com/A7H3NA/CEH/blob/master/CTF/2021/ImaginaryCTF%202021/Web/SaaS/SaaS_002-1.jpg)

If you check the behavior while switching input characters, you will find the following.

・If you enter `"`, nothing will be displayed.`""` the contents of stuff.txt will be displayed. In other words, `""` closes the sed process.

・Add values to stuff.txt with `<<`. (`<` is NG.)

・The cat command cannot be used, so substitute the strings or echo command.

・You cannot use flag.txt, so substitute a regular expression for `*.txt.`

![SaaS_003-1.jpg](https://github.com/A7H3NA/CEH/blob/master/CTF/2021/ImaginaryCTF%202021/Web/SaaS/SaaS_003-1.jpg)

###### flag : ictf{:roocu:roocu:roocu:roocu:roocu:roocursion:rsion:rsion:rsion:rsion:rsion:_473fc2d1}

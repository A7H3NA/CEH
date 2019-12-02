# The Droid You're Looking For(Web) - Writeup

## Question

````
Sometimes you need a little Star Wars in your life to come across the flag.

http://chal.tuctf.com:30003
````

## Answer

Access the specified URL. Presumed to be a problem related to User-Agent.

````
root@kali:~# curl http://chal.tuctf.com:30003/
<html>
        <head>
                <title>Droids</title>
                <link href="index.css" rel="stylesheet" type="text/css">
        </head>
        <body>
        <h1 align=center>Google Chrome? Unfortunately More Common than Firefox (or Edge?)</h1>
          <hr>
        <p> You definitely won't find the flag right here, but you definitely might find it somewhere around here. Think like our lovely Google overlords do, and you'll soon have it in your grasp. Anyways, here's a pretty picture.
        <br><br>
        <img src=https://tinyurl.com/y6lkg5xh class=center>        
    </body>
</html>
````


The message changes when you access robots.txt.

````
root@kali:~# curl http://chal.tuctf.com:30003/robots.txt
<html>
        <head>
                <title>Not Quite</title>
                <link href="index.css" rel="stylesheet" type="text/css">
        </head>
        <body>
        <h1 align=center>Mmmm Do You Have the Right to View That?</h1>
          <hr>
        <p> I think you should look to Google to see how to view that page. Maybe you need some new agency to get that right.
    </body>
</html>
````


As a result of hard confirmation, robots.txt can be confirmed by specifying Googlebot.

````
root@kali:~# curl -H "User-Agent: Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" http://chal.tuctf.com:30003/robots.txt
User-agent: *
Disallow: googleagentflagfoundhere.html
````


If you check the URL described in robots.txt, flag is described.

````
root@kali:~# curl -H "User-Agent: Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" http://chal.tuctf.com:30003/googleagentflagfoundhere.html
<html>
        <head>
                <title>Hacked!</title>
                <link href="index.css" rel="stylesheet" type="text/css">
        </head>
        <body>
        <h1 align=center>TUCTF{463nt_6006l3_r3p0rt1n6_4_r0b0t}</h1>
    </body>
</html>
````

###### TUCTF{463nt_6006l3_r3p0rt1n6_4_r0b0t}

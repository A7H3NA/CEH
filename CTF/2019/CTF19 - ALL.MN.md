# CTF19 - ALL.MN Writeup

Participated in the GW during the GW. 21 questions out of 33 are 2400 points.
Write a simple Writup.

## Free point
Sainy Check. The problem statement contains a flag.
###### ALLMN{Y3S_TH1S_1S_Y0UR_FLAG_27SJAL}

## Discord
After opening the Discord, a flag is described in #flag.
###### ALLMN{TH4NKS_F0R_J01N1NG_US_2JKLPO}

## Saturn.MN
After accessing http://saturn.mn/p/425, the flag is described in the "үнэй дата" tab.
###### ALLMN{W3LC0M3_T0_S4TURN_MN}

## Easy Math
Problem of calculating the least common multiple (LCM) and the greatest common divisor (GCD) when a = 128 and b = 56. Just do it.
###### ALLMN{896_8}

## Funny Website
When I access the specified URL, I can see part of the flag commented out.
There is also a flag commented out in style.css and function.js, so connect them.
###### ALLMN{W3LL_Y0U_C4N_1NSP3CT_M3_I28DJA}

## Robots
Check robots.txt from the problem statement.

````
root@kali:~# curl http://chall.all.mn/robot/robots.txt
User-agent: *
Disallow: /allmnflag.php
````

Since there is allmnflag.php, access and check the flag.
###### ALLMN{W3B_R0B0TS_TXT_7DHJ9E}

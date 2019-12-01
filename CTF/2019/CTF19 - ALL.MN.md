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


## Question & Answer
rot24 the contents of answer.txt → rot8 "eqra_cpf_rcuvg_vjku".
Submit the "copy_and_paste_this" that came out to http://chall.all.mn/asuult/index.php?q=1 and check the flag.
###### ALLMN{Y3S_Y0U_D1D_1T_4Y7JKP}


## What is this?
Analyze it because it is brainf*ck.
###### ALLMN{1_L0V3_BR41N_F4CK_3IQKAP}


## Damn!
Analyze it because it is JSF*CK.
###### ALLMN{f4ckjs}


## Find that word
Among the many files there is a description of flag in 76.txt.
###### ALLMN{GR3P_1S_S1mple}


## Doesn't contain K integer
Calculate the integer number excluding 3 among the integers from 1 to 281939942. Since a similar problem is found when examining with google, solve it accordingly.
###### ALLMN{120597740}


## SQL
The login screen is displayed. The flag is displayed below.

````
username=' or 'a'='a' --
password=<Any>
````

###### ALLMN{W0W_Y0U_D1D_SQLi} 


## Button
http://chall.all.mn/button/
→ go to http://chall.all.mn/button/button1.php
→ go to http://chall.all.mn/button/button2.php

````
Noooo! You just clicked to wrong button and you let the dogs out.(失敗)
````

http://chall.all.mn/button/
→ flag is displayed by moving directly to http://chall.all.mn/button/button2.php (rewrite connection destination with local proxy).

````
YESSSS! You just clicked the exact right button!
Here you go, your wanted flag:
ALLMN{S0M30N3_L3T_TH3_D0GS_0UT_1ODJWK}
````

###### ALLMN{S0M30N3_L3T_TH3_D0GS_0UT_1ODJWK}


## My favorite number
Looking at the main function with gdb-peda etc. 0x4d1 (1233) is compared with the input value.Therefore, when 1233 is input, the flag is displayed. (The flag will also be 1233.)
###### ALLMN{1233}


## ROCK 
Parse the password attached to the PDF. I solved using john the Ripper.
The password for the PDF is 551731.
###### ALLMN{Y0U_DID_IT!}


## Windows XP
There is a specification to access from WindowsXP and IE from the problem statement.
Change User-Agent to Windows XP and IE and access it.
Example. User-Agent: Mozilla / 5.0 (MSIE 7.0; Windows NT 5.1; Win64; x64; rv: 66.0)]
###### ALLMN{CH4NG3_TH3_US3R_AG3NT_WD8KKA}


## Important files
Extract files from images with binwalk -e.
Among the extracted files, wow.txt has the following description.

````
flag is: NYYZA{J3_Y0I3_Z0AT0Y1N_2UAQ8W}
````

After that rot13 and check the flag.
###### ALLMN{W3_L0V3_M0NG0L1A_2HND8J}


## My Computer
Confirm the image file with the strings command.

````
root@kali# strings laptop.jpg | grep {
?sF%{
2{_K
w{W<
{Kc4x
{Kc4x
{Kc4x
{Kc4x
{Kc4x
{Kc4x
{Kc4x
{Kc4x
(O\{
A0K{d
{shkCeB
bI:L{uV
{H3X_F4N}
'{UzR
a sydh sja A L L M N { H 3 X _ E D 1 T 1 N G _ W 4 S _ F U N _ 2 J D N 8 J }Q(
````

Among the above, {H3X_F4N} is a flag. Is the dummy at the bottom?
###### ALLMN{H3X_F4N}


## What is your name?
The entered name is compared with the value, and the flag is displayed if the name is correct.

````
test eax, eax
jnz short loc_8E8
````

Compare values with test and branch to the abend path when it terminates abnormally
Therefore, change the jnz instruction of 000008c9 to nop and always branch to normal end.

````
root@kali# ./name_patch 
Chamaig hen gedeg ve?
123
Sain uu? ALLMN{H3LL0_D4MN_D4N1EL_W73JFL}
````

※ When the internal processing is followed, the input value is compared with DAMN_DANIEL of the RSI register. Therefore, the flag is displayed even if DAMN_DANIEL is input. (Because flag enters register as it is, the problem itself can be solved even if it does not get there ...)
###### ALLMN{H3LL0_D4MN_D4N1EL_W73JFL}


## President's panel
Set the following to cockie from the provided sources.

````
sanchir　=yeahsanchirispassword
usernameallmn =19b1850feff49657ded09d2a6f9e904d
````

With the above implemented, enter the following on the login screen and POST.

````
username=sanchirisusername
password=yeahsanchirispassword
````

→flag is displayed.

````
You have entered. Flag: ALLMN{C00K13_1S_WH4T_Y0U_N33D_D6AJSD}
````

###### ALLMN{C00K13_1S_WH4T_Y0U_N33D_D6AJSD}


## Admin panel
Crack the provided shadow file with john the Ripper .
You can see that the root user's password is "hellokitty".
Execute login and enter root / hellokitty to check the flag.

````
# ./login 
Sain uu? Ta nevtreh ner bolon nuuts ugee oruulna uu! CTF19.ALL.MN Admin Login
Nevtreh ner: 
root
Nuuts ug: 
hellokitty
ALLMN{G00DJ0B_Y0U_AR3_T00_G00D_SHD8WK}
````

###### ALLMN{G00DJ0B_Y0U_AR3_T00_G00D_SHD8WK}


## The problem I wanted to solve
* ROCK-2 (Unknown after opening docx with password of ROCK-1. Whitespace?)
* New password (Can not catch up with the behavior after the input value is set to cockie ...)
* Button 2 (in the first place unknown)]


## General
・I was not good at reverse problem, so it was good to be able to study how to use gdb-peda and IDA on such a simple problem like this one. 
・Since the problem of the web question is not solved, I would like to review it including how to solve it.]

# Movie-Login-3(Web) - Writeup

## description

````
By Yusuf

I think the final addition to the Gerard series is coming out! I heard the last few movies got their poster leaked. I'm pretty sure they've increased their security, though. Could you help me find the poster again?

http://web.bcactf.com:49162/

Hint 1 of 2
Does there seem to be anything different about this problem?

Hint 2 of 2
How can you get around the new keywords being detected?
````


## answer

The number of characters to be filtered listed in denylist.json has increased.

````
[
    "and",
    "1",
    "0",
    "true",
    "false",
    "/",
    "*",
    "=",
    "xor",
    "null",
    "is",
    "<",
    ">"
]
````

As with Movie-Login-2, you can log in with ' or 'a' like 'a' --.


###### bcactf{gu3ss_th3r3s_n0_st0pp1ng_y0u!}

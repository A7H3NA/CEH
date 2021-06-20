# Movie-Login-2(Web) - Writeup

## description

````
By Yusuf

It's that time of year again! Another movie is coming out, and I really want to get some insider information. I heard that you leaked the last movie poster, and I was wondering if you could do it again for me?

http://web.bcactf.com:49153/

Hint 1 of 2
What steps are they taking to prevent an injection?

Hint 2 of 2
Check the denylist maybe?
````


## answer

Same SQLinjection problem as Movie-Login-1. The following characters are filtered.

`1,0,/,=`

The = is replaced by the like clause.

````
' or 'a' like 'a' -- can be used.
````

###### bcactf{h0w_d1d_y0u_g3t_h3r3_th1s_t1m3?!?}

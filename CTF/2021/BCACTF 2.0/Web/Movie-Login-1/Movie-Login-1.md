# Movie-Login-1(Web) - Writeup

## description

````
By Yusuf

I heard a new movie was coming out... apparently it's supposed to be the SeQueL to "Gerard's First Dance"? Is there any chance you can help me find the flyer?

http://web.bcactf.com:49160/

Hint 1 of 1
Are the inputs sanitized?
````


## answer

When you access the specified URL, a screen for entering your user name and password will be displayed.

There is a sql injection vulnerability, which allows users to bypass login authentication by entering the following parameters and log in to the screen with the flag.

````
' or 'a' = 'a' -- 
```

###### bcactf{s0_y0u_f04nd_th3_fl13r?}

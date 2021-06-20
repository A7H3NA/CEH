# Wasm Protected Site 1(Web) - Writeup

## description

````
By Andrew

Check out my super safe website! Enter the password to get the flag

http://web.bcactf.com:49157/

Hint 1 of 1
How does the Web Assembly check the password you entered, and what is it looking for?
````


## answer

If you access the main.js in the source code, you will find the url /code.wasm.

When you access /code.wasm, WebAssenbly will be downloaded. The flag is shown in the downloaded file.

If you enter WASMP4S5W0RD in the password check, the flag is displayed.

###### bcactf{w4sm-m4g1c-xRz5}

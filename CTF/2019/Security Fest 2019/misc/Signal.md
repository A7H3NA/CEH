I participated in the team. Since it was held on weekdays as a private question, I wrote Writeup.

### Signal (misc)

````
There is no signal, everything is silent.Nothing is impossible.
````

When the problem file is decompressed, the signal.pdf file is expanded.
The keyboard part is clearly suspicious ...
Assuming a morse code and connecting them, the following message is displayed.
![Signal](https://github.com/A7H3NA/CEH/blob/master/CTF/2019/Security%20Fest%202019/misc/Signal.jpg)

````
... - .-. .- -. --. . .-.-.- -.- . -.-- -... --- .- .-. -.. .-.-.- ... . -.-. ..-. .-.-.- -- .---- .-.. .---- - ....- .-. -.-- ..--.- --. .-. ....- -.. ...-- ..--.- ...-- -. -.-. .-. -.-- .--. - .---- ----- -. .-.-.- .-- .- .. - .-.-.- -.-. --- ..- .-.. -.. .-.-.- .-. .- -. -.. --- --
````

When analyzed, the following message is displayed.

````
strange.keyboard.secf.m1l1t4ry_gr4d3_3ncrypt10n.wait.could.random
````

In this message, sctf {m1l1t4ry_gr4d3_3ncrypt10n} becomes flag.
###### sctf {m1l1t4ry_gr4d3_3ncrypt10n}

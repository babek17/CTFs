# Cookie Monster Secret Recipe | picoCTF

## Analysis
When launching the website, we get two inputs: username and password. First, I tried to inspect the page to see if I have something useful. I found that this login page goes to `login.php`. Nothing else. Then I just tried to **admin** username and password to see what input I get. The page said:

Access Denied

Cookie Monster says: 'Me no need password. Me just need cookies!'

Hint: Have you checked your cookies lately?

Go back

## Solution
The output after wrong password says it doesnt need an password and we should check the cookie. To check cookie we can use Burpsuite. In burpsuite, after pressing `login` we get the following output

```bash
POST /login.php HTTP/1.1
Host: verbal-sleep.picoctf.net:49398
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Origin: http://verbal-sleep.picoctf.net:49398
Connection: keep-alive
Referer: http://verbal-sleep.picoctf.net:49398/
Cookie: secret_recipe=cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzc4QjRDMzkwfQ%3D%3D
Upgrade-Insecure-Requests: 1
Priority: u=0, i

username=admin&password=admin
```

As you can see we have some Cookie header here with some value. I tried to use Cyberchef to check the following cookie value for different types of popular encoding. ROT13 did not work. BUT! Base64 worked like a charm and we got our flag for this CTF!


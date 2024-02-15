<h1>Hit the gym bro</h1>
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/image2.PNG>

on the website we got login and signup forms
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture7.PNG>
First, I tried some SQL injection, but nothing was useful there. So I made an account and logged in with that account. and we got: 
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture8.PNG>

So looking at the cookie value, we found that it was a JWT token.
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture9.PNG>

After that, I tried to break the JWT secret, but we got nothing.
then I tried some usual attacks like null, none.
and we were successful on
 ```jwt none Algorithm attack```

and the way to exploit is by using jwt_tool:
```python3 jwt_tool.py <jwt-here> -X a```
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture10.PNG>

I also changed our roles from user to admin.
submitting the tampered JWT, we got:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture11.PNG>

then tried to play with the values of username,email, blog, or upload fields, but all of that was for nothing.
Then I changed the value of
 ```http://51.12.211.165:8500/app/user``` to ```http://51.12.211.165:8500/app/admin```

and we got the flag :)
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture12.PNG>

Flag: ```securintes{35c4p3_d1llu510n5_637_600d}```





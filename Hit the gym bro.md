<h1>Hit the gym bro</h1>
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/image2.PNG>

on the website we got login and signup forms
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture7.PNG>
first i tried some sql injection but nothing was usefull there. so i made an account the logged in 
with that account. and we got:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture8.PNG>

so looking at the cookie value we found that it was jwt token.
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture9.PNG>

after that i tried to break the jwt secret but we got nothing.
then i tried some usuall attacks like null,none
and we got successful on ```jwt none Algorithm attack```

and the way to exploit is by using jwt_tool:
```python3 jwt_tool.py <jwt-here> -X a```
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture10.PNG>
i also changed our role from user to admin then
submitting the tampered jwt we got:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture11.PNG>

then tried to play with the values of username,email,blog or upload fields but all of that was for nothing.
then i changed the value of ```http://51.12.211.165:8500/app/user``` to ```http://51.12.211.165:8500/app/admin```

and we got the flag :)
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture12.PNG>

Flag: ```securintes{35c4p3_d1llu510n5_637_600d}```





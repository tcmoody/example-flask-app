### How to Use
1. Open a terminal and use the `cd` command to get into your application folder
2. Run `. .venv/bin/activate` in your terminal
3. Run `flask --app <app-name> run`

<app-name> Corresponds to the name of the python file where this code snippet exists:
```
from flask import Flask

app = Flask(__name__)
```
So if our python file was named `hello.py` the command we would run to execute the code is `flask --app hello run`

### File/Folder Structure
 - app and routes should exist within <app-name.py>
 - all html files are housed under the templates folder
 - you can use <blank>_service.py files to house helper functions or to split your code so you don't have massive methods

### Examples
Route Example:
```
@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

HTML Example:
```
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', person=name)
```
**Important to note: the `render_template` function looks for html files under the `templates` folder by default!**
**Also important to note: the `@app` decorator is only useable in the same file as where you generate the app with this line: `app = Flask(__name__)`**
### Extending Your Application
As your codebase grows you might want to split up your methods so that things are easier to test or read. One way of structuring
things is to use <blank>_service.py files to house helper methods. 

E.G.:
You're writing a character builder for <insert_rpg_here>. You have a microservice that is responsible for managing class selection and name selection. You
would likely have endpoints for `getName` and `getClass`.
```
@app.route("/getName/")
def getName():
    return someGarbageName

@app.route("/getClass")
def getClass():
    return someGarbageClass
```
Let's say that for your application to return `someGarbageName` you need to do some additional work (maybe pick from a selection of first/last names). You could split your code so that the majority of that work is performed in a function in a <name_service.py> file, and just have the getName function associated to the route call to it. Pseudo-code below:

<app_name>.py
```
@app.route("/getName/")
def getName():
    name = generateName()
    return name
```

<name_service>.py
```
def generateName():
    first = selectFirstName()
    last = selectLastName()
    return first + last
```

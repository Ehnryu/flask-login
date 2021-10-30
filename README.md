Website on <a href="https://flask-login.eris9.repl.co"> HERE </a>

```python
from flask import Flask, render_template, request
import json
import requests # imports all needed modules

app = Flask(__name__)


@app.route("/")
def index():
  return render_template("index.html") # returns templates/index.html as homepage

@app.route('/login/',methods=['GET', 'POST'])
def login():
	action = request.args.get('action') # get action
	if request.method == 'POST': # get method
		usr = request.form.get('username') # get data from form
		pwd = request.form.get('password')
		with open("data/login/login.json","r") as f: # load json
			login = json.load(f)
		if action == "removemydata":
			if str(usr) not in login:
				return "You never signed up"
			if str(usr) in login:
				del login[usr]["password"]
				del login[usr]
				with open("data/login/login.json","w") as z:
					json.dump(login,z)
				return "deleted your data..."				
		if str(usr) in login:
			if pwd == login[usr]["password"]:
				return render_template("index.html")
			if pwd != login[usr]["password"]:
				return "Wrong password"
		else:
			login[usr] = {}
			login[usr]["password"] = pwd
			with open("data/login/login.json","w") as z:
				json.dump(login,z)
			return f"Signed up, your login details are {usr} and {pwd}"
		
	return """
	<!DOCTYPE html>
	<html lang="en" dir="ltr">
		<head>
			<meta charset="utf-8">
			<title>Login</title>
			<link rel="stylesheet" href="static/style.css">
			<link href="https://fonts.googleapis.com/css?family=Coiny" rel="stylesheet">
			<link href="https://fonts.googleapis.com/css?family=Libre+Baskerville" rel="stylesheet">
			<script src="https://cdnjs.cloudflare.com/ajax/libs/brython/3.8.8/brython.js" integrity="sha256-rA89wPrTJJQFWJaZveKW8jpdmC3t5F9rRkPyBjz8G04=" crossorigin="anonymous"></script>

			<script src="https://cdnjs.cloudflare.com/ajax/libs/brython/3.8.8/brython_stdlib.js" integrity="sha256-Gnrw9tIjrsXcZSCh/wos5Jrpn0bNVNFJuNJI9d71TDs=" crossorigin="anonymous"></script>
	</head>
		<body>
			<form method = "POST" class="container">
				<h1>LOGIN</h1>
				<div><label>Username: <input type="text" name="username"></label></div>
        <div><label>Password: <input type="text" name="password"></label></div>
				<input type="submit" value="Login">
			</form>
		</body>
	</html>
	"""
@app.route('/removemydata',methods=['GET', 'POST'])
def removemydata():
	with open("data/login/login.json","r") as f:
		login = json.load(f)

@app.route('/form-example', methods=['GET', 'POST'])
def form_example():
    # handle the POST request
    if request.method == 'POST':
        language = request.form.get('language')
        framework = request.form.get('framework')
        return '''
                  <h1>The language value is: {}</h1>
                  <h1>The framework value is: {}</h1>'''.format(language, framework)

    # otherwise handle the GET request
    return '''
           <form method="POST">
               <div><label>Language: <input type="text" name="language"></label></div>
               <div><label>Framework: <input type="text" name="framework"></label></div>
               <input type="submit" value="Submit">
           </form>'''
@app.route("/test")
def test():

	if request.method == 'GET':
		language = request.args.get('usr')
		framework = request.args.get('pwd')

		return f'''<h1>The language value is: {language}\nThe framework is {framework}</h1>'''

        
if __name__ == '__main__':
  app.run(host='0.0.0.0', port=8080)
  app.run(debug=True)
```

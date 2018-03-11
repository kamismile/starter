# Flask

## envirionment setup

```shell
mkdir fooProject
cd fooProject
virutalenv env
cd env
cd Scripts
./activate
# (env) /fooProjects
pip install flask
mkdir -p static/{css,js} templates

```

## first flask web app

```shell
touch app.py

from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
	return '<h1 style="color: red">hello flask</h1>'
	
	
if __name__ == '__main__':
	app.run()
```


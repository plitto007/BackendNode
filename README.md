# Python Flask + uWsgi
Use Python3
* Create and enter project directory (assume name "py_demo")
```
mkdir py_demo
cd py_demo
```

* Setup virtual env
Assume the name of virtual env is "manh-env"
```
python3 -m venv manh-env
source manh-env/bin/active
```

* Setup Flask + uwsgi
```
pip3 install uwsgi
pip3 install flask
```

* Create a sample python Flask app

- Create python file
```
nano py_demo.py
```
- Enter content of this file
```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "<h1 style='color:blue'>Hello There!</h1>"

if __name__ == "__main__":
    app.run(host='0.0.0.0')

```
- Setup environment of Flask app and Run
```
export FLASK_APP=app
flask run
```
* Run with uWSGI

```
uwsgi --socket 0.0.0.0:5000 --protocol=http -w py_demo --callable app
```

 

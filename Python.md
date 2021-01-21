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

 # Python Flask + uWsgi + nginx + docker compose
* Create and enter app dir (we set "flask_uwsgi_docker_nginx")
```
mkdir flask_uwsgi_docker_nginx
cd flask_uwsgi_docker_nginx
```
## Setup the Python Flask app
* Create "flask" directory for python flask app
```
mkdir flask
cd flask
```
* We create a dir name "app" for python app code
```
mkdir app
cd app
```

* Init python file
```
touch __init__.py
```

content:
```python

from flask import Flask
app = Flask(__name__)

from app import views
````
* Python file for views
```
touch views.py
```


```python
from app import app
@app.route("/")
def index():
   return ":Hello from Flask"
```
* Go back to "flask" directory for next setup
```
cd ..
```

* Create the execute file for python app

Set the name "run.py"
```
touch run.py
```

```python
from app import app

if __name__ == "__main__":
    app.run()
```

* Create Dockerfile
```
touch Dockerfile
```

```python
# Use the python:3.7.2-stretch image
FROM python:3.7.2-stretch


# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install the dependencies
RUN pip3 install -r requirements.txt

# Run the command to start uWSGI
CMD ["uwsgi", "app.ini"]
```

* Create requirements.txt

This file declare the python module need to be installed with "pip"

```
touch requirements.txt
```

```
click==7.1.2 # don't need this, can remove
Flask==1.1.2
itsdangerous==1.1.0 # don't need this, can remove
Jinja2==2.11.2 # don't need this, can remove
MarkupSafe==1.1.1 # don't need this, can remove
uWSGI==2.0.19.1
Werkzeug==1.0.1 # don't need this, can remove
flask_sqlalchemy # use for db, currently not use
psycopg2-binary # use for db, currently not use
```

* Create "app.ini"

We will use this file for store config of uWSGI

```
touch app.ini
```

```ini
[uwsgi]
wsgi-file = run.py # define file to run
callable = app  # module
socket = :8080 #protocol + port
processes = 4
threads = 2
master = true
chmod-socket = 660
vacuum = true
die-on-term = true
```

## Setup nginx
* Back to nginx folder

```
cd .. 
cd nginx
```

* Setup Dockerfile

```
touch Dockerfile
```

```Dockerfile
# Use the nginx image
FROM nginx

# Remove the default nginx.conf
RUN rm /etc/nginx/conf.d/default.conf

# Replace with your own nginx.conf
COPY nginx.conf /etc/nginx/conf.d/
```

* Nginx config file

```
touch nginx.conf
```

```conf
server{
    listen 80; # listen at port 80

    location / {
        include uwsgi_params;
        uwsgi_pass flask_manh:8080; #route request from contaner "flask_manh" and port 8080
    }
}
```

## Docker compose
* Back to parent folder
```
cd ..
```

* Setup docker-compose file
```
touch docker-compose.yml
```

```yml
version: "3.4"

services:

  # Define your individual services
  flask:
    # Build the flask service using the docker file in "flask" directory
    build: ./flask
    image: fask_manh
    # Give our flask container a friendly name
    container_name: flask_manh

    restart: always
    environment: 
      - APP_NAME=MyFlaskApp
    expose: 
    - 8080
  
  nginx:
    build: ./nginx
    container_name: nginx_manh
    image: nginx_manh
    restart: always
    ports: 
     - "80:80"
```

# Python-uwsgi: Connection reset by peer
* in setting of uwsgi (eg: app.ini)
```
[uwsgi]
wsgi-file = run.py
callable = app
http = :5000
# http-workers = 2
# http-buffer-size = 32768
processes = 8
threads = 4
master = true
chmod-socket = 660
vacuum = true
die-on-term = true
listen = 1024
# harakiri=50
# offload-threads = 4
# enable-threads = true
# buffer-size = 32768
# post-buffering = 1
```

The point is increase processes and threads
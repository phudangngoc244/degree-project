## Getting started

#### Install library to install virtual enviroments

Follow this link to [https://virtualenv.pypa.io/en/latest/installation.html](https://virtualenv.pypa.io/en/latest/installation.html) install.

For fast
```
    python -m pip install --user virtualenv
```

#### Install virtual enviroments

* Windows
```
    python3 -m venv virtual_name
```

* Mac/Linux
```
    virtualenv virtual_name
```

#### Active virtual enviroments

* Windows
```
    .\virtual_name\Script\activate
```

* Mac/Linux
```
    source venv/bin/activate
```

#### Intall libraries
```
    pip install --upgrade pip
    pip install -r requirements.txt
```

#### Make Database
```
    python manage.py makemigrations
    python manage.py migrate
```

#### Create Super User 
```
    python manage.py createsuperuser
```
Then type username, and password

#### Run project
```
    python manage.py runserver
```

Go to [http://localhost:8000/](http://localhost:8000/) and explore.

#### EXTRA: Deactive virtual enviroments

* Windows
```
    .\virtual_name\Script\deactivate
```

* Mac/Linux
```
    deactivate
```

## Interact with docker

#### Build Image
```
    docker image build -t imagename:version . 
```

#### Run container
```
    docker container run -p -d external_port:internal_port image_name
```
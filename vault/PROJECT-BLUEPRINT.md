---
id: pr6PIrg04IqmwQDQki8TD
title: PROJECT-BLUEPRINT
desc: ''
updated: 1625393863515
created: 1624264141343
---

# Typical Project structure:

.
├── app.py ( called to start the server )
├── migrations
│   ├── README
│   ├── alembic.ini
│   ├── env.py
│   ├── script.py.mako
│   └── versions
│       ├── 4a4cf04e46af_created_puppies_and_owners.py
├── myproject
│   ├── __init__.py    ( **_I need this for 1. configurations and for 2. Blueprint registration of each views.py_** )
│   ├── data.sqlite
│   ├── models.py **_( are the DB tables )_**
│   ├── owners
│   │   ├── forms.py
│   │   ├── templates
│   │   │   └── owners
│   │   │       └── add_owner.html
│   │   └── views.py
│   ├── puppies
│   │   ├── forms.py
│   │   ├── templates
│   │   │   └── puppies
│   │   │       ├── add.html
│   │   │       ├── delete.html
│   │   │       └── list.html
│   │   └── views.py
│   └── templates
│       ├── base.html
│       └── home.html
└── requirements.txt

---

## Imports for average  Flask Project:

```python

# pip3 install <module>... or use a requirements.txt file: 
# ex. pip3 install flask

# for DB
import os
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

# for views:
from flask import Blueprint,render_template,redirect,url_for

```

## Create a Flask ENV in Conda:

```python
conda create -n flaskAllEnv python=3.6
conda activate flaskAllEnv

# P.S.
If you have already requirements.txt for modules, run:
pip3 install -r requirements.txt

flask db init
flask db migrate -m "created xyz...”
flask db upgrade
flask run 3 or python3 <filename>.py


# CONDA to list all envs:
Conda env list
```

---

## Create a virtual env in Python

commands
```python
# from parent folder location: run
python3 -m venv <folder-project-name>
# it will appear a file called pyvenv.cfg
#from parent folder run 
. <folder-project-name>/bin/activate
# and again from same location run: pip3 install flask
# re-enter folder and set up loc env:
#run: 
export FLASK_APP=<server-name>.py
#run: 
export FLASK_ENV=development (for local dev env)
# .html files go into a 'templates' folder and css/js go into 'static' folder
```

VSCode
env in VSCode you can choose from blue bar at the bottom


---


## How to run a Flask app (3 steps):

Set up environmental variables:

```zsh
export FLASK_APP=<main-file-to-run>.py

export FLASK_ENV=development 
# or just
app.run(debug=True)  # in server… to run the dev mode

pip3 install -r requirements.txt
```

---

## Issue to env/localhost already running on that port:

```zsh
ps -fA | grep python
Kill -9 <process number>
```

## Jinjia:

DRY:
- url_for get the name of the function

## SQLlite DB and Migrations:

ORM object relational mapper,
let connection between Flask and SQL DB via Python

```zsh
pip3 install Flask-SQLAlchemy
```

How to update change once created a db in Flask?
To migrate new addition/changes import 

```python
export FLASK_APP=<server-name>.py
# Flask-migrate:
# Sets up the migrations directory
flask db init # it create a data.sqlite file

# to Sets up the migration file
flask db migrate -m "some message" # Migrations folder creation

# to Updates the database with the migration
flask db upgrade

# And then, everytime I change something in the model, run:
flask db migrate -m “new message for updates“
flask db upgrade
```

---


## CRUD operation in DB - REST Representational State Transfer

Rest is supposed to be STATELESS, servers are stateless (they don't remember, every request is a new request...)

Python use `dict` to send data but via the HTTP we need JSON so the module `jsonify` can convert `dict -> json`

Ex. from GitHub: [reference](https://github.com/criscara-dev/simple-flask-api)

- [x] is REST supported in the APP? -> RESTful API

1. Postman
2. REST verbs:
    1. ├── POST
    2. ├── GET
    3. ├── PUT
    4. ├── DELETE

3. Auth
4. Implement a RESTful API web App

CRUD IN A SIMPLE WAY within ORM (not raw DBs SQL):
* C 
    ```python
    new_instance = Class('...',...)
    db.session.add(new_instance)
    db.session.commit()
    ```
* R
    ```python
    Class.query.all() or ... other ORM options
    ```
* U
    ```python
    Class.query.get(Id).property = <new_value>
    db.session.add(Class.query.get(Id).property)
    db.session.commit()
    ```
* D
    ```python
    Class.query.get(Id)
    db.session.delete(Class.query.get(Id))
    db.session.commit()
    ```


## Auth:

```python

from flask_bcrypt import Bcrypt
from werkzeug.security import generate_password_hash, ...
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user
```
---

## OAuth:

To Auth from an external service like Google, FB etc.

OAth 2.0
Flask_Oath
[Flask-Dance](https://flask-dance.readthedocs.io/en/latest/)

```python
from flask_dance.contrib.github import make_github_blueprint, github
```

---

## Designing an API

- Test first API Design:
start in Postman by creating the API url:
1) create a collection:
2: define the purpose of each path:

methiod | path | description 
-----|----- | -----
GET | `http://localhost:5000/item`  | this should return a list of items, each in JSON format 
GET |`http://localhost:5000/item/<string:name>`  | this will return a specific item, uniquely identify by its name. 
POST | `http://localhost:5000/item`  | this should return a list of items, each in JSON format 
DELETE |`http://localhost:5000/item/<string:name>`  | this will delete the specific item, uniquely identify by its name. 
PUT |`http://localhost:5000/item/<string:name>`  | this will update the specific item, uniquely identify by its name. If it exists

In a POST/PUT request you need to add:
1. Headers: content-type: application/json
2. the body: 
    ```{
         "example:":"value here"
         ...
         }
    ```

---

## how to make packages: `__init__`

create a __init__.py in a folder ( a normal folder is not a package)

Folders
* Models: internal representation (helper)
* Resources: external representation


---

### REST API basic process

#### 1. starting template

```python
# app.py
from flask import Flask
from flask_restful import Resource, Api

app = Flask(__name__)
api = Api(app)

class ListItem(Resource):

    def get(self,name):
        pass

    def post(self,name):
        pass

    def delete(self,name):
       pass
    
    def put(self,name):
       pass

class Lists(Resource):
    pass

api.add_resource(ListItem,'/ListItem/<string:name>')
api.add_resource(Lists,'/ListItems/')

if __name__ == '__main__':
    app.run(debug=True)
```

> The Resource Object is the One that let us create Flask-RESTful Classes

#### Test API endpoints on Postman

Easy to do; no auth require yet
```python
# key/value to add
Content-Type / Application/json
```


#### 2. Who can access the API?
everyone, so let's add:
[Flask-JWT](https://pythonhosted.org/Flask-JWT/) for authentication
```zsh
pip3 install Flask-JWT
```
and then add the authentication from JWT:
```python
from flask_jwt import JWT, jwt_required, current_identity
from werkzeug.security import safe_str_cmp
from user import User

# external imported here:
# user.py
class User(object):
    def __init__(self, id, username, password):
        self.id = id
        self.username = username
        self.password = password

    def __str__(self):
        return "User(id='%s')" % self.id

users = [
    User(1, 'user1', 'abcxyz'),
    User(2, 'user2', 'abcxyz'),
]

username_table = {u.username: u for u in users}
userid_table = {u.id: u for u in users}

def authenticate(username, password):
    user = username_table.get(username, None)
    if user and safe_str_cmp(user.password.encode('utf-8'), password.encode('utf-8')):
        return user

def identity(payload):
    user_id = payload['identity']
    return userid_table.get(user_id, None)
# end user.py

# and add those too:
app.config['SECRET_KEY'] = 'super-secret'
jwt = JWT(app, authenticate, identity)

```
Class User that has authenticate and identity functions

```python
# app.py

# How we make sure that auth is required to people trying our API? -> Use the decorator

# ...
class ListItem(Resource):

    @jwt_required()
    def get(self,name):
        pass
# ...
```
#### Test API endpoints on Postman

This time navigate to the path create automatically by JWT: `/auth`

- create a body:
```python
    {
        'username':'user1',
        'password':'abcxyz'
    }

# and pass the auth token...
```

#### 3. Create a: Model in Flask App (using the ORM Flask-SQLAlchemy) and perform CRUD operations in Resource

[Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)

```python
#app.py
from flask-sqlalchemy import Flask-SQLAlchemy
from flask-migrate import Migrate
import os

# add more config:
basedir = os.path.abspath(os.path.dirname(__file__))
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///' + os.path.join(basedir,'data.sqlite') # basically create a DB in the folder that contain this app.py file
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
Migrate(app,db)

db = SQLAlchemy(app)

```

Now, on the same file (not recommended) or another one to import, create the Model (that generates the DB table/s )

```python
# app.py
# Model creation; 

class Item(db.Model):
    # optionally create a table name
    __tablename__ = 'items'

    # create column/s
    name = db.Column(db.String(80),primary_key=True)

    # add methods for __init__ and __repr__
    def  __init__(self,name):
        self.name = name
    
    def __repr__(self):
        pass # for debug like return f'{self.name}'
        
        # def json(self):
        #     return { 'name':self.name}      
```

and for the classes for Resource, create the methods as:

```python

class Item(Resource):
    def get(self,name)):
    item = Class.query.filter_by(name=name).first()
    
    if item:
        return item.json()
    else:
        return {'name':None}, 404

    def post(self,name):
        db.session.add(item)
        db.session.commit()
        return item.json()

    def delete(self, name):
        item = Class.query.filter_by(name=name).first()
        db.session.delete(item)
        db.session.commit()
        return {'note':'delete success'}

    def put(self,name):
        pass

class Lists(Resource): # all items
    def get(self):
        items = Class.query.all()
        return  [item.json() for item in items]

```

Finally, don't forget to run:

```zsh
export FLASK_APP=<server-name>.py
# Flask-migrate:
# Sets up the migrations directory
flask db init # it create a data.sqlite file

# to Sets up the migration file
flask db migrate -m "some message" # Migrations folder creation

# to Updates the database with the migration
flask db upgrade

# And then, everytime I change something in the model, run:
flask db migrate -m “new message for updates“
flask db upgrade
```
to create the DB an add migration services, to use everytime I change the Model.

Now, you should be able to see the file for Migrations and thew `data.sqlite` file

#### 4. Create Views functions - that have their forms
```python
# app.py

@app.route('/')
def index():
    return render_template('/home.html')

# other routes...

# and create the forms for those Views:
def add_item():
    # addForm is the one we created in the forms.py or here below... *
    form = addForm() 
    if form.validate_on_submit():
        name = form.name.data

        new_item = Item(name)
        db.session.add(new_item)
        db.session.commit()

        return redirect(url_for('list_items')) # if successful add redirect here
    return render_template('add.html',form=form)

# forms are on this file or better on another an imported to keep everything clean

# * below, the addForm here or to be imported...
class AddForm(FlaskForm):
    name = StringField('Name of Owner:')
    pup_id = IntegerField("Id of Puppy: ")
    submit = SubmitField('Add Owner')
```

## consider Bootstrap Forms to improve look of forms in flask

https://getbootstrap.com/docs/5.0/forms/overview/


## env var:
app.config['SECRET_KEY'] = 'secret'


---

• Going to a page will always do a GET
• But there are many other things we can do, such as POST, DELETE,
PUT, OPTIONS, HEAD, etc.

• Rest are stateless



---

when a retrieve a DB column, if is an ID is ok that I retrieve the first, sine is unique but what if is a name?
I can have more items with a name but first() retrieve only the first; so let's add `unique=True` in the db.Column creation.


---

## Data serialization with Marshmallow

> parsing incoming data

Do you want to send data?
Marshmallow serialization is turning `classes` (objects) into `Dict`.

```python

from  marshmallow import Schema, fields

class BookSchema(Schema):
    title = fields.Str()
    author = fields.Str()

class Book:
    def __init__(self, title,author, description):
        self.title = title
        self.author = author
        self.description = description

book = Book('Fastlane','MJ Demarco','How to escape the rat race')
print(book)
book_schema = BookSchema()
book_dict = book_schema.dump(book)
print(book_dict)
```

Do you want to use serialized data ( so, de-serialize it, in order to use it with Python) coming over from the web?
```python
`varname_json = request.json()
  varname_data = var_schema.load(varname_json)
  # and where I want to use the data
  varname_schema.dump(obj_instance)
  ```

Marshmallow de-serialization is turning `Dict` into `classes` (objects) .
```python
class BookSchema(Schema):
    title = fields.Str()
    author = fields.Str()
    description = fields.Str()

class Book:
    def __init__(self, title,author, description):
        self.title = title
        self.author = author
        self.description = description

book_data = {
    'title':'Fastlane',
    'author':'MJ Demarco',
    'description':'How to escape the rat race'
}

book_schema = BookSchema()
book = book_schema.load(book_data)
book_obj = Book(**book)
print(book_obj.title)
```

> Pipenv add a layer of security inb top of Virtualenv

## MIND Pipfile and Pipfile.lock ( real installed modules)

on PyCharm be sure you've installed compatible version and check always you've got the modules in the lock file!

## [Flask or Django](https://testdriven.io/blog/django-vs-flask/)

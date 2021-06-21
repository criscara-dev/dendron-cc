---
id: pr6PIrg04IqmwQDQki8TD
title: PROJECT-BLUEPRINT
desc: ''
updated: 1624266229733
created: 1624264141343
---

# Typical Project structure:

.
├── app.py
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
Pip install Flask-SQLAlchemy
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

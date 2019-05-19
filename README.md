Flask-Migrate
=============

[![Build Status](https://travis-ci.org/miguelgrinberg/Flask-Migrate.png?branch=master)](https://travis-ci.org/miguelgrinberg/Flask-Migrate)

Flask-Migrate is an extension that handles SQLAlchemy database migrations for Flask applications using Alembic. The database operations are provided as command-line arguments under the `flask db` command.

Installation
------------

Install Flask-Migrate with `pip`:

    pip install Flask-Migrate

Example
-------

This is an example application that handles database migrations through Flask-Migrate:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'

db = SQLAlchemy(app)
migrate = Migrate(app, db)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(128))
```

With the above application you can create the database or enable migrations if the database already exists with the following command:

    $ flask db init

Note that the `FLASK_APP` environment variable must be set according to the Flask documentation for this command to work. This will add a `migrations` folder to your application. The contents of this folder need to be added to version control along with your other source files. 

You can then generate an initial migration:

    $ flask db migrate
    
The migration script needs to be reviewed and edited, as Alembic currently does not detect every change you make to your models. In particular, Alembic is currently unable to detect indexes. Once finalized, the migration script also needs to be added to version control.

Then you can apply the migration to the database:

    $ flask db upgrade
    
Then each time the database models change repeat the `migrate` and `upgrade` commands.

To sync the database in another system just refresh the `migrations` folder from source control and run the `upgrade` command.

To see all the commands that are available run this command:

    $ flask db --help

Resources
---------

- [Documentation](http://flask-migrate.readthedocs.io/en/latest/)
- [pypi](https://pypi.python.org/pypi/Flask-Migrate) 
- [Change Log](https://github.com/miguelgrinberg/Flask-Migrate/blob/master/CHANGELOG.md)

## Getting Started with Flask
![what is a flask](../resources/assets/images/what_is_flask.jpg)
- [What is a Flask](#what-is-a-flask)
- [Installing Flask](#installing-flask)
- [Simple APIs with Flask](#simple-apis-with-flask)
  - [Get](#get)
  - [Post](#post)
  - [Put](#put)
  - [Delete](#delete)
  - [Workshop]()

## What is Flask
Python being a very popular language used in many fields including web development, it is also used in many other fields.
[Flask](https://flask.palletsprojects.com/en/2.2.x/) is a micro framework that is used to create web applications. It
depends on the [Jinja2](https://jinja.palletsprojects.com/en/2.10.x/) template engine to render templates and 
[Werkzeug](https://werkzeug.palletsprojects.com/en/1.0.x/) to handle requests making it light and a prominent framework 
for building fast and light APIs.

## Installing Flask
As all safe or legal python packages, to install Flask you need to install the [Python](https://python.org) and use the
[pip](https://pip.pypa.io/en/stable/) command to install Flask.

  ```bash
    pip install flask
   ```
## Simple APIs with Flask
Let's start building a simple API with Flask just very basic.

🤔 What ingredients are needed to build a Flask application and get it running?
- The flask package imported
- A function defining the scope of our application 
- a route decorator for the function path 
- The function `app.run()` is used to start the application 

```python
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        return 'Hello, It\'s Yokejuste!'
 
    if __name__== '__main__':
      app.run(debug=True)
```
Now we're sure the application is running, and we can test it.

```bash
    curl localhost:5000
    Hello, It's Yokejuste!
```
We can now get things more serious than before. We dive into the next section. Before making use of the different methods
of the Flask framework, we need to create a database schema and liking it to our database. The choice of the database is
sqlite3.

The following code is with respect to a library to get or put a put, nothing serious just some samples.


The code becomes:

* Part One: (app definition)
```python
    from flask import Flask
    from flask_sqlalchemy import SQLAlchemy
    from flask_marshmallow import Marshmallow
    from flask_cors import CORS
    
    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.sqlite3'
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
    db = SQLAlchemy(app)
    CORS(app)
```
  Let breaks it down:
    
  - `app = Flask(__name__)` creates an instance of the flask web application.
  - `app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.sqlite'` sets the database location.
  - `app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False` disables the modification tracking.
  - `db = SQLAlchemy(app)` creates an instance of the SQLAlchemy database.
  - `CORS(app)` enables CORS.

* Part Two: (database schema)
```python
class Books(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(80), nullable=False)
    description = db.Column(db.String(120), unique=True, nullable=False)
    def __repr__(self):
        return '<Book %r>' % self.name
    
    def create(self):
        db.session.add(self)
        db.session.commit()
    
    def __init__(self):
        self.name = name
        self.description = description

db.create_all()
```
  - db.create_all() makes the application create all the defined tables in the database.

  - db.session.add(self) adds the Todo instance into the SQLAlchemy database connection session.

  - db.session.commit() executes all the database operations that are available in the session.

### The GET method
The GET method is to run queries from the database requesting for contents from there.
- All db content query
  ```python
  @app.route('/books/')
  def get_books():
      books = Books.query.all()
      output = []
      for book in books:
        book_data = {'name': book.name, 'description': book.description}
        output.append(book_data)
      return {'drinks': output}
  ```
- Single element query
  ```python
  @app.route('books/<id>')
  def get_book(id):
      book = Books.query.get_or_404(id)
      return {'name': book.name, 'description': book.description }
  ```

### The POST method
The POST method is to add new contents to the database. The following code is an example of the POST method.

```python
@app.route('/books/', methods=['POST'])
def add_book():
    name = request.json['name']
    description = request.json['description']
    new_book = Books(name=name, description=description)
    new_book.create()
    return {'message': 'Book created successfully.'}, 201
```
### The PUT method(update)
The PUT method is to update the contents of the database. The following code is an example of the PUT method. With this method,
It is possible to update the contents of the database on a particular element.

```python
@app.route('/books/<id>', methods=['PUT'])
def update_book(id):
    book = Books.query.get_or_404(id)
    name = request.json['name']
    description = request.json['description']
    book.name = name
    book.description = description
    db.session.commit()
    return {'message': 'Book updated successfully.'}
```

### The DELETE method(delete)
The DELETE method is to delete the contents of the database. The following code is an example of the DELETE method. With this method,
It is possible to delete the contents of the database on a particular element.

```python
@app.route('/books/<id>', methods=['DELETE'])
def delete_book(id):
    book = Books.query.get_or_404(id)
    db.session.delete(book)
    db.session.commit()
    return {'message': 'Book deleted successfully.'}
```

### Bonus:  The OPTIONS method
The OPTIONS method is to allow the browser to make a request to the server without actually running the request.

```python
@app.route('/books/', methods=['OPTIONS'])
def options():
    return {'message': 'ok'}, 200, {'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Headers': 'Content-Type,Authorization,true'}
```

On what follows we'll see how to use the Flask framework to build a RESTful API. Thus using this framework in a more effective 
way.
<br>
<p align="center"><a href="../introduction/introduction.md#introduction">&laquo; &nbsp;&nbsp; 
Previous
</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
<a href="#">Next &nbsp;&nbsp; &raquo;</a></p>
<br><br>


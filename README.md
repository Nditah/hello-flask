## Flask-App

[Completed App](https://github.com/Nditah/hello_flask) | 
[Official Website](https://flask.palletsprojects.com/en/2.1.x/installation/) | 
[Quick Start](https://flask.palletsprojects.com/en/2.1.x/quickstart/)


1. Make a Directory for the Project and navigate into it
     > mkdir flask-app && cd flask-app

2. Create a Python Virtual Environment and Activate it
     > python3 -m venv venv 
     > ls
     > source venv/bin/activate

3. Install Flask and SQLAlchemy
     > pip install Flask Flask-SQLAlchemy

4. The Open the Project with VSCode.

5. Create a file `app.py` and the `templates` directory with a `base.html` file

## app.py

```py
from turtle import title
from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.sqlite'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

class Todo(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100))
    complete = db.Column(db.Boolean)

db.create_all()

@app.get("/")
def home():
    todo_list = db.session.query(Todo).all()
    return render_template("base.html", todo_list=todo_list)

@app.post("/add")
def add():
    title = request.form.get("title")
    new_todo = Todo(title=title, complete=False)
    db.session.add(new_todo)
    db.session.commit()
    return redirect(url_for("home"))

@app.get("/update/<int:todo_id>")
def update(todo_id):
    todo = db.session.query(Todo).filter(Todo.id == todo_id).first()
    todo.complete = not todo.complete
    db.session.commit()
    return redirect(url_for("home"))

@app.get("/delete/<int:todo_id>")
def delete(todo_id):
    todo = db.session.query(Todo).filter(Todo.id == todo_id).first()
    db.session.delete(todo)
    db.session.commit()
    return redirect(url_for("home"))


```


## templates/base.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Todo App - Flask</title>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css">
        <script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script>
    </head>
    <body>
        <div style="margin-top: 50px;" class="ui container">
            <h1 class="ui center aligned header">Flask ToDo App</h1>

            <form class="ui form" action="/add" method="post">
                <div class="field">
                    <label>Todo Title</label>
                    <input type="text" name="title" placeholder="Enter ToDo task...">
                    <br>
                </div>
                <button class="ui blue button" type="submit">Add</button>
            </form>

            <hr>

            {% for todo in todo_list %} 
            <div class="ui segment">
                <p class="ui big header">{{ todo.id }} | {{ todo.title }}</p>

                {% if todo.complete == False %}
                <span class="ui gray label">Not Complete</span>
                {% else %}
                <span class="ui green label">Complete</span>
                {% endif %}

                <a class="ui blue button" href="/update/{{ todo.id }}">Update</a>
                <a class="ui red button" href="/delete/{{ todo.id }}">Delete</a>

            </div>
            {% endfor %}

        </div>


    </body>

</html>

```

6. To run the app, Export Enviromental variables

     > export FLASK_APP=app.py

     > export FLASK_ENV=development

     > flask run


7. Commit and push your code to github.com and Deactivate the Virtual Env.
   
    $ deactivate
    $ conda deactivate


###

- 📚 Sam's other blogs: [Dev.to](https://dev.to/nditah) | [Hashnode.dev](https://nditah.hashnode.dev/) | [Medium.com](https://nditah.medium.com/)
- [Github](https://github.com/Nditah)
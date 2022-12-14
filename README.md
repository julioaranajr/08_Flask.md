# Flask 

### Python Flask move app

In this exercise we will create python Flask app that will query data from your MySQL database created in previous exercise.
Make sure your mysql docker container is running, as otherwise there is nothing to connect to.

Create new directory for the project
```sh
mkdir ~/src/Talent-Academy/flask-app
```

Inside flask-app directory create new source file `movie_app.py`

```python
import json

from flask import Flask, render_template
from flask_mysqldb import MySQL

app = Flask("MovieApp")

app.config["MYSQL_HOST"] = "127.0.0.1"
app.config["MYSQL_USER"] = "root"
app.config["MYSQL_PASSWORD"] = "" # TODO enter your supersecret password
app.config["MYSQL_DB"] = "movie_db"

mysql = MySQL(app)

@app.route("/")
# Just to test that flask is working with Hello World
def hello_world():
    return "<p>Hello World!</p>"

@app.route("/movies/")
def list_movies():
    cursor = mysql.connection.cursor()
    query_string = "SELECT * FROM movies_tbl;"
    cursor.execute(query_string)
    data = cursor.fetchall()
    cursor.close()
    return json.dumps(data) # Return data as a string

@app.route("/movies-table/")
def list_movie_table():
    cursor = mysql.connection.cursor()
    query_string = "SELECT * FROM movies_tbl;"
    cursor.execute(query_string)
    data = cursor.fetchall()
    cursor.close()
    return render_template("movies.html.tpl", movies_data=data) # Return data as rendered html template

if __name__ == "__main__":
    app.run(host="127.0.0.1")

```

### Jinja2 html template 
First create `templates` folder for templates, this is the default location for Flask
```sh
mkdir ~/src/Talent-Academy/flask-app/templates
```

Inside `templates` folder create a new Jinja2 template file `movies.html.tpl` 

```html
<table>
<tr>
  <th>Title</th>
  <th>Year</th>
</tr>
{% for movie in movies_data %}
<tr>
  <td>{{movie[1]}}</td>
  <td>{{movie[2]}}</td>
</tr>
{% endfor %}
</table>
```

### Run Flask app

```sh
python movie_app.py
```

You can test your page in the web browser, e.g. http://127.0.0.1:5000/movies-table/

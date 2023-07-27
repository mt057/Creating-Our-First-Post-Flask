# Create Our First Post Flask

To create your first post using Flask and MySQL, you will need to follow these steps:
## Installation

```bash
from flask import Flask, render_template
import json
from flask_sqlalchemy import SQLAlchemy
```

## Create Json File

Connect mysql 

```json
{
    "params":
    {
        "local_server":"True",
        "local_uri":"mysql+pymysql://root:@localhost/personal",
        "prod_uri":"mysql+pymysql://root:@localhost/personal",
        "ins_uri":"https://www.instagram.com/__muhammad_tariq/",
        "fb_uri":"https://www.facebook.com/muhammad6t",
        "link_uri":"https://www.linkedin.com/in/muhammad3/",
        "twitter_uri":"https://twitter.com/_muhammad_tariq",
        "title":"Personal - BEQuickStudy",
        "sidetitle":"BE QuickStudy"

    }
}
```
## Configure the Extension

```python
# Open Config.json file with read mode
with open('templates\config.json','r') as c:
    params = json.load(c)["params"]
local_server = True

app = Flask(__name__) #creating the Flask class object   


if(local_server):
    # configure the SQL database, relative to the app instance folder
    app.config["SQLALCHEMY_DATABASE_URI"] = params['local_uri']

else:
    # configure the SQL database, relative to the app instance folder
    app.config["SQLALCHEMY_DATABASE_URI"] = params['prod_uri']

# create the extension
db = SQLAlchemy()

# initialize the app with the extension
db.init_app(app)

```

## Define Modules
```python
class Projects(db.Model):
    # name, email, subject, msg
    id = db.Column(db.Integer, primary_key=True)
    ptitle = db.Column(db.String, nullable=True)
    pdescription = db.Column(db.String, nullable=False)
    slug = db.Column(db.String, nullable=True)
    image = db.Column(db.String, nullable=True)
    footer = db.Column(db.String, nullable=True)
```

##  App Route
```python
@app.route('/<string:post_slug>', methods=['GET'])  # The "slug" variable will capture the post's slug from the URL
def show_post(post_slug):
    # Your code to fetch and display the post with the given slug
    post = Projects.query.filter_by(slug = post_slug).first()
    return render_template('projects.html', params=params, post=post)
```

# flask
Single Create, Read, Update, Delete with Mysql

### Cài flask (nếu chưa cài)
pip3 install flask

### Cài Mysql (nếu chưa cài)
pip3 install flask_mysqldb

### Bật chế độ dev:
```
$ export FLASK_APP=example 
$ export FLASK_ENV=development
```

### Tạo file app.py
```
from flask import Flask, render_template, request, json
from flask_mysqldb import MySQL

app = Flask(__name__)
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = ''
app.config['MYSQL_DB'] = 'flask'

mysql = MySQL(app)

@app.route("/")
def main():
    # return "Welcome!"
    return render_template('index.html')
	
@app.route('/post/create', methods=['POST'])
def create_post():
    name = 'hoan4'
    content = 'dev4'
    cursor = mysql.connection.cursor()
    cursor.execute(''' INSERT INTO posts_post (`name`, `content`) VALUES (%s,%s)''', (name, content))
    mysql.connection.commit()
    cursor.close()
    return render_template('index.html')
	
@app.route('/post/<int:id>')
def show_post(id):
    # Shows the post with given id.
    return f'This post has the id {id}'

@app.route('/post/<int:id>', methods=['DELETE'])
def delete_post(id):
    cursor = mysql.connection.cursor()
    cursor.execute(''' DELETE FROM posts_post WHERE id ={0} '''.format(id))
    mysql.connection.commit()
    cursor.close()
    return render_template('index.html')
	
@app.route('/post/<int:id>', methods=['PUT'])
def update_post(id):
    cursor = mysql.connection.cursor()
    cursor.execute(''' UPDATE posts_post SET `name`='abcd'  WHERE id ={0} '''.format(id))
    mysql.connection.commit()
    cursor.close()
    return render_template('index.html')
	

if __name__ == "__main__":
    # app.run()
    app.run(debug=True, port=5000)
	
```

### Tạo file template
\templates\index.html
```
>Hello world
```


### chạy thử
python app.py
# FlaskNote

### Flask import config
通过app.config.from_object('ClassName')导入，

app.py
```py
from flask import Flask

app = Flask(__name__)
app.config.from_object('settings.DevelopSettings')


@app.route('/')
def hello_world():
    return 'Hello World!'


if __name__ == '__main__':
    app.run()
```
settings.py
```py
class Baseconfig(object):
    DEBUG =  True
    Testing = False


class ProductionConfig(Baseconfig):
    DB_URL = 'mysql://user@locolhost:3306'


class DevelopmentConfig(Baseconfig):
    Testing = False
```
### Flask route
Flask使用Python装饰器绑定路由
```py
@app.route('/user/<username>')
@app.route('/post/<int:post_id>')
@app.route('/post/<float:post_id>')
@app.route('/post/<path:path>')
@app.route('/login', methods=['GET', 'POST'])
```
### Flask请求和响应
主要使用Flask以下内置的属性和方法
render_template, make_response, request, url_for, redirect, session
```py
from flask import Flask, render_template, make_response, request, url_for, redirect, session

app = Flask(__name__)
app.config.from_object('settings.ProductionConfig')


@app.route('/')
def hello_world():
    return 'Hello World!'

@app.route('/index')
def index():
    print(request.url, request.args)
    response = make_response(render_template('index.html'))
    response.headers['http-status'] = 200
    print(response)
    return response

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))

@app.route('/logout')
def logout():
    session.pop('username')
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run()
```
### Flask闪现
闪现用于传递消息，from flask import flash, get_flashed_messages
每运行一次flash()，都会把flash()里面的东西加入到一个列表里面，然后可以通过get_flashed_messages()拿到这个列表
```py
from flask import Flask, render_template, make_response, request, url_for, redirect, session, flash, get_flashed_messages
@app.route('/flash')
def flashtest():

    flash('flask messages')
    return 'Hello, Flask. testing flash ....'

@app.route('/getflash')
def getflash():
    # get_flashed_messages will return a list
    messages = get_flashed_messages()
    print(messages)
    return 'get flash'

if __name__ == '__main__':
    app.run()
```
### Flask蓝图
将app对象和蓝图对象关联起来
url_prefix
给某一类url加上前缀
可以给一类url（即蓝图加上@ad.before_request)
### flask-session的使用

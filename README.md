# mysql-flask-vue-schoolProjectStudentNet
# 1.	项目简介

采用mysql+flask+vue，前后端分离式开发的项目

项目 ：学生校园网

简介：介绍思路和整体要求 该项目为针对校园学生的网站，设计学生共同关注的信息服务及论坛。 业务场景：描述相关的真实企业业务背景。从真实场景中，适当简化或者提炼出适合项目场景 学生校园网是以全国各类学校为信息节点的校园社区平台，关注学生创业、就业、生活、情感、心理、学习、留学。提供服务及论坛。

功能性需求 基本功能： 1.实现基本公共信息服务； 2.实现学生论坛服务； 3.实现学生二手市场服务；

扩展功能 ：根据校园情景，提供1到2项扩展服务（可加分）

非功能性需求 1.代码要有规范的说明文档； 2.所有代码要注明具体开发人员 ；

其他限制条件：开发环境、实验平台、开发语言、数据库、编译器等限制条件 服务器： windows操作系统， Flask服务器或其它。

开发语言 ： javascript , Python或其他

开发工具： Hbuilder或其它

数据库 ： mysql

测试数据或平台： 测试环境： 1.移动客户端基于Android或IOS，或移动端浏览器均可； 2.pc端环境以IE、FireFox、Chrome主流浏览器为主。 其他要求

# 2.  项目结构

- 目录

  ```html
  │  front.rar
  │  GITHUB操作.docx
  │  list.txt
  │  README.md
  │  新建文本文档.txt
  │  说明文档.docx
  │  
  ├─back	后端项目
  │  │  config.py	配置文件
  │  │  myapp.py	启动程序
  │  │     
  │  ├─app	项目代码文件夹
  │  │  │  models.py	模型
  │  │  │  __init__.py	构造文件
  │  │  │  
  │  │  ├─api	接口文件夹
  │  │  │  │  communityApi.py	用户交流平台接口
  │  │  │  │  loginregistApi.py	登录注册接口
  │  │  │  │  productApi.py	二手市场接口
  │  │  │  └─__init__.py	构造文件
  │  │  │          
  │  │  └─main	主文件夹
  │  │      │  views.py	视图页面
  │  │      └─__init__.py	构造文件
  │  │          
  │  └─venv	虚拟环境
  │
  └─front
      │  communityHt.html	用户交流页面
      │  index.html	主页
      │  login.html	注册页面
      │  regist.html	登录页面
      │  productHt.html	二手市场页面
      │  
      ├─config	配置文件夹
      ├─scr
      └─static	静态文件夹
          ├─css	css文件夹
          │      productCss.css	二手市场CSS文件
          │      
          ├─img	图片文件夹
          └─js	js文件夹
  				vue-resource@151.js
                  vue-resource.min.js
                  vue.js
  ```

  

# 3.  数据库配置

### 1.	账号密码配置

```mysql
mysql -u root -p
账号：root
密码：123456
```

### 2.	创建并配置用户表

- 创建用户表并添加数据

  ```mysql
  USERS
  ID 序号
  NAME 用户名
  PASSWORD 密码
  EMAIL 电子邮箱
  CREATE TABLE users(ID INT NOT NULL AUTO_INCREMENT,NAME VARCHAR(20) NOT NULL,PASSWORD VARCHAR(50) NOT NULL,EMAIL VARCHAR(100) ,PRIMARY KEY (ID));
  INSERT INTO users(ID,NAME,PASSWORD,EMAIL)VALUES(1,"管理员","123456","1534273733@qq.com");
  INSERT INTO users(ID,NAME,PASSWORD,EMAIL)VALUES(2,"用户1","12345","153427373@qq.com");
  INSERT INTO users(ID,NAME,PASSWORD,EMAIL)VALUES(3,"用户2","1234","15342737@qq.com");
  ```

- 创建市场产品表

  ```mysql
  PRODUCT
  ID 序号
  NAME 卖家的用户名
  PNAME 产品名
  PRICE 价格
  DESCRIPTION 产品描述
  CREATE TABLE PRODUCT(ID int NOT NULL ,NAME varchar(20) NOT NULL ,PNAME varchar(20) NOT NULL ,PRICE int NOT NULL ,DESCRIPTION varchar(100) NULL ,PRIMARY KEY (ID));
  INSERT INTO PRODUCT(ID,NAME,PNAME,PRICE,DESCRIPTION)VALUES(1,"管理员","二手小米6",500,"用过几个月，成新，买了新手机打算换掉");
  INSERT INTO PRODUCT(ID,NAME,PNAME,PRICE,DESCRIPTION)VALUES(2,"用户1","全新公牛插排",20,"不小心买多了个插排，全新的公牛！");
  INSERT INTO PRODUCT(ID,NAME,PNAME,PRICE,DESCRIPTION)VALUES(3,"用户2","全新TYPE-C充电线",10,"全新的充电线线");
  ```
  
- 创建论坛帖子表

  ```mysql
  COMMUNITY
  ID 序号
  NAME 发帖人的用户名
  DESCRIPTION 帖子内容
  CREATE TABLE COMMUNITY(ID int NOT NULL ,NAME varchar(20) NOT NULL ,DESCRIPTION varchar(100) NULL ,PRIMARY KEY (ID));
  INSERT INTO COMMUNITY(ID,NAME,DESCRIPTION)VALUES(1,"管理员","这交流平台真好用！！！");
  INSERT INTO COMMUNITY(ID,NAME,DESCRIPTION)VALUES(2,"用户1","有人吗？有人吗？有人吗？有人吗？");
  INSERT INTO COMMUNITY(ID,NAME,DESCRIPTION)VALUES(3,"用户2","呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐呐");
  ```

# 4.  API接口

### 评论接口

```python
# 获取数据
@api.route('/todo/api/communityApi', methods=['GET'])
# 添加数据
@api.route('/todo/api/addCommunityApi', methods=['POST'])
```

### 二手市场接口

```python
# 获取数据
@api.route('/todo/api/Productions', methods=['GET'])
# 添加数据
@api.route('/todo/api/addProduction', methods=['POST'])
# 删除数据
@api.route('/todo/api/deleteProduction', methods=['POST'])
```



# 5.  后端开发说明

### myapp.py

- 开始文件

```python
from app import create_app

# 配置app
app = create_app('default')

# 屏蔽JINJA2 防止和VUE冲突
app.jinja_env.variable_start_string = '{['
app.jinja_env.variable_end_string = ']}'

if __name__ == '__main__':
    app.run()
```

#### app/models.py

- models主要配置用户模型，方便调用数据库时候使用，我们将数据库的数据转换为JSON格式方便与前端VUE数据交互传递

```python
from . import db
from flask import Flask as _Flask
from flask.json import JSONEncoder as _JSONEncoder
from datetime import date
import json
from flask import jsonify
app = _Flask(__name__)

# 将数据库数据转换为JSON格式
class JSONEncoder(_JSONEncoder):
    def default(self, o):
        if hasattr(o, 'keys') and hasattr(o, '__getitem__'):
            return dict(o)
        if isinstance(o, date):
            return o.strftime('%Y-%m-%d %H:%M:%S')
        return json.JSONEncoder.default(self, o)

class Flask(_Flask):
    json_encoder = JSONEncoder

# 设置用户模型
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    password = db.Column(db.String(128))
    email = db.Column(db.String(64))

    def __repr__(self):
        return 'User:%s' % self.name

    def keys(self):
        return ['id', 'name', 'password', 'email']

    def __getitem__(self, item):
        return getattr(self, item)

# 设置评论模型
class Community(db.Model):
    __tablename__ = 'community'
    id = db.Column(db.Integer, primary_key=True)
    cname = db.Column(db.String(64))
    description = db.Column(db.String(128))

    def __repr__(self):
        return 'Community:%s' % self.cname

    def keys(self):
        return ['id', 'cname', 'description']

    def __getitem__(self, item):
        return getattr(self, item)

# 设置市场模型
class Product(db.Model):
    __tablename__ = 'product'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    pname = db.Column(db.String(128))
    price = db.Column(db.Integer)
    description = db.Column(db.String(128))

    def __repr__(self):
        return 'Community:%s' % self.name

    def keys(self):
        return ['id', 'name', 'pname', 'price', 'description']

    def __getitem__(self, item):
        return getattr(self, item)
```

#### app/config.py

- 配置数据库

```python
import os

class Config:
    SECRET_KEY = '123456'
    SQLALCHEMY_COMMIT_ON_TEARDOWN = True
    SQLALCHEMY_RECORD_QUERIES = True
    SQLALCHEMY_TRACK_MODIFICATIONS = True
    SQLALCHEMY_DATABASE_URI = True
    FLASKY_ADMIN = '1534273733@qq.com'
    check_same_thread = False
    SQLALCHEMY_COMMIT_TEARDOWN = True
    MAIL_SERVER = 'smtp.live.com'
    MAIL_PORT = '587'
    MAIL_USE_TLS = 'true'

    MAIL_USERNAME = os.environ.get('MAIL_USERNAME')
    MAIL_PASSWORD = os.environ.get('MAIL_PASSWORD')

    FLASKY_MAIL_SUBJECT_PREFIX = ''
    FLASKY_MAIL_SENDER = '<1534273733@qq.com>'

    LANGUAGES = ['en', 'zh']

    @staticmethod
    def init_app(app):
        pass

class DevelopmentConfig(Config):
    DEBUG = True
    SQLALCHEMY_DATABASE_URI = 'mysql+mysqlconnector://root:123456@127.0.0.1:3306/studentsystem'


class TestingConfig(Config):
    TESTING = True
    SQLALCHEMY_DATABASE_URI = 'mysql+mysqlconnector://root:123456@127.0.0.1:3306/studentsystem'

class ProductionConfig(Config):
    SQLALCHEMY_DATABASE_URI = 'mysql+mysqlconnector://root:123456@127.0.0.1:3306/studentsystem'

config = {
    'development': DevelopmentConfig,
    'testing': TestingConfig,
    'production': ProductionConfig,

    'default': DevelopmentConfig
}
```

#### app/__init__.py

- 导入配置文件、设置目录、数据库、蓝图

```python
from flask import Flask
from .config import config
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app(config_name):
    # 配置Flask静态目录、URL目录
    app = Flask(__name__,template_folder='../../front',static_url_path='',static_folder='../../front')
    # 导入配置文件
    app.config.from_object(config[config_name])
    config[config_name].init_app(app)
    # 配置数据库
    db.init_app(app)
    # 配置文件蓝图
    from .main import main as main_blueprint
    app.register_blueprint(main_blueprint)
    from .api import api as api_blueprint
    app.register_blueprint(api_blueprint)
    # 返回app
    return app
```

#### app/main

##### app/main/__init__.py

```python
from flask import Blueprint

main = Blueprint('main', __name__)

from . import views
```

##### app/main/views.py

```python
from flask import render_template
from . import main

# 主页
@main.route('/')
def index():
    return render_template('index.html')
# 评论页面
@main.route('/communityHt')
def communityHt():
    return render_template('communityHt.html')
# 市场页面
@main.route('/productHt')
def productHt():
    return render_template('productHt.html')
# 登录页面
@main.route('/login')
def login():
    return render_template('login.html')
# 注册页面
@main.route('/regist')
def regist():
    return render_template('regist.html')
# 测试页面
@main.route('/cs')
def cs():
    return render_template('cs.html')
```

#### app/api

##### app/api/__init__.py

```python
from flask import Blueprint

api = Blueprint('api', __name__)

from . import communityApi,loginregistApi,productApi
```

##### app/api/communityApi.py

```python
from flask import jsonify, make_response, request, abort
from . import api
from .. import db
from ..models import Community
from ..models import JSONEncoder
import json

# 获取数据
@api.route('/todo/api/communityApi', methods=['GET'])
def getCommunity():
	test_data = Community.query.all()
	tasks = json.loads(json.dumps(test_data, cls=JSONEncoder))
	return jsonify({'tasks': tasks})

# 添加数据
@api.route('/todo/api/addCommunityApi', methods=['POST'])
def addCommunity():
	test_data = Community.query.all()
	tasks = json.loads(json.dumps(test_data, cls=JSONEncoder))
	if request.json['cname'] == "":
		abort(400)
	task = {
		'id' : tasks[-1]['id'] + 1,
		'cname': request.json['cname'],
		'description': request.json.get('description', ""),
	}
	tas = Community(id= tasks[-1]['id']+1,cname= request.json['cname'],description= request.json.get('description', ""))
	db.session.add(tas)
	db.session.commit()
	tasks.append(task)
	return jsonify({'tasks': tasks}), 201
```

##### app/api/loginregistApi.py

```python

```

##### app/api/productApi.py

```python
from flask import Flask, jsonify,render_template,request,abort,make_response
from flask import jsonify, make_response, request, abort
from . import api
from .. import db
from ..models import Product
from ..models import JSONEncoder
import json

# 添加数据
@api.route('/todo/api/Productions', methods=['GET'])
def getTasks():
	test_data = Product.query.all()
	Production = json.loads(json.dumps(test_data, cls=JSONEncoder))
	return jsonify({'productH': Production})

# 添加数据
@api.route('/todo/api/addProduction', methods=['POST'])
def add_task():
	test_data = Product.query.all()
	Production = json.loads(json.dumps(test_data, cls=JSONEncoder))
	if request.json['name'] == "":
		abort(400)
	task = {
		'id' : Production[-1]['id'] + 1,
		'name': request.json['name'],
		'pname': request.json.get('pname', ""),
		'description' : request.json.get('description', ""),
		'price': request.json.get('price',""),
	}
	tas = Product(id=Production[-1]['id'] + 1, name=request.json['name'], pname=request.json.get('pname', ""),
				  description=request.json.get('description', ""), price=request.json.get('price', ""), )
	db.session.add(tas)
	db.session.commit()
	Production.append(task)
	return jsonify({'productH': Production}), 201

# 删除数据
@api.route('/todo/api/deleteProduction', methods=['POST'])
def delete_task():
	test_data = Product.query.all()
	Production = json.loads(json.dumps(test_data, cls=JSONEncoder))
	task_id = request.json['id']
	for task in Production:
		if task['id'] == task_id:
			Production.remove(task)
			test_data = Product.query.filter(Product.id == task_id).first()
			db.session.delete(test_data)
			db.session.commit()
			return jsonify({'productH': Production})
```

# 6.  前端开发说明

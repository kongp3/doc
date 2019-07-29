# Mongo

## 版本
* 3.4

## 安装及删除

[官方文档(ubuntu, 其他系统网页的侧边栏可选择)](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

## 配置文件及启动

### 配置文件
1. 配置文件位置 `/etc/mongod.conf`
2. 配置文件修改项 
    * bindIp: 如果仅仅在本地使用写成`127.0.0.1`; 如果需要其他服务器连接, 改成`0.0.0.0`
    * prot: 默认是27017, 安全考虑,建议修改

### 启动
* **这里我们的目的是校验启动**

1. 安装之后先进入mongo创建一个用户
 
 	```
 	$ mongo
 	
	MongoDB shell version v3.4.10
   connecting to: mongodb://127.0.0.1:21644/
   MongoDB server version: 3.4.10
   > use admin
   switched to db admin
   > db.createUser({
        user: "admin",
        pwd: "h@ndihexi@tu",
        roles: [
            { role: "userAdminAnyDatabase", db: "admin"}
        ]
    })
   >
   $
	```

2. 停掉mongod以校验模式启动 
    * `sudo service mongod stop`
    
3. 以校验模式启动 
    * `sudo mongod --auth --fork  -f /etc/mongod.conf `
    * `--auth` 是启动校验模式
    * `--fork` 是以守护进程启动
    * `-f` 是指定配置文件启动

4. 再次进入mongo 
    * `$ mongo --port 27017`
    * `--port` 是指定端口, 如果是默认的27017端口可以不加参数
   
    ```
    MongoDB shell version v3.4.10
	connecting to: mongodb://127.0.0.1:21644/
	MongoDB server version: 3.4.10
	> use admin
	switched to db admin
	> db.auth('admin', 'h@ndihexi@tu')
	1  # 这里表示验证成功
	> show dbs
	admin  0.000GB
	local  0.000GB
	> 
    ```
## 创建数据库及给数据库创建用户

* **mongo本来没有数据库, use一下，创建个用户，就有了数据库。---鲁迅**

### 创建数据库及用户

* 这里可以看到，创建用户的时候`roles.role`字段有变化，关于用户角色请参考[官网文档](https://docs.mongodb.com/manual/core/security-built-in-roles/)

```
MongoDB shell version v3.4.10
connecting to: mongodb://127.0.0.1:21644/
MongoDB server version: 3.4.10
> use kk
    switched to db kk
> db.createUser({
    user: "te_admin",
    pwd: "tear1Sk",
    roles: [
    { role: "readWrite", db: "trisk_tmp"}
    ]
    }
)
> exit
```

### 校验用户

```
$ mongo --prot 27017

MongoDB shell version v3.4.10
connecting to: mongodb://127.0.0.1:21644/
MongoDB server version: 3.4.10
> use kk
switched to db kk
> db.auth('kk', 'kongfm')
1
> show collections
> # 因为没有创建collections 
```

### 修改密码

```
db.changeUserPassword("te_admin", "handihexiatu")
```

## 代码读写

### python

```
import traceback

from pymongo import MongoClient


def get_mongodb_conn(mongodb_conn, db_name):
    try:
        conn = MongoClient(mongodb_conn)
        db = conn[db_name]
    except Exception as e:
        traceback.print_exc()
        raise e
    else:
        return conn, db

if __name__ == '__main__':
    import shortuuid

    # TRISK_MONGODB_CONN = 'mongodb://your_db_user:your_db_pwd@your_mongo_host:your_mongo_port/your_db_name
    TRISK_MONGODB_CONN = 'mongodb://kk:kongfm@testapi.transfereasy.com:21644/kk'

    conn, db = get_mongodb_conn(TRISK_MONGODB_CONN, 'kk')

    try:
        no = shortuuid.uuid()
		 
		  # 写
        db['test_collection'].find_one_and_update(
            {
                'no': no
            },

            {
                '$set': {
                    'test_key1': 'test_value1',
                    'test_key2': 'test_value2',
                    'test_key3': 'test_value3',
                }
            },
            upsert=True
        )
        
        # 读
        data_dict = db['test_collection'].find_one({"no": no})
        print data_dict

    except Exception as e:
        raise e
    finally:
        conn.close()

```
    
### php

```
不会 θ..θ
```

## RoboMongo连接

* 就不截图了, Authentication 里面必须指定数据库、用户和密码。



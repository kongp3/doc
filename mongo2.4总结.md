# mongo总结 

```
添加用户(2.4.9)
db.addUser({'user':'user_name', 'pwd':'your_pwd', roles:[]});

导出
mongodump -h your_host -d db_name -o . -u user_name -p your_pwd

导入
mongorestore -h localhost -d trisk  /home/admin/db_name

修改密码
db.addUser('ad', 'msb20!7');
db.addUser('admin', '0905cdbfa19b803d');

```
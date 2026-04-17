---
date: 2025-05-23
---
[[MySQL基础]]

---

使用python操作mysql：

```
#导入操作模块
from pymysql import Connection  

#创建连接  
conn = Connection(  
    host='localhost',  
    port=3306,  
    user='root',  
    password='missy11'  
)  

#创建实例对象
cursor = conn.cursor()  
  
# 选择数据库  
conn.select_db('ye')  
  
# 查看指定表的建表语句  
cursor.execute('select * from link_status order by id')  
  
#打印输出  
result = cursor.fetchall()  
for i in result:  
    print(i)  
    
# 关闭连接  
cursor.close()  
conn.close()
```


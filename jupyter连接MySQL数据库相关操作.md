# jupyter连接MySQL数据库相关操作

## 一,jupyter连接数据库读取数据表

```python
#方法一:
import pymysql
import pandas as pd
# 连接mysql
conn = pymysql.connect(host='localhost', user='root',password='123456',
                       database='testdb',charset="utf8")
sql_1 = "select * from meal_order_detail1"
#利用pandas直接获取数据
data = pd.read_sql(sql_1, conn)
data.head()  #前五行

#conn = pymysql.connect(host='你的主机名', user='用户名',password='密码',
                       database='数据库名称',charset="utf8")
# 详细连接可参考:https://blog.csdn.net/qq_44883214/article/details/102824109



#方法二: 《Python数据分析与应用》 因为仅SQLAlchemy可连接支持read_sql_table

#SQLAlchemy连接MySQL数据库的代码
from sqlalchemy import create_engine
#创建一个MySQL连接器，用户名为root,密码为123456
#地址为127.0.0.1，数据库名称为testdb，编码为UTF-8
engine = create_engine('mysql+pymysql://root:123456@127.0.0.1:3306/testdb?charset=utf8')
print(engine)

#用read_sql_query，read_sql_table，read_sql读取数据

#用read_sql_query
import pandas as pd
formlist = pd.read_sql_query("show tables",con=engine) #show tables 查看所有的表
print(formlist)
formlist
#关于pd.read_sql_query("x",con=engine)  这个x可以是一个select查询语句,例如"select * from meal_order_detail1"



#用read_sql_table
import pandas as pd
detail1 = pd.read_sql_table('表名',con=engine)
print(len(detail1))
detail1

#使用read_sql读取订单详情表
detail2 = pd.read_sql('select * from star',con= engine)
print('使用read_sql函数+SQL语句读取的订单详情表长度为：',len(detail1))
detail2





```



## 二, jupyter连接数据库插入数据

1. ```python
   import pandas as pd
   import pymysql

   from sqlalchemy import create_engine
   engine = create_engine('mysql+pymysql://root:123456@127.0.0.1/test2?charset=utf8')
   print(engine) #可以不打印

   #balance, account,charges 是字段名
   df = pd.DataFrame({"balance":[10000.000],"account":['13579.SH'], "charges":[0.0005]})

   # name是表明, con=engine是数据库连接,  
   df.to_sql(name='my_ba',con=engine,if_exists='replace',index=False, index_label='id')

   #重点****  if_exists
   '''
   if_existsl可以有三个参数：
   fail的意思如果表存在，啥也不做
   replace的意思，如果表存在，删了表，再建立一个新表，把数据插入
   append的意思，如果表存在，把数据插入，如果表不存在创建一个表！！
   '''

   ```

   ​










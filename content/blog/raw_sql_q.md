+++
title = "Python 原生 SQL 如何预防 SQL 注入"
description = "技术"
tags = [
    "2021",
    "技术",
]
date = "2021-10-08"
categories = [
    "技术",
]
type = "blog"
+++

1. 什么是 SQL 注入
腾讯云博客

SQL 注入攻击是一种常见的Web攻击方法，攻击者通过把 SQL 命令注入到 Web 后台数据库的查询字符串中，最终达到欺骗服务器执行恶意 SQL 命令的目的。例如可以从数据库获取敏感信息，或者利用数据库的特性执行添加用户、导出文件等一系列恶意操作，甚至有可能获取数据库乃至系统用户最高权限。
2. SQL 注入例子
SQL 注入只会发生在字符串拼接的例子中，常见的攻击有拖库、拖表、篡改网页内容、收集数据库信息为其它攻击做准备等等。
# 假设代码中是这样一段代码
username = input("enter in username")
sql = f"""
    select *
    from `content`
    where `create_user` = "{username}"
"""

# 用户输入 username = 'testc" or 1 = "1', SQL 就会变成
sql = f"""
    select *
    from `content`
    where `create_user` = "testc" or 1 = "1"
"""
3. SQL 注入预防通用方法
+ 外部参数动态拼接
`["\"", "\\", "/", "*", "'", "=", "-", "#", ";", "<", ">", "+", "&", "$", "(", ")", "%", "@", ","]`
1. 过滤特殊符号
2. 遇到固定格式的变量，在 SQL 执行前严格按照固定格式检查
3. 特殊符号转义使用（SQL 中的转义字符是单引号）

+ 参数化查询
4. 绑定变量使用预编译语句（预防 SQL 注入的最佳方式）

+ ORM
5. 使用 ORM 避免 SQL 注入

+ 存储
6. 敏感信息加密存储
# 将 username = 'testc" or 1 = "1' 中的特殊符号转译，除了左右两侧的单引号
# 这样 "testc'" or 1 = '"1" 就变成了一个值，而非表达式的一部分
sql = f"""
    select *
    from `content`
    where `create_user` = "testc'" or 1 = '"1"
"""
4. Sqlalchemy / PyMySQL
Sqlalchemy-Doc

本文中的 mysql_conn 实例使用的是 Git Repo Felix `/app/db/mysql.py` 文件中的内容
4.1 SELECT

from sqlalchemy import text

select_params = {"event_id": event_id}
pre_sql = """
    select user_id, user_name, nickname
    from xxx_tbl
    where event_id = :event_id
"""
bind_sql = text(pre_sql)
ret = mysql_conn.connect().execute(bind_sql, select_params).fetchall()
4.2 INSERT

from sqlalchemy import text

insert_params = {
    "user_id": user_id,
    "user_name": user_name,
    "nickname": nickname
}
pre_sql = """
    insert into
    from xxx_tbl (user_id, user_name, nickname) values (:user_id, :user_name, :nickname)
"""

stmt = text(pre_sql)
stmt = stmt.bindparams(**insert_params)
row_id = mysql_conn.connect().execute(stmt).lastrowid
4.3 ORM

import pymysql

# 数据库连接
conn = pymysql.connect(**MYSQL_CONF)
cursor = conn.cursor()

# ORM 对象
expr = Session().query(...)
expr = expr.statement.compile(compile_kwargs={"literal_binds": True})
raw_sql = str(expr)

# commit
cursor.execute(raw_sql)
row_1 = cursor.fetchone()
conn.commit()

# 关闭连接
cursor.close()
conn.close()
4.5 PyMySQL

import pymysql

conn = pymysql.connect(**MYSQL_CONF)
cursor = conn.cursor()

# 内部执行参数化生成的SQL语句，对特殊字符进行了加\转义，避免注入语句生成
pre_sql = "select user,pwd from User where user='%s' and pwd='%s'"
raw_sql = cursor.mogrify( pre_sql, (username, password))

# commit
cursor.execute(raw_sql)
row_1 = cursor.fetchone()
conn.commit()

# 关闭连接
cursor.close()
conn.close()
5. 题外话
拿了四份 offer 以后，很奇怪为什么没有公司使用 Tornado 做 To C 的产品的。

当然大部分公司不会选择使用 Python 来做 To C 产品的开发语言，这里只说使用 Python 的。

后来我想了几个方面的原因：
1. 开发难度
少数人掌握 Python asyncio，能进行无错的高并发编程，Flask 的多进程部署或许更适合大部分开发人员的水平，且同步开发更容易维护。
2. 没有完善的 异步组件库 支持
比如异步的 orm 库就没有，但是只要能转换为 HTTP 请求的都能使用异步库
3. 避免重复造轮子

+++
title = "FastAPI Depend 实现原理(个人猜测+粗略代码实现)"
description = "工具文档"
tags = [
    "2023",
    "技术",
]
date = "2023-07-15"
categories = [
    "技术",
]
type = "blog"
+++

#### 0. 依赖注入

```python
from fastapi import Depends, FastAPI, HTTPException
from sqlalchemy.orm import Session

from . import crud, models, schemas
from .database import SessionLocal, engine

models.Base.metadata.create_all(bind=engine)

app = FastAPI()


# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.post("/users/", response_model=schemas.User)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
    pass
```

#### 1. 猜测原理

```markdown
线索
1. 依赖注入的参数是作为参数从函数传入的，因此排除装饰器实现
2. 表现行为是 handler 开始执行前 db 被实例化好, handler 结束后 db 被 close；表现类似 contextmanager
3. 不结合装饰器 @app.post 使用，发现 db 无法倍正确实例化，而是一个 Depend 类的对象

结论
1. Depend 负责给参数打上一个标识符，以便路由装饰器知道哪个参数是需要 依赖注入 的
2. 路由装饰器遍历参数，找到需要依赖注入的参数，通过 with 语法 调用这个 Callable 对象得到正确实例
3. 在 with 语句内将实例当作参数回传给 handler

PS
猜测应该也是在 Depend 那步将 Callable 装饰为 contextmanager 的
```

#### 2. 代码实现

```python
import inspect
from contextlib import contextmanager


class Depend():
    def __init__(self, depend_obj):
        self.depend_obj = depend_obj
    
    def get_obj(self):
        return self.depend_obj()


def get_default_args(func):
    signature = inspect.signature(func)
    return {
        k: v.default
        for k, v in signature.parameters.items()
        if v.default is not inspect.Parameter.empty
    }


@contextmanager
def manage_resource():
    try:
        print("get db")
        yield 1
    except:
        msg = __import__("traceback").format_exc()
        print(f"raise exception: {msg}")
    finally:
        print("release db")


def depend(callable_obj):
    obj = Depend(callable_obj)
    return obj


def route(func):
    def inner(*args, **kwargs):
        key = None
        depend_obj = None
        kw = get_default_args(func)
        for k, v in kw.items():
            if isinstance(v, Depend):
                depend_obj = v
                key=k

        with depend_obj.get_obj() as b:
            kwargs = {**kwargs, key: b}
            ret = func(*args, **kwargs)
            return ret

    return inner


@route
def handler(a=depend(manage_resource)):
    print("enter handler")


c = handler()
```

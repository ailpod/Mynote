---
date: 2025-05-30
---
[[bilibili]]  [[Python语法]]

---
# 路径操作

> 这里的「路径」指的是 URL 中从第一个 `/` 起的后半部分
> 
> 这里的**操作**指的是一种 *HTTP* 方法 *IN (POST, GET, PUT, DELETE 及 OPTIONS...)*


>[!tip]+ @装饰器
> @something 语法在 Python 中被称为「装饰器」。
> 
> 像一顶漂亮的装饰帽一样，将它放在一个函数的上方（我猜测这个术语的命名就是这么来的）。
> 
> 装饰器接收位于其下方的函数并且用它完成一些工作。
> 
> 在例子中，这个装饰器告诉 **FastAPI** 位于其下方的函数对应着**路径** `/` 加上 get **操作**。
> 
> 它是一个「**路径操作装饰器**」。

---
## part介绍

```  python
from fastapi import FastAPI                                                        
app =  FastAPI()                                                              
  
@app.get("/")                                                                         
async def root():                                                                   
    return {"message": "Hello World"}  
```
- **路径**：是 `/ `
- **操作**：是`get`
- **函数**：是位于装饰器下方的函数： `async def root`
- **返回内容**：可以返回`dict`、`list`，像 `str`、`int` 一样的单个值，还可以返回 Pydantic 模型

---
## 路径参数

> 路径参数（Path Parameters） 是 URL 路径中的一部分，它允许从请求的 URL 中提取动态值。这些参数通常用花括号 {} 包裹，并通过 Python 的类型注解语法来声明其名称和数据类型

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/items/{item_id}")

async def read_item(item_id : str):
    return {"item_id": item_id}
```

这段代码把路径参数 `item_id` 的值传递给路径函数的参数 `item_id` 

>[!learn]- 类型注解
>类型声明将为函数提供错误检查与代码补全等编辑器支持
>
>并且fastapi会通过类型声明自动解析请求中的数据
>
>如示例中传参 3 会解析为 **str** 而非 **int**
>

==访问顺序也很重要==

>FastAPI 按照**定义路由的顺序**进行路径匹配，因此要把更具体的路径（如 /users/me）写在带有路径参数的通用路径（如 /users/{user_id}）之前，否则通用路径会“劫持”本应匹配特定路径的请求

- 例如有两个路由： @*app.get("/user/me")*  和 @*app.get("/user/{user_id})* ，此时使用/user/chai 若无其他，会先调用第一个而非第二个的函数，**原因就是上段话的解释**

==预设值==

>路径操作使用 *Enum* 类型接收预设的*路径参数*

```python
from enum import Enum
from fastapi import FastAPI

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

app = FastAPI()

@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:               -------------------比较
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```

- 在这个例子里，定义了一个继承自 *str* 和 *enum* 的子类，通过 *str* 可以让API将值类型定义为字符串，*enum* 可以保证输入内容是预定义的合法值，还能带来更好的代码结构、更清晰的文档 ==(在FastApi文档下可以显示 *路径参数* 的可用值)== 以及更高的类型安全性

- 并且枚举类*ModelName*元素支持比较操作：使用*model_name.value*获取枚举元素的值

-  即使嵌套在 *JSON* 请求体里也可以返回枚举元素的具体值

==包含路径的路径参数==

- 假设_路径操作_的路径为 */files/{file_path}*，但需要 *file_path* 中也包含_路径_，比如，*home/ye/test*，此时，该文件的 URL 是这样的：*/files/home/ye/test*  

不过，不太支持这种操作，因为会使得测试更麻烦，因此可以使用路径转换器

- 直接使用 Starlette 的选项声明包含_路径_的_路径参数_：*/files/{file_path:path}* 本例中，参数名为 *file_path*，结尾部分的 *:path* 说明该参数应匹配 *路径*

>[!tip] 用法
>```
>@app.get("/files/{file_path:path}") 
>async def read_file(file_path: str): 
>	return {"file_path": file_path}
>```
>==注意==：包含 *file_path* 的路径参数要以 / 开头 
>
>   本例中的URL是 */files//home/ye/test*         
>   
>    ==*files*后是双斜杠==

---

## 查询参数

> 声明的参数不是路径参数时，路径操作函数会把该参数自动解释为**查询**参数
> 
> 经验之谈，查询参数在路由定义下的函数体中，如下

==所有适用于路径参数的流程也适用于查询参数==
```python
from fastapi import FastAPI

app = FastAPI()
fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):  --------查询参数
    return fake_items_db[skip : skip + limit]
```

查询字符串是键值对的集合，这些键值对位于 URL 的 *?* 之后，以 *&* 分隔。

例如，以下 URL 中：`http://127.0.0.1:8000/items/?skip=0&limit=10` 查询参数为 *skip、limit*


==默认值==

查询参数不是路径的固定内容，它是可选的，还支持默认值，

如 *skip: int = 0, limit: int = 10*

==可选参数==

把默认值设为 `None` 即可声明**可选的**查询参数

如 *item_id: str, q: str | None = None* 此时q就是可选的

==多个路径和查询参数==

**FastAPI** 可以识别同时声明的多个路径参数和查询参数。

而且声明查询参数的顺序并不重要

==必选查询参数==

为不是路径参数的参数声明默认值（至此，仅有查询参数），该参数就<mark style="background: #21D16EA6;">不是必选</mark>的了

如果只想把参数设为**可选**，但又不想指定参数的值，则要把默认值设为 *None*

如果要把查询参数设置为**必选**，就不要声明默认值：

```python
from fastapi import FastAPI 
app = FastAPI() 

@app.get("/items/{item_id}") 
async def read_user_item(item_id: str, needy: str):     
	item = {"item_id": item_id, "needy": needy}    
	return item
```

这里的查询参数 *needy* 是类型为 *str* 的必选查询参数，若无该参数会报错

---
# *HTTP协议*

## 参数校验

> *FastAPI* 允许为参数声明额外的信息和校验

- 以上文例子中的可选参数 *q* 为例，想要对他加一个约束条件如 字符串长度不大于 50
```python
from fastapi import Query  ------导入查询模块

async def  read_item( q: Union[str , None]=Query(default=None , max_length = 50) ) :
   ...
```

==字符串校验（对查询参数操作）==

>[!tip]-  *Query*参数
Query(
>default=...,           # 默认值（第一个参数）
>default_factory=...,   # 默认工厂函数（例如生成默认值的函数）
>alias=...,             # 参数别名（当请求中的参数名与函数参数名不同）
>title=...,             # 在文档中显示的标题
>description=...,       # 描述
>gt=...,                # 大于某个值（数值验证）
>ge=...,                # 大于等于
>lt=...,                # 小于
>le=...,                # 小于等于
>min_length=...,        # 最小长度（字符串）
>max_length=...,        # 最大长度（字符串）
>pattern=...,             # 正则表达式匹配
>deprecated=...,        # 是否已弃用
>example=...,           # 示例值（用于文档）
>examples=...,          # 多个示例值
>...
>)

- 这里若想使 *q* 变为一个必需参数，只需去掉 *query* 中的默认值

- 当使用 *Query* 显式地定义查询参数时，还可以声明它去接收一组值，如类型注解为 *list*

==数值校验（对路径参数操作）==

- 能够以与<mark style="background: #D2B3FFA6;">查询参数和字符串校验</mark>相同的方式使用 *Query、Path*（以及其他你还没见过的类）声明元数据和字符串校验

- 路径参数的使用方式与 *Query* 相似，只需将 *Query* 换成 *Path* 类即可

---
## 请求体

>**请求体**是客户端发送给 API 的数据。**响应体**是 API 发送给客户端的数据。
>API 基本上肯定要发送**响应体**，但是客户端不一定发送**请求体**。

- 使用 *Pydantic* 模型声明请求体 
```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.put("/items/{item_id}") 
async def update_item(item_id: int, item: Item, q: str | None = None): 
	result = {"item_id": item_id, **item.dict()} 
	if q: result.update({"q": q}) 
	return result
```

(在 *Apifox* 中的 *Body* 内输请求体)

*FastAPI* 支持同时声明**请求体**、**路径参数**和**查询参数**。
### 参数识别机制

1. <mark style="background: #ADCCFFA6;">路径参数 (`item_id`)</mark>：
    
    - 在路由装饰器 `@app.put("/items/{item_id}")` 中定义。
    - 函数参数中与路径中的占位符 `{item_id}` 名称匹配的参数（如 `item_id: int`）将被识别为路径参数。
    - 这意味着 FastAPI 将从 URL 路径中提取 `item_id` 的值，并将其转换为指定的类型（这里是 `int`）。

2. <mark style="background: #ADCCFFA6;">请求体 (item)</mark>：
    
    - 如果一个参数是一个 Pydantic 模型类（如 `Item`），FastAPI 会自动将其识别为来自请求体的数据。
    - 在这个例子中，`item: Item` 表示期望客户端发送一个 JSON 对象，其结构应符合 `Item` 类的定义。
    - FastAPI 会自动解析并验证该 JSON 数据，如果数据不符合模型定义，则会返回一个错误响应。

3. <mark style="background: #ADCCFFA6;">查询参数 (q)</mark>：
    
    - 如果一个参数既不是路径参数也不是请求体的一部分（即没有出现在路径模板中且不是一个 Pydantic 模型），那么它将被视为查询参数或可选参数。
    - 在这个例子中，`q: str | None = None` 表示 `q` 是一个可选的查询参数，默认值为 `None`
    - 如果请求中包含查询参数 `q`，则 FastAPI 会将其值传递给函数；如果没有提供，则使用默认值 `None`。
---
### 多个参数

> 当想要声明多个请求体参数时可以如下做法

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, user: User):
    results = {"item_id": item_id, "item": item, "user": user}
    return results
```

- 在发送的请求体中以<mark style="background: #FFB8EBA6;">多个键对值</mark>的形式发送 json 数据
```json
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    }
}

```
- 当想要<mark style="background: #ABF7F7A6;">额外增加参数</mark>到请求体的时候，可以使用<mark style="background: #FF5582A6;">Body</mark>来限定参数，这样就会将他也放入请求体中，并且想要单个 *Pydantic* 类对象也展示如上的 *JSON* 格式的时候，只需
```JSON
part 1 : 
@app.put("/items/{item_id}") 
async def update_item( 
	item_id: int, item: Item, user: User, importance: Annotated[int, Body()] 
	):

part 2 : 
item : Item = Body(embed = True)
```

---
### 字段

>和*Path*、*Query* 类似，限定请求体内的参数时使用 *Filed* 来限定 ，参数与前者相似

---
### 嵌套模型

> 可以给 *Pydantic* 模型中的字段嵌套其他类型 ，如 *List* 、*Set* 甚至是定义的其他 *Pydantic*

- 例如在上述基础上定义一个新的 *Pydantic* 类型对象 *image* 含有字段<mark style="background: #D2B3FFA6;"> url 、name</mark>，此时期望如下请求体
```JSON
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": ["rock", "metal", "bar"],
    "image": {
        "url": "http://example.com/baz.jpg",
        "name": "The Foo live"
    }
}
```


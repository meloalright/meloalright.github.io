---
title: Tornado扫码登录实践
date: 2018-06-01
---

```
思路: 基于[Tornado]异步io实现异步非阻塞长链接。
```
    
> 本文作者为    
[简书-melo的微博](https://www.jianshu.com/users/f6323e43dd6c) / [掘金-melo的微博](https://juejin.im/user/579a313f5bbb500064c4a697)   
[Github-meloalright](https://github.com/meloalright) / [Medium-@meloalright](https://medium.com/@meloalright)   
转载请注明出处哦。
   
   
   
   
## 1.扫码登录需求
    
```
部门项目需要实现一个APP扫码即可在PC端实现登录的需求。
结合部门内部的资源协调，适合使用一个异步io的框架实现简单扫码登录接口快速上线。
初期考虑使用[node.js]或者[Tornado]
最终选择使用[Tornado]
```
   
## 2.实现思路    

```python
- 基于 uuid.uuid4() 为二维码生成唯一 uuid
- 基于 tornado.locks 实现长链接 handler 协程的通知等待
- 基于 await http_client.fetch 实现扫码确认请求handler的内部校验接口调用
```

<!-- more -->


## 3.代码结构
      
```python
│ 
│   # @ Python Tornado 工程代码
│ 
├── tornado-project
│   │
│   │   # @ 各种handler放这里
│   │
│   ├── handlers
│   │   │ 
│   │   ├── __init__.py
│   │   │ 
│   │   ├── confirm_handler.py # 确认扫码请求的handler
│   │   │ 
│   │   │ 
│   │   ├── uuid_handler.py # 获取uuid请求的handler
│   │   │ 
│   │   ├── wait_handler.py # 长链接请求的handler
│   │   │ 
│   │   │  # @ redis_map 放这里
│   │   │ 
│   │   └── redis_map 
│   │       ├── __init__.py
│   │       └── redis_map.py
│   │ 
│   ├── main.py # 入口文件
│   │ 
│   └── requirements.txt # 项目依赖
```
   
   
## 4. wait_handler
   
`/wait 长链接接口`   
   
```python
class WaitHandler(tornado.web.RequestHandler):
    async def post(self):
        data = tornado.escape.json_decode(self.request.body)
        cursor = data["cursor"]

        status = int(RedisMap.get_status(cursor))
        '''
        some code ...
        '''

        while status < 2 and status > -1:
            self.wait_future = RedisMap.wait()

            try:
                await self.wait_future
            except asyncio.CancelledError:
                return

            status = int(RedisMap.get_status(cursor))
            '''
            some code ...
            '''


        if self.request.connection.stream.closed():
            RedisMap.delete(cursor) # delete
            return

        RedisMap.delete(cursor) # delete
        self.write({
            "status": status 
            '''
            other key value ...
            ''' 
        })

    def on_connection_close(self):
        self.wait_future.cancel()
```
    
    
## 5. confirm_handler
   
`/confirm 确认扫码登录接口`      
   
```Python

class ConfirmHandler(tornado.web.RequestHandler):

    async def auth(self, param):
        http_client = tornado.httpclient.AsyncHTTPClient()
        response = await http_client.fetch("An auth uri blau blau ..." , headers=param)
        '''
        some code ...
        '''
        response_data = tornado.escape.json_decode(response.body)
        return response_data['status'] is 200

    async def post(self):
        data = tornado.escape.json_decode(self.request.body)
        authorization = data["authorization"]
        '''
        some code ...
        '''
        is_auth = await self.auth(authorization)
        if not is_auth:
            self.write({
                "status": 401,
            })
            return
        else:
            self.write({
                "status": 200
                })
```
   
   
## 结语:


> 代码只是示例 仅供参考哈(∩_∩)  

   
## 鸣谢:

> 参考文档:   
[tornadoweb.org](http://www.tornadoweb.org/en/stable/)      
[tornado.locks – 同步原语](http://tornado-zh.readthedocs.io/zh/latest/locks.html)   


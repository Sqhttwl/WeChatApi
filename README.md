# 微信Api/WeChatApi
基于windows平台开发的微信Api,仅支持2.9.0.122版本。
Api程序入群自取，现免费。
## [文档](https://frz2one.github.io/Api.html)
## Demo

### Python

> websocket连接服务端，把接收到的消息以文本的方式转发到微信消息助手

```python
import requests
import  json
#import qrcode
import random
import time

import websocket
# -*- coding: utf-8 -*-

foo = ['filehelper']
url = "http://127.0.0.1:2222/"
header = {"Content-Type": "application/json"}
body_1 = {"funid": 1, "key": "初始化填写的你key"}
body_1 = str(body_1)
body_1 = bytes(body_1, encoding="utf-8")
response_1 = requests.post(url, data=body_1, headers=header)
print(response_1.text)
def Merge(dict1, dict2):
    return(dict2.update(dict1))
def WeChatPost(funid,adddict={},WeChatID=0,type=0,wxid="",centent=""):
    Merge({"funid": funid, "WeChatID": WeChatID, "wxid": wxid, "content": centent, "type": type}, adddict)
    url = "http://127.0.0.1:2222/"
    header = {"Content-Type": "application/json"}
    body_1 =adddict
    body_1 = str(body_1)
    body_1 = bytes(body_1, encoding="utf-8")
    response_1 = requests.post(url, data=body_1, headers=header)
    return response_1.content.decode('utf-8')
    pass


def on_message(ws, message):
    message_0 = json.loads(message)
    print(message_0["WeChatID"])
    WeChatPost(20,WeChatID=message_0["WeChatID"],type=5,centent=message_0["content"],wxid=random.choice(foo))
def on_error(ws, error):
    print(ws)
    print(error)
def on_close(ws):
    print(ws)
    print("### closed ###")
websocket.enableTrace(True)
ws = websocket.WebSocketApp("ws://127.0.0.1:2222/",on_message=on_message,on_error=on_error,on_close=on_close)
ws.run_forever()

# 二维码内容
#data = "https://www.baidu.com"
# 生成二维码
#img = qrcode.make(data=data)
# 直接显示二维码
#img.show()
# 保存二维码为文件
# img.save("baidu.jpg")

```

## 更新日志

### 2020.6.21

1. 优化了消息处理逻辑（视频（`type:43`）和图片（`type:3`）返回文件的地址）
2. 简化json数据
3. 新增接收消息附带WeChatID标识符，多开更容易识别消息来源微信
4. 登录设置禁止更新，避免新版本自动更新

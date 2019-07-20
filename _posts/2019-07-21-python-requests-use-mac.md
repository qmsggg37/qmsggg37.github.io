---
layout: post
title:  "Python爬虫库requests使用(mac)"
categories: python
tags:  python requests
author: qmsggg37
---

* content
{:toc}

# Python爬虫库requests使用(mac)
- install
#2 
- 简单请求
1.发送一个get请求
```
>>> import requests
>>> response = requests.get("http://httpbin.org/get")
>>> print(response.text)
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "origin": "222.180.200.102", 
  "url": "http://httpbin.org/get"
}
```
这样就发送了一个get请求，并且还打印了返回的内容，这个不再需要知道网页是哪个编码的，不过有时会出现编码问题，但是你也可以指定编码类型，如：
```
response.encoding = 'utf-8'
```
指定完成后就可以正常编码了，前提你得知道网页的编码类型。
出了上面这些，我们还可以获取下面的信息
```
print(response.headers)
{'Content-Length': '268', 'X-Processed-Time': '0', 'X-Powered-By': 'Flask', 'Server': 'meinheld/0.6.1', 'Connection': 'keep-alive', 'Via': '1.1 vegur', 'Access-Control-Allow-Credentials': 'true', 'Date': 'Fri, 23 Mar 2018 08:41:54 GMT', 'Access-Control-Allow-Origin': '*', 'Content-Type': 'application/json'}

# 请求状态码
print(response.status_code)
200

# 获取网页的二进制内容
print(response.content)
'{\n  "args": {}, \n  "headers": {\n    "Accept": "*/*", \n    "Accept-Encoding": "gzip, deflate", \n    "Connection": "close", \n    "Host": "httpbin.org", \n    "User-Agent": "python-requests/2.18.4"\n  }, \n  "origin": "222.180.200.102", \n  "url": "http://httpbin.org/get"\n}\n'

print(response.url) 
# 获取请求的urlprint(response.cookies) 
# 获取cookie

```
2.添加请求头
```
# 还可以添加请求头进行请求
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36'}response = requests.get('httpbin.org/get', headers=headers )print(response.headers)print(response.text)
```

3.添加get参数
```
# 进行带参数的get请求
data = {'name': 'june', 'password': 123456}response = requests.get('http://httpbin.org/get', params=data)print(response.text)
```
4.post请求
```
# 进行post请求
data = {'name': 'june', 'password': 123456}response = requests.post('http://httpbin.org/post', data=data, headers=headers)print(response.text)
```
等等
```
requests.put()
requests.delete()
```
- 进行复杂点的请求
在登陆的时候我们有时候需要输入验证码，那怎样输呢？爬虫的看不了网页，最简单的做法就是把这个验证码的图片下载下来然后手动输入，那么我们怎样下载呢？我们可以向这个图片的url发送请求，然后把返回内容以二进制方法存入文件里面就可以了。

代码如下：
```
# 从网上读取二进制数据，比如图片
response = requests.get('https://www.baidu.com/img/bd_logo1.png', headers=headers)
# 这个是直接获取字节码，这个是要保存的文件
print(response.content)
# 这个是获取解码后的返回内容，这个是乱码
print(response.text)
# 用文件来把图片下载下来
with open('baidu.png', 'wb') as f: # 注意写的方式是以二进制方式写入 
f.write(response.content) 
print('下载完毕')
```
>当我们需要上传文件的时候，比如图片，我们还可以用post方法把他发送出去
```
# 上传文件
files = {'picture': open('baidu.png', 'rb')}
response = requests.post('http://httpbin.org/post', files=files)
print(response.text)
```
>获取cookie并简单处理一下
```
response = requests.get('https://www.baidu.com')
for k, v in response.cookies.items():
 print(k, '=', v)
```
>当网页返回内容是json格式是，我们不需要用json库来解析，我们可以直接利用requests的方法进行解析，两者的效果是一样的
```
# 解析json
j = response.json() # 可以用json库来解析，结果一样
```
>在urllib库时保存登陆信息需要把cookie保存下来，但是在requests库里面，我们只需要用requests.session()来保存信息就可以了。
```
# 用会话来保持登陆信息
session = requests.session()
response = session.get('http://httpbin.org/cookies/set/number/123456')
print(response.text)
```
>这样就可以保存登陆了，不需要为cookie操心了，但是每次获取一个session就可以了，然后用来请求或者其他操作。不需要每次请求或者操作都创建一个sesion出来，这样是保存不了登陆信息的

>当一个网站不安全，需要你用证书验证的，比如这个网站
![image](https://user-images.githubusercontent.com/28669743/37820314-32565224-2ebb-11e8-92ab-3f7718ad6681.png)
>这时要访问里面的网站内容，我们就需要进行验证，代码如下
```
# 证书验证
response = requests.get('https://www.12306.cn', verify=False) # 不加这个关键字参数的话会出现验证错误问题，因为这个网站的协议不被信任
```
>这样就可以进行访问了，但是会有一条警告
```
E:\anaconda\lib\site-packages\urllib3\connectionpool.py:858: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings InsecureRequestWarning)
```
>遇到需要认证的网站，我们也可以这样
```
from requests.auth import HTTPBasicAuth# 设置认证# requests.get('需要认证的网址', auth=HTTPBasicAuth('user', 'passwd')) # 由于找不到需要认证的网址，所以先写个主体# 还可以这样认证# requests.get('需要认证的网址', auth=('user', 'passwd')) # 这样就简单点
```
>requests还可以用代理ip来进行请求网站来防止ip被封以至于自己爬不了的尴尬。使用代理ip也比urllib库简单得多，代码如下：
```
# 设置代理
proxies = {'http': 'http://122.114.31.177:808',   'https': 'https://119.28.223.103:8088'}
# 在请求时添加上列代理
response = requests.get('http://httpbin.org/get', proxies=proxies)print(response.text)
```
- 请求异常处理
>在程序运行时，遇到错误时程序就会被强行停止，如果想要继续运行，就需要进行捕捉异常来让程序继续运行。在requests库中有个处理异常的库requests.exceptions这里简单地处理下请求超时的处理情况
```
import requestsfrom 
requests.exceptions import ReadTimeout, ConnectTimeout, HTTPError, ConnectionError, RequestException
# 捕捉异常
try:
 response = requests.get('http://httpbin.org/get', timeout=0.1) # 规定时间内未响应就抛出异常 print(response.text)
except ReadTimeout as e: 
print('请求超时')
except ConnectionError as e: 
print('连接失败')
except RequestException as 
e: print('请求失败')
```
>这里捕捉了三个异常，因为ReadTimeout是ConnectionError的子类，所以先捕捉ReadTimeout,再捕捉父类的。而ConnectionError 和 RequestException 同理
- 最后
[requests](http://docs.python-requests.org/zh_CN/latest/index.html)
[学习参考资料](https://edu.hellobi.com/course/157)
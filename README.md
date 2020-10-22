# 从0开始的python web server

> 意译自：https://ruslanspivak.com/lsbaws-part1/。python版本3.7+

## 基于Socket API的web server

`socket api`就不在这里介绍了。

```python
# socket_web_server.py
import socket

HOST, PORT = '', 8888

listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
listen_socket.bind((HOST, PORT))
listen_socket.listen(1)
print(f'Serving HTTP on port {PORT}...')

while True:
    client_connection, client_address = listen_socket.accept()
    request_data = client_connection.recv(1024)
    print(request_data.decode('utf-8'))

    http_response = b"""\
HTTP/1.1 200 ok

Hello World!
"""
    client_connection.sendall(http_response)
    client_connection.close()
```

## WSGI server

`WSGI`是`Web Server Gateway Interface`的缩写。

在第一部分中，`web server`和`web app`是混在一起写的。WSGI的出现将`web server`和`web app`分开。`web server`是基于底层的`socket api`接口的，而`web app`是基于WSGI的。



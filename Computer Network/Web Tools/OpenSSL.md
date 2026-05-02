**OpenSSL**: 一种多功能的命令行工具，可以实现加密、解密、自建 CA、创建证书、吊销证书等功能
# CA & CRT
**CA（Certificate Authority，证书授权中心）**：负责发放和管理数字证书 CRT的权威机构，作为**受信任的第三方**，承担公钥体系（PKI）中公钥的合法性检查的责任。
**CRT（数字证书）**：一份文件，内容包括**公钥用户的信息、公钥、CA的签名、签发CA的信息、和有效期**等。证书的格式和验证方法普遍遵循 X.509 国际标准。
**CSR（Certificate Signing Request，证书签名请求）**：用于申请证书的请求，在制作 CSR 文件时，需要使用私钥文件。CSR 文件必须由 CA 进行签名，才可形成CRT。

**CRT颁发过程**：
- 用户产生自己的密钥对
- 将公共密钥和部分个人身份信息传递给证书授权中心
- 证书授权中心在核实用户身份后，将给用户颁发CRT，附带**CA的签名**
CA自己也拥有一套的CRT和私钥。根 CA 的证书是**自签名的（self-signed）**，一般是通过操作系统和浏览器厂商**预置**在系统/浏览器内而被信任。Windows查看本机证书：`certmgr.msc`。

**证书链（Certificate Chain）**：一连串X.509 CRT，典型格式为：Leaf（终端/网站证书） → Intermediate CA（中间CA） → … → Root CA（根CA）
	**Leaf（终端/网站证书）**：包含域名、公钥等，由某个 CA 签名
	**Intermediate CA（中间CA）**：由更高一级 CA 签名，用来隔离根CA风险、分层管理
	**Root CA（根CA）**：自签名证书，作为**信任锚 trust anchor**预置在系统/浏览器里，证书链一定包含根CA。
	CRT间的连接逻辑：签发和被签发关系，并且可以用签发者（issuer）的公钥验证被签发者（subject）的证书签名，直至达到自签名的根证书。
	
**CRL（Certificate Revocation List，证书吊销列表）**：
    用于指定证书发布者认为无效的证书列表。CRL 一定是被 CA 签署的，CRL 中包含被吊销的证书的序列号。CA 可以指定证书被吊销的起始日期，也可以在证书吊销列表中加入吊销证书的理由。在吊销的证书到期之后，CRL 中的有关条目会被删除，以缩短 CRL 列表的大小。
    在验证签名期间，应用程序可以检查 CRL，以确定给定证书和密钥对是否可信。如果不可信，应用程序可以判断吊销的理由或日期对使用有疑问的证书是否有影响。**如果日期早于该证书被吊销的日期，那么该证书仍被认为有效。**
    应用程序在获得 CRL 之后，可以缓存下来，在它到期之前，一直使用它。**如果 CA 发布了新 CRL，那么拥有有效 CRL 的应用程序不会使用新 CRL，直到应用程序拥有的 CRL 到期为止**

**CRT工作原理**：
- 使用接收双方约定的 HASH 算法对报文计算出固定位数的摘要。在数学上保证：只要改动报文中的任何一位，重新计算出的报文摘要值就与原先的值不相符。这样即可保证报文的不可更改性
- 使用发送者的私钥对报文摘要值进行加密，然后连同原报文一起发送给接收者，加密后的报文摘要即为数字签名
- 接收方收到数字签名后，用同样的 HASH 算法对原报文计算出报文摘要值，然后与使用发送者的公开密钥对数字签名进行解密得到的报文摘要值进行比较，如果相等，则说明报文确实来自所称的发送者
- 之所以对报文摘要进行加密，而不是对原报文进行加密，是因为 RSA 加解密非常耗时，被加密的报文越大，耗时越多
# OpenSSL
创建一个自签名根 CA 证书，同时生成这个 CA 的私钥：
```
openssl req -x509 -newkey rsa -out cacert.pem -outform PEM -days 365 -config /c/Users/z00955590/Desktop/workplace/MyCA/openssl.cnf
或
OPENSSL_CONF=/var/MyCA/openssl.cnf openssl req -x509 -newkey rsa -out cacert.pem -outform PEM -days 365
```
查看生成的CRT：`openssl x509 -in cacert.pem -text -noout`

## 使用 OpenSSL 生成证书
- 创建私钥
```
openssl genrsa -out prvtkey.pem 2048 # without password protected
openssl genrsa -des3 -out prvtkey.pem 2048 # password protected
```
- 根据私钥创建CSR文件 cert.csr
```
openssl req -new -key prvtkey.pem -out cert.csr

# Common Name很重要，填域名
Common Name (e.g. server FQDN or YOUR name) []: www.baidu.com
```
- 使用自建的 CA 为 CSR 文件签名
    `openssl ca -in cert.csr -config /var/MyCA/openssl.cnf`，输入CA私钥的密码123456
    生成的证书将被放到 certs/ 目录，并且 index.txt 和 serial 的内容也将发生变化
## 验证证书
接下来使用两个 Python 程序验证证书。
client.py：
```python
import socket
import ssl

def main():
    context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
    context.load_verify_locations(<CA 根证书>)
    with socket.create_connection(("127.0.0.1", 9443)) as sock:
        with context.wrap_socket(sock, server_hostname=<域名>) as ssock:
            msg = "do I connect with server ?".encode("utf-8")
            ssock.send(msg)
            msg = ssock.recv(1024).decode("utf-8")
            print(f"receive msg from server: {msg}")
            ssock.close()
if __name__ == "__main__":
    main()
```
server.py：
```python
import ssl
import socket

def main():
    context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
    context.load_cert_chain(<证书>, <私钥>, <密码>)
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0) as sock:
        sock.bind(('127.0.0.1', 9443))
        sock.listen(5)
        with context.wrap_socket(sock, server_side=True) as ssock:
            while True:
                client_socket, addr = ssock.accept()
                msg = client_socket.recv(1024).decode("utf-8")
                print(f"receive msg from client {addr}:{msg}")
                msg = f"yes, you have connected with server.\r\n".encode("utf-8")
                client_socket.send(msg)
                client_socket.close()

if __name__ == "__main__":
    main()
```

#### 证书的原理：用户提供私钥和csr文件，第三方机构对私钥和csr文件进行签名


##  一、证书签名请求（CSR）生成


- 场景1 无私钥，同时生成私钥和csr文件
   生成产生证书的csr文件和私钥
```
	openssl req \
       -newkey rsa:2048 -nodes -keyout domain_private.key \
       -out domain.csr
```

- 场景2 有私钥，只生成csr文件
 根据已有私钥生成csr
```
	openssl req \
        -key domain_private.key \
        -new -out domain.csr
```

- 场景3 续订证书时，无csr文件。需要根据证书和私钥生成csr
```
	openssl x509 \
       -in domain.crt \
       -signkey domain_private.key \
       -x509toreq -out domain.csr
```
- 场景4 根据私钥导出公钥（可选步骤）
```
	openssl rsa 
       -in domain_private.key 
       -pubout > domain_public.key
```


## 二、证书生成
### 场景1 自签名证书，同样可以使用https安全传输数据，但需要手动添加信任
1、无私钥情况下，创建一个2048位的私钥（domain.key）以及一个自签名证书（domain.crt），-x509选项指出我们要创建自签名证书，-days 365选项声明该证书的有效期为365天。 在上面的命令执行过程中将创建一个临时CSR来收集与证书相关的CSR信息。
```
	openssl req \
       -newkey rsa:2048 -nodes -keyout domain.key \
       -x509 -days 365 -out domain.crt
```
2、有私钥情况下，创建自签名证书
```
	openssl req \
       -key domain.key \
       -new \
       -x509 -days 365 -out domain.crt
```
#### 场景2 第三方机构签发，终端内置根证书，不需要手动添加信任
1、模拟创建CA机构证书及私钥
```
	mkdir demoCA 
	cd demoCA 
	mkdir certs crl newcerts 
	touch index.txt serial 
	echo 00 > serial 
	cd ..
```
2、生成ca密钥
```
	openssl genrsa \
		-out ca.key 2048
```
3、生成ca证书
```
	openssl req \
		-new -x509 \
		-key ca.key \
		-out ca.crt
```
4、用ca对服务器证书请求文件进行签名
```
	openssl ca \
		-in server_req.csr \
		-out server.crt \
		-cert ca.crt \
		-keyfile ca.key \
		-config /usr/ssl/openssl.cnf
```
5、把服务端的私钥和已签名的证书合并到一个pkcs12格式的文件（可选）
```
	openssl pkcs12 \
		-export -out server.pfx \
		-inkey domain_private.key \
		-in domain.crt
```
6、把pkcs12格式转化为java常用的jks格式（可选）
```
	keytool -importkeystore -v \
		-srckeystore server.pfx \
		-srcstoretype pkcs12 \
		-srcstorepass 123456 \
		-destkeystore server.jks \
		-deststoretype jks \
		-deststorepass 123456
```


## 三、证书查看
 证书和CSR文件都采用PEM编码格式
#### 3.1 查看CSR条目
```
openssl req -text -noout -verify -in domain.csr
```
#### 3.2 查看CER证书，可以指定格式
```
openssl x509 -in domain.crt -noout -text
openssl x509 -inform pem -in domain.crt -noout -text
openssl x509 -inform der -in domain.crt -noout -text
```
#### 3.2 下面的命令可以查看证书文件的明文文本：
```
openssl x509 -text -noout -in domain.crt
```
#### 3.3 验证证书是否由CA签发
下面的命令用来验证证书doman.crt是否由证书颁发机构（ca.crt）签发：
```
openssl verify -verbose -CAFile ca.crt domain.crt
```
## 证书格式转换
我们之前接触的证书都是X.509格式，采用ASCII的PEM编码。还有其他 一些证书编码格式与容器类型。OpenSSL可以用来在众多不同类型之间 转换证书。这一部分主要介绍与证书格式转换相关的OpenSSL命令
- 1. PEM转DER，可以将PEM编码的证书domain.crt转换为二进制DER编码的证书domain.der（DER格式通常用于Java）
```
	openssl x509 \  
	   -in domain.crt \  
	   -outform der -out domain.der
```
- 2. DER转PEM
同样，可以将DER编码的证书（domain.der）转换为PEM编码（domain.crt）
```
	openssl x509 \  
	   -inform der -in domain.der \  
	   -out domain.crt
```
- 3. PEM转PKCS7，可以将PEM证书（domain.crt和ca-chain.crt）添加到一个PKCS7（domain.p7b） 文件中
```
	openssl crl2pkcs7 -nocrl \
       -certfile domain.crt \
       -certfile ca-chain.crt \
       -out domain.p7b
```

使用-certfile选项指定要添加到PKCS7中的证书。
PKCS7文件也被称为P7B，通常用于Java的Keystore和微软的IIS中保存证书的ASCII文件。

- 4. PKCS7转换为PEM，使用下面的命令将PKCS7文件（domain.p7b）转换为PEM文件
```
	openssl pkcs7 \
       -in domain.p7b \
       -print_certs -out domain.crt
```
如果PKCS7文件中包含多个证书，例如一个普通证书和一个中间CA证书，那么输出的 PEM文件中将包含所有的证书。
- 5. PEM转换为PKCS12，可以将私钥文件（domain.key）和证书文件（domain.crt）组合起来生成PKCS12 文件（domain.pfx）
```
	openssl pkcs12 \
       -inkey domain.key \
       -in domain.crt \
       -export -out domain.pfx
```
上面的命令将提示你输入导出密码，可以留空不填。
PKCS12文件也被称为PFX文件，通常用于导入/导出微软IIS中的证书链。
- 6. PKCS12转换为PEM
也可以将PKCS12文件（domain.pfx）转换为PEM格式（domain.combined.crt）
```
	openssl pkcs12 \
       -in domain.pfx \
       -nodes -out domain.combined.crt
```
注意如果PKCS12文件中包含多个条目，例如证书及其私钥，那么生成的PEM 文件中将包含所有条目。



## 公私钥生成
- 生成私钥
```
openssl genrsa -out rsa_private_key.pem 2048
```

- 把RSA私钥转换成PKCS8格式
```
openssl pkcs8 -topk8 -in rsa_private_key.pem -out pkcs8_rsa_private_key.pem -nocrypt
```
- 生成公钥
```
openssl rsa -in rsa_private_key.pem -out rsa_public_key.pem -pubout
```
- 加密私钥
```
	openssl rsa -des3 \  
	   -in unencrypted.key \  
	   -out encrypted.key
```
- 解密私钥
```
	openssl rsa \  
		-in encrypted.key \  
		-out decrypted.key
```
## Keytool工具
keytool主要可以帮我们：
- 创建一个新的JKS(Java Key Store)文件（里面包含了一个新生成的服务器密钥）
- 导出一个CSR(Certificate Signung Request)证书申请文件
- 导入一个签名后的证书文件到jks文件中

以下是操作步骤：
- 生成新的jks文件：
  ```keytool -genkeypair -alias server -keyalg RSA -keystore server.jks ```
- 导出证书请求文件：
```keytool -certreq -alias server -file server.csr -keystore server.jks```
- 用ca对请求文件进行签名（ca的生成请参考上面）：
```openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config demoCA/config/openssl.cnf```
- 导入已签名的证书到jks：
```keytool -importcert -alias server -file server.crt -keystore server.jks```


- [] http://blog.hubwiz.com/2019/03/08/openssl-essentials/
- [] https://juejin.cn/post/6844904001868136455




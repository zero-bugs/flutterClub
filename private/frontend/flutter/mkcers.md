##证书的原理：用户提供私钥和csr文件，第三方机构对私钥和csr文件进行签名


## 一、证书请求生成


##场景1 无私钥，同时生成私钥和csr文件
####生成产生证书的csr文件和私钥
```
openssl req \
       -newkey rsa:2048 -nodes -keyout domain.key \
       -out domain.csr
```

##场景2 有私钥，只生成csr文件
####根据已有私钥生成csr
```
openssl req \
        -key domain.key \
        -new -out domain.csr
```

##场景3 续订证书时，无csr文件。需要根据证书和私钥生成csr
```
openssl x509 \
       -in domain.crt \
       -signkey domain.key \
       -x509toreq -out domain.csr
```


## 二、证书生成
### 场景1 自签名证书，同样可以使用https安全传输数据，但需要手动添加信任
### 无私钥情况下，创建一个2048位的私钥（domain.key）以及一个自签名证书（domain.crt），-x509选项指出我们要创建自签名证书，-days 365选项声明该证书的有效期为365天。 在上面的命令执行过程中将创建一个临时CSR来收集与证书相关的CSR信息。
```
openssl req \
       -newkey rsa:2048 -nodes -keyout domain.key \
       -x509 -days 365 -out domain.crt
```

#### 有私钥情况下，创建自签名证书
```
openssl req \
       -key domain.key \
       -new \
       -x509 -days 365 -out domain.crt
```
#### 场景2 第三方机构签发，终端内置根证书，不需要手动添加信任

## 三、证书查看
#### 证书和CSR文件都采用PEM编码格式
#### 3.1 查看CSR条目
```
openssl req -text -noout -verify -in domain.csr
```

#### 3.2 查看CER证书，可以指定格式
```
openssl x509 -in cerfile.cer -noout -text
openssl x509 -inform pem -in cerfile.cer -noout -text
openssl x509 -inform der -in cerfile.cer -noout -text
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
cer格式证书转pem格式：
```
openssl x509 -inform der -in apple_pay.cer -out apple_pay_certificate.pem
```

- [] http://blog.hubwiz.com/2019/03/08/openssl-essentials/

##附公私钥生成：
####生成私钥
```
openssl genrsa -out rsa_private_key.pem 2048
```

####把RSA私钥转换成PKCS8格式
```
openssl pkcs8 -topk8 -in rsa_private_key.pem -out pkcs8_rsa_private_key.pem -nocrypt
```

####生成公钥
```
openssl rsa -in rsa_private_key.pem -out rsa_public_key.pem -pubout
```
